# 超大规模小说分析平台 - 设计文档

**日期**: 2026-05-21
**状态**: 待审核

---

## 1. 项目概述

### 1.1 背景

网络小说通常体量巨大（15MB+ TXT，约 500 万字），现有 AI 工具（如 GPT 网页端）无法完整读取和分析整部小说。用户需要一种能**全面、准确、交互式**分析超大文本的方案。

### 1.2 目标

构建一个云端 Web 应用，支持用户上传超长篇小说，通过分层 LLM 分析引擎，实现：
- 故事线/情节发展分析
- 人物关系图谱
- 重要场景/名场面提取
- 世界观/设定提取
- 写作风格分析
- 全文/章节摘要
- 基于全文的对话问答

### 1.3 核心约束

- 单文件可达 15MB+（约 300 万 tokens）
- API 调用有速率限制（RPM/TPM），需批处理和限流
- 分析需要数分钟到数十分钟，必须异步
- 跨块分析结果需保持全局一致性
- 成本控制：不同分析层使用不同模型

---

## 2. 系统架构

### 2.1 技术栈

| 层 | 技术 | 理由 |
|---|---|---|
| 前端 | Next.js (React) + TypeScript | SSR/SEO，React 可视化生态 |
| 后端 API | Python FastAPI | 异步高性能，NLP 生态 |
| 任务队列 | Redis + Celery | 异步任务调度 |
| 数据库 | PostgreSQL + pgvector | 关系数据 + 向量检索一体化 |
| 缓存 | Redis | 任务状态、分析结果缓存 |
| LLM | OpenAI / Claude / 国内 API | 多模型路由，成本优化 |

### 2.2 架构图

```
┌─────────────────────────────────────────────────────────┐
│                    前端 (Next.js)                        │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────┐  │
│  │ 文件管理  │ │ 对话界面  │ │ 图谱可视化 │ │ 抽屉面板    │  │
│  └──────────┘ └──────────┘ └──────────┘ └────────────┘  │
└────────────────────────┬────────────────────────────────┘
                         │ REST + WebSocket
┌────────────────────────▼────────────────────────────────┐
│                  API Gateway (FastAPI)                    │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────┐  │
│  │ 文件上传  │ │ 任务管理  │ │ 结果查询  │ │ 对话/问答   │  │
│  └──────────┘ └──────────┘ └──────────┘ └────────────┘  │
└────────────────────────┬────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────┐
│              异步任务队列 (Redis + Celery)                 │
│  优先级队列: 实时分析 > 后台分析 > 低成本摘要              │
└────────────────────────┬────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────┐
│              分析引擎 (Python Analysis Layer)              │
│  ┌──────────────────────────────────────────────────┐    │
│  │  L1: 预处理 (分章/分块/清洗)                       │    │
│  │  L2: 摘要生成 (章节→卷→全书)                       │    │
│  │  L3: 人物提取 (角色/属性/出场轨迹)                  │    │
│  │  L4: 关系图谱 (人物关系/势力/阵营)                  │    │
│  │  L5: 世界观提取 (设定/体系/地理/时间线)             │    │
│  │  L6: 风格分析 (节奏/文风/质量评估)                  │    │
│  │  L7: 场景提取 (名场面/高潮/转折点)                  │    │
│  └──────────────────────────────────────────────────┘    │
└────────────────────────┬────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────┐
│              LLM Router & Batch Manager                   │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────────┐  │
│  │ 模型路由     │  │ 批量调度器    │  │ 限流/重试管理器  │  │
│  │ 摘要→便宜   │  │ 请求聚合      │  │ Rate Limit      │  │
│  │ 人物→强模型  │  │ 并发控制      │  │ Exponential Back│  │
│  │ 关系→强模型  │  │ 优先级队列    │  │ Circuit Breaker │  │
│  │ 风格→便宜   │  │ Chunk合并     │  │ Fallback模型    │  │
│  └─────────────┘  └──────────────┘  └─────────────────┘  │
│  ┌─────────────────────────────────────────────────────┐  │
│  │  Batch API Adapter (OpenAI/Claude/智谱/通义)         │  │
│  └─────────────────────────────────────────────────────┘  │
└────────────────────────┬────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────┐
│                    存储层                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────────┐  │
│  │ PostgreSQL   │  │ pgvector     │  │  Redis Cache   │  │
│  │ (结构化数据)  │  │ (语义检索)    │  │  (任务/缓存)    │  │
│  └──────────────┘  └──────────────┘  └────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

---

## 3. 分析层设计

### 3.1 层依赖关系

```
Layer 1 (预处理)
    │
    ├──→ Layer 2 (摘要) ──→ 可独立查询
    │
    ├──→ Layer 3 (人物) ──┐
    │                     ├──→ Layer 4 (关系图谱)
    │                     │
    ├──→ Layer 5 (世界观) ──→ 可独立查询
    │
    ├──→ Layer 6 (风格) ──→ 可独立查询
    │
    └──→ Layer 7 (场景) ──→ 可独立查询
```

### 3.2 各层详细说明

#### Layer 1: 文本预处理
- **输入**: 原始 TXT 文件
- **处理**:
  - 编码自动检测 (UTF-8 / GBK / Big5)
  - 清洗 (去除乱码、广告、无关内容)
  - 章节识别 (正则模式 + LLM 辅助)
  - 语义分块 (~4000 tokens/块，不跨章节，保留 200 tokens 重叠)
- **输出**: Chunk 列表，带元数据 (chapter, chunk_index, token_count, boundary_type)

#### Layer 2: 摘要生成
- **模型**: gpt-4o-mini (便宜)
- **处理**: 每章生成摘要 → 每卷汇总 → 全书摘要
- **输出**: 三级摘要 (chapter / volume / full)

#### Layer 3: 人物提取
- **模型**: gpt-4o (强模型)
- **处理**:
  - 逐块提取人物 (名称、别名、描述、上下文)
  - 全局实体消歧 + 别名合并
  - 计算重要性评分 (出场次数、对话占比、情节影响)
- **输出**: 人物列表 (name, aliases[], description, importance_score, appearances[])

#### Layer 4: 关系图谱
- **模型**: gpt-4o (强模型)
- **依赖**: Layer 3 输出
- **处理**:
  - 基于归一化人物，提取两两关系
  - 关系类型: 师徒/敌对/情侣/亲属/盟友/上下级
  - 关系强度: 1-5 星
  - 证据链: 记录支撑关系的 chunk_ids
- **输出**: 关系列表 (from_char, to_char, relation_type, strength, evidence[])

#### Layer 5: 世界观提取
- **模型**: gpt-4o (强模型)
- **处理**: 按类别提取
  - power_system: 功法/修炼体系/等级
  - geography: 地点/地图/区域
  - faction: 势力/宗门/组织
  - timeline: 时间线/历史事件
- **输出**: 分类条目 (category, name, description, attributes{}, related_chunks[])

#### Layer 6: 风格分析
- **模型**: gpt-4o-mini (便宜)
- **处理**:
  - 章节节奏分析 (快/中/慢)
  - 文风变化检测
  - 章节质量评分 (基于情节密度、对话比例、描写丰富度)
- **输出**: 节奏曲线数据、文风标签、章节评分

#### Layer 7: 场景提取
- **模型**: gpt-4o (强模型)
- **处理**:
  - 识别重要场景类型: battle / climax / turning_point / romance / revelation
  - 提取场景摘要、关键人物、重要性评分
- **输出**: 场景列表 (title, type, chapter_range, summary, significance_score, key_characters[])

### 3.3 全局一致性保障

```
Phase 1: 局部提取
  每个 chunk 独立提取 → 原始列表

Phase 2: 全局归一化
  汇总所有局部结果 → LLM 实体消歧 + 别名合并
  输入: [("萧炎", "主角"), ("炎帝", "称号"), ("萧师弟", "称呼")]
  输出: 统一实体 "萧炎", aliases: ["炎帝", "萧师弟"]

Phase 3: 关系/场景合并
  基于归一化实体合并跨块结果
  冲突解决: 最强证据 / 时间线最新 / LLM 仲裁
```

---

## 4. LLM 调用管理

### 4.1 模型路由策略

| 分析层 | 推荐模型 | 原因 |
|---|---|---|
| 摘要 (L2) | gpt-4o-mini | 摘要任务不需要强推理 |
| 人物 (L3) | gpt-4o | 需要准确识别和消歧 |
| 关系 (L4) | gpt-4o | 关系推理需要强模型 |
| 世界观 (L5) | gpt-4o | 设定提取需要理解上下文 |
| 风格 (L6) | gpt-4o-mini | 统计型分析，不需要强推理 |
| 场景 (L7) | gpt-4o | 场景识别需要情节理解 |
| 问答 | gpt-4o | 回答质量直接影响体验 |
| Embedding | text-embedding-3-small | 专用嵌入模型 |

### 4.2 批调用策略

- **聚合窗口**: 30-60 秒内收集请求，合并为 Batch API 提交
- **优先级队列**: 实时分析 > 后台自动分析 > 低成本摘要
- **并发控制**: 根据 API 的 RPM/TPM 动态调整
- **降级策略**: 限流时自动降级到备用模型或排队

### 4.3 成本估算 (15MB 小说 ≈ 500 万字 ≈ 300 万 tokens)

| 层 | 模型 | 预估 tokens | 成本 |
|---|---|---|---|
| 摘要 | gpt-4o-mini | ~500K | ~$0.15 |
| 人物提取 | gpt-4o | ~800K | ~$8.00 |
| 关系图谱 | gpt-4o | ~600K | ~$6.00 |
| 世界观 | gpt-4o | ~400K | ~$4.00 |
| 风格分析 | gpt-4o-mini | ~300K | ~$0.10 |
| 场景提取 | gpt-4o | ~500K | ~$5.00 |
| **合计** | | ~3.1M | **~$23** |

---

## 5. 数据存储设计

### 5.1 核心表结构

```sql
-- 小说主表
novels (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    title VARCHAR(255),
    author VARCHAR(255),
    file_size BIGINT,
    token_count INTEGER,
    chunk_count INTEGER,
    status VARCHAR(50),  -- uploading / processing / ready / failed
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)

-- 文本分块
chunks (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    chapter INTEGER,
    chapter_title VARCHAR(255),
    chunk_index INTEGER,
    text TEXT,
    token_count INTEGER,
    is_dialogue_heavy BOOLEAN,
    boundary_type VARCHAR(50)
)

-- 分析任务
analysis_jobs (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    layer VARCHAR(50),  -- summary / character / relationship / worldbuilding / style / scene
    status VARCHAR(50),  -- pending / running / completed / failed
    progress FLOAT,
    started_at TIMESTAMP,
    completed_at TIMESTAMP,
    error_message TEXT
)

-- 人物
characters (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    name VARCHAR(255),
    aliases TEXT[],
    description TEXT,
    importance_score FLOAT,
    first_appearance INTEGER,  -- chunk_index
    last_appearance INTEGER,
    appearances INTEGER[],  -- chunk_ids
    attributes JSONB
)

-- 人物关系
relationships (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    from_char_id UUID REFERENCES characters(id),
    to_char_id UUID REFERENCES characters(id),
    relation_type VARCHAR(100),  -- master_disciple / enemy / lover / family / ally / superior
    strength INTEGER,  -- 1-5
    description TEXT,
    evidence_chunks INTEGER[]
)

-- 世界观条目
worldbuilding (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    category VARCHAR(100),  -- power_system / geography / faction / timeline
    name VARCHAR(255),
    description TEXT,
    attributes JSONB,
    related_chunks INTEGER[]
)

-- 重要场景
scenes (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    title VARCHAR(255),
    type VARCHAR(100),  -- battle / climax / turning_point / romance / revelation
    start_chapter INTEGER,
    end_chapter INTEGER,
    summary TEXT,
    significance_score FLOAT,
    key_character_ids UUID[]
)

-- 摘要
summaries (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    level VARCHAR(50),  -- chapter / volume / full
    chapter_start INTEGER,
    chapter_end INTEGER,
    text TEXT
)

-- 向量嵌入 (pgvector)
embeddings (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    chunk_id UUID REFERENCES chunks(id),
    embedding vector(1536),
    metadata JSONB
)

-- 对话历史
chat_sessions (
    id UUID PRIMARY KEY,
    novel_id UUID REFERENCES novels(id),
    user_id UUID REFERENCES users(id),
    created_at TIMESTAMP
)

chat_messages (
    id UUID PRIMARY KEY,
    session_id UUID REFERENCES chat_sessions(id),
    role VARCHAR(50),  -- user / assistant
    content TEXT,
    metadata JSONB,  -- 引用来源、触发的分析层等
    created_at TIMESTAMP
)
```

### 5.2 索引策略

```sql
-- 向量检索
CREATE INDEX ON embeddings USING ivfflat (embedding vector_cosine_ops);

-- 常用查询
CREATE INDEX ON chunks (novel_id, chapter, chunk_index);
CREATE INDEX ON characters (novel_id, importance_score DESC);
CREATE INDEX ON relationships (novel_id, from_char_id, to_char_id);
CREATE INDEX ON scenes (novel_id, type, significance_score DESC);
CREATE INDEX ON analysis_jobs (novel_id, layer, status);

-- 全文搜索 (可选，作为向量检索的补充)
CREATE INDEX ON chunks USING gin (to_tsvector('chinese', text));
```

---

## 6. 前端交互设计

### 6.1 核心理念：对话即界面

主界面是**对话窗口**，所有分析能力通过对话自然触发。可视化组件（图谱、时间线、面板）作为对话中的嵌入组件或按需滑出的抽屉面板。

### 6.2 页面结构

**Dashboard (首页)**
- 上传区域 (Drag & Drop)
- 对话历史列表 (过往分析会话)
- 文件管理 (已上传小说列表 + 状态)

**Chat Interface (主界面)**
- 对话流 (核心交互区)
- 消息类型:
  - 纯文本消息
  - 嵌入组件 (图谱、时间线、卡片、表格)
  - 进度消息 (分析任务实时状态)
  - 快捷操作按钮 (展开/筛选/导出)
- 输入框 (支持自然语言 + 快捷命令)
- 右侧抽屉 (按需滑出: 原文定位、人物详情、关系详情、世界观总览)

### 6.3 对话驱动的分析触发

| 用户输入示例 | 系统行为 |
|---|---|
| "帮我分析这本书" | 启动 L1-L7 全量分析，对话中推送进度 |
| "人物关系怎么样" | 未分析 → 触发 L3+L4；已分析 → 展示结果 |
| "画出关系图" | 对话流中嵌入可交互力导向图 |
| "列出重要场景" | 触发 L7 或查询已分析结果 |
| "第几章讲了什么" | 向量检索 + 原文定位 |
| "对比萧炎和魂天帝" | 多维度对比卡片嵌入对话 |
| "萧炎的成长轨迹" | 时间线组件嵌入对话 |

### 6.4 分析进度融入对话

```
[AI] 正在分析《XXX》...

✅ 文本预处理完成 (2.3s)
🔄 正在生成章节摘要... 45/320 章
⏳ 人物提取排队中...

[AI] 摘要已完成，要我先展示概览吗？还是继续等待全部分析完成？
```

### 6.5 可视化组件

- **关系图谱**: D3.js / Cytoscape.js 力导向图，嵌入对话流
- **故事时间线**: 横向时间轴，支持按卷/章切换
- **人物卡片**: 头像、简介、出场次数、重要性、关联人物
- **风格图表**: Recharts/ECharts 节奏曲线、章节评分柱状图
- **场景卡片**: 类型标签、摘要、关键人物、原文定位

---

## 7. 问答系统设计

### 7.1 检索流程

```
用户问题
  │
  ▼
Query Expansion (LLM 扩展同义词、别名、相关概念)
  │
  ▼
混合检索
  ├── 向量检索 (Top-20 chunks by similarity)
  ├── 人物索引 (问题提到人名 → 定位相关 chunks)
  └── 场景索引 (问题涉及场景 → 定位场景 chunks)
  │
  ▼
Rerank (Cross-encoder 精排 Top-5)
  │
  ▼
Context Assembly (组装上下文: 前后 chunk + 章节信息)
  │
  ▼
LLM 回答 (带引用要求: 必须标注出处章节)
```

### 7.2 回答格式

```
[AI] 萧炎和药老是师徒关系。

药老在第三章首次出现，当时他是附身在萧炎戒指中的灵魂体。
萧炎拜药老为师，学习炼药术和斗技。

关键节点:
• 第3章 初次相遇 [查看原文→]
• 第45章 药老真实身份暴露 [查看原文→]
• 第128章 陨落心炎事件中药老被救 [查看原文→]

[人物关系详情] [查看完整时间线]
```

---

## 8. 错误处理与恢复

| 场景 | 处理策略 |
|---|---|
| API 限流 | 指数退避重试 → 切换备用模型 → 加入等待队列 |
| 某层分析失败 | 标记该层 failed，其他层继续，用户可手动重跑 |
| 文件编码异常 | 自动尝试 GBK/UTF-8/Big5，失败则提示用户 |
| 超长章节 (>50K tokens) | 递归二次分块，保持层级关系 |
| 分析中断 | 断点续传，已完成的层不重复执行 |
| 网络中断 | WebSocket 重连，任务状态持久化 |

---

## 9. 成本控制

- **预分析估算**: 上传后先估算 token 量和成本，用户确认后再开始
- **分层模型路由**: 便宜模型用于摘要/风格，强模型用于人物/关系/场景
- **Batch API**: 非实时任务批量提交，成本降低 ~50%
- **结果缓存**: 相同分析请求直接返回缓存
- **用户配额**: 免费/付费 tier 限制分析次数和文件大小

---

## 10. 项目目录结构 (初步)

```
novel-analyzer/
├── frontend/                    # Next.js 前端
│   ├── src/
│   │   ├── app/                 # App Router 页面
│   │   ├── components/
│   │   │   ├── chat/            # 对话相关组件
│   │   │   ├── graph/           # 关系图谱组件
│   │   │   ├── timeline/        # 时间线组件
│   │   │   ├── upload/          # 上传组件
│   │   │   └── panels/          # 抽屉面板组件
│   │   ├── hooks/               # 自定义 hooks
│   │   ├── lib/                 # 工具函数
│   │   └── types/               # TypeScript 类型
│   └── package.json
│
├── backend/                     # Python FastAPI 后端
│   ├── app/
│   │   ├── api/                 # API 路由
│   │   ├── core/                # 配置、安全
│   │   ├── models/              # SQLAlchemy 模型
│   │   ├── schemas/             # Pydantic schemas
│   │   └── services/            # 业务逻辑
│   ├── analysis/                # 分析引擎
│   │   ├── layers/              # L1-L7 分析层
│   │   ├── chunking/            # 分块逻辑
│   │   ├── llm/                 # LLM 路由和批处理
│   │   └── consistency/         # 全局一致性处理
│   ├── tasks/                   # Celery 任务
│   └── requirements.txt
│
├── migrations/                  # 数据库迁移
├── docker/                      # Docker 配置
├── docs/                        # 文档
└── README.md
```

---

## 11. 实施阶段规划

### Phase 1: 基础架构 (Week 1-2)
- 项目脚手架搭建 (Next.js + FastAPI + PostgreSQL + Redis)
- 用户认证 + 文件上传
- 文本预处理 (Layer 1)
- 基础对话界面

### Phase 2: 核心分析 (Week 3-4)
- 摘要生成 (Layer 2)
- 人物提取 + 全局归一化 (Layer 3)
- 关系图谱 (Layer 4)
- 异步任务队列 + 进度推送

### Phase 3: 高级分析 (Week 5-6)
- 世界观提取 (Layer 5)
- 风格分析 (Layer 6)
- 场景提取 (Layer 7)
- LLM 路由 + 批处理 + 限流

### Phase 4: 问答与可视化 (Week 7-8)
- 向量检索 + 混合问答系统
- 关系图谱可视化 (D3.js)
- 时间线、人物卡片、风格图表
- 抽屉面板 + 原文定位

### Phase 5: 优化与打磨 (Week 9-10)
- 成本控制 + 配额管理
- 错误处理 + 断点续传
- 性能优化 + 缓存策略
- UI/UX 打磨
