## 问题描述

### 3.1 问题背景

本文研究在数据驱动的需求预测下，第四方物流（4PL）视角的新能源汽车动力电池供应链网络优化问题。所考虑的供应链网络为三层结构：**原材料供应商 ，电池制造环节（自建工厂或第三方外部采购） ，整车制造工厂**。与传统确定性供应链设计不同，本文同时纳入以下关键要素：（1）基于情景的多周期随机需求预测，刻画整车厂需求不确定性；（2）第三方电池供应商的包裹式组合采购（Winner Determination Problem, WDP），允许决策者灵活选择自建电池工厂或外部采购的混合策略；（3）全网络碳排放约束与碳超额惩罚机制，响应碳中和政策约束；（4）设施开通与路径建立的跨周期一次性固定成本，通过指示变量实现多周期耦合。在此基础上，构建以总成本最小化为目标的多周期、多情景混合整数非线性规划模型，协同优化原材料供应网络、电池生产与采购网络以及电池分销网络。

定义有向图 $\mathcal{G} = (\mathcal{N}, \mathcal{E})​$ 表示供应链网络，其中：

| 节点类型 | 符号 | 含义 |
|---------|------|------|
| 原材料供应商 | $g \in G$ | 提供锂、钴、镍等电池原材料的上游节点 |
| 电池生产工厂 | $s \in S$ | 候选自建电池工厂，将原材料加工为动力电池 |
| 电池外部供应商 | $j \in J$ | 以包裹形式提供成品电池的第三方供应商 |
| 汽车生产工厂 | $f \in F$ | 下游整车制造节点，产生电池需求 |

| 边类型 | 符号 | 含义 |
|--------|------|------|
| 原材料物流路径 | $l \in L_{gs}$ | 原材料供应商 $g$ 至电池工厂 $s$ 的运输路线 |
| 电池物流路径 | $l \in L_{sf}$ | 电池工厂 $s$ 至整车工厂 $f$ 的运输路线 |
| 外部采购信息流 | $k \in K_j$ | 第三方供应商 $j$ 的包裹选项，表示采购合约配置 |

网络具有如下拓扑特征：**多层有向结构**，原材料层、电池制造/采购层与整车需求层逐级向下游传递，原材料物流路径与电池物流路径构成物理层，包裹采购选项构成信息-合约层。

### 3.3 数学模型

#### 3.3.1 目标函数

模型以全周期总期望成本最小化为目标，包含六类成本构成：原材料供应商启用固定成本、电池工厂建设固定成本、原材料运输路径开通固定成本、电池运输路径开通固定成本、第三方供应商开通固定成本，以及情景依赖的运行成本期望值。

$$
\begin{aligned}
\min \quad Z = &\underbrace{\sum_{t \in T} \sum_{g \in G} v_{gt} y_{gt} (1 - R_{gt})}_{\text{原材料供应商启用成本}}
+ \underbrace{\sum_{t \in T} \sum_{s \in S} H_{st} y_{st} (1 - R_{st})}_{\text{电池工厂建设成本}} \\
&+ \underbrace{\sum_{t \in T} \sum_{g \in G} \sum_{s \in S} \sum_{l \in L_{gs}} h_{gslt} x_{gslt} (1 - r_{gslt})}_{\text{原材料路径开通成本}} \\
&+ \underbrace{\sum_{t \in T} \sum_{s \in S} \sum_{f \in F} \sum_{l \in L_{sf}} h_{sflt} x_{sflt} (1 - r_{sflt})}_{\text{电池路径开通成本}} \\
&+ \underbrace{\sum_{t \in T} \sum_{j \in J} \sum_{k \in K_j} v_{jt} p_{jkt} (1 - \gamma_{jt})}_{\text{第三方供应商开通成本}} \\
&+ \underbrace{\frac{1}{|\Xi|} \sum_{t \in T} \sum_{\varsigma \in \Xi} \mathcal{Q}_{t\varsigma}}_{\text{运行成本期望值}}
\end{aligned}
$$

其中情景运行成本 $\mathcal{Q}_{t\varsigma}$ 定义为：

$$
\begin{aligned}
\mathcal{Q}_{t\varsigma} = &\sum_{g \in G} \sum_{s \in S} \sum_{l \in L_{gs}} \sum_{m \in M} c^{R}_{gmt} z^{R}_{gslmt\varsigma}
+ \sum_{g \in G} \sum_{s \in S} \sum_{l \in L_{gs}} \sum_{m \in M} c^{TR}_{gslmt} z^{R}_{gslmt\varsigma} \\
&+ \sum_{s \in S} c_{st} sz_{st\varsigma}
+ \sum_{s \in S} \sum_{f \in F} \sum_{l \in L_{sf}} kc_{sflt} z_{sflt\varsigma} \\
&+ \bar{C}_{\varsigma t} e_{\varsigma}
+ \sum_{f \in F} p_{ft} \phi_{ft\varsigma}
+ \sum_{j \in J} \sum_{k \in K_j} b_{jkt} \varpi_{jkt\varsigma}
\end{aligned}
$$

该运行成本依次包含：原材料采购成本、原材料运输成本、电池加工成本、电池运输成本、碳超额惩罚成本、整车厂缺货惩罚成本、第三方电池采购成本。

全网络碳排放量定义为：

$$
\begin{aligned}
E_{t\varsigma} = &\sum_{g \in G} e^{FG}_{gt} y_{gt} (1 - R_{gt})
+ \sum_{s \in S} e^{FS}_{st} y_{st} (1 - R_{st}) \\
&+ \sum_{g \in G} \sum_{s \in S} \sum_{l \in L_{gs}} \sum_{m \in M} e^{P}_{gmt} z^{R}_{gslmt\varsigma}
+ \sum_{g \in G} \sum_{s \in S} \sum_{l \in L_{gs}} \sum_{m \in M} e^{RT}_{gslmt} z^{R}_{gslmt\varsigma} \\
&+ \sum_{s \in S} e^{M}_{st} sz_{st\varsigma}
+ \sum_{s \in S} \sum_{f \in F} \sum_{l \in L_{sf}} e^{BT}_{sflt} z_{sflt\varsigma}
+ \sum_{j \in J} \sum_{k \in K_j} e^{J}_{jkt} \varpi_{jkt\varsigma}
\end{aligned}
$$

碳排放依次来源于：原材料供应商启用、电池工厂启用、原材料采购、原材料运输、电池加工、电池运输、第三方电池采购等环节。

#### 3.3.2 约束条件

**（1）包裹唯一采购约束。** 每个第三方供应商在每个周期内最多选择一个采购包裹：

$$\sum_{k \in K_j} p_{jkt} \leq 1, \quad \forall j \in J,\ \forall t \in T$$

**（2）采购数量约束。** 各周期选择的第三方供应商包裹总数需介于上下界之间：

$$N_{\min}^{t} \leq \sum_{j \in J} \sum_{k \in K_j} p_{jkt} \leq N_{\max}^{t}, \quad \forall t \in T$$

**（3）碳排放容量约束。** 每个情景下各周期碳排放量不超过碳配额与碳超额购买量之和：

$$E_{t\varsigma} \leq Cap_t + \bar{C}_{\varsigma t}, \quad \forall t \in T,\ \forall \varsigma \in \Xi$$

**（4）原材料分品种供应上限约束。** 各原材料供应商对各类原材料的供应量不超过其供给能力，且仅在被启用时方可供应：

$$\sum_{s \in S} \sum_{l \in L_{gs}} z^{R}_{gslmt\varsigma} \leq M^{R}_{gmt} y_{gt}, \quad \forall g \in G,\ m \in M,\ t \in T,\ \varsigma \in \Xi$$

**（5）原材料消耗平衡约束。** 电池工厂的原材料投入量与产出量满足物料平衡关系（$a_m$ 为单位电池对第 $m$ 类原材料的消耗系数）：

$$\sum_{g \in G} \sum_{l \in L_{gs}} z^{R}_{gslmt\varsigma} \geq a_m \cdot sz_{st\varsigma}, \quad \forall s \in S,\ m \in M,\ t \in T,\ \varsigma \in \Xi$$

**（6）电池工厂启用约束。** 电池工厂仅在已被启用时方可进行生产加工（$\Lambda$ 为足够大的常数）：

$$sz_{st\varsigma} \leq \Lambda \cdot y_{st}, \quad \forall s \in S,\ \forall t \in T,\ \forall \varsigma \in \Xi$$

**（7）电池工厂产出-运输平衡约束。** 电池工厂的总运出量不超过其加工产出量：

$$\sum_{f \in F} \sum_{l \in L_{sf}} z_{sflt\varsigma} \leq sz_{st\varsigma}, \quad \forall s \in S,\ \forall t \in T,\ \forall \varsigma \in \Xi$$

**（8）电池工厂生产能力上限约束。** 电池工厂加工量不超过其额定产能：

$$sz_{st\varsigma} \leq Sq_{st}, \quad \forall s \in S,\ \forall t \in T,\ \forall \varsigma \in \Xi$$

**（9）原材料运输量约束。** 每条原材料物流路径的实际运输量不超过其运输能力，且仅在被开通时方可运输：

$$\sum_{m \in M} z^{R}_{gslmt\varsigma} \leq q_{gslt} \cdot x_{gslt}, \quad \forall g \in G,\ s \in S,\ l \in L_{gs},\ t \in T,\ \varsigma \in \Xi$$

**（10）电池运输量约束。** 电池物流路径的运输量不超过其运输能力，且仅在被开通时方可运输：

$$z_{sflt\varsigma} \leq q_{sflt} \cdot x_{sflt}, \quad \forall s \in S,\ f \in F,\ l \in L_{sf},\ t \in T,\ \varsigma \in \Xi$$

**（11）电池流通量守恒约束。** 对每个整车工厂，自建工厂运入量与第三方采购到货量之和等于需求量减去缺货量（$A_{fjkt}$ 为指示包裹 $k$ 是否覆盖工厂 $f$ 的 0-1 辅助矩阵）：

$$\sum_{s \in S} \sum_{l \in L_{sf}} z_{sflt\varsigma} + \sum_{j \in J} \sum_{k \in K_j} A_{fjkt} \varpi_{jkt\varsigma} = \xi_{ft\varsigma} - \phi_{ft\varsigma}, \quad \forall f \in F,\ \forall t \in T,\ \forall \varsigma \in \Xi$$

**（12）第三方原材料供应上限约束。** 从各原材料供应商采购的每类原材料不超过其外部供给上限：

$$\sum_{s \in S} \sum_{l \in L_{gs}} z^{R}_{gslmt\varsigma} \leq U^{R}_{gmt} \cdot y_{gt}, \quad \forall g \in G,\ m \in M,\ t \in T,\ \varsigma \in \Xi$$

**（13）第三方电池采购量上限约束。** 从第三方供应商的实际采购量不超过其包裹供应能力，且仅在被选定包裹时方可采购：

$$\varpi_{jkt\varsigma} \leq p_{jkt} \cdot US_{jkt}, \quad \forall j \in J,\ k \in K_j,\ \forall t \in T,\ \forall \varsigma \in \Xi$$

**（14）连续决策变量非负约束：**

$$z^{R}_{gslmt\varsigma} \geq 0, \quad \forall g \in G,\ s \in S,\ l \in L_{gs},\ m \in M,\ t \in T,\ \varsigma \in \Xi$$

$$z_{sflt\varsigma} \geq 0, \quad \forall s \in S,\ f \in F,\ l \in L_{sf},\ t \in T,\ \varsigma \in \Xi$$

$$\phi_{ft\varsigma} \geq 0, \quad \forall f \in F,\ t \in T,\ \varsigma \in \Xi$$

$$sz_{st\varsigma} \geq 0, \quad \forall s \in S,\ t \in T,\ \varsigma \in \Xi$$

$$\varpi_{jkt\varsigma} \geq 0, \quad \forall j \in J,\ k \in K_j,\ t \in T,\ \varsigma \in \Xi$$

**（15）跨周期一次性开通费用指示变量约束。** 以下约束通过指示变量实现"仅在首次启用的周期支付固定成本，后续周期不再重复计费"的跨周期耦合逻辑：

**（15a）原材料供应商历史启用指示变量：**

$$R_{gt} = \begin{cases}
1, & \displaystyle\sum_{v=1}^{t-1} y_{gv} > 0, \quad \forall g \in G,\ t \in T \setminus \{1\} \\[6pt]
0, & \displaystyle\sum_{v=1}^{t-1} y_{gv} = 0, \quad \forall g \in G,\ t \in T \setminus \{1\} \\[6pt]
0, & \forall g \in G,\ t = 1
\end{cases}$$

**（15b）电池工厂历史启用指示变量：**

$$R_{st} = \begin{cases}
1, & \displaystyle\sum_{v=1}^{t-1} y_{sv} > 0, \quad \forall s \in S,\ t \in T \setminus \{1\} \\[6pt]
0, & \displaystyle\sum_{v=1}^{t-1} y_{sv} = 0, \quad \forall s \in S,\ t \in T \setminus \{1\} \\[6pt]
0, & \forall s \in S,\ t = 1
\end{cases}$$

**（15c）原材料路径历史开通指示变量：**

$$r_{gslt} = \begin{cases}
1, & \displaystyle\sum_{v=1}^{t-1} x_{gslv} > 0, \quad \forall g \in G,\ s \in S,\ l \in L_{gs},\ t \in T \setminus \{1\} \\[6pt]
0, & \displaystyle\sum_{v=1}^{t-1} x_{gslv} = 0, \quad \forall g \in G,\ s \in S,\ l \in L_{gs},\ t \in T \setminus \{1\} \\[6pt]
0, & \forall g \in G,\ s \in S,\ l \in L_{gs},\ t = 1
\end{cases}$$

**（15d）电池路径历史开通指示变量：**

$$r_{sflt} = \begin{cases}
1, & \displaystyle\sum_{v=1}^{t-1} x_{sflv} > 0, \quad \forall s \in S,\ f \in F,\ l \in L_{sf},\ t \in T \setminus \{1\} \\[6pt]
0, & \displaystyle\sum_{v=1}^{t-1} x_{sflv} = 0, \quad \forall s \in S,\ f \in F,\ l \in L_{sf},\ t \in T \setminus \{1\} \\[6pt]
0, & \forall s \in S,\ f \in F,\ l \in L_{sf},\ t = 1
\end{cases}$$

**（15e）第三方供应商历史启用指示变量：**

$$\gamma_{jt} = \begin{cases}
1, & \displaystyle\sum_{v=1}^{t-1} \sum_{k \in K_j} p_{jkv} > 0, \quad \forall j \in J,\ t \in T \setminus \{1\} \\[6pt]
0, & \displaystyle\sum_{v=1}^{t-1} \sum_{k \in K_j} p_{jkv} = 0, \quad \forall j \in J,\ t \in T \setminus \{1\} \\[6pt]
0, & \forall j \in J,\ t = 1
\end{cases}$$

**（16）0-1 二元决策变量约束：**

$$y_{gt} \in \{0, 1\}, \quad \forall g \in G,\ t \in T$$

$$y_{st} \in \{0, 1\}, \quad \forall s \in S,\ t \in T$$

$$x_{gslt} \in \{0, 1\}, \quad \forall g \in G,\ s \in S,\ l \in L_{gs},\ t \in T$$

$$x_{sflt} \in \{0, 1\}, \quad \forall s \in S,\ f \in F,\ l \in L_{sf},\ t \in T$$

$$p_{jkt} \in \{0, 1\}, \quad \forall j \in J,\ k \in K_j,\ t \in T$$

### 3.4 符号说明

#### 3.4.1 集合与索引

| 符号 | 含义 |
|------|------|
| $T$ | 规划周期集合，$t \in T$ |
| $G$ | 候选原材料供应商集合，$g \in G$ |
| $S$ | 候选电池生产工厂集合，$s \in S$ |
| $F$ | 汽车生产工厂集合，$f \in F$ |
| $J$ | 第三方电池供应商集合，$j \in J$ |
| $M$ | 原材料种类集合，$m \in M$ |
| $L_{gs}$ | 原材料供应商 $g$ 至电池工厂 $s$ 的候选运输路线集合 |
| $L_{sf}$ | 电池工厂 $s$ 至整车工厂 $f$ 的候选运输路线集合 |
| $K_j$ | 第三方供应商 $j$ 的采购包裹选项集合，$k \in K_j$ |
| $\Xi$ | 需求情景集合，$\varsigma \in \Xi$ |

#### 3.4.2 二元决策变量

| 符号 | 含义 |
|------|------|
| $y_{gt}$ | 周期 $t$ 是否启用原材料供应商 $g$ |
| $y_{st}$ | 周期 $t$ 是否启用电池生产工厂 $s$ |
| $x_{gslt}$ | 周期 $t$ 是否开通原材料路径 $(g,s,l)$ |
| $x_{sflt}$ | 周期 $t$ 是否开通电池路径 $(s,f,l)$ |
| $p_{jkt}$ | 周期 $t$ 是否选择第三方供应商 $j$ 的包裹 $k$ |
| $R_{gt}$ | 原材料供应商 $g$ 在周期 $t$ 之前是否已被启用过 |
| $R_{st}$ | 电池工厂 $s$ 在周期 $t$ 之前是否已被启用过 |
| $r_{gslt}$ | 原材料路径 $(g,s,l)$ 在周期 $t$ 之前是否已开通 |
| $r_{sflt}$ | 电池路径 $(s,f,l)$ 在周期 $t$ 之前是否已开通 |
| $\gamma_{jt}$ | 第三方供应商 $j$ 在周期 $t$ 之前是否已开通 |

#### 3.4.3 连续决策变量

| 符号 | 含义 |
|------|------|
| $z^{R}_{gslmt\varsigma}$ | 情景 $\varsigma$ 下第 $m$ 类原材料经路径 $(g,s,l)$ 在周期 $t$ 的运输量 |
| $z_{sflt\varsigma}$ | 情景 $\varsigma$ 下电池经路径 $(s,f,l)$ 在周期 $t$ 的运输量 |
| $sz_{st\varsigma}$ | 情景 $\varsigma$ 下电池工厂 $s$ 在周期 $t$ 的加工量 |
| $\varpi_{jkt\varsigma}$ | 情景 $\varsigma$ 下第三方供应商 $j$ 包裹 $k$ 在周期 $t$ 的实际采购量 |
| $\phi_{ft\varsigma}$ | 情景 $\varsigma$ 下整车工厂 $f$ 在周期 $t$ 的缺货量 |
| $\bar{C}_{\varsigma t}$ | 情景 $\varsigma$ 下周期 $t$ 的超额碳排放量 |

#### 3.4.4 成本参数

| 符号 | 含义 |
|------|------|
| $v_{gt}$ | 周期 $t$ 启用原材料供应商 $g$ 的固定成本 |
| $H_{st}$ | 周期 $t$ 启用电池工厂 $s$ 的固定成本 |
| $v_{jt}$ | 周期 $t$ 启用第三方供应商 $j$ 的固定成本 |
| $h_{gslt}$ | 周期 $t$ 维护原材料路径 $(g,s,l)$ 的固定成本 |
| $h_{sflt}$ | 周期 $t$ 维护电池路径 $(s,f,l)$ 的固定成本 |
| $c^{R}_{gmt}$ | 周期 $t$ 从供应商 $g$ 采购第 $m$ 类原材料的单位成本 |
| $c^{TR}_{gslmt}$ | 周期 $t$ 第 $m$ 类原材料经 $(g,s,l)$ 运输的单位成本 |
| $c_{st}$ | 周期 $t$ 电池工厂 $s$ 的单位加工成本 |
| $kc_{sflt}$ | 周期 $t$ 电池经 $(s,f,l)$ 运输的单位成本 |
| $b_{jkt}$ | 周期 $t$ 第三方供应商 $j$ 包裹 $k$ 的采购成本 |
| $p_{ft}$ | 周期 $t$ 整车工厂 $f$ 的单位缺货惩罚成本 |
| $e_{\varsigma}$ | 超额碳排放单位惩罚成本 |

#### 3.4.5 能力与供给参数

| 符号 | 含义 |
|------|------|
| $Sq_{st}$ | 周期 $t$ 电池工厂 $s$ 的加工能力上限 |
| $q_{gslt}$ | 周期 $t$ 原材料路径 $(g,s,l)$ 的运输能力上限 |
| $q_{sflt}$ | 周期 $t$ 电池路径 $(s,f,l)$ 的运输能力上限 |
| $U^{R}_{gmt}$ | 周期 $t$ 供应商 $g$ 对第 $m$ 类原材料的供给上限 |
| $US_{jkt}$ | 周期 $t$ 第三方供应商 $j$ 包裹 $k$ 的供应能力上限 |
| $N_{\min}^{t} / N_{\max}^{t}$ | 周期 $t$ 第三方供应商采购包裹数量的下/上限 |

#### 3.4.6 碳排放参数

| 符号 | 含义 |
|------|------|
| $e^{FG}_{gt}$ | 周期 $t$ 启用原材料供应商 $g$ 的固定碳排放 |
| $e^{FS}_{st}$ | 周期 $t$ 启用电池工厂 $s$ 的固定碳排放 |
| $e^{P}_{gmt}$ | 周期 $t$ 采购供应商 $g$ 第 $m$ 类原材料的单位碳排放 |
| $e^{RT}_{gslmt}$ | 周期 $t$ 第 $m$ 类原材料经 $(g,s,l)$ 运输的单位碳排放 |
| $e^{M}_{st}$ | 周期 $t$ 电池工厂 $s$ 加工的单位碳排放 |
| $e^{BT}_{sflt}$ | 周期 $t$ 电池经 $(s,f,l)$ 运输的单位碳排放 |
| $e^{J}_{jkt}$ | 周期 $t$ 第三方供应商 $j$ 包裹 $k$ 采购的单位碳排放 |
| $Cap_t$ | 周期 $t$ 全网络碳排放配额上限 |

#### 3.4.7 其他参数

| 符号 | 含义 |
|------|------|
| $\xi_{ft\varsigma}$ | 情景 $\varsigma$ 下整车工厂 $f$ 在周期 $t$ 的电池需求量 |
| $A_{fjkt}$ | 0-1 辅助矩阵，第三方供应商 $j$ 的包裹 $k$ 是否覆盖工厂 $f$ 在周期 $t$ |
| $a_m$ | 单位电池对第 $m$ 类原材料的消耗系数（物料清单系数） |
| $\Lambda$ | 足够大的正数（Big-M 常数） |
| $M^{R}_{gmt}$ | 足够大的上界常数 |

---

**模型特征总结：** 上述模型为**多周期、多情景、多商品流的混合整数非线性规划（MINLP）**，其非线性来源于目标函数中二元变量 $y_{gt}$ 与指示变量 $(1-R_{gt})$ 的乘积项，实质为分段线性结构，可通过线性化技巧转化为等价的混合整数线性规划（MILP）进行求解。