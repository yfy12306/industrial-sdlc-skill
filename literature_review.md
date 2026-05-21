# 2. 文献综述

本章从四个相互关联的研究领域对现有文献进行系统回顾：新能源汽车动力电池制造供应链网络设计、胜者确定问题（WDP）的应用、时间序列需求预测方法以及数据驱动优化框架。各子方向严格按照时间脉络组织，梳理研究演进路径，并在末尾指出研究空白。

## 2.1 新能源汽车动力电池制造供应链网络设计

新能源汽车动力电池制造供应链（EVB Manufacturing Supply Chain, EVBMSC）的网络设计研究伴随全球电动汽车产业的崛起而逐步展开。本节按时间顺序梳理该领域从微观关系管理到宏观网络韧性、从确定性优化到不确定性决策、从单一设施选址到第四方物流整合的演进历程。该领域的早期研究主要聚焦于动力电池供应链的微观层面关系管理和回收网络设计。Kalaitzi 等（2019）率先通过多案例研究揭示了动力电池制造网络中核心供应商作为"协调者"的关键角色，指出网络设计并非静态的物理连接，而是需要有效的协调策略来管理多层级资源依赖关系，为后续网络结构优化研究奠定了概念基础。Wang 等（2020）从电动汽车制造商视角提出了考虑碳排放的动力电池回收网络优化模型，构建了包含回收、再制造和处置三种处理策略的网络配置框架，通过中国某电动汽车制造商的实证案例证明了碳税政策对网络选址决策的显著影响，开启了动力电池供应链碳约束研究的先河。随着第四方物流（4PL）模式在供应链管理中的兴起，研究视角开始从传统的企业内部优化转向跨组织协同。Huang 等（2021）在确定性环境下将客户满意度纳入第四方物流网络设计问题，构建了以最大化供需双方满意度为目标的优化模型。Wang 等（2021）进一步整合了供应商和客户的满意度指标，在有限预算约束下为行业创新者第四方物流提供商设计了网络优化方案。Yin 等（2022）将设施与第三方物流的双重中断风险纳入第四方物流网络设计，在不确定性环境下构建了随机规划模型，标志着该领域研究从确定性假设向不确定性决策的重要转变。面对全球地缘政治紧张局势的加剧，网络设计的关注点进一步转向韧性（Resilience）和客户行为。Chen 等（2023）从动态演化视角分析了动力电池供应链的建立过程，强调协同创新与信息共享模块是形成抵御地缘政治冲突的"软防御"机制不可或缺的组成部分。Huang 等（2024）利用复杂网络理论对动力电池制造网络的拓扑结构进行了定量重构，识别出当前无标度网络结构中的安全脆弱性，并通过网络重构提升了供应链抵御针对性攻击的能力。Zhang 等（2024）率先开展了面向服务的制造供应链研究，将服务时间和交付数量作为两个独立的服务水平维度，从4PL视角探讨了有限理性客户行为对多周期分销网络设计的影响。Yin 等（2024）将临时外包服务策略纳入多周期第四方物流网络设计，提出了应对需求溢出问题的动态机制，进一步增强了供应链应对不确定性的能力。最新研究呈现出多维度融合的趋势。Li 等（2025）对东盟地区电池电动汽车生产网络的分布进行了系统分析，揭示了现代动力电池制造网络设计的核心焦点已转向跨国区域比较优势的整合，旨在建立专业化的劳动分工体系。Çakır 和 Serdarasan（2025）提出了考虑循环经济的多目标动力电池供应链网络设计模型，通过马尔可夫链建模电池健康状态转换，同时优化经济、环境和社会三重目标。Han 等（2025）构建了考虑需求不确定性的动力电池回收与再制造供应链鲁棒优化模型，采用模糊随机规划与条件风险价值（CVaR）方法提升网络的整体韧性与稳健性。Zhang 等（2025a）将客户满意度驱动的需求波动机制融入网络设计，量化了忽视客户心理因素所导致的利润偏差。Zhang 等（2025b）从可行性视角深化了网络设计研究，同时纳入敏捷性、韧性和生存可持续性，并开发了嵌入双层 Q-learning 算法的协同超启发式求解方法。Zhang 等（2025c）将碳配额交易政策纳入绿色第四方物流网络设计，量化了服务时间约束下成本与碳排放的权衡关系。Zhang 等（2026）进一步引入承诺服务时间和后悔行为概念，将决策者的心理偏差纳入多周期第四方物流网络设计模型。

尽管现有研究在动力电池供应链网络设计的多个维度取得了显著进展，但多数研究仍将设施选址决策与外部采购策略割裂处理，缺乏从4PL整合视角对自建产能与第三方采购的协同优化。此外，现有网络设计研究普遍采用确定性需求假设或预设分布的随机规划，未能将数据驱动的需求预测结果直接嵌入网络优化框架，导致预测质量与决策效果之间的反馈机制尚未被充分探索。

## 2.2 胜者确定问题（Winner Determination Problem, WDP）

胜者确定问题（WDP）作为组合拍卖的核心挑战，其研究经历了从公共资源分配到供应链采购应用、从确定性模型到不确定性优化、从单一资源分配到多目标协同的演进过程。本节按时间顺序梳理该领域的发展脉络。

**（1）理论奠基阶段（1982–2003）：组合拍卖机制与计算复杂性分析**

WDP 的研究起源于稀缺公共资源的高效分配。Rassenti 等（1982）在机场时段分配中的开创性应用确立了 WDP 作为大规模资源配置有效工具的地位，首次提出了组合拍卖机制的设计框架。Rothkopf 等（1998）系统分析了组合拍卖的计算可管理性问题，证明了在特定投标结构下 WDP 可以在多项式时间内求解，为后续算法设计奠定了理论基础。De Vries 和 Vohra（2003）对组合拍卖进行了全面综述，系统性地刻画了组合拍卖作为揭示物品间价值依赖关系并实现最优资源配置的有效工具的理论框架。Sandholm（2002）提出了 WDP 的最优求解算法，深入分析了该问题的计算复杂性，奠定了精确求解算法的理论基础。

**（2）不确定性建模阶段（2010–2019）：随机规划与鲁棒优化方法**

早期 WDP 研究主要采用确定性模型，而实际场景中存在显著的不确定性。Ma 等（2010）率先将随机规划方法引入卡车运输采购的 WDP 模型中，在货运量不确定性下构建了两阶段随机规划框架。Remli 和 Rekik（2013）提出了不确定性货运量下的鲁棒 WDP 模型，采用鲁棒优化方法处理需求波动。Zhang 等（2015）进一步开发了可处理的两阶段鲁棒 WDP 模型，通过组合拍卖实现卡车运输服务采购的鲁棒决策。Hu 等（2016）将 transit time 约束纳入运输服务采购问题，拓展了 WDP 在实际物流场景中的应用边界。Remli 等（2019）将货运量和承运商能力的双重不确定性纳入扩展模型，构建了更为全面的鲁棒优化框架。

**（3）可持续发展与灵活性扩展阶段（2020–2022）：绿色属性与中断风险**

随着供应链制造焦点向可持续性、灵活性和韧性转移，WDP 的应用范围持续扩展。Qian 等（2020）将中断风险和混合缓解策略整合到两阶段随机 WDP 模型中，构建了应对运输服务采购拍卖中不确定性的灵活采购策略。Qian 等（2021）将供应商的可持续性属性纳入 WDP 模型，为损失厌恶的第四方物流提供商设计了多属性逆向拍卖中的绿色第三方物流供应商选择机制。Yin 等（2021）将中断风险和数量折扣同时整合到物流服务采购拍卖的 WDP 模型中，构建了更为灵活的采购策略。Khakdaman 等（2022）量化了物流服务的灵活性并将其纳入决策框架，分析了托运人使用灵活运输服务的意愿。Abusalama 等（2022）在应急救援场景中通过改进 WDP 算法提升了任务分配与路线调度的效率，拓展了 WDP 在紧急物流中的应用。

**（4）前沿集成阶段（2023–2025）：风险指标、区块链与4PL视角**

最新研究呈现出 WDP 与复杂优化问题深度集成的趋势。Li 等（2023）提出了预算约束下的两阶段样本鲁棒 WDP 模型，通过引入风险指标显著降低了成本超支预期和尾部风险，为运输采购中的成本控制提供了新视角。Li 等（2024）在众包物流领域引入区块链技术，通过双层分配机制解决了众包与自营配送的协同调度问题，将 WDP 从价格驱动竞争转化为包含时空约束的路径优化问题。Palacios-Huerta 等（2024）对组合拍卖在实际应用中的部署进行了全面综述，强调了行为经济学因素和信任机制在 WDP 实践中的重要性。Triki 等（2025）将碳减排绩效和折扣投标机制融入采购拍卖，在碳税和碳配额-抵消两种碳排放法规下构建了绿色 WDP 模型，实现了成本降低与效率提升的双重目标。Zhou 等（2025）从第四方物流视角研究了生鲜农产品物流服务采购拍卖中的 WDP 问题，同时考虑了时效性和可持续性，开发了基于拉丁超立方采样的近似求解算法。

**研究空白：** 尽管 WDP 在运输服务采购和物流资源配置领域已建立了坚实的理论基础，但将其嵌入多周期供应链网络设计（ND）问题的研究仍然稀缺。现有 WDP 研究主要聚焦于单一采购周期的资源分配，缺乏与长期设施选址、产能规划和多周期需求波动的协同优化框架。此外，WDP 与动力电池供应链网络的集成尚未被探索，特别是在考虑碳约束和需求预测嵌入的场景下，如何通过 WDP 机制实现自建产能与外部采购的动态平衡仍是一个开放问题。

## 2.3 时间序列需求预测方法

需求预测（Demand Forecasting, DF）作为供应链管理的核心环节，其方法论经历了从经典统计模型到机器学习、从浅层神经网络到深度学习、从单一模型到混合架构的演进历程。本节按时间顺序梳理预测方法的发展脉络。

**（1）经典统计模型阶段（1976–2013）：线性模型与计量经济学方法**

需求预测的理论基础可追溯至 Sasser（1976）提出的服务业供需匹配框架，奠定了需求预测在运营管理中的核心地位。Chandra 和 Fisher（1994）系统分析了生产与分销计划协调中需求预测的关键作用，证明了高质量预测对供应链整体效率的直接影响。传统需求预测主要基于经典线性模型。得益于其对线性趋势的强大解释能力，自回归积分滑动平均（ARIMA）模型及其季节性变体被广泛应用于电力负荷预测（Wang 等, 2012），线性回归模型则被用于预测意大利全国电力消费（Bianco 等, 2013）。Haberleitner 等（2010）利用提前订单信息实现了需求规划系统的有效部署，验证了传统统计方法在工业实践中的适用性。然而，传统计量经济学模型受限于线性假设，难以准确捕捉现实世界中普遍存在的非线性动态关系（Gao 等, 2023b）。

**（2）机器学习过渡阶段（2001–2015）：人工神经网络与混合模型**

为克服传统方法的局限性，研究者的注意力逐渐转向机器学习。Alon 等（2001）在零售销售预测的对比研究中率先证明了人工神经网络（ANNs）相较于传统方法的预测优势，开启了机器学习在需求预测领域的应用先河。Valenzuela 等（2008）将 ARIMA 的线性建模能力与神经网络的非线性映射能力相结合，开发了混合预测模型，通过先处理线性分量再进行非线性建模的策略在时间序列预测任务中取得了优越性能。Wang 和 Yao（2012）研究了低质量数据条件下模型可靠性和泛化能力的维持方法，为机器学习在实际供应链场景中的应用提供了理论支撑。Liu 等（2015）提出了处理缺失数据的张量分解方法，拓展了机器学习在不完整数据环境下的适用性。

**（3）深度学习兴起阶段（1997–2021）：RNN、LSTM 与 GRU**

深度学习技术的快速发展推动了需求预测方法的根本性变革。Hochreiter 和 Schmidhuber（1997）提出的长短期记忆网络（LSTM）通过门控机制有效解决了传统 RNN 的梯度消失问题，为序列数据建模奠定了理论基础。Villegas 等（2018）将支持向量机应用于需求预测中的模型选择，为预测方法的系统评估提供了框架。Sagheer 和 Kotb（2019）利用深度 LSTM 循环网络进行石油产量时间序列预测，证明了 LSTM 在处理长序列依赖关系中的优越性能。Weng 等（2019）将 LightGBM 与 LSTM 组合模型应用于供应链销售预测，验证了混合深度学习方法在实际场景中的有效性。Abbasimehr 等（2020）利用优化的 LSTM 网络进行需求预测，在家具企业客户需求预测任务中取得了显著优于传统方法的效果。Marques da Fonseca（2020）系统比较了统计方法与 LSTM 网络在生鲜产品需求预测中的表现，进一步验证了深度学习的优势。Chandriah 和 Naraganahalli（2021）采用改进 Adam 优化器的 RNN/LSTM 方法进行汽车零部件需求预测，提升了深度学习模型的收敛性能。Li 等（2021）开发了 GRU-Prophet 复合模型并引入注意力机制，在服装销售预测领域取得了优异表现，证明了 GRU 因与 LSTM 相当的性能和更简化的架构而受到青睐。Perrusquía 和 Yu（2021）对循环神经网络和强化学习在非线性系统识别与最优控制中的应用进行了全面综述。Fan 等（2021）将 ARIMA-LSTM 混合模型应用于油井产量预测，通过线性与非线性分量的解耦建模实现了预测精度的显著提升。

**（4）Transformer 与前沿阶段（2014–2025）：自注意力机制与系统综述**

近年来，Transformer 模型在时间序列预测领域的应用日益增多。Cho 等（2014）提出的编码器-解码器架构为序列到序列预测提供了基础框架。Vaswani 等（2017）提出的自注意力机制使模型能够并行处理整个序列并直接捕捉任意两个位置之间的依赖关系，在长期依赖建模方面展现出显著潜力。Abolghasemi 等（2020）通过 843 个真实需求时间序列的实证分析，系统评估了不同预测模型在需求波动场景下的表现，发现混合模型在不同变异系数（CoV）的需求序列中均表现出稳健的预测精度和库存性能。Gao 等（2023a）开发了基于平滑组 Lasso 的区间二型模糊神经网络，在特征选择与系统识别方面取得了突破。Gao 等（2023b）系统评估了统计算法在具有不同方差和季节性特征的数据缺失填补中的有效性。Park 等（2023）将多步 Transformer 架构应用于模型预测控制，证明了其在复杂工业预测任务中的有效性。Walter 等（2025）对人工智能在供应链需求规划中的应用进行了系统文献综述，识别出 AI 工具与技术、供应链功能应用以及对数字化 SCM 的影响三个知识集群，为未来研究指明了方向。

**研究空白：** 尽管 LSTM、GRU 和 Transformer 等深度学习方法在时间序列预测领域取得了显著进展，但现有研究主要基于预测精度指标（如 MAPE、RMSE）评估模型性能，忽视了预测结果对最终供应链决策质量的实际影响。不同预测模型在供应链网络优化中的决策效果差异尚未被系统比较，如何从决策质量视角评估和选择预测模型仍缺乏理论框架。此外，将多种预测模型（GRU、LSTM、Transformer）的预测结果直接嵌入多周期随机规划网络设计框架的研究仍然稀缺。

## 2.4 数据驱动优化框架

数据驱动优化方法作为一种从数据中学习并指导决策的范式，其研究经历了从理论探索到供应链应用、从单一不确定性处理到多目标协同优化、从预测与决策分离到集成框架的演进过程。本节按时间顺序梳理该领域的发展脉络。

**（1）理论奠基阶段（1989–2002）：数据驱动发现与不确定性表征**

数据驱动方法的理论基础可追溯至 Langley 和 Zytkow（1989）提出的数据驱动经验发现方法，将数据学习视为科学研究的重要途径。Reichert 和 Vanrolleghem（2001）早期研究确立了模型对不确定性增强表征能力的必要性，为后续数据驱动不确定性建模奠定了概念基础。Borsuk 等（2002）应用贝叶斯推断通过观测数据更新模型参数后验分布，将数据驱动方法与概率推理相结合，实现了更准确的决策风险评估。

**（2）数据质量与泛化能力阶段（2012–2015）：低质量数据下的模型可靠性**

随着数据驱动方法在实际应用中的推广，数据质量问题逐渐成为研究焦点。Wang 和 Yao（2012）系统分析了多类别不平衡问题的潜在解决方案，为低质量数据条件下的模型可靠性提供了理论支撑。Liu 等（2015）提出了处理缺失数据的迹范数正则化张量分解方法，拓展了数据驱动方法在不完整数据环境下的适用性。

**（3）供应链网络设计应用阶段（2020–2022）：数据驱动不确定性集与滚动时域**

在供应链网络设计领域，数据驱动方法开始被系统性地应用于不确定性建模。Fattahi（2020）提出了考虑社会关切的不确定性下数据驱动供应链网络设计方法，通过数据驱动的不确定性集构建替代传统分布假设，避免了因错误分布假设导致的次优决策。Fattahi 和 Govindan（2022）进一步开发了数据驱动滚动时域方法，用于中断和需求不确定性下的动态供应链分销网络设计，实现了多周期决策的自适应调整。

**（4）前沿集成阶段（2024–2025）：鲁棒优化、区块链与生成模拟**

最新研究呈现出数据驱动方法与新兴技术深度集成的趋势。Lotfi 等（2024）将开放创新和区块链技术作为提升供应链反脆弱性的策略，通过数据驱动鲁棒优化模型验证了其在成本降低方面的有效性，同时考虑了 CO₂ 排放和能源消耗的可持续性约束。Gao 等（2024）研究了考虑不确定需求和碳配额交易政策的双渠道闭环供应链数据驱动鲁棒优化问题，将数据驱动不确定性集与碳约束相结合。Yahyapour Ganji 等（2025）在循环经济背景下利用数据驱动方法论优化回收中心选址，建立了兼具灵活性和响应性的闭环网络。Kolcheva 等（2025）基于实时乘客和车辆数据建立了在线控制框架，通过动态调整策略显著提升了公交网络的运营效率。Bai 等（2025）提出了基于生成模拟和迭代决策策略的供应链优化框架，将自回归建模与历史-未来双感知决策模型相结合，在真实数据集上显著提升了及时交付率和利润。

**研究空白：** 尽管数据驱动优化框架在供应链网络设计中取得了显著进展，但现有研究普遍将预测和决策作为分离的任务处理，构成了数据驱动决策系统的根本性局限。在大多数情况下，预测模型主要基于预测精度进行评估，而其在支持决策过程中的最终作用往往被忽视。因此，如何从最终决策质量的视角评估和选择预测模型尚未被充分探索。此外，将时间序列预测（GRU、LSTM、Transformer）、胜者确定问题（WDP）和第四方物流网络设计（4PLND）进行深度集成以解决动力电池供应链复杂挑战的研究仍然稀缺。

## 2.5 本文贡献

针对上述研究空白，本文研究数据驱动的多周期新能源汽车动力电池制造供应链网络设计问题，其特征是将需求预测嵌入胜者确定问题，并从第四方物流视角进行建模。本文的创新性在于通过决策结果评估预测模型的有效性。具体而言，本文采用三种不同的时间序列预测模型（GRU、LSTM、Transformer）进行未来需求预测，随后基于预测结果构建数据驱动的两阶段随机规划模型，实现物流服务采购与供应网络结构的协同优化。通过整合第四方物流网络设计、数据驱动方法、胜者确定问题和时间序列预测技术，提升了多周期动力电池供应链系统的灵活性、服务水平和决策智能化程度。

---

## 参考文献

Abbasimehr, H., Shabani, M., Yousefi, M., 2020. An optimized model using LSTM network for demand forecasting. Computers & Industrial Engineering 143, 106435.

Abolghasemi, M., Beh, E., Tarr, G., Gerlach, R., 2020. Demand forecasting in supply chain: The impact of demand volatility in the presence of promotion. Computers & Industrial Engineering 142, 106380.

Abusalama, J., Razali, S., Choo, Y.H., 2022. An enhanced approach for solving winner determination problem in reverse combinatorial auctions. Indonesian Journal of Electrical Engineering and Computer Science 28, 934.

Alon, I., Qi, M., Sadowski, R.J., 2001. Forecasting aggregate retail sales: A comparison of artificial neural networks and traditional methods. Journal of Retailing and Consumer Services 8, 147–156.

Bai, H., Wang, H., Gong, N., Wang, X., Ying, W., Chen, H., Fu, Y., 2025. Supply chain optimization via generative simulation and iterative decision policies. arXiv preprint arXiv:2507.07355.

Bianco, V., Manca, O., Nardini, S., 2013. Linear regression models to forecast electricity consumption in Italy. Energy Sources, Part B: Economics, Planning, and Policy 8, 86–93.

Bishop, C.M., 2006. Pattern Recognition and Machine Learning. Springer-Verlag, New York.

Borsuk, M.E., Stow, C.A., Reckhow, K.H., 2002. Predicting the frequency of water quality standard violations: A probabilistic approach for TMDL development. Environmental Science & Technology 36, 2109–2115.

Çakır, M.S., Serdarasan, S., 2025. A multi-objective approach for designing a circular supply chain for electric vehicle batteries. Journal of Cleaner Production 504, 145438.

Chandriah, K.K., Naraganahalli, R.V., 2021. RNN/LSTM with modified Adam optimizer in deep learning approach for automobile spare parts demand forecasting. Multimedia Tools and Applications 80, 26145–26159.

Chandra, P., Fisher, M.L., 1994. Coordination of production and distribution planning. European Journal of Operational Research 72, 503–517.

Chen, Y., Wen, X., Liao, K., 2023. The establishment of electric vehicle supply chain and its resilience to supply chain interruptions. International Journal of Technology, Policy and Management 23, 410–430.

Cho, K., Van Merriënboer, B., Gulçehre, Ç., Bahdanau, D., Bougares, F., Schwenk, H., Bengio, Y., 2014. Learning phrase representations using RNN encoder–decoder for statistical machine translation. Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing (EMNLP), pp. 1724–1734.

De Vries, S., Vohra, R.V., 2003. Combinatorial auctions: A survey. INFORMS Journal on Computing 15, 284–309.

Fan, D., Sun, H., Yao, J., Zhang, K., Yan, X., Sun, Z., 2021. Well production forecasting based on ARIMA-LSTM model considering manual operations. Energy 220, 119708.

Fattahi, M., 2020. A data-driven approach for supply chain network design under uncertainty with consideration of social concerns. Annals of Operations Research 288, 265–284.

Fattahi, M., Govindan, K., 2022. Data-driven rolling horizon approach for dynamic design of supply chain distribution networks under disruption and demand uncertainty. Decision Sciences 53, 150–180.

Gao, T., Wang, C., Zheng, J., Wu, G., Ning, X., Bai, X., Yang, J., Wang, J., 2023a. A smoothing group lasso based interval type-2 fuzzy neural network for simultaneous feature selection and system identification. Knowledge-Based Systems 280, 111028.

Gao, Y., Lu, S., Cheng, H., Liu, X., 2024. Data-driven robust optimization of dual-channel closed-loop supply chain network design considering uncertain demand and carbon cap-and-trade policy. Computers & Industrial Engineering 187, 109811.

Gao, Y., Semiromi, M.T., Merz, C., 2023b. Efficacy of statistical algorithms in imputing missing data of streamflow discharge imparted with variegated variances and seasonalities. Environmental Earth Sciences 82, 476.

Haberleitner, H., Meyr, H., Taudes, A., 2010. Implementation of a demand planning system using advance order information. International Journal of Production Economics 128, 518–526.

Han, B., Wang, M., Xu, Y., Park, Y., 2025. An electric vehicle battery recycling and remanufacturing supply chain network design with sustainability and robustness under demand uncertainty. Journal of Environmental Management 390, 126202.

Hochreiter, S., Schmidhuber, J., 1997. Long short-term memory. Neural Computation 9, 1735–1780.

Hu, Q., Zhang, Z., Lim, A., 2016. Transportation service procurement problem with transit time. Transportation Research Part B: Methodological 86, 19–36.

Huang, M., Dong, L., Kuang, H., Jiang, Z.Z., Lee, L.H., Wang, X., 2021. Supply chain network design considering customer psychological behavior: A 4PL perspective. Computers & Industrial Engineering 159, 107484.

Huang, M., Ren, L., Lee, L.H., Wang, X., 2015. 4PL routing optimization under emergency conditions. Knowledge-Based Systems 89, 126–133.

Huang, Z., Zhou, Y., Lin, Y., Zhao, Y., 2024. Resilience evaluation and enhancing for China's electric vehicle supply chain in the presence of attacks: A complex network analysis approach. Computers & Industrial Engineering 195, 110416.

Kalaitzi, D., Matopoulos, A., Clegg, B., 2019. Managing resource dependencies in electric vehicle supply chains: A multi-tier case study. Supply Chain Management: An International Journal 24, 256–270.

Khakdaman, M., Rezaei, J., Tavasszy, L., 2022. Shippers' willingness to use flexible transportation services. Transportation Research Part A: Policy and Practice 160, 1–20.

Langley, P., Zytkow, J.M., 1989. Data-driven approaches to empirical discovery. Artificial Intelligence 40, 283–312.

Li, N., Zhang, Y., Tiwari, S., Kou, G., 2023. Winner determination problem with purchase budget for transportation procurement under uncertain shipment volume. Transportation Research Part E: Logistics and Transportation Review 176, 103182.

Li, Y., Guan, C., Purwanto, A.J., Chen, B., Zhao, Y., 2025. Distribution of the ASEAN battery electric vehicle production network: Mapping the interplay of endowments, policies, and global integration. Energy for Sustainable Development 85, 101649.

Li, Y., Yang, Y., Zhu, K., Zhang, J., 2021. Clothing sale forecasting by a composite GRU–Prophet model with an attention mechanism. IEEE Transactions on Industrial Informatics 17, 8335–8344.

Li, Y., Zhao, J., Chen, Z., 2024. Analyzing the delivery determination problem of new retail stores considering crowdsourcing under the background of blockchain. Research in Transportation Business & Management 52, 101083.

Liu, Y., Shang, F., Jiao, L., Cheng, J., Cheng, H., 2015. Trace norm regularized CANDECOMP/PARAFAC decomposition with missing data. IEEE Transactions on Cybernetics 45, 2437–2448.

Lotfi, R., Hazrati, R., Aghakhani, S., Afshar, M., Amra, M., Ali, S.S., 2024. A data-driven robust optimization in viable supply chain network design by considering Open Innovation and Blockchain Technology. Journal of Cleaner Production 436, 140369.

Ma, Z., Kwon, R.H., Lee, C., 2010. A stochastic programming winner determination model for truckload procurement under shipment uncertainty. Transportation Research Part E: Logistics and Transportation Review 46, 49–60.

Marques da Fonseca, R.A., 2020. A comparison on statistical methods and long short-term memory network forecasting the demand of fresh fish products. Master's thesis, University of Porto.

Palacios-Huerta, I., Parkes, D.C., Steinberg, R., 2024. Combinatorial auctions in practice. Journal of Economic Literature 62, 517–553.

Park, J., Babaei, M.R., Munoz, S.A., Venkat, A.N., Hedengren, J.D., 2023. Simultaneous multistep transformer architecture for model predictive control. Computers & Chemical Engineering 178, 108392.

Perrusquía, A., Yu, W., 2021. Identification and optimal control of nonlinear systems using recurrent neural networks and reinforcement learning: An overview. Neurocomputing 438, 145–154.

Qian, X., Chan, F.T., Yin, M., Zhang, Q., Huang, M., Fu, X., 2020. A two-stage stochastic winner determination model integrating a hybrid mitigation strategy for transportation service procurement auctions. Computers & Industrial Engineering 149, 106703.

Qian, X., Fang, S.C., Yin, M., Huang, M., Li, X., 2021. Selecting green third party logistics providers for a loss-averse fourth party logistics provider in a multiattribute reverse auction. Information Sciences 548, 357–377.

Rassenti, S.J., Smith, V.L., Bulfin, R.L., 1982. A combinatorial auction mechanism for airport time slot allocation. The Bell Journal of Economics 13, 402–417.

Reichert, P., Vanrolleghem, P.A., 2001. Identifiability and uncertainty analysis of the river water quality model no.1 (RWQM1). Water Science and Technology 43, 329–338.

Remli, N., Amrouss, A., El Hallaoui, I., Rekik, M., 2019. A robust optimization approach for the winner determination problem with uncertainty on shipment volumes and carriers' capacity. Transportation Research Part B: Methodological 123, 127–148.

Remli, N., Rekik, M., 2013. A robust winner determination problem for combinatorial transportation auctions under uncertain shipment volumes. Transportation Research Part C: Emerging Technologies 35, 204–217.

Rothkopf, M.H., Pekeč, A., Harstad, R.M., 1998. Computationally manageable combinational auctions. Management Science 44, 1131–1147.

Sagheer, A., Kotb, M., 2019. Time series forecasting of petroleum production using deep LSTM recurrent networks. Neurocomputing 323, 203–213.

Sandholm, T., 2002. Algorithm for optimal winner determination in combinatorial auctions. Artificial Intelligence 135, 1–54.

Sasser, W.E., 1976. Match supply and demand in service industries. Harvard Business Review 54, 133–140.

Triki, C., Hasan, M.R., Elomri, A., 2025. Solving the winner determination problem with discounted bids in transportation auctions. Annals of Operations Research 351, 35–58.

Valenzuela, O., Rojas, I., Rojas, F., Pomares, H., Herrera, L.J., Guillén, A., Márquez, L., Pasadas, M., 2008. Hybridization of intelligent techniques and ARIMA models for time series prediction. Fuzzy Sets and Systems 159, 821–845.

Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A.N., Kaiser, Ł., Polosukhin, I., 2017. Attention is all you need. Advances in Neural Information Processing Systems, pp. 5998–6008.

Villegas, M.A., Pedregal, D.J., Trapero, J.R., 2018. A support vector machine for model selection in demand forecasting applications. Computers & Industrial Engineering 121, 1–7.

Walter, A., Ahsan, K., Rahman, S., 2025. Application of artificial intelligence in demand planning for supply chains: A systematic literature review. The International Journal of Logistics Management 36, 672–719.

Wang, H., Huang, M., Ip, W.H., Wang, X., 2021. Network design for maximizing service satisfaction of suppliers and customers under limited budget for industry innovator fourth-party logistics. Computers & Industrial Engineering 158, 107404.

Wang, L., Wang, X., Yang, W., 2020. Optimal design of electric vehicle battery recycling network – From the perspective of electric vehicle manufacturers. Applied Energy 275, 115328.

Wang, S., Yao, X., 2012. Multiclass imbalance problems: Analysis and potential solutions. IEEE Transactions on Systems, Man, and Cybernetics, Part B (Cybernetics) 42, 1119–1130.

Wang, Y., Wang, J., Zhao, G., Dong, Y., 2012. Application of residual modification approach in seasonal ARIMA for electricity demand forecasting: A case study of China. Energy Policy 48, 284–294.

Weng, T., Liu, W., Xiao, J., 2019. Supply chain sales forecasting based on LightGBM and LSTM combination model. Industrial Management and Data Systems 120, 265–279.

Wu, C., Wang, J., Chen, X., Du, P., Yang, W., 2020. A novel hybrid system based on multi-objective optimization for wind speed forecasting. Renewable Energy 146, 149–165.

Yahyapour Ganji, V., Hozan, E., Babolhavaeji, P., Tajally, A., Ghanavati-Nejad, M., 2025. A robust design of a circular supply chain network based on the resilience and responsiveness dimensions: A data-driven model. Socio-Economic Planning Sciences 101, 102294.

Yin, M., Huang, M., Wang, D., Fang, S.C., Qian, X., Wang, X., 2024. Multi-period fourth-party logistics network design with the temporary outsourcing service under demand uncertainty. Computers & Operations Research 164, 106564.

Yin, M., Huang, M., Wang, X., Lee, L.H., 2022. Fourth-party logistics network design under uncertainty environment. Computers & Industrial Engineering 167, 108002.

Yin, M., Qian, X., Huang, M., Zhang, Q., 2021. Winner determination for logistics service procurement auctions under disruption risks and quantity discounts. Engineering Applications of Artificial Intelligence 105, 104424.

Zhang, B., Yao, T., Friesz, T.L., Sun, Y., 2015. A tractable two-stage robust winner determination model for truckload service procurement via combinatorial auctions. Transportation Research Part B: Methodological 78, 16–31.

Zhang, Y., Gao, Z., Huang, M., Jiang, S., Yin, M., Fang, S.C., 2024. Multi-period distribution network design with boundedly rational customers for the service-oriented manufacturing supply chain: A 4PL perspective. International Journal of Production Research 62, 7412–7431.

Zhang, Y., Huang, M., Cao, Z., Wang, X., Shen, Z., Zhang, J., 2026. Multi-period fourth-party logistics network design with promised service time and regret behavior. Omega 138, 103400.

Zhang, Y., Huang, M., Fu, Y., Jiang, S., Wang, X., Fang, S.C., 2025a. Mathematical modeling and optimization of multi-period fourth-party logistics network design problems with customer satisfaction-sensitive demand. Expert Systems with Applications 278, 127219.

Zhang, Y., Huang, M., Gao, Z., Jiang, S., Fang, S.C., Wang, X., 2025b. Multi-period fourth-party logistics network design from the viability perspective: A collaborative hyper-heuristic embedded with double-layer Q-learning algorithm. International Journal of Production Research 63, 3300–3330.

Zhang, Y., Huang, M., Wu, Y., Cao, Z., Lin, Y., Zhang, J., Wang, X., 2025c. Green fourth-party logistics network design under carbon cap-and-trade policy. International Journal of Production Economics 282, 109540.

Zhou, Y., Yin, M., Liu, Q., Qian, X., Jin, D., Lang, X., 2025. Optimizing winner determination for sustainability and timeliness in fresh agricultural product logistics service procurement auctions: Insights from a fourth-party logistics perspective. Frontiers in Sustainable Food Systems 9, 1585053.
