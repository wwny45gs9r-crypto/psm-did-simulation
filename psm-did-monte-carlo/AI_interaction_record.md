# AI 交互聊天记录

说明：本文件为本项目完成过程中与 AI 工具的交互记录，保留内容主要包括模拟设计、代码思路、结果解释、文献评价和报告修改相关对话。

## 1. 作业要求导入

**User：** 
作业目标

本次作业要求你使用 Monte Carlo 模拟研究 PSM-DID 方法的识别逻辑、优势、局限和适用场景。你需要自行构造数据生成过程，比较 PSM-DID、单独 PSM 和单独 DID 的估计表现，并结合模拟结果进行严格的理论解释。最后，你需要选择一篇使用 PSM-DID 的中文或英文经济学期刊论文，评价其方法适用性与结论可信度。

本作业对应课程前六讲内容：潜在结果框架、混淆变量、DAG、匹配、工具变量、面板数据与双重差分。

背景一：Monte Carlo 模拟是什么

Monte Carlo 模拟是一种用重复随机抽样来研究统计方法性质的工具。它的核心思想是：如果我们知道真实的数据生成过程，就可以在许多次重复实验中观察不同估计方法的表现。

在因果推断中，真实世界通常看不到反事实结果，因此无法直接知道估计量是否有偏。但在模拟中，你可以自行设定：

真实处理效应 
个体特征 
未观测异质性 
时间冲击 
处理分配机制 
潜在结果 
这样，每次模拟都能比较估计值 
 与真实值  的差异。

常见评价指标包括：

你也可以报告标准差、中位数偏误、置信区间覆盖率，或估计值分布图。请注意，Monte Carlo 不是简单地“跑很多次代码”，而是用可控的数据生成过程检验理论命题。

背景二：PSM-DID 的基本思想

PSM-DID 将倾向得分匹配和双重差分结合起来。它通常用于政策或项目在非随机分配下实施、但研究者可以观察到处理组和控制组在政策前后的结果时。

一个典型两期设定为：

 表示个体或地区  接受处理
 表示处理后时期， 表示处理前时期
 是结果变量
 是处理前协变量
DID 估计量比较处理组和控制组在处理前后的变化：

PSM-DID 的做法是先基于处理前协变量估计倾向得分：

然后在共同支持区域内为处理组寻找可比的控制组，最后在匹配后的样本上进行 DID 估计。

PSM-DID 的关键假设

你在作业中需要清楚说明并检验以下假设：

处理前协变量可观测

匹配只能平衡可观测的处理前变量。不要使用处理后变量进行匹配。

共同支持或重叠条件

对处理组中的个体，必须存在倾向得分相近的控制组：

条件平行趋势

在给定  或匹配后的可比样本中，如果没有处理，处理组和控制组的结果变化趋势应当相同：

SUTVA 与无提前反应

个体之间不存在处理溢出，且处理前结果没有受到未来处理预期的影响。

稳定样本与可比测量

处理前后样本构成、变量定义和测量方式不应发生会污染识别的系统性变化。

任务一：自行设计数据生成过程

请设计一个面板数据或重复截面数据的模拟环境。最低要求如下：

至少包含一个处理前协变量 
至少包含处理前和处理后两个时期
明确写出处理分配机制 
明确写出潜在结果或观测结果生成方程
明确设定真实处理效应 
至少重复模拟 500 次
可以使用 Stata、Python 或 R
你需要至少设计三个场景：

场景	目的	建议设计
场景 A	DID 表现较好的基准情形	处理组和控制组满足无条件平行趋势
场景 B	PSM-DID 可能优于 DID 的情形	趋势差异与处理前协变量  有关，匹配后条件平行趋势成立
场景 C	PSM-DID 也可能失败的情形	存在不可观测的时间变化混淆、共同支持不足、或使用错误匹配变量
你可以增加更多场景，但不能少于三个。

任务二：比较 PSM、DID 与 PSM-DID

在每个模拟场景中，请至少比较以下三类估计方法：

单独 PSM

使用处理后结果或结果变化作为被解释变量，在匹配样本上比较处理组和控制组。

单独 DID

不进行匹配，直接使用全部样本估计 DID。建议同时比较不控制协变量的基础 DID，以及控制处理前协变量的 DID 回归版本，说明两者在不同数据生成机制下的差异。

PSM-DID

先使用处理前协变量进行匹配，再在匹配样本上估计 DID。

建议报告下列结果：

各方法估计值的平均值
Bias
RMSE
标准差
估计值分布图
匹配前后协变量平衡情况
倾向得分共同支持图
请不要只报告“哪个方法更准”。你需要说明在什么数据生成机制下，为什么它更准或更差。

任务三：解释模拟结果背后的理论原因

请围绕以下问题进行理论分析：

当处理选择只由可观测协变量决定时，PSM 为什么可能有效？
当存在时间不变的未观测异质性时，DID 为什么可能有效？
当趋势差异依赖于  时，为什么 PSM-DID 可能优于未匹配 DID？
当存在时间变化的未观测混淆时，为什么 PSM-DID 仍然可能失败？
匹配会改变估计对象吗？你的 PSM-DID 估计更接近 ATE 还是 ATT？
共同支持不足时，为什么偏误和方差都可能恶化？
你的解释必须和模拟设计逐一对应。不要把理论部分写成泛泛的教材复述。

任务四：评价一篇使用 PSM-DID 的经济学论文

请在主流中文或英文学术期刊中选择一篇使用 PSM-DID 的论文。论文可以来自经济学、金融、管理、公共政策、劳动、产业组织、环境经济学等相关领域。

你的评价至少应包括：

研究问题和处理变量是什么？
作者为什么使用 PSM-DID，而不是只用 PSM 或 DID？
匹配变量是否都是处理前变量？
是否报告共同支持和平衡性检验？
DID 的平行趋势假设是否可信？作者如何检验？
是否存在不可观测的时间变化混淆或政策同时发生的问题？
估计结果更接近 ATE 还是 ATT？
根据你的模拟经验，这篇论文的识别策略最可能在哪些条件下可信，在哪些条件下脆弱？
请完整列出论文引用信息。

提交内容

请将全部材料推送到 GitHub 或 Gitee 仓库，并提交你的仓库克隆地址。仓库中应至少包含：

研究报告

Markdown 格式，建议 3000-5000 字。报告应包含问题背景、模拟设计、结果展示、理论解释、文献评价和结论。可以同时导出 PDF 或 Word 作为补充，但 Markdown 文件是必交材料。

可复现代码

Stata do 文件、R 脚本、R Markdown、Python 脚本或 Jupyter Notebook 均可。代码必须能够从头生成模拟数据、运行估计并输出主要图表。

结果图表

如果图表已经包含在报告中，可以不单独提交；否则请放入 figures/ 文件夹。

论文材料

提交所评价论文的题名、作者、期刊、年份、DOI 或稳定链接。

AI 工具使用记录

如果使用人工智能工具辅助编程、写作、调试或文献梳理，请保留全流程记录。记录应包括你的关键 prompt、工具或 agent 的主要输出、你如何判断和修改这些输出，以及最终哪些内容被采纳。要求根据你的记录，读者可以基本复现当时的对话过程或 agent 操作结果。

评分标准

项目	权重	评分要点
Monte Carlo 设计	25%	数据生成过程清楚，真实效应明确，场景设计能对应方法假设
方法实现	20%	PSM、DID、PSM-DID 实现正确，代码可复现，结果指标完整
理论解释	25%	能严格解释模拟结果背后的识别逻辑、偏误来源和估计对象变化
文献评价	20%	能结合 PSM-DID 假设评价论文适用性和可信度
写作与呈现	10%	结构清晰，图表规范，引用完整，语言准确
技术提示

Stata 可考虑使用 teffects psmatch、didregress、xtdidregress、reghdfe、psmatch2 或 kmatch。R 可考虑使用 MatchIt、fixest、tidyverse。Python 可考虑使用 pandas、numpy、statsmodels、scikit-learn。

你可以自行实现最近邻匹配，也可以使用已有包。无论使用哪种方式，都必须说明匹配方法、卡尺或邻居数、是否放回匹配，以及如何处理共同支持。

学术诚信

可以与同学讨论思路，但每位同学必须独立完成模拟设计、代码实现和报告写作。使用 AI 工具辅助编程或写作时，必须在报告末尾说明使用方式，并在仓库中提交完整 AI 使用记录。你需要对所有代码、结果解释和论文评价结论负责。

严禁复制他人代码、报告或论文评价。引用论文、教材、网页和代码片段时必须标注来源。

建议阅读

Rosenbaum, P. R., & Rubin, D. B. (1983). The central role of the propensity score in observational studies for causal effects.
Heckman, J. J., Ichimura, H., & Todd, P. E. (1997). Matching as an econometric evaluation estimator.
Smith, J. A., & Todd, P. E. (2005). Does matching overcome LaLonde’s critique of nonexperimental estimators?
Angrist, J. D., & Pischke, J. S. (2009). Mostly Harmless Econometrics.
Cunningham, S. (2021). Causal Inference: The Mixtape.
你看一下要求  

**AI：** 我看完了。这个作业复杂，但结构其实很清楚：不是让你炫代码，而是让你用 Monte Carlo 模拟把 PSM-DID 的识别条件讲明白。老师要看的不是“我跑了500次”，而是你能不能证明：

在什么样的数据生成机制下，DID 有效；
在什么样的数据生成机制下，PSM-DID 比 DID 更好；
在什么样的数据生成机制下，PSM-DID 也救不了；
然后把这个逻辑拿去评价一篇真实论文。

我建议你按下面这个路线做，稳，而且不会写散。

⸻

一、这个作业到底在考什么？

它表面上分四个任务，实际考五种能力：

1. 能不能设计合理的 DGP

DGP 就是 Data Generating Process，数据生成过程。

你不能只是随便造几列变量，而要明确写出：

* 个体协变量怎么生成：比如教育水平、收入、地区基础条件等；
* 处理变量怎么分配：谁更可能接受政策；
* 处理前结果怎么生成；
* 处理后结果怎么生成；
* 真实处理效应是多少；
* 哪些偏误是你故意放进去的。

这个是整个作业的地基。

⸻

2. 能不能比较三种方法

至少要比较：

方法	本质	适合解决什么
PSM	匹配相似个体	解决可观测选择偏误
DID	比较处理前后变化差异	解决时间不变的未观测异质性
PSM-DID	先匹配，再做差分	同时缓解可观测选择偏误和部分趋势差异

但注意，PSM-DID 不是无敌缝合怪。如果有时间变化的未观测混淆，它照样寄。

⸻

3. 能不能把模拟结果和理论假设对应起来

这点最关键。

不能写成：

场景 B 中 PSM-DID 的 Bias 最小，说明 PSM-DID 更好。

这太浅了。

要写成：

场景 B 中，处理组更可能来自较高 X 的个体，而 X 同时影响结果变化趋势，因此未匹配 DID 违反无条件平行趋势。PSM 根据处理前 X 缩小处理组与控制组的协变量差异，使匹配样本更接近条件平行趋势，因此 PSM-DID 的 Bias 和 RMSE 明显低于未匹配 DID。

这才是老师想看的。

⸻

4. 能不能评价真实论文

这个部分不是文献综述，而是方法审计。

你要像审稿人一样问：

* 它的政策是不是非随机实施？
* PSM 用的变量是不是处理前变量？
* 它有没有共同支持检验？
* 它有没有平衡性检验？
* 它的平行趋势检验可信不可信？
* 有没有同时发生的政策污染？
* 它估计的是 ATE 还是 ATT？
* 它在哪些条件下可信，在哪些条件下脆弱？

这一部分要和你的模拟结果形成呼应。

⸻

二、建议你最终仓库结构这样做

你可以用 Python 做，最方便。仓库建议：

psm-did-monte-carlo/
│
├── README.md
├── report.md
├── ai_usage_record.md
│
├── code/
│   ├── 01_simulation.py
│   ├── 02_analysis.ipynb
│
├── figures/
│   ├── scenario_A_distribution.png
│   ├── scenario_B_distribution.png
│   ├── scenario_C_distribution.png
│   ├── balance_before_after.png
│   ├── common_support.png
│
├── outputs/
│   ├── simulation_summary.csv
│   ├── balance_summary.csv
│
└── paper/
    └── paper_info.md

如果你只想简单一点，也可以一个 Jupyter Notebook + 一个 report.md，但从评分角度看，前面这种更像正式作业。

⸻

三、报告结构建议

建议报告 3000-5000 字，结构可以这样：

#用 Monte Carlo 模拟理解 PSM-DID
##1. 研究背景与问题意识
##2. 方法简介：PSM、DID 与 PSM-DID
##3. Monte Carlo 模拟设计
###3.1 基础设定
###3.2 场景 A：无条件平行趋势成立
###3.3 场景 B：趋势差异依赖于可观测协变量 X
###3.4 场景 C：存在时间变化的未观测混淆
##4. 估计方法与评价指标
##5. 模拟结果
###5.1 场景 A 结果
###5.2 场景 B 结果
###5.3 场景 C 结果
###5.4 匹配平衡性与共同支持
##6. 理论解释
##7. 真实论文评价
##8. 结论
##9. AI 工具使用说明
##参考文献

这个结构和评分标准高度贴合，比较稳。

⸻

四、三个场景应该怎么设计？

这是整个作业最核心的部分。我建议你不要设计太复杂，复杂不等于高级。要让每个场景的理论含义非常清楚。

⸻

场景 A：DID 表现较好的基准情形

设计目的

证明：如果处理组和控制组满足无条件平行趋势，那么 DID 本身就可以表现很好，PSM-DID 不一定明显更优。

数据生成逻辑

设定：

* 个体协变量：
    X_i \sim N(0,1)
* 个体固定异质性：
    \alpha_i \sim N(0,1)
* 处理分配：
    D_i = 1[\gamma_0 + \gamma_1 X_i + u_i > 0]

也就是说，X 会影响谁进入处理组。

* 处理前结果：
    Y_{i0} = \alpha_i + \beta X_i + \epsilon_{i0}
* 处理后结果：
    Y_{i1} = \alpha_i + \beta X_i + \lambda + \tau D_i + \epsilon_{i1}

关键点是：X 影响结果水平，但不影响结果变化趋势。

因此，即使处理组和控制组的 X 分布不同，只要它们的趋势一样，DID 仍然能识别真实处理效应。

预期结果

方法	预期表现
PSM	可能有偏，因为只看处理后水平，不能消除个体固定差异
DID	表现好
DID + X	表现也好，可能方差更小
PSM-DID	表现好，但不一定明显优于 DID

这部分可以写成：PSM-DID 不是总比 DID 好，在无条件平行趋势已经成立时，匹配的边际收益有限。

⸻

场景 B：PSM-DID 优于未匹配 DID

设计目的

证明：当趋势差异和处理前协变量 X 有关时，未匹配 DID 会违反无条件平行趋势；但如果匹配后 X 分布接近，PSM-DID 可以恢复条件平行趋势。

数据生成逻辑

还是：

X_i \sim N(0,1)

处理分配：

P(D_i=1|X_i)=\text{logit}^{-1}(\gamma_0+\gamma_1X_i)

也就是说，X 越高，越可能接受处理。

处理前结果：

Y_{i0} = \alpha_i + \beta X_i + \epsilon_{i0}

处理后结果：

Y_{i1} = \alpha_i + \beta X_i + \lambda + \delta X_i + \tau D_i + \epsilon_{i1}

这里多了一个关键项：

\delta X_i

它表示：X 会影响个体从处理前到处理后的自然增长趋势。

如果处理组平均 X 更高，那么即使没有政策，处理组的结果增长也会更快。未匹配 DID 会把这部分自然增长误认为政策效果。

预期结果

方法	预期表现
PSM	可能比不匹配好，但仍然不如 PSM-DID，因为它没有利用处理前差分
DID	明显有偏
DID + X	如果模型控制正确，可能有所改善
PSM-DID	表现最好或接近最好

这就是本作业最核心的场景。

你要在报告里重点解释：

DID 需要处理组和控制组在没有处理时具有相同趋势，但在场景 B 中，趋势由 X 决定，而处理组与控制组的 X 分布不同，因此未匹配 DID 不再可信。PSM-DID 通过匹配处理前协变量 X，使处理组和控制组在 X 上更加相似，从而更接近条件平行趋势。

⸻

场景 C：PSM-DID 也失败

这个场景可以有几种设计。我建议你用 时间变化的未观测混淆，理论上最清楚。

设计目的

证明：PSM-DID 只能处理“可观测 X 导致的趋势差异”和“时间不变未观测异质性”，但不能处理时间变化的未观测冲击。

数据生成逻辑

加入一个不可观测变量：

U_i \sim N(0,1)

处理分配：

P(D_i=1|X_i,U_i)=\text{logit}^{-1}(\gamma_0+\gamma_1X_i+\gamma_2U_i)

但是研究者只能观察到 X，看不到 U。

处理前：

Y_{i0} = \alpha_i + \beta X_i + \theta U_i + \epsilon_{i0}

处理后：

Y_{i1} = \alpha_i + \beta X_i + \lambda + \delta X_i + \rho U_i \cdot Post + \tau D_i + \epsilon_{i1}

更简单地写：

\Delta Y_i(0) = \lambda + \delta X_i + \rho U_i + \eta_i

这里的关键是：

* U 影响是否接受处理；
* U 也影响处理前后结果变化；
* 但 U 不可观测，无法匹配。

所以即使用 X 匹配，处理组和控制组在 U 上仍然不平衡，PSM-DID 仍然有偏。

预期结果

方法	预期表现
PSM	有偏
DID	有偏
DID + X	仍有偏
PSM-DID	也有偏

报告里可以写一句比较有判断力的话：

PSM-DID 的强项不是“消灭所有选择偏误”，而是在可观测协变量能够解释处理选择和趋势差异时，构造更可比的处理组与控制组。只要关键趋势差异来自不可观测且随时间变化的因素，匹配和差分都无法自动修复识别问题。

这句可以直接当理论结论。

⸻

五、估计方法怎么实现？

你用 Python 比较合适。每次模拟生成一个面板数据，然后估计几种方法。

1. 单独 PSM

有两种做法：

方案一：匹配后比较处理后结果

Y_{post} \sim D

也就是：

\hat{\tau}_{PSM-post}=E[Y_{post}|D=1, matched]-E[Y_{post}|D=0, matched]

这个更接近“传统 PSM”。

方案二：匹配后比较结果变化

\Delta Y = Y_{post} - Y_{pre}

这个其实已经有一点 DID 的味道，但作业允许说“使用处理后结果或结果变化作为被解释变量”。

我建议你同时报告一个主要版本：PSM-post。
然后把 PSM-DID 明确设为匹配后比较变化。

⸻

2. 单独 DID

基础 DID 回归：

Y_{it} = \beta_0 + \beta_1 D_i + \beta_2 Post_t + \tau (D_i \times Post_t) + \epsilon_{it}

其中交互项系数就是 DID 估计量。

也可以用变化值写：

\Delta Y_i = \tau D_i + \epsilon_i

两期面板下两者等价。

⸻

3. 控制 X 的 DID

可以估计：

\Delta Y_i = \tau D_i + \beta X_i + \epsilon_i

或者长数据格式：

Y_{it} = \beta_0 + \beta_1D_i+\beta_2Post_t+\tau(D_i\times Post_t)+\beta_3(X_i\times Post_t)+\epsilon_{it}

注意，如果 X 影响趋势，应该控制的是：

X_i \times Post_t

不是单纯控制 X。因为 X 是时间不变变量，真正影响的是“变化趋势”。

所以报告里可以专门点一下：

在场景 B 中，若只在 DID 回归中加入 X 水平项，未必能修正趋势差异；更合理的是允许 X 对时间趋势产生异质性影响，即加入 X_i \times Post_t。

这会显得你真的懂，不是机械跑回归。

⸻

4. PSM-DID

步骤：

1. 用处理前 X 估计倾向得分：

P(D=1|X)

2. 只用处理前变量匹配，不能用处理后 Y。
3. 最近邻匹配，比如 1:1 nearest neighbor with replacement。
4. 在匹配样本上估计：

\Delta Y_i = \tau D_i + \epsilon_i

或者直接算：

\hat{\tau}_{PSM-DID}=E[\Delta Y_i | D=1, matched]-E[\Delta Y_i | D=0, matched]⸻

六、结果表应该怎么呈现？

你最后最好有一张总表，类似这样：

场景	方法	Mean Estimate	Bias	RMSE	Std
A	PSM				
A	DID				
A	DID + X trend				
A	PSM-DID				
B	PSM				
B	DID				
B	DID + X trend				
B	PSM-DID				
C	PSM				
C	DID				
C	DID + X trend				
C	PSM-DID				

其中真实处理效应可以设为：

\tau = 2

这样结果解释直观：估计值越接近 2 越好。

⸻

七、图表建议

不需要图太多，别把作业做成可视化堆料。建议至少做 4 类：

1. 各方法估计值分布图

每个场景一张图，横轴是估计值，竖线标出真实值 \tau=2。

可以展示：

* DID
* PSM
* PSM-DID
* DID + X trend

这个图最有说服力。

⸻

2. 匹配前后 X 的标准化差异

展示匹配前后协变量平衡情况。

标准化均值差异：

SMD = \frac{\bar{X}_{treat} - \bar{X}_{control}}{s}

如果匹配后 SMD 明显下降，说明匹配改善了可观测变量平衡。

⸻

3. 倾向得分共同支持图

画处理组和控制组的 propensity score 分布。

场景 B 里应该有一定重叠。
场景 C 如果你设计为不可观测混淆，也可以仍然有重叠；如果你想增加一个共同支持不足场景，也可以让处理组和控制组倾向得分高度分离。

⸻

4. Bias / RMSE 柱状图

这个用于总结每个场景下哪个方法更稳。

⸻

八、文献评价部分怎么选论文？

你后面需要找一篇使用 PSM-DID 的经济学论文。这里我建议优先选中文论文，因为写评价比较容易，而且很多政策评估文章会用 PSM-DID。

可选方向：

1. 数字普惠金融 / 企业创新
2. 低碳城市试点 / 绿色创新
3. 高铁开通 / 区域经济增长
4. 环保政策 / 企业绩效
5. 精准扶贫 / 农户收入
6. 营商环境 / 企业投资
7. 开发区政策 / 企业生产率

你选论文时，不要只看“有没有 PSM-DID”，要看它有没有这些材料：

检查点	为什么重要
是否有政策处理组和控制组	DID 基础
是否有政策前后数据	DID 必需
是否报告平行趋势	识别可信度核心
是否报告匹配变量	判断有没有用处理后变量
是否有平衡性检验	判断 PSM 是否有效
是否有共同支持图	判断样本是否可比
是否讨论稳健性	判断结论是不是脆

我建议你不要选太冷门的论文。冷门论文可能资料少、方法写得含糊，不利于评价。

⸻

九、这个作业最容易踩的坑

坑 1：把 PSM-DID 写成“PSM + DID 所以更强”

这是大忌。

正确说法是：

PSM-DID 的有效性依赖于处理前可观测协变量能够充分刻画处理组和控制组之间的趋势差异。它不能自动解决不可观测的时间变化混淆。

⸻

坑 2：匹配用了处理后变量

绝对不要。

匹配变量必须是处理前变量。否则会引入 bad control 或 post-treatment bias。

⸻

坑 3：DID 控制 X 的方式不对

如果 X 只影响结果水平，那么普通 DID 已经可以处理。

如果 X 影响结果趋势，就要考虑：

X_i \times Post_t

或者用变化值：

\Delta Y_i = \tau D_i + \beta X_i + \epsilon_i

⸻

坑 4：只报告平均估计值，不报告 Bias 和 RMSE

评分标准明确要求方法表现。至少要有：

* Mean Estimate
* Bias
* RMSE
* Std

⸻

坑 5：理论解释和模拟场景脱节

你每个理论点都要对应场景：

理论问题	对应场景
DID 为什么有效	场景 A
PSM-DID 为什么优于 DID	场景 B
PSM-DID 为什么失败	场景 C
匹配改变估计对象	场景 B/C
共同支持不足	可以作为场景 C 的扩展，或在 C 中讨论

⸻

十、我建议你的整体完成路线

你现在可以按这个顺序推进：

第一步：确定模拟设计

先把三个场景的 DGP 写成数学公式。
这是报告和代码共同的基础。

第二步：写 Python 代码

代码功能包括：

1. 生成单次模拟数据；
2. 实现 PSM、DID、DID+X、PSM-DID；
3. 重复 500 次；
4. 汇总 Bias、RMSE、Std；
5. 输出图表。

第三步：根据结果写解释

不要先写理论大段文字。
先看模拟结果，再写“为什么会这样”。

第四步：找论文并评价

论文评价一定要服务于前面的模拟结论，不要写成孤立文献介绍。

第五步：整理 GitHub/Gitee

把代码、报告、图表、AI 使用记录放进去。

⸻

十一、这个作业我建议采用的核心观点

你报告最后可以围绕这个主线写：

PSM-DID 的价值不在于机械地把两个方法叠加，而在于通过处理前协变量构造更可比的样本，使 DID 的平行趋势假设从“无条件平行趋势”转化为“条件平行趋势”。当趋势差异主要来自可观测协变量时，PSM-DID 能有效降低偏误；但当趋势差异来自不可观测且随时间变化的因素，或者共同支持不足时，PSM-DID 仍然会失败。因此，PSM-DID 更像是一种依赖强假设的设计改进，而不是对非随机政策评估的万能修复工具。

这个观点很适合做全文结论，也比较有批判性。

⸻

十二、下一步最应该做什么？

我建议下一步不要急着写报告。先做 模拟设计方案。

也就是我们先把三个场景整理成一张完整表：

场景	DGP 公式	哪个假设成立	哪个假设被破坏	预期哪个方法最好	理论原因

这张表一旦定下来，后面的代码、图表、报告都能顺着写。

## 2. 前置信息引导与逻辑构建

**User：** 行，那我先完成代码和报告，最后你再指导我上传GitHub吧，注意每次你都要告诉我要写成什么格式的文档，你是否明白，另外，过程中有不清楚的地方务必先向我确认  
**AI：** 明白。

接下来我会按这个原则配合你：

1. 先完成代码和报告，最后再处理 GitHub 上传
2. 每一步我都会明确告诉你：
    * 这一部分应该放在哪个文件里
    * 文件格式是什么，比如 .ipynb、.py、.md、.csv、.png
    * 文件名建议叫什么
    * 最后仓库里应该怎么组织
3. 遇到会影响作业方向的地方，我会先向你确认；比如：
    * 你准备用 Python / R / Stata 哪个？
    * 是否要用 Jupyter Notebook？
    * 论文评价选中文还是英文？
    * 模拟场景是否要加共同支持不足？
4. 但如果只是小的技术细节，我会直接给你一个稳妥方案，不反复打断你。

我们后面建议按这个顺序推进：

第一步：确定模拟设计方案 → report.md 的“模拟设计”部分
第二步：写代码 → code/psm_did_simulation.ipynb
第三步：生成结果表 → outputs/simulation_summary.csv
第四步：生成图表 → figures/*.png
第五步：写完整报告 → report.md
第六步：写论文评价 → report.md + paper/paper_info.md
第七步：写 AI 使用记录 → ai_usage_record.md
第八步：上传 GitHub

现在我需要先确认一个关键点：你准备用 Python 做，对吗？
如果是，我建议用 Jupyter Notebook 格式 .ipynb，因为它最适合同时放代码、结果表和图，后面写报告也方便。

**User：** 是的，我会用Jupyter，对于里面会用到的方法原理，我先给你传一些资料，你先告诉我，这份作业涉及到什么理论方法？  
**AI：** 是的，这份作业涉及的理论方法主要集中在 因果推断前六讲，再加一点 Monte Carlo 模拟和基础回归实现。你后面传资料时，我会按这些模块帮你归类，不会把所有课件内容都硬塞进作业。

这一步建议你后续整理到：

report.md

对应报告里的：

##2. 方法简介：PSM、DID 与 PSM-DID
##3. Monte Carlo 模拟设计
#6. 理论解释

⸻

一、Monte Carlo 模拟

这是作业的外壳方法。

你要用它回答：

在已知真实处理效应的情况下，不同估计方法到底有没有偏？

核心概念包括：

概念	在作业中的作用
数据生成过程 DGP	你自己设定真实世界如何产生数据
真实处理效应 \tau	比如设定为 2，作为判断估计是否准确的标准
重复模拟	至少重复 500 次
Bias	平均估计值与真实效应的差距
RMSE	综合衡量偏误和波动
估计值分布	观察方法是否稳定、有无系统偏误

你要理解的是：Monte Carlo 不是一个因果识别方法，而是一个检验估计方法表现的实验工具。

⸻

二、潜在结果框架

这是因果推断的基础语言。

你需要用它说明：

Y_i(1)

表示个体接受处理后的潜在结果。

Y_i(0)

表示个体未接受处理时的潜在结果。

但现实中，一个个体只能观察到其中一个结果：

Y_i = D_iY_i(1)+(1-D_i)Y_i(0)

作业里 PSM、DID、PSM-DID 都是在解决同一个问题：

看不到反事实怎么办？

比如一个地区实施了政策后，我们看不到它“如果没有实施政策会怎样”。DID 用控制组趋势构造反事实，PSM 用相似个体构造反事实，PSM-DID 则是在相似样本中用变化趋势构造反事实。

⸻

三、处理效应：ATE 和 ATT

这个作业一定会用到。

ATE

平均处理效应：

ATE = E[Y_i(1)-Y_i(0)]

意思是：如果所有人都接受处理，平均效果是多少。

ATT

处理组平均处理效应：

ATT = E[Y_i(1)-Y_i(0)|D_i=1]

意思是：对实际接受处理的人来说，政策效果是多少。

PSM-DID 通常更接近：

ATT

因为它通常是以处理组为中心，为处理组寻找相似控制组。也就是说，它回答的是：

对那些实际被政策影响的个体或地区，政策平均产生了什么效果？

不是：

如果把政策推广给所有个体，平均会怎样？

这一点在报告里必须写。

⸻

四、选择偏误与混淆变量

这是 PSM 的理论基础。

现实中，处理不是随机分配的。比如：

* 经济基础更好的城市更容易成为试点；
* 管理能力更强的企业更容易采用某项政策；
* 教育水平更高的人更可能参加培训。

这些因素既影响是否接受处理，也影响结果变量，就会形成混淆。

用 DAG 表示就是：

X \rightarrow D

X \rightarrow Y

所以如果不控制 X，你观察到的处理组和控制组差异，可能不是政策效果，而是原本条件不同。

PSM 的作用就是基于可观测协变量 X，让处理组和控制组更可比。

⸻

五、倾向得分匹配 PSM

这是作业中的第一个估计方法。

核心思想是：

e(X_i)=P(D_i=1|X_i)

也就是个体接受处理的概率。

PSM 不直接用一堆协变量匹配，而是先把协变量压缩成一个倾向得分，再在倾向得分相近的处理组和控制组之间比较结果。

你需要掌握几个点：

内容	作业里怎么用
倾向得分	用 Logistic 回归或机器学习模型估计
最近邻匹配	给每个处理组找最相近的控制组
卡尺 caliper	限制最大匹配距离
是否放回匹配	控制组能不能被重复使用
共同支持	处理组和控制组是否有重叠
平衡性检验	匹配后 X 是否更接近

PSM 的核心假设是：

(Y(1),Y(0)) \perp D | X

意思是：在控制可观测协变量 X 后，处理分配可以看作近似随机。

但它的弱点也明显：

PSM 只能解决可观测变量造成的选择偏误，不能解决不可观测混淆。

⸻

六、共同支持条件

这是 PSM 和 PSM-DID 都必须讨论的。

共同支持条件是：

0 < P(D_i=1|X_i)<1

通俗说：

每个处理组个体都应该能在控制组中找到相似对象。

如果处理组倾向得分普遍很高，而控制组普遍很低，就说明两组本来差太多。强行匹配会导致：

* 匹配质量差；
* 样本大量丢失；
* 估计对象改变；
* 方差变大；
* 偏误可能更严重。

所以报告中要有共同支持图，最好放在：

figures/common_support.png

⸻

七、平衡性检验

这是 PSM 是否做得像样的证据。

你需要比较匹配前后处理组和控制组在 X 上的差异。

常见指标是标准化均值差异：

SMD = \frac{\bar{X}_{treat}-\bar{X}_{control}}{s}

如果匹配后 SMD 明显下降，说明匹配让处理组和控制组更可比。

这部分结果可以放在：

outputs/balance_summary.csv

图可以放在：

figures/balance_check.png

⸻

八、双重差分 DID

这是作业中的第二个估计方法。

DID 比较的是处理组和控制组在处理前后的变化差异：

\hat{\tau}_{DID}
=
(\bar{Y}_{treat,post}-\bar{Y}_{treat,pre})
-
(\bar{Y}_{control,post}-\bar{Y}_{control,pre})

它的优势是可以消除时间不变的未观测异质性。

比如：

Y_{it} = \alpha_i + \lambda_t + \tau D_iPost_t + \epsilon_{it}

其中：

* \alpha_i：个体固定差异；
* \lambda_t：共同时间冲击；
* D_iPost_t：处理组在政策后的处理状态。

通过前后差分，\alpha_i 会被消掉。

所以 DID 可以解决：

处理组和控制组一开始水平不同，但这种差异是稳定的。

但 DID 依赖核心假设：

E[Y(0)_{post}-Y(0)_{pre}|D=1]
=
E[Y(0)_{post}-Y(0)_{pre}|D=0]

也就是平行趋势。

⸻

九、平行趋势假设

这是 DID 和 PSM-DID 的灵魂。

平行趋势不是说处理组和控制组结果水平一样，而是说：

如果没有政策，两组的变化趋势应该一样。

场景 A 就是在设计：

虽然处理组和控制组 X 不一样、初始水平不一样，但没有处理时，它们的变化趋势一样。

所以 DID 表现好。

场景 B 是：

处理组和控制组的自然变化趋势不同，而这种趋势差异来自可观测变量 X。

所以未匹配 DID 会有偏，PSM-DID 更可能有效。

场景 C 是：

趋势差异来自不可观测的时间变化因素 U。

所以 PSM-DID 也失败。

⸻

十、条件平行趋势

这是 PSM-DID 的关键。

PSM-DID 不要求处理组和所有控制组无条件平行趋势，而是要求：

E[\Delta Y(0)|D=1,X]
=
E[\Delta Y(0)|D=0,X]

意思是：

在给定处理前协变量 X 后，处理组和控制组如果没有处理，其结果变化趋势相同。

这正好对应你的场景 B。

场景 B 的逻辑应该是：

1. X 影响处理分配；
2. X 也影响自然增长趋势；
3. 所以未匹配 DID 不成立；
4. 但匹配后 X 分布更接近；
5. 条件平行趋势更可信；
6. PSM-DID 表现更好。

⸻

十一、PSM-DID

这是作业的核心方法。

它的流程是：

1. 用处理前协变量 X 估计倾向得分；
2. 根据倾向得分匹配处理组和控制组；
3. 在匹配后的样本上做 DID；
4. 得到处理效应估计。

它结合了两类逻辑：

方法	解决的问题
PSM	处理组和控制组在可观测变量上不可比
DID	存在时间不变的未观测差异
PSM-DID	在可比样本上用前后变化识别政策效果

但你要有批判性地写：

PSM-DID 不是 PSM 和 DID 的简单叠加，也不是万能修复工具。它仍然依赖处理前协变量充分、共同支持充分、条件平行趋势成立、没有时间变化未观测混淆等假设。

⸻

十二、DAG 有向无环图

这部分不一定要画图，但可以用来帮你解释偏误来源。

你可以在报告中用文字或简单图说明三种场景。

场景 A

X → D
X → Y 水平
D → Y

但 X 不影响趋势，所以 DID 有效。

场景 B

X → D
X → ΔY(0)
D → Y

X 同时影响处理分配和自然增长趋势，所以未匹配 DID 有偏。

场景 C

U → D
U → ΔY(0)
D → Y

U 不可观测，无法匹配，所以 PSM-DID 失败。

DAG 的作用不是为了漂亮，而是帮你说明：

偏误到底从哪里来。

⸻

十三、回归估计

代码里会用到基础回归。

主要包括：

DID 回归

Y ~ D + Post + D × Post

关心交互项系数。

控制趋势差异的 DID

Y ~ D + Post + D × Post + X × Post

或者用变化值：

ΔY ~ D + X

这在两期面板里更简单。

PSM-DID

匹配后估计：

ΔY ~ D

也可以直接计算处理组和匹配控制组的平均变化差异。

⸻

十四、标准误、方差和 RMSE

你不用特别深入讲统计推断，但要报告模拟评价指标。

重点是：

Bias = mean(\hat{\tau})-\tau

RMSE = \sqrt{mean((\hat{\tau}-\tau)^2)}

Std = sd(\hat{\tau})

区别：

指标	含义
Bias	是否系统性高估或低估
Std	每次模拟结果波动大不大
RMSE	综合反映偏误和不稳定性

如果共同支持不足，可能出现：

* Bias 变大；
* Std 变大；
* RMSE 变大。

因为匹配变差、样本变少、估计对象也变了。

⸻

十五、真实论文评价涉及的方法

任务四不是新的模型，而是把前面的理论拿去审论文。

你需要用这些标准评价：

评价维度	对应理论
为什么用 PSM-DID	非随机处理 + 有前后数据
匹配变量是否合理	是否都是处理前变量
平衡性检验	PSM 是否改善可比性
共同支持	是否存在可比控制组
平行趋势	DID 核心假设
同期政策干扰	时间变化混淆
估计对象	更接近 ATT 还是 ATE
脆弱性	对应你的场景 C

你写论文评价的时候，最好用这种句式：

根据本文 Monte Carlo 模拟，PSM-DID 在场景 B 中表现较好，是因为趋势差异主要由可观测协变量 X 解释。因此，如果该论文中的匹配变量能够充分覆盖影响政策选择和结果趋势的处理前因素，且共同支持和平衡性检验较好，那么其识别策略相对可信。相反，如果存在同期政策冲击、地方政府治理能力变化、产业周期变化等不可观测且随时间变化的因素，则其估计结果可能类似场景 C，仍存在偏误风险。

这就能把模拟和文献评价连起来。

⸻

十六、这份作业涉及理论方法总表

你可以把下面这张表作为学习地图：

模块	理论/方法	作业中用途
模拟方法	Monte Carlo	反复生成数据，比较估计方法表现
因果基础	潜在结果框架	定义处理效应和反事实问题
估计对象	ATE / ATT	判断 PSM-DID 估计的是谁的效果
选择偏误	混淆变量	解释为什么简单比较有偏
图模型	DAG	说明偏误路径
匹配方法	PSM	平衡处理前可观测协变量
匹配诊断	共同支持	判断是否有可比控制组
匹配诊断	平衡性检验	判断匹配是否改善 X 分布
面板方法	DID	利用前后变化消除时间不变差异
核心假设	平行趋势	DID 是否可信
核心假设	条件平行趋势	PSM-DID 是否可信
组合方法	PSM-DID	匹配后做 DID
失效机制	时间变化未观测混淆	解释 PSM-DID 的边界
评价指标	Bias / RMSE / Std	比较估计方法表现
文献审查	方法适用性评价	评价真实 PSM-DID 论文

⸻

你接下来传资料时，我建议按这个顺序发：

1. PSM / 匹配相关资料
2. DID / 面板数据相关资料
3. 潜在结果 / DAG 相关资料
4. Monte Carlo 或代码示例资料

## 3. 资料传输与要点提取

## 4. 任务执行

**User：** OK，下面请你解释一下任务一模拟数据生成的设计思路与逻辑 
**AI：** 可以。任务一的“模拟数据生成”其实是整个作业最关键的部分，因为 Monte Carlo 不是随便造一组数据跑模型，而是要人为设定真实世界的因果结构，然后看 PSM、DID、PSM-DID 能不能把你设定好的真实处理效应估计出来。

你可以把任务一理解成：

我们先创造一个“已知真相”的世界，再让不同方法去估计，最后检查这些方法什么时候靠谱、什么时候失效。

⸻

一、模拟数据生成的基本逻辑

在真实研究里，我们不知道真实处理效应是多少，所以很难判断估计方法准不准。

但在 Monte Carlo 模拟里，我们自己设定：

\tau = 2

也就是说，接受处理以后，结果变量真实会增加 2 个单位。

然后我们生成很多个体，每个个体都有：

变量	含义	在模拟中的作用
X_i	可观测协变量	影响是否接受处理，也可能影响结果水平或趋势
D_i	是否接受处理	处理组为 1，对照组为 0
Y_{i0}	处理前结果	用来构造 DID 的前期结果
Y_{i1}	处理后结果	包含真实处理效应
\Delta Y_i	结果变化	Y_{i1}-Y_{i0}，是 DID 和 PSM-DID 的核心比较对象
U_i	不可观测变量	用在失败场景中，模拟看不见的混淆因素

最基础的数据结构是：

Y_{i0} = \text{处理前结果}

Y_{i1} = \text{处理后结果}

\Delta Y_i = Y_{i1} - Y_{i0}

然后比较不同方法对 \tau=2 的估计值是否接近。

⸻

二、为什么要设计三个场景？

因为作业不是只想看一个方法跑出来的结果，而是想让你证明：

方法表现好不好，取决于识别假设是否成立。

所以三个场景分别对应三种因果识别环境：

场景	核心问题	目的
场景 A	DID 本来就有效吗？	检验无条件平行趋势成立时，DID 和 PSM-DID 都应该接近真实效应
场景 B	PSM-DID 什么时候比 DID 更好？	检验趋势差异由可观测变量 X 造成时，匹配可以改善 DID
场景 C	PSM-DID 什么时候也失败？	检验存在不可观测时变混淆时，匹配和 DID 都救不了

这三个场景是递进关系：

A：DID 可以解决。
B：单独 DID 不够，PSM-DID 有用。
C：PSM-DID 也不够。

⸻

三、场景 A：无条件平行趋势成立

1. 设计目的

场景 A 要模拟一种最理想的 DID 环境：

处理组和对照组虽然在处理前结果水平上可能不同，但如果没有处理，两组的变化趋势是一样的。

这对应第六讲 DID 的核心假设：平行趋势。

⸻

2. 数据生成逻辑

可以设定：

X_i \sim N(0,1)

P(D_i=1|X_i)=logit^{-1}(\gamma_0+\gamma_1X_i)

这表示：
个体是否接受处理和 X_i 有关。比如 X 越高，越可能接受处理。

然后生成处理前结果：

Y_{i0}=\alpha_i+\beta X_i+\epsilon_{i0}

这里 X_i 会影响处理前结果水平。也就是说，处理组和对照组可能一开始就不一样。

再生成处理后结果：

Y_{i1}=\alpha_i+\beta X_i+\lambda+\tau D_i+\epsilon_{i1}

其中：

项	含义
\alpha_i	个体固定特征
\beta X_i	X 对结果水平的影响
\lambda	所有人共同面对的时间趋势
\tau D_i	真实处理效应
\epsilon	随机噪声

关键在于：
X_i 只影响结果水平，不影响结果变化趋势。

因为：

Y_{i1}-Y_{i0}=\lambda+\tau D_i+(\epsilon_{i1}-\epsilon_{i0})

可以看到，如果没有处理，也就是 D_i=0，结果变化只是：

\Delta Y_i(0)=\lambda+\text{噪声}

这和 X_i 没关系。

所以，即使处理组和对照组的 X 分布不同，它们在没有处理时的变化趋势仍然一样。

⸻

3. 这个场景想说明什么？

场景 A 说明：

当无条件平行趋势成立时，DID 本身已经可以识别真实处理效应，PSM-DID 不一定比 DID 明显更好。

预期结果：

方法	预期表现
PSM	可能有偏，因为它只比较处理后水平
DID	接近真实效应 2
PSM-DID	也接近真实效应 2
DID + X	接近真实效应 2

所以场景 A 是一个基准组。

它证明：
PSM-DID 不是永远必要的。如果 DID 的平行趋势已经成立，直接 DID 就足够。

⸻

四、场景 B：趋势差异由可观测变量 X 造成

1. 设计目的

场景 B 是 PSM-DID 最能发挥作用的情况。

它模拟的是：

处理组和对照组的结果变化趋势不同，但这种趋势差异主要来自可观测协变量 X。

也就是说，单独 DID 的无条件平行趋势不成立，但在控制 X 或匹配 X 后，条件平行趋势成立。

⸻

2. 数据生成逻辑

仍然设定：

X_i \sim N(0,1)

P(D_i=1|X_i)=logit^{-1}(\gamma_0+\gamma_1X_i)

也就是说，X 越高的人越可能进入处理组。

处理前结果：

Y_{i0}=\alpha_i+\beta X_i+\epsilon_{i0}

处理后结果：

Y_{i1}=\alpha_i+\beta X_i+\lambda+\delta X_i+\tau D_i+\epsilon_{i1}

这里比场景 A 多了一个关键项：

\delta X_i

这个项表示：
X 不仅影响结果水平，还影响结果变化趋势。

于是结果变化变成：

\Delta Y_i=\lambda+\delta X_i+\tau D_i+(\epsilon_{i1}-\epsilon_{i0})

如果没有处理：

\Delta Y_i(0)=\lambda+\delta X_i+\text{噪声}

也就是说，哪怕没有政策，X 高的人本来就会增长更快。

⸻

3. 为什么单独 DID 会有偏？

因为处理组和对照组的 X 分布不同。

如果 X 高的人更容易接受处理，那么处理组平均 X 更高。

而 X 又会带来更快的自然增长：

\delta X_i

所以处理组即使没有处理，结果也会增长更快。

这时 DID 比较的是：

\text{处理组变化} - \text{对照组变化}

但这个差异里混入了两部分：

\text{真实处理效应} + \text{X 导致的自然趋势差异}

所以单独 DID 会把 X 造成的增长误认为处理效应。

⸻

4. 为什么 PSM-DID 有用？

PSM 会先根据 X_i 估计倾向得分：

e(X)=P(D=1|X)

然后让处理组和对照组在 X 上更相似。

匹配后，处理组和对照组的 X 分布接近，那么它们由 X 导致的自然趋势也接近。

也就是说：

E[\Delta Y(0)|D=1, matched] \approx E[\Delta Y(0)|D=0, matched]

于是匹配后的 DID 更接近真实处理效应。

⸻

5. 这个场景想说明什么？

场景 B 是整个作业最核心的场景。它说明：

PSM-DID 的价值在于，当处理组和对照组的趋势差异来自可观测协变量 X 时，匹配可以改善组间可比性，使 DID 的条件平行趋势更可信。

预期结果：

方法	预期表现
PSM	仍可能有偏
DID	明显有偏
DID + X	改善明显
PSM-DID	接近真实效应 2

这个场景要重点写，因为它正好证明 PSM-DID 的合理性。

⸻

五、场景 C：存在不可观测时变混淆

1. 设计目的

场景 C 是失败场景。

它要证明：

PSM-DID 不是万能的。如果真正造成趋势差异的是不可观测变量 U，那么即使 X 匹配得很好，估计仍然有偏。

⸻

2. 数据生成逻辑

引入一个不可观测变量：

U_i \sim N(0,1)

这个变量可以理解为能力、动机、风险偏好、信息渠道等。

处理分配由 X 和 U 共同决定：

P(D_i=1|X_i,U_i)=logit^{-1}(\gamma_0+\gamma_1X_i+\gamma_2U_i)

但是研究者只能观察 X，看不到 U。

处理前结果：

Y_{i0}=\alpha_i+\beta X_i+\theta U_i+\epsilon_{i0}

处理后结果：

Y_{i1}=\alpha_i+\beta X_i+\lambda+\delta X_i+\rho U_i+\tau D_i+\epsilon_{i1}

其中关键是：

\rho U_i

它表示 U 会影响结果的自然变化趋势。

于是：

\Delta Y_i(0)=\lambda+\delta X_i+\rho U_i+\text{噪声}

⸻

3. 为什么 PSM-DID 会失败？

PSM 只能匹配可观测变量 X。

但在这个场景里，处理组和对照组真正的趋势差异还来自 U。

由于 U 不可观测，匹配后：

X_{treat} \approx X_{control}

但不一定有：

U_{treat} \approx U_{control}

所以即使匹配后 X 平衡了，处理组和对照组在没有处理时的变化趋势仍然不同。

这就违反了条件平行趋势：

E[\Delta Y(0)|D=1,X] \neq E[\Delta Y(0)|D=0,X]

因此 PSM-DID 仍然有偏。

⸻

4. 这个场景想说明什么？

场景 C 的理论意义很重要。它说明：

PSM-DID 只能处理可观测协变量造成的选择偏差和趋势差异，不能处理不可观测且随时间变化的混淆因素。

预期结果：

方法	预期表现
PSM	有偏
DID	有偏
DID + X	仍有偏
PSM-DID	仍有偏

这里你要写得稍微“批判”一点：
即使匹配后的平衡性检验很好，也只能说明 X 平衡了，不能证明 U 也平衡了。

⸻

六、为什么要重复 500 次？

单次模拟结果可能受随机噪声影响。

比如某一次数据里，DID 估计值可能是 2.2；另一次可能是 1.8。单次结果不能说明方法稳定性。

所以要重复至少 500 次，每次都：

1. 重新生成数据；
2. 重新估计 PSM、DID、PSM-DID；
3. 记录估计值；
4. 最后计算平均估计值、Bias、RMSE、Std。

这些指标含义是：

指标	公式	含义
Mean Estimate	\frac{1}{R}\sum \hat{\tau}_r	平均估计值
Bias	\frac{1}{R}\sum \hat{\tau}_r-\tau	系统性偏误
RMSE	\sqrt{\frac{1}{R}\sum(\hat{\tau}_r-\tau)^2}	总体误差，包括偏误和波动
Std	sd(\hat{\tau}_r)	估计稳定性

其中 R=500，\tau=2。

判断标准是：

* Bias 越接近 0，说明越无偏；
* RMSE 越小，说明整体表现越好；
* Std 越小，说明估计越稳定。

⸻

七、任务一的整体设计逻辑可以这样写

你后面报告里可以直接写成这一段：

本研究通过 Monte Carlo 模拟构造三个具有不同识别条件的数据生成过程，以比较 PSM、DID 与 PSM-DID 在不同情形下的估计表现。模拟设计的核心思路是先人为设定真实处理效应 \tau=2，再生成处理前结果 Y_0、处理后结果 Y_1、处理状态 D 和协变量 X，从而在“已知真实效应”的环境下检验不同估计方法能否恢复该效应。三个场景分别对应 PSM-DID 的三种典型适用条件：场景 A 中，协变量只影响结果水平而不影响结果变化，无条件平行趋势成立，因此 DID 与 PSM-DID 都应表现良好；场景 B 中，协变量同时影响处理选择和结果变化，导致未匹配样本不满足无条件平行趋势，但在给定协变量后满足条件平行趋势，因此 PSM-DID 应优于单独 DID；场景 C 中，引入不可观测变量影响处理选择和结果变化，即使匹配可观测协变量后，条件平行趋势仍被破坏，因此 PSM-DID 也会失效。通过这种递进式设计，本文能够展示 PSM-DID 的优势、适用边界与潜在风险。

⸻

你现在最需要抓住一句话：

模拟数据生成的目的不是“造一个像真实世界的数据”，而是“造出不同识别假设成立或失败的因果世界”，用来检验方法表现。

**User：** 我计划用python来构造这个模拟环境，根据你上面的理解完成任务一
**AI：** 下面这版可以直接作为任务一的 Python 模拟框架。
你在本地新建：

code/psm_did_simulation.ipynb

然后按照下面的顺序分成几个代码块放进去。

⸻

0. Notebook 开头说明

先在 Notebook 第一个 Markdown 单元格写：

#任务一：PSM-DID Monte Carlo 模拟
本部分使用 Python 构造三个数据生成过程（DGP），比较 PSM、DID、控制协变量的 DID 和 PSM-DID 在不同识别条件下的估计表现。
三个场景分别为：
1. 场景 A：无条件平行趋势成立；
2. 场景 B：趋势差异由可观测协变量 X 决定，匹配后条件平行趋势成立；
3. 场景 C：存在不可观测时间变化混淆，PSM-DID 失效。
真实处理效应设定为 tau = 2。每个场景重复 500 次，报告平均估计值、Bias、RMSE 和 Std。

⸻

1. 导入包

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import statsmodels.api as sm
from sklearn.linear_model import LogisticRegression
from sklearn.neighbors import NearestNeighbors
import warnings
warnings.filterwarnings("ignore")
#设置随机种子，保证结果可复现
np.random.seed(42)
#中文显示设置
plt.rcParams["font.sans-serif"] = ["Arial Unicode MS", "SimHei", "DejaVu Sans"]
plt.rcParams["axes.unicode_minus"] = False

⸻

2. 基础工具函数

这一部分负责：估计倾向得分、做匹配、算各种估计量、算 Bias/RMSE。

def sigmoid(x):
    """Logistic 函数，用于生成处理概率。"""
    return 1 / (1 + np.exp(-x))
def estimate_propensity_score(data, covariates):
    """
    使用 Logistic 回归估计倾向得分 P(D=1|X)。
    
    参数：
    data: DataFrame，包含处理变量 D 和协变量
    covariates: list，用于估计倾向得分的协变量名称
    
    返回：
    data: 增加 pscore 列后的 DataFrame
    """
    model = LogisticRegression(max_iter=1000)
    X = data[covariates]
    y = data["D"]
    model.fit(X, y)
    
    data = data.copy()
    data["pscore"] = model.predict_proba(X)[:, 1]
    return data
def nearest_neighbor_match(data, caliper=None):
    """
    基于倾向得分进行 1:1 最近邻匹配，有放回匹配。
    
    参数：
    data: 包含 D、pscore 的 DataFrame
    caliper: 卡尺。如果为 None，则不设卡尺。
    
    返回：
    matched_data: 匹配后的样本
    """
    treated = data[data["D"] == 1].copy()
    control = data[data["D"] == 0].copy()
    
    if len(treated) == 0 or len(control) == 0:
        return pd.DataFrame()
    
    nn = NearestNeighbors(n_neighbors=1)
    nn.fit(control[["pscore"]])
    
    distances, indices = nn.kneighbors(treated[["pscore"]])
    
    matched_treated = []
    matched_control = []
    
    for i, dist in enumerate(distances.flatten()):
        if caliper is not None and dist > caliper:
            continue
        
        treated_i = treated.iloc[[i]].copy()
        control_i = control.iloc[[indices.flatten()[i]]].copy()
        
        matched_treated.append(treated_i)
        matched_control.append(control_i)
    
    if len(matched_treated) == 0:
        return pd.DataFrame()
    
    matched_data = pd.concat(matched_treated + matched_control, axis=0)
    return matched_data
def estimate_psm_post(data):
    """
    单独 PSM：匹配后比较处理后结果 Y1 的均值差。
    """
    data = estimate_propensity_score(data, ["X"])
    matched = nearest_neighbor_match(data)
    
    if matched.empty:
        return np.nan
    
    treated_mean = matched[matched["D"] == 1]["Y1"].mean()
    control_mean = matched[matched["D"] == 0]["Y1"].mean()
    return treated_mean - control_mean
def estimate_did(data):
    """
    单独 DID：比较处理组和对照组的结果变化差异。
    等价于 DeltaY 对 D 的简单回归。
    """
    data = data.copy()
    data["DeltaY"] = data["Y1"] - data["Y0"]
    
    X_reg = sm.add_constant(data["D"])
    model = sm.OLS(data["DeltaY"], X_reg).fit()
    return model.params["D"]
def estimate_did_with_x(data):
    """
    控制 X 的 DID：DeltaY 对 D 和 X 回归。
    如果 X 影响结果变化趋势，这个模型可以修正部分偏误。
    """
    data = data.copy()
    data["DeltaY"] = data["Y1"] - data["Y0"]
    
    X_reg = sm.add_constant(data[["D", "X"]])
    model = sm.OLS(data["DeltaY"], X_reg).fit()
    return model.params["D"]
def estimate_psm_did(data):
    """
    PSM-DID：先根据 X 估计倾向得分并匹配，再在匹配样本上比较 DeltaY。
    """
    data = data.copy()
    data["DeltaY"] = data["Y1"] - data["Y0"]
    
    data = estimate_propensity_score(data, ["X"])
    matched = nearest_neighbor_match(data)
    
    if matched.empty:
        return np.nan
    
    treated_mean = matched[matched["D"] == 1]["DeltaY"].mean()
    control_mean = matched[matched["D"] == 0]["DeltaY"].mean()
    return treated_mean - control_mean
def summarize_results(results_df, true_effect=2.0):
    """
    汇总 Monte Carlo 结果：Mean Estimate, Bias, RMSE, Std。
    """
    summary = []
    
    for scenario in results_df["scenario"].unique():
        temp_s = results_df[results_df["scenario"] == scenario]
        
        for method in ["PSM", "DID", "DID_X", "PSM_DID"]:
            estimates = temp_s[method].dropna()
            
            mean_est = estimates.mean()
            bias = mean_est - true_effect
            rmse = np.sqrt(np.mean((estimates - true_effect) ** 2))
            std = estimates.std()
            
            summary.append({
                "Scenario": scenario,
                "Method": method,
                "Mean_Estimate": mean_est,
                "Bias": bias,
                "RMSE": rmse,
                "Std": std
            })
    
    return pd.DataFrame(summary)

⸻

3. 场景 A：无条件平行趋势成立

这个场景的重点是：
X 影响处理选择和结果水平，但不影响结果变化趋势。

def generate_data_scenario_A(n=1000, tau=2.0):
    """
    场景 A：无条件平行趋势成立。
    
    X 影响处理选择和结果水平，但不影响结果变化趋势。
    因此，未处理状态下，处理组和对照组具有相同的自然变化趋势。
    """
    X = np.random.normal(0, 1, n)
    alpha = np.random.normal(0, 1, n)
    
    # 处理分配：X 越高，越可能接受处理
    p = sigmoid(-0.2 + 0.8 * X)
    D = np.random.binomial(1, p, n)
    
    # 处理前结果
    eps0 = np.random.normal(0, 1, n)
    Y0 = alpha + 1.5 * X + eps0
    
    # 共同时间趋势
    lambda_t = 1.0
    
    # 处理后结果：X 不影响趋势，只影响水平
    eps1 = np.random.normal(0, 1, n)
    Y1 = alpha + 1.5 * X + lambda_t + tau * D + eps1
    
    data = pd.DataFrame({
        "X": X,
        "D": D,
        "Y0": Y0,
        "Y1": Y1
    })
    
    return data

报告里可以解释为：

在场景 A 中，协变量 X 影响个体是否接受处理，也影响处理前和处理后的结果水平，但不影响未处理状态下的结果变化趋势。因此，即使处理组和对照组在 X 的分布上存在差异，只要没有处理，两组的结果变化仍然相同。该场景满足无条件平行趋势假设，因此 DID 本身应能识别真实处理效应。

⸻

4. 场景 B：趋势差异由可观测变量 X 造成

这个场景的重点是：
X 不仅影响处理选择，还影响结果变化趋势。
所以单独 DID 会有偏，但 PSM-DID 应该改善。

def generate_data_scenario_B(n=1000, tau=2.0):
    """
    场景 B：趋势差异由可观测协变量 X 决定。
    
    X 同时影响处理选择和结果变化趋势。
    未匹配时，无条件平行趋势不成立；
    但在给定 X 或匹配 X 后，条件平行趋势成立。
    """
    X = np.random.normal(0, 1, n)
    alpha = np.random.normal(0, 1, n)
    
    # 处理分配：X 越高，越可能接受处理
    p = sigmoid(-0.2 + 0.8 * X)
    D = np.random.binomial(1, p, n)
    
    # 处理前结果
    eps0 = np.random.normal(0, 1, n)
    Y0 = alpha + 1.5 * X + eps0
    
    # 共同时间趋势
    lambda_t = 1.0
    
    # X 对自然增长趋势的影响
    delta_x = 1.0
    
    # 处理后结果：X 影响结果变化趋势
    eps1 = np.random.normal(0, 1, n)
    Y1 = alpha + 1.5 * X + lambda_t + delta_x * X + tau * D + eps1
    
    data = pd.DataFrame({
        "X": X,
        "D": D,
        "Y0": Y0,
        "Y1": Y1
    })
    
    return data

报告里可以解释为：

在场景 B 中，X 不仅影响个体接受处理的概率，还影响其未处理状态下的结果变化趋势。由于处理组的 X 分布通常高于对照组，即使没有处理，处理组也会因为 X 的作用而具有更快的自然增长。因此，未匹配样本不满足无条件平行趋势，单独 DID 会将 X 导致的趋势差异误认为处理效应。PSM-DID 通过先在 X 上构造可比样本，再比较结果变化，从而更接近条件平行趋势假设。

⸻

5. 场景 C：不可观测时间变化混淆

这个场景的重点是：
存在 U，研究者看不到。
PSM 只能匹配 X，不能匹配 U，所以 PSM-DID 也失败。

def generate_data_scenario_C(n=1000, tau=2.0):
    """
    场景 C：存在不可观测时间变化混淆。
    
    U 同时影响处理选择和结果变化趋势，但研究者无法观察 U。
    即使匹配 X，也无法消除 U 带来的趋势差异。
    因此 PSM-DID 也会有偏。
    """
    X = np.random.normal(0, 1, n)
    U = np.random.normal(0, 1, n)  # 不可观测变量
    alpha = np.random.normal(0, 1, n)
    
    # 处理分配由 X 和 U 共同决定
    p = sigmoid(-0.2 + 0.6 * X + 0.9 * U)
    D = np.random.binomial(1, p, n)
    
    # 处理前结果
    eps0 = np.random.normal(0, 1, n)
    Y0 = alpha + 1.5 * X + 1.0 * U + eps0
    
    # 共同时间趋势
    lambda_t = 1.0
    
    # X 和 U 都影响自然增长趋势
    delta_x = 1.0
    rho_u = 1.2
    
    # 处理后结果
    eps1 = np.random.normal(0, 1, n)
    Y1 = alpha + 1.5 * X + 1.0 * U + lambda_t + delta_x * X + rho_u * U + tau * D + eps1
    
    # 注意：返回的数据不包含 U，因为研究者观察不到它
    data = pd.DataFrame({
        "X": X,
        "D": D,
        "Y0": Y0,
        "Y1": Y1
    })
    
    return data

报告里可以解释为：

在场景 C 中，除可观测协变量 X 外，还存在不可观测变量 U。U 同时影响处理选择和未处理状态下的结果变化趋势。由于研究者无法观察 U，倾向得分匹配只能基于 X 进行，无法平衡处理组和对照组在 U 上的差异。因此，即使匹配后 X 达到平衡，条件平行趋势仍然可能被破坏，PSM-DID 也无法恢复真实处理效应。

⸻

6. 单次模拟函数

def run_one_simulation(data_generator, scenario_name, n=1000, tau=2.0):
    """
    对某一个场景运行一次模拟，并返回四种方法的估计结果。
    """
    data = data_generator(n=n, tau=tau)
    
    try:
        psm_est = estimate_psm_post(data)
    except Exception:
        psm_est = np.nan
    
    try:
        did_est = estimate_did(data)
    except Exception:
        did_est = np.nan
    
    try:
        did_x_est = estimate_did_with_x(data)
    except Exception:
        did_x_est = np.nan
    
    try:
        psm_did_est = estimate_psm_did(data)
    except Exception:
        psm_did_est = np.nan
    
    return {
        "scenario": scenario_name,
        "PSM": psm_est,
        "DID": did_est,
        "DID_X": did_x_est,
        "PSM_DID": psm_did_est
    }

⸻

7. Monte Carlo 主循环

这里按作业要求，每个场景重复 500 次。

def run_monte_carlo(R=500, n=1000, tau=2.0):
    """
    对三个场景分别进行 Monte Carlo 模拟。
    
    R: 每个场景重复次数
    n: 每次模拟的样本量
    tau: 真实处理效应
    """
    all_results = []
    
    scenarios = {
        "A_Unconditional_Parallel_Trend": generate_data_scenario_A,
        "B_Conditional_Parallel_Trend": generate_data_scenario_B,
        "C_Unobserved_Time_Varying_Confounding": generate_data_scenario_C
    }
    
    for scenario_name, generator in scenarios.items():
        print(f"Running scenario: {scenario_name}")
        
        for r in range(R):
            result = run_one_simulation(
                data_generator=generator,
                scenario_name=scenario_name,
                n=n,
                tau=tau
            )
            result["replication"] = r + 1
            all_results.append(result)
    
    results_df = pd.DataFrame(all_results)
    return results_df

运行：

true_effect = 2.0
results_df = run_monte_carlo(R=500, n=1000, tau=true_effect)
results_df.head()

⸻

8. 汇总结果表

summary_df = summarize_results(results_df, true_effect=true_effect)
summary_df

保存结果：

import os
os.makedirs("../outputs", exist_ok=True)
results_df.to_csv("../outputs/simulation_raw_results.csv", index=False)
summary_df.to_csv("../outputs/simulation_summary.csv", index=False)

你最后应该生成两个文件：

outputs/simulation_raw_results.csv
outputs/simulation_summary.csv

报告主要使用 simulation_summary.csv。

⸻

9. 画估计值分布图

os.makedirs("../figures", exist_ok=True)
def plot_distribution_by_scenario(results_df, scenario_name, true_effect=2.0):
    """
    绘制某一场景下四种方法的估计值分布。
    """
    temp = results_df[results_df["scenario"] == scenario_name]
    
    methods = ["PSM", "DID", "DID_X", "PSM_DID"]
    
    plt.figure(figsize=(10, 6))
    
    for method in methods:
        estimates = temp[method].dropna()
        plt.hist(estimates, bins=30, alpha=0.4, density=True, label=method)
    
    plt.axvline(true_effect, linestyle="--", linewidth=2, label=f"True Effect = {true_effect}")
    plt.xlabel("Estimated Treatment Effect")
    plt.ylabel("Density")
    plt.title(f"Distribution of Estimates: {scenario_name}")
    plt.legend()
    plt.tight_layout()
    
    file_name = f"../figures/{scenario_name}_distribution.png"
    plt.savefig(file_name, dpi=300)
    plt.show()
    
    print(f"Saved to {file_name}")

运行：

for scenario in results_df["scenario"].unique():
    plot_distribution_by_scenario(results_df, scenario, true_effect=true_effect)

会生成：

figures/A_Unconditional_Parallel_Trend_distribution.png
figures/B_Conditional_Parallel_Trend_distribution.png
figures/C_Unobserved_Time_Varying_Confounding_distribution.png

⸻

10. Bias 和 RMSE 对比图

def plot_bias_rmse(summary_df):
    """
    绘制不同场景和方法的 Bias 与 RMSE 对比图。
    """
    os.makedirs("../figures", exist_ok=True)
    
    for metric in ["Bias", "RMSE"]:
        plt.figure(figsize=(12, 6))
        
        pivot_df = summary_df.pivot(index="Scenario", columns="Method", values=metric)
        pivot_df.plot(kind="bar", figsize=(12, 6))
        
        plt.axhline(0, linewidth=1)
        plt.ylabel(metric)
        plt.title(f"{metric} Comparison Across Scenarios")
        plt.xticks(rotation=30, ha="right")
        plt.tight_layout()
        
        file_name = f"../figures/{metric.lower()}_comparison.png"
        plt.savefig(file_name, dpi=300)
        plt.show()
        
        print(f"Saved to {file_name}")

运行：

plot_bias_rmse(summary_df)

生成：

figures/bias_comparison.png
figures/rmse_comparison.png

⸻

11. 平衡性检验：标准化偏差 SMD

任务要求里有“匹配平衡性”，所以要展示匹配前后 X 是否平衡。

def standardized_mean_difference(data, var):
    """
    计算某个变量在处理组和对照组之间的标准化均值差异 SMD。
    """
    treated = data[data["D"] == 1][var]
    control = data[data["D"] == 0][var]
    
    mean_diff = treated.mean() - control.mean()
    pooled_sd = np.sqrt((treated.var() + control.var()) / 2)
    
    return mean_diff / pooled_sd
def balance_check_one_dataset(data_generator, scenario_name, n=1000, tau=2.0):
    """
    对某一个场景生成一份数据，展示匹配前后 X 的平衡性。
    """
    data = data_generator(n=n, tau=tau)
    data = estimate_propensity_score(data, ["X"])
    
    matched = nearest_neighbor_match(data)
    
    smd_before = standardized_mean_difference(data, "X")
    smd_after = standardized_mean_difference(matched, "X")
    
    balance_df = pd.DataFrame({
        "Scenario": [scenario_name, scenario_name],
        "Stage": ["Before Matching", "After Matching"],
        "SMD_X": [smd_before, smd_after]
    })
    
    return balance_df, data, matched

运行：

balance_results = []
for scenario_name, generator in {
    "A_Unconditional_Parallel_Trend": generate_data_scenario_A,
    "B_Conditional_Parallel_Trend": generate_data_scenario_B,
    "C_Unobserved_Time_Varying_Confounding": generate_data_scenario_C
}.items():
    balance_df, _, _ = balance_check_one_dataset(generator, scenario_name)
    balance_results.append(balance_df)
balance_summary = pd.concat(balance_results, axis=0)
balance_summary

保存：

balance_summary.to_csv("../outputs/balance_summary.csv", index=False)

画 Love 图简化版：

plt.figure(figsize=(10, 6))
for scenario in balance_summary["Scenario"].unique():
    temp = balance_summary[balance_summary["Scenario"] == scenario]
    plt.plot(temp["Stage"], temp["SMD_X"], marker="o", label=scenario)
plt.axhline(0.1, linestyle="--", linewidth=1, label="|SMD| = 0.1")
plt.axhline(-0.1, linestyle="--", linewidth=1)
plt.ylabel("Standardized Mean Difference of X")
plt.title("Covariate Balance Before and After Matching")
plt.legend()
plt.tight_layout()
plt.savefig("../figures/balance_before_after.png", dpi=300)
plt.show()

生成：

outputs/balance_summary.csv
figures/balance_before_after.png

⸻

12. 共同支持图

任务里还要求共同支持图。这里用场景 B 做示范即可，因为场景 B 最能体现 PSM-DID。

def plot_common_support(data_generator, scenario_name, n=1000, tau=2.0):
    """
    绘制处理组和对照组的倾向得分分布，检查共同支持域。
    """
    data = data_generator(n=n, tau=tau)
    data = estimate_propensity_score(data, ["X"])
    
    treated_ps = data[data["D"] == 1]["pscore"]
    control_ps = data[data["D"] == 0]["pscore"]
    
    plt.figure(figsize=(10, 6))
    plt.hist(treated_ps, bins=30, alpha=0.5, density=True, label="Treated")
    plt.hist(control_ps, bins=30, alpha=0.5, density=True, label="Control")
    
    plt.xlabel("Estimated Propensity Score")
    plt.ylabel("Density")
    plt.title(f"Common Support Check: {scenario_name}")
    plt.legend()
    plt.tight_layout()
    
    file_name = f"../figures/{scenario_name}_common_support.png"
    plt.savefig(file_name, dpi=300)
    plt.show()
    
    print(f"Saved to {file_name}")

运行：

plot_common_support(
    generate_data_scenario_B,
    "B_Conditional_Parallel_Trend",
    n=1000,
    tau=true_effect
)

生成：

figures/B_Conditional_Parallel_Trend_common_support.png

⸻

13. 结果应该怎么解读

你跑完后，正常情况下应该看到类似规律：

场景 A

预期：

DID 和 PSM-DID 的 Bias 接近 0。

解释：

场景 A 中，无条件平行趋势成立，因此 DID 可以较好恢复真实处理效应。PSM-DID 由于在匹配后继续使用变化值进行比较，也能得到接近真实值的估计。但单独 PSM 主要比较处理后水平，未充分利用处理前信息，因此可能仍存在偏误。

场景 B

预期：

DID 有明显偏误；
DID_X 和 PSM-DID 接近真实效应。

解释：

场景 B 中，X 同时影响处理选择和结果变化趋势，导致处理组与对照组在未处理状态下也会呈现不同增长趋势。单独 DID 将这种由 X 引起的趋势差异误认为处理效应，因此出现偏误。控制 X 的 DID 和 PSM-DID 均能够部分恢复条件平行趋势，因此估计结果更接近真实处理效应。

场景 C

预期：

DID、DID_X 和 PSM-DID 都有偏。

解释：

场景 C 中，不可观测变量 U 同时影响处理选择和结果变化趋势。由于 U 不被研究者观测，倾向得分匹配只能平衡 X，无法平衡 U。因此，即使匹配后 X 的标准化偏差下降，条件平行趋势仍然不成立，PSM-DID 依然无法消除偏误。

⸻

14. 最终你这个 Notebook 会生成哪些文件

跑完后，你的文件夹应该是：

psm-did-monte-carlo/
├── code/
│   └── psm_did_simulation.ipynb
├── outputs/
│   ├── simulation_raw_results.csv
│   ├── simulation_summary.csv
│   └── balance_summary.csv
└── figures/
    ├── A_Unconditional_Parallel_Trend_distribution.png
    ├── B_Conditional_Parallel_Trend_distribution.png
    ├── C_Unobserved_Time_Varying_Confounding_distribution.png
    ├── bias_comparison.png
    ├── rmse_comparison.png
    ├── balance_before_after.png
    └── B_Conditional_Parallel_Trend_common_support.png

任务一最核心的文件是：

code/psm_did_simulation.ipynb
outputs/simulation_summary.csv
figures/*.png

⸻

15. 你下一步该做什么

先把上面的代码放进 code/psm_did_simulation.ipynb 跑一遍。重点看三个东西：

1. summary_df 是否正常输出；
2. 三个场景的估计结果是否符合预期；
3. figures/ 和 outputs/ 是否成功生成。

如果某个结果和预期不一致，优先不要急着改报告，先把 summary_df 的结果发我，我帮你判断是参数设计问题、代码问题，还是随机波动。

**User：** 任务要求中特别提到，“Monte Carlo 不是简单地“跑很多次代码”，而是用可控的数据生成过程检验理论命题。”你怎么理解这句话？在上述代码中是否遵循这一点？  
**AI：** 这句话的意思是：Monte Carlo 的重点不在于重复 500 次、1000 次本身，而在于你先设定一个“理论上应该发生什么”的数据世界，然后用反复模拟去验证不同方法在这个数据世界里是否真的表现如理论预期。

也就是说，它不是：

随机生成一堆数据 → 跑很多次模型 → 看谁结果好

而应该是：

先提出理论命题 → 设计能体现这个命题的数据生成过程 DGP → 让 PSM、DID、PSM-DID 在这个环境中竞争 → 看模拟结果是否支持理论判断

⸻

1. 这句话怎么理解？

以你的作业为例，老师真正想看的不是你会不会写循环，而是你是否理解：

理论命题一：DID 依赖平行趋势

如果处理组和对照组虽然初始水平不同，但在没有处理时趋势相同，那么 DID 可以识别处理效应。

所以 Monte Carlo 里就要故意设计一个场景：

X 影响处理选择，也影响结果水平，
但 X 不影响结果变化趋势。

这样处理组和对照组可能一开始不一样，但变化趋势一样。

这个时候理论预期是：

DID 应该接近真实处理效应；
PSM-DID 也应该接近；
单纯 PSM 不一定最好。

这就是场景 A。

⸻

理论命题二：PSM-DID 可以处理“由可观测变量导致的趋势差异”

如果处理组和对照组的自然增长趋势不同，而且这种趋势差异来自可观测变量 X，那么普通 DID 会有偏。

但如果我们先用 X 做匹配，使两组的 X 分布接近，再做 DID，那么条件平行趋势更可能成立。

所以 Monte Carlo 里就要故意设计：

X 影响处理选择；
X 也影响未处理状态下的结果变化趋势。

这个时候理论预期是：

普通 DID 会有偏；
PSM-DID 会明显改善；
DID + X trend 也会改善。

这就是场景 B。

⸻

理论命题三：PSM-DID 不能解决不可观测的时间变化混淆

如果有一个不可观测变量 U，它既影响谁接受处理，又影响结果变化趋势，那么匹配 X 是没用的，因为关键混淆变量根本没被观测到。

所以 Monte Carlo 里要设计：

U 影响处理选择；
U 影响结果变化；
但研究者看不到 U，只能用 X 做匹配。

这个时候理论预期是：

PSM-DID 仍然有偏；
DID 也有偏；
DID + X 也救不了；
单纯 PSM 也救不了。

这就是场景 C。

这就是“用可控的数据生成过程检验理论命题”的意思。

⸻

2. 上述代码是否遵循了这一点？

整体上是遵循的。

因为代码不是随便生成数据，而是围绕三个明确的因果识别命题设计 DGP：

场景	DGP 设计逻辑	要检验的理论命题
A	X 影响处理和水平，但不影响趋势	无条件平行趋势成立时，DID 有效
B	X 同时影响处理和自然变化趋势	条件平行趋势成立时，PSM-DID 优于普通 DID
C	不可观测 U 同时影响处理和趋势	PSM-DID 无法处理未观测时间变化混淆

所以它不是机械地“重复跑代码”，而是在三个受控世界中检验：

DID 什么时候有效？
PSM-DID 什么时候更好？
PSM-DID 什么时候也会失败？

这符合任务要求。

⸻

3. 但代码里要特别注意一个问题

为了更清楚地体现“可控 DGP”，你在 Notebook 和报告里必须把每个场景的理论预期写出来，不能只放代码和结果。

建议你在 code/psm_did_simulation.ipynb 每个场景前都加 Markdown 说明，比如：

###场景 A：无条件平行趋势成立
本场景中，协变量 X 会影响个体接受处理的概率，也会影响结果变量的初始水平，但不会影响未处理状态下的结果变化趋势。因此，即使处理组和对照组在处理前水平不同，只要两组在没有处理时的变化趋势相同，普通 DID 仍然可以识别真实处理效应。
理论预期是：DID 和 PSM-DID 的平均估计值应接近真实处理效应 τ=2；单纯 PSM 由于只比较处理后水平，可能仍受初始差异影响。

场景 B 前写：

###场景 B：趋势差异由可观测协变量 X 导致
本场景中，X 不仅影响处理选择，也影响未处理状态下的结果变化趋势。因此，处理组和对照组如果 X 分布不同，就会违反无条件平行趋势，普通 DID 会有偏。但由于趋势差异来自可观测变量 X，基于 X 估计倾向得分并匹配后，处理组和对照组在 X 上更接近，条件平行趋势更可能成立。
理论预期是：普通 DID 会偏离真实处理效应；PSM-DID 和控制 X 趋势的 DID 应更接近 τ=2。

场景 C 前写：

###场景 C：存在不可观测的时间变化混淆
本场景中，不可观测变量 U 同时影响处理选择和未处理状态下的结果变化趋势，但研究者只能观察到 X，无法在匹配或回归中控制 U。因此，即使进行 PSM-DID，仍然无法消除由 U 造成的选择偏误。
理论预期是：PSM-DID、DID 和 PSM 都会产生系统性偏误。

这样就能直接回应老师那句话。

⸻

4. 报告里可以这样写

这段建议放到：

report.md

对应位置：

##3. Monte Carlo 模拟设计

可以写成：

本研究中的 Monte Carlo 模拟并不是简单地重复运行估计代码，而是通过可控的数据生成过程检验 PSM、DID 与 PSM-DID 的识别逻辑。具体而言，本文先设定真实处理效应为 τ=2，然后分别构造三类数据环境：无条件平行趋势成立、趋势差异由可观测协变量解释、以及存在不可观测时间变化混淆。在每一种环境中，真实处理效应和偏误来源都是由研究者预先设定的，因此可以将各方法的估计结果与真实值进行比较，计算 Bias、RMSE 和 Std，从而判断不同方法是否符合理论预期。
这种设计的核心在于：如果模拟结果显示 DID 在场景 A 中表现良好、PSM-DID 在场景 B 中优于普通 DID、而 PSM-DID 在场景 C 中仍然失效，那么模拟就不仅是在重复计算，而是在用数据生成过程验证因果识别理论。

⸻

5. 一句话总结

你的代码方向是对的，但最终作业里一定要把逻辑说清楚：

我不是为了跑 500 次而跑 500 次，
而是先构造三个有明确识别假设的数据世
再用反复模拟观察 PSM、DID、PSM-DID 是否按照理论预期表现。

这句话可以作为你报告中“模拟设计”的核心解释。

**User：** 这部分太长了，怎么压缩？  
**AI：** 建议优先压缩文献评价和理论铺垫，保留模拟设计、核心结果表格和三个场景解释……# AI 交互聊天记录（整理版）

说明：本文件为本项目完成过程中与 AI 工具的交互记录整理版。为避免泄露个人账号、截图、无关闲聊和个人信息，已对部分内容进行删减和脱敏。保留内容主要包括模拟设计、代码思路、结果解释、文献评价和报告修改相关对话。

**User：** “你可以自行实现最近邻匹配，也可以使用已有包。无论使用哪种方式，都必须说明匹配方法、卡尺或邻居数、是否放回匹配，以及如何处理共同支持。”对于这个要求呢？  
**AI：** 这个要求是在提醒你：不能只说“我做了 PSM”，而要把 PSM 的具体实现规则交代清楚。因为不同匹配规则会影响估计结果、样本保留数量和估计对象。

你前面那版代码已经有匹配方法，但还需要补充说明，并且建议稍微改进一下：加入共同支持处理 + 明确卡尺设置。

⸻

一、这句话具体要求你交代什么？

老师要求你至少说明四件事：

要求	你需要写清楚什么
匹配方法	最近邻匹配、卡尺匹配、核匹配等
卡尺或邻居数	1:1 最近邻？1:k？卡尺是多少？
是否放回匹配	同一个控制组个体能不能被多个处理组个体重复匹配
如何处理共同支持	是否剔除倾向得分没有重叠的样本

你现在最稳妥的方案是：

使用基于倾向得分的 1:1 最近邻匹配；
邻居数 k=1；
设置卡尺 caliper = 0.2 × SD(pscore)；
采用有放回匹配；
共同支持采用 min-max 规则，只保留处理组和对照组倾向得分重叠区间内的样本。

这个方案很适合作业，因为简单、可解释、符合课程第四讲内容。

⸻

二、建议你在代码里补充“共同支持处理”

你前面代码里的 nearest_neighbor_match() 已经实现了 1:1 最近邻、有放回匹配，但共同支持没有显式处理，卡尺默认也是 None。

建议改成下面这个版本。

替换原来的共同支持和匹配函数

放在：

code/psm_did_simulation.ipynb

对应“基础工具函数”部分。

def apply_common_support(data):
    """
    使用 min-max 规则处理共同支持域。
    
    只保留处理组和对照组倾向得分分布重叠区间内的样本。
    """
    treated = data[data["D"] == 1]
    control = data[data["D"] == 0]
    
    treated_min, treated_max = treated["pscore"].min(), treated["pscore"].max()
    control_min, control_max = control["pscore"].min(), control["pscore"].max()
    
    lower = max(treated_min, control_min)
    upper = min(treated_max, control_max)
    
    data_cs = data[(data["pscore"] >= lower) & (data["pscore"] <= upper)].copy()
    
    return data_cs
def nearest_neighbor_match(data, caliper="auto"):
    """
    基于倾向得分进行 1:1 最近邻匹配，有放回匹配，并可设置卡尺。
    
    匹配规则：
    1. 先使用 min-max 规则保留共同支持域样本；
    2. 对每个处理组个体，在对照组中寻找倾向得分最近的 1 个个体；
    3. 采用有放回匹配，即同一个对照组个体可以被多个处理组个体匹配；
    4. 若 caliper="auto"，则设定 caliper = 0.2 × SD(pscore)；
    5. 若最近邻距离超过 caliper，则该处理组个体被剔除。
    """
    data = data.copy()
    
    # 1. 共同支持域处理
    data = apply_common_support(data)
    
    treated = data[data["D"] == 1].copy()
    control = data[data["D"] == 0].copy()
    
    if len(treated) == 0 or len(control) == 0:
        return pd.DataFrame()
    
    # 2. 自动设定卡尺
    if caliper == "auto":
        caliper_value = 0.2 * data["pscore"].std()
    elif caliper is None:
        caliper_value = None
    else:
        caliper_value = caliper
    
    # 3. 最近邻匹配
    nn = NearestNeighbors(n_neighbors=1)
    nn.fit(control[["pscore"]])
    
    distances, indices = nn.kneighbors(treated[["pscore"]])
    
    matched_treated = []
    matched_control = []
    
    for i, dist in enumerate(distances.flatten()):
        if caliper_value is not None and dist > caliper_value:
            continue
        
        treated_i = treated.iloc[[i]].copy()
        control_i = control.iloc[[indices.flatten()[i]]].copy()
        
        matched_treated.append(treated_i)
        matched_control.append(control_i)
    
    if len(matched_treated) == 0:
        return pd.DataFrame()
    
    matched_data = pd.concat(matched_treated + matched_control, axis=0)
    
    return matched_data

这样就完整回应了老师的要求。

⸻

三、为什么选择这种匹配方法？

你可以这样理解：

1. 为什么用最近邻匹配？

因为它最直观：

给每个处理组个体，找一个倾向得分最接近的对照组个体。

你的作业重点是理解 PSM-DID，不是比较各种复杂匹配算法。所以用最近邻匹配最合适。

2. 为什么用 1:1 匹配？

因为解释简单，估计对象清楚：

每个处理组个体匹配一个最相近的对照组个体。

虽然 1:k 匹配可以使用更多对照样本，但会增加解释复杂度。任务一用 1:1 足够。

3. 为什么用有放回匹配？

因为有放回匹配允许一个优质对照组个体被多个处理组个体使用，能提高匹配质量。

如果无放回匹配，前面匹配走了好的对照个体，后面的处理组个体可能只能匹配到较差的对照，反而增加偏误。

所以你可以写：

本文采用有放回匹配，以优先保证匹配质量。

4. 为什么设置卡尺？

没有卡尺时，即使最近的对照组个体也可能和处理组差很多，形成“坏匹配”。

卡尺的作用是：

如果最近邻距离仍然太远，就不强行匹配。

常见做法是：

caliper = 0.2 \times SD(\hat e(X))

你不用过多解释来源，只需要说这是经验上常用的设置，可以避免质量较差的匹配。

5. 为什么要处理共同支持？

共同支持要求处理组和对照组的倾向得分有重叠：

0 < P(D=1|X) < 1

如果某些处理组个体的倾向得分很高，而对照组没有类似个体，那么这些处理组样本没有可靠反事实。

所以要用 min-max 规则，只保留两组倾向得分重叠区间内的样本。

⸻

四、报告里可以直接这样写

放在：

report.md

对应：

##4. 估计方法与评价指标

可以写：

###4.1 倾向得分匹配方法
本文在 PSM 和 PSM-DID 中均采用基于倾向得分的 1:1 最近邻匹配。首先，使用处理前协变量 \(X\) 估计个体接受处理的概率：
\[
\hat e(X_i)=P(D_i=1|X_i)
\]
其中，倾向得分通过 Logistic 回归估计。随后，对于每一个处理组个体，在对照组中寻找倾向得分距离最近的一个个体作为匹配对象。
具体匹配规则如下：第一，邻居数设定为 \(k=1\)，即 1:1 最近邻匹配；第二，采用有放回匹配，同一个对照组个体可以被多个处理组个体重复使用，以提高匹配质量；第三，设置卡尺约束，卡尺取 \(0.2\times SD(\hat e(X))\)，若最近邻距离超过卡尺，则该处理组个体不参与匹配；第四，采用 min-max 规则处理共同支持域，只保留处理组和对照组倾向得分分布重叠区间内的样本。
这样处理的目的是避免将倾向得分差异过大的个体强行匹配，从而减少“坏匹配”对估计结果的影响。同时，共同支持处理也意味着 PSM-DID 的估计对象更接近于“具有可比对照组的处理组个体”的平均处理效应，而不是全体样本的 ATE。

⸻

五、Notebook 里也建议加一段 Markdown 说明

放在匹配函数前面：

###倾向得分匹配设定
本模拟采用 1:1 最近邻倾向得分匹配。倾向得分由 Logistic 回归基于处理前协变量 X 估计。匹配时使用有放回匹配，并设置卡尺为 \(0.2 \times SD(\hat e(X))\)。在匹配前，先使用 min-max 规则处理共同支持域，即只保留处理组和对照组倾向得分分布重叠区间内的样本。这样可以避免缺乏可比对照组的样本被强行匹配。

⸻

六、这个要求会影响前面的代码结果吗？

会有一点影响。

加入共同支持和卡尺后：

1. 一些处理组样本可能被剔除；
2. 匹配质量通常更好；
3. PSM-DID 的 Bias 可能下降；
4. Std 可能略微上升，因为样本变少；
5. 估计对象会从全体处理组 ATT 变成“可匹配处理组”的 ATT。

这正好可以在报告里体现一个重要认识：

匹配不是无成本的。它提高了可比性，但也可能改变估计对象。

这句话很加分。

**User：** 这样处理的改进和优劣是什么？ 
**AI：** 这种处理指的是：

1:1 最近邻匹配
+ 有放回匹配
+ 卡尺约束
+ min-max 共同支持处理

它的核心改进是：不再为了“匹配上”而强行匹配，而是优先保证匹配质量和估计可解释性。

⸻

一、相对于原始代码，它改进了什么？

原始版本基本是：

每个处理组个体都找一个最近的对照组个体

问题是：
即使最近的对照组个体也可能和处理组差得很远，尤其是在倾向得分分布不重叠时，这种匹配就只是“形式上匹配”，不是真正可比。

改进后加入了两个限制：

1. 共同支持处理

只保留处理组和对照组倾向得分有重叠的区域。

也就是：

\max(\min p_T,\min p_C) \leq p_i \leq \min(\max p_T,\max p_C)

这样可以避免处理组中那些“根本找不到相似对照”的个体被强行纳入估计。

2. 卡尺约束

即使进入共同支持区间，也不是任意最近邻都接受。只有倾向得分距离小于卡尺的匹配才保留：

|\hat e_i-\hat e_j| < 0.2\times SD(\hat e)

这样可以进一步排除质量较差的匹配。

所以改进点可以概括为：

从“只要最近就匹配”变成“只有足够相似才匹配”。

⸻

二、优点是什么？

1. 匹配质量更高

卡尺可以避免“坏匹配”。

比如一个处理组个体倾向得分是 0.82，最近的控制组个体是 0.55。虽然 0.55 是最近的，但两者并不相似。没有卡尺时会强行匹配；有卡尺时会剔除这个处理组个体。

这样匹配后的处理组和对照组在 X 上更接近，平衡性更好。

⸻

2. 更符合 PSM 的理论要求

PSM 依赖重叠假设：

0<P(D=1|X)<1

如果某些 X 水平下几乎只有处理组，没有对照组，那么该区域的反事实无法从数据中构造。

共同支持处理正是在操作层面回应这个假设。

所以它比原始最近邻匹配更严谨。

⸻

3. 降低 PSM-DID 的偏误风险

在场景 B 中，趋势差异来自 X。如果匹配后 X 更平衡，那么处理组和对照组的自然趋势更接近。

因此：

E[\Delta Y(0)|D=1, matched] \approx E[\Delta Y(0)|D=0, matched]

PSM-DID 更可能接近真实处理效应。

⸻

4. 估计结果更容易解释

你可以明确说明：

本文估计的是具有共同支持、且能够找到合格匹配对象的处理组个体的 ATT。

这比“全样本上机械估计一个平均效应”更规范。

⸻

5. 有放回匹配提高局部匹配质量

有放回匹配允许同一个优秀对照个体被重复使用。

优点是：
每个处理组个体都能优先匹配到最相似的控制组个体，而不是因为控制组被前面的匹配“占用”导致后面匹配质量下降。

这在处理组和控制组分布不完全重叠时尤其有用。

⸻

三、缺点是什么？

1. 会损失样本

共同支持和卡尺都会剔除一部分样本。

尤其是处理组中倾向得分很高、但控制组中没有相似对象的个体，会被删掉。

这意味着样本量减少，估计方差可能变大。

表现到 Monte Carlo 结果里，可能是：

Bias 下降，但 Std 上升

也就是偏误更小，但波动更大。

⸻

2. 会改变估计对象

这是最重要的缺点。

原本你可能想估计所有处理组的 ATT：

ATT=E[Y(1)-Y(0)|D=1]

但剔除不满足共同支持或卡尺的样本后，估计对象变成：

ATT_{matched}=E[Y(1)-Y(0)|D=1, matched]

也就是：

可匹配处理组的平均处理效应。

如果被剔除的人和保留下来的人差异很大，那么这个估计不能简单推广到所有处理组。

这点报告里一定要写。

⸻

3. 卡尺选择带有一定主观性

虽然 0.2\times SD(\hat e) 是常见经验规则，但它不是唯一标准。

卡尺太宽：

保留样本多，但坏匹配风险高

卡尺太窄：

匹配质量高，但样本损失严重

所以卡尺本质上存在一个偏误—方差权衡：

\text{更严格匹配} \Rightarrow \text{偏误下降，但方差可能上升}

⸻

4. 有放回匹配会让少数控制组个体被重复使用

有放回匹配虽然提高了匹配质量，但也可能导致估计过度依赖少数控制组个体。

比如 100 个处理组个体中，有 20 个都匹配到同一个控制组个体。这样对照组信息的有效多样性下降，标准误估计也会更复杂。

在你的模拟作业里问题不大，但报告里可以简单说明。

⸻

5. 仍然不能解决不可观测混淆

这是原则性限制。

共同支持、卡尺、最近邻匹配，都只能改善可观测变量 X 上的可比性。

如果场景 C 中存在不可观测变量 U，且 U 同时影响处理选择和结果趋势，那么即使匹配做得再精细，也无法消除这部分偏误。

所以：

更严格的匹配提高的是“可观测层面”的可比性，不等于保证因果识别一定成立。

⸻

四、可以在报告里怎么写？

放在：

report.md

对应：

##4. 估计方法与评价指标

可以写成：

在匹配实现上，本文没有直接采用无约束最近邻匹配，而是进一步加入共同支持处理和卡尺约束。这样做的主要改进在于避免将倾向得分差异过大的个体强行匹配，从而提高处理组和对照组在可观测协变量上的可比性。共同支持处理保证匹配样本来自处理组和对照组倾向得分分布重叠的区域，卡尺约束则进一步排除距离过远的匹配对象。因此，相比简单最近邻匹配，该方法更符合 PSM 对重叠假设和平衡性的要求，也有助于降低由可观测协变量差异造成的估计偏误。
但这种处理也存在代价。首先，共同支持和卡尺会剔除部分样本，尤其是缺乏相似对照组的处理组个体，因此估计方差可能增加。其次，样本剔除会改变估计对象，本文估计的处理效应更接近于“具有共同支持且成功匹配的处理组个体”的 ATT，而不是全体处理组的 ATT 或总体 ATE。此外，卡尺大小具有一定经验性，有放回匹配也可能使少数控制组个体被重复使用。更重要的是，这些改进只能改善可观测协变量上的平衡，无法解决不可观测且随时间变化的混淆因素。因此，在场景 C 中，即使采用共同支持和卡尺匹配，PSM-DID 仍可能产生系统性偏误。

⸻

五、简短版结论

可以总结为一句话：

共同支持和卡尺匹配提高了匹配质量，降低了坏匹配带来的偏误，但代价是损失样本、改变估计对象，并且仍无法处理不可观测混淆。

这句话可以放在报告的理论解释或局限性部分。

##5. 结果分析

看到了，结果整体非常理想，而且基本完全符合你前面设定的三个理论命题。可以直接作为任务一结果使用，不需要大改 DGP。

1. 核心结果判断

你的 summary_df 是：

场景	方法	Mean Estimate	Bias	RMSE	Std
A	PSM	2.010	0.010	0.127	0.127
A	DID	1.999	-0.001	0.088	0.088
A	DID_X	1.999	-0.001	0.094	0.094
A	PSM_DID	1.993	-0.007	0.134	0.134
B	PSM	1.993	-0.007	0.135	0.135
B	DID	2.697	0.697	0.706	0.112
B	DID_X	2.001	0.001	0.095	0.095
B	PSM_DID	1.997	-0.003	0.130	0.130
C	PSM	3.691	1.691	1.704	0.210
C	DID	3.357	1.357	1.363	0.123
C	DID_X	2.929	0.929	0.936	0.114
C	PSM_DID	2.923	0.923	0.936	0.157

真实处理效应是：

\tau = 2

所以判断标准就是：估计值越接近 2，Bias 越接近 0，方法越符合该场景下的识别假设。

⸻

2. 场景 A：结果很好，证明 DID 本身有效

场景 A 中：

DID Mean = 1.999
Bias = -0.001
RMSE = 0.088

这几乎完全贴近真实效应 2。

PSM-DID 也接近：

PSM-DID Mean = 1.993
Bias = -0.007

这说明你场景 A 的设计是成功的：
虽然 X 影响处理选择和结果水平，但不影响结果变化趋势，所以无条件平行趋势成立，DID 可以恢复真实处理效应。

可以写：

场景 A 的结果表明，当协变量只影响结果水平而不影响结果趋势时，DID 已经能够较好识别真实处理效应。PSM-DID 的估计也接近真实值，但相较 DID 并没有明显优势，说明匹配并不是在所有情形下都必要。

这里还可以顺手解释 Std：

DID 的 Std = 0.088
PSM-DID 的 Std = 0.134

说明 PSM-DID 因为匹配、卡尺和共同支持处理损失了一部分样本，所以波动略大。这是合理的。

⸻

3. 场景 B：结果最关键，证明 PSM-DID 的优势

场景 B 中最重要的对比是：

DID Mean = 2.697
Bias = 0.697

普通 DID 明显高估真实效应。

而：

DID_X Mean = 2.001
Bias = 0.001
PSM-DID Mean = 1.997
Bias = -0.003

这非常漂亮，说明场景 B 正好检验出了你想表达的理论命题：

当趋势差异来自可观测协变量 X 时，未匹配 DID 会违反无条件平行趋势；但控制 X 或基于 X 匹配后，条件平行趋势更接近成立。

也就是说，场景 B 证明了 PSM-DID 的价值。

报告里可以这样写：

场景 B 中，普通 DID 的平均估计值为 2.697，明显高于真实处理效应 2，说明当 X 同时影响处理选择和结果变化趋势时，未匹配 DID 会将由 X 导致的自然增长差异误认为处理效应。相比之下，PSM-DID 的平均估计值为 1.997，Bias 仅为 -0.003，说明在趋势差异主要由可观测协变量解释时，倾向得分匹配能够改善处理组和对照组的可比性，使匹配后的 DID 更接近条件平行趋势。

这部分是你任务一结果分析里最该重点写的。

⸻

4. 场景 C：结果也很好，证明 PSM-DID 不是万能的

场景 C 中：

PSM-DID Mean = 2.923
Bias = 0.923

明显高估真实效应。

DID 和 DID_X 也都明显有偏：

DID Bias = 1.357
DID_X Bias = 0.929

这说明你场景 C 的失败机制成立：
不可观测变量 U 同时影响处理选择和结果变化趋势，但估计时只能匹配/控制 X，无法处理 U。

这个结果正好回应老师很看重的一点：

PSM-DID 不是 PSM 和 DID 机械叠加后的万能方法。

报告里可以写：

场景 C 中，即使采用 PSM-DID，平均估计值仍为 2.923，Bias 达到 0.923。这说明当存在不可观测变量 U 同时影响处理分配和未处理状态下的结果变化趋势时，匹配只能平衡可观测协变量 X，无法平衡 U。因此，条件平行趋势仍然不成立，PSM-DID 仍然存在系统性偏误。

这段很重要，可以放到“理论解释”或“方法局限”中。

⸻

5. 平衡性结果也很好

你的 balance_summary 是：

场景	匹配前 SMD_X	匹配后 SMD_X
A	0.794	0.001
B	0.827	-0.001
C	0.616	-0.000

这个结果非常适合用来说明：

匹配确实显著改善了处理组和对照组在可观测协变量 X 上的平衡性。

尤其场景 B，匹配前 SMD 是 0.827，匹配后几乎为 0。这说明 PSM-DID 在场景 B 有效，不是偶然，而是因为匹配确实消除了处理组和对照组在 X 上的可观测差异。

但场景 C 更有解释价值：
场景 C 匹配后 X 的 SMD 也几乎为 0，但 PSM-DID 仍然有偏。

这说明：

平衡性检验只能证明可观测变量 X 平衡了，不能证明不可观测变量 U 也被平衡。

这个点非常加分。

可以写：

场景 C 中，匹配后 X 的标准化均值差异接近 0，但 PSM-DID 估计仍然明显偏离真实效应。这说明协变量平衡性检验只是对可观测变量的检验，不能排除不可观测时间变化混淆。

⸻

6. 匹配诊断也正常

你的匹配诊断结果是：

场景	原始样本量	匹配后样本量	平均匹配距离
A	1000	878	0.00115
B	1000	942	0.00101
C	1000	924	0.00064

这说明：

1. 共同支持和卡尺确实剔除了一些样本；
2. 但保留样本量仍然充足；
3. 平均匹配距离非常小，说明匹配质量很好。

可以写：

匹配诊断显示，各场景匹配后仍保留了较多样本，平均匹配距离均接近 0，说明卡尺约束并未导致严重样本损失，同时有效避免了倾向得分差异过大的坏匹配。

不过场景 A 里你有一个小小的不一致：
balance_summary 里 A 匹配后 N 是 986，但 matching_diagnostics 里 A 匹配后 N 是 878。这个不是代码错误，大概率是因为两个函数分别重新生成了一份随机数据，所以样本数不同。

如果想避免报告里看起来冲突，可以在报告中不要同时列两个表，或者说明：

平衡性检验与匹配诊断基于单次随机生成数据，样本保留数量会随模拟数据略有波动。

更稳妥的是：报告只用 balance_summary 说明 SMD，matching_diagnostics 只用文字概括，不必放完整表。

⸻

7. 当前结果有没有需要调整？

我的判断：不用调整。

原因是三个场景的结果非常符合预期：

场景	理论预期	结果是否符合
A	DID 和 PSM-DID 接近 2	符合
B	DID 有偏，DID_X 和 PSM-DID 接近 2	符合，非常明显
C	PSM-DID 仍有偏	符合，非常明显

如果非要微调，我只建议一个小点：
PSM 在场景 B 也很接近 2，这可能会让“PSM-DID 优于 PSM”的对比不够强。但这不是问题，因为你的主要比较是 DID vs PSM-DID，而且作业要求比较 PSM、DID、PSM-DID，不一定要求 PSM 在 B 中必须失败。

如果你想让 PSM 更有偏，可以加强个体固定效应或让处理前水平差异更复杂。但我不建议现在改，因为目前结果已经很干净，理论主线非常清楚。

⸻

8. 你报告里可以直接使用的结果分析段落

放在：

report.md

对应：

##5. 模拟结果

可以写：

###5.1 场景 A：无条件平行趋势成立
场景 A 中，DID 的平均估计值为 1.999，Bias 仅为 -0.001，PSM-DID 的平均估计值为 1.993，Bias 为 -0.007，二者均接近真实处理效应 \(\tau=2\)。这说明当协变量 \(X\) 只影响处理选择和结果水平，而不影响结果变化趋势时，处理组和对照组满足无条件平行趋势，DID 已经能够较好恢复真实处理效应。PSM-DID 的估计同样接近真实值，但相较 DID 并没有明显优势，并且其标准差略高，说明匹配可能带来一定样本损失和估计波动。
###5.2 场景 B：趋势差异由可观测协变量解释
场景 B 中，普通 DID 的平均估计值为 2.697，Bias 达到 0.697，明显高估真实处理效应。这是因为 \(X\) 同时影响处理选择和未处理状态下的结果变化趋势，处理组和对照组在 \(X\) 分布上的差异导致无条件平行趋势不成立。相比之下，控制 \(X\) 的 DID 平均估计值为 2.001，PSM-DID 平均估计值为 1.997，二者均接近真实值。这说明当趋势差异主要来自可观测协变量时，匹配能够改善处理组和对照组在 \(X\) 上的平衡性，使匹配后的 DID 更接近条件平行趋势。
###5.3 场景 C：存在不可观测时间变化混淆
场景 C 中，PSM-DID 的平均估计值为 2.923，Bias 为 0.923，仍然明显偏离真实处理效应。DID 和控制 \(X\) 的 DID 也分别存在 1.357 和 0.929 的偏误。这说明当不可观测变量 \(U\) 同时影响处理选择和未处理状态下的结果变化趋势时，基于 \(X\) 的匹配无法阻断由 \(U\) 形成的偏误路径。即使处理组和对照组在可观测变量 \(X\) 上实现平衡，条件平行趋势仍可能不成立，因此 PSM-DID 也会失效。
###5.4 平衡性与共同支持
匹配前后标准化均值差异显示，PSM 显著改善了处理组和对照组在可观测协变量 \(X\) 上的平衡性。以场景 B 为例，匹配前 \(X\) 的 SMD 为 0.827，匹配后下降至接近 0，说明倾向得分匹配有效缩小了两组在 \(X\) 上的差异。这也解释了为什么 PSM-DID 在场景 B 中能够显著降低普通 DID 的偏误。
但场景 C 的结果也表明，良好的可观测协变量平衡并不等于因果识别一定成立。场景 C 中匹配后 \(X\) 的 SMD 同样接近 0，但 PSM-DID 仍然存在明显偏误，原因是不可观测变量 \(U\) 仍然同时影响处理分配和结果变化趋势。因此，平衡性检验只能支持可观测变量层面的可比性，不能排除不可观测时间变化混淆。

⸻

9. 最终结论

你这个 Notebook 的结果可以用。

目前最重要的结论是：

A：DID 有效，PSM-DID 没有额外明显优势。
B：DID 明显有偏，PSM-DID 成功修正，证明 PSM-DID 的价值。
C：PSM-DID 仍然有偏，证明它不是万能方法。

## 5. 报告初稿

**AI：** 对，现在这几部分建议统一写在 report.md 里，不要分散写在 Notebook 里。

Notebook 主要承担：

code/psm_did_simulation.ipynb

作用是：代码、运行结果、图表生成。

正式文字报告写在：

report.md

你现在要完成的是 report.md 的前半部分，建议结构如下：

#用 Monte Carlo 模拟理解 PSM-DID
##1. 问题背景
##2. 模拟设计
###2.1 数据生成过程的总体思路
###2.2 场景 A：无条件平行趋势成立
###2.3 场景 B：趋势差异由可观测协变量解释
###2.4 场景 C：存在不可观测时间变化混淆
##3. 估计方法与评价指标
##4. 模拟结果展示
###4.1 总体结果
###4.2 场景 A 结果
###4.3 场景 B 结果
###4.4 场景 C 结果
###4.5 匹配平衡性与共同支持
##5. 理论解释

下面我先给你一版可以直接放进 report.md 的 Markdown 草稿。后面你再根据图表文件名插入图片。

⸻

#用 Monte Carlo 模拟理解 PSM-DID
##1. 问题背景
在观察性研究中，处理变量通常并非随机分配。个体、企业或地区是否接受某项政策或项目，往往与其处理前特征、发展基础或潜在趋势有关。因此，简单比较处理组和对照组的结果差异，往往不能直接解释为因果效应。
PSM、DID 和 PSM-DID 是政策评估中常用的三类方法。PSM 试图通过处理前协变量构造可比的处理组和对照组，减少由可观测变量导致的选择偏误；DID 利用处理前后的变化差异识别政策效应，可以消除时间不变的个体异质性；PSM-DID 则先进行匹配，再在匹配样本上实施 DID，试图在更可比的样本中识别处理效应。
不过，PSM-DID 并不是 PSM 与 DID 的机械叠加，也不是自动消除偏误的万能方法。它的有效性依赖于处理前协变量是否足以解释处理组和对照组之间的趋势差异，以及匹配后是否满足条件平行趋势。本文通过 Monte Carlo 模拟构造不同数据生成过程，在已知真实处理效应的前提下，比较 PSM、DID、控制协变量的 DID 和 PSM-DID 在不同场景下的表现。
本文设定真实处理效应为：
\[
\tau = 2
\]
并通过 500 次重复模拟计算各方法的平均估计值、Bias、RMSE 和标准差，以检验不同方法在不同识别条件下是否能够恢复真实处理效应。

⸻

2. 模拟设计

##2. 模拟设计
###2.1 数据生成过程的总体思路
本文的 Monte Carlo 模拟并不是简单地重复运行估计代码，而是通过可控的数据生成过程检验 PSM、DID 与 PSM-DID 的识别逻辑。具体而言，本文先设定真实处理效应为 \(\tau=2\)，再构造处理前协变量 \(X_i\)、处理状态 \(D_i\)、处理前结果 \(Y_{i0}\) 和处理后结果 \(Y_{i1}\)。
每次模拟中，样本量设定为 \(n=1000\)，并重复模拟 500 次。基本数据结构为：
\[
\Delta Y_i = Y_{i1} - Y_{i0}
\]
其中，\(\Delta Y_i\) 是 DID 和 PSM-DID 中用于比较的结果变化。本文分别设计三个场景，对应 PSM-DID 的三种典型识别环境：DID 本身有效、PSM-DID 可能优于 DID，以及 PSM-DID 也可能失败。
###2.2 场景 A：无条件平行趋势成立
场景 A 用于模拟 DID 表现较好的基准情形。在该场景中，处理前协变量 \(X_i\) 影响处理分配和结果水平，但不影响未处理状态下的结果变化趋势。
具体设定为：
\[
X_i \sim N(0,1)
\]
\[
P(D_i=1|X_i)=logit^{-1}(\gamma_0+\gamma_1X_i)
\]
处理前结果为：
\[
Y_{i0}=\alpha_i+\beta X_i+\epsilon_{i0}
\]
处理后结果为：
\[
Y_{i1}=\alpha_i+\beta X_i+\lambda+\tau D_i+\epsilon_{i1}
\]
其中，\(\alpha_i\) 表示时间不变的个体异质性，\(\lambda\) 表示共同时间趋势，\(\tau\) 表示真实处理效应。在该场景中，\(X_i\) 只影响结果水平，不影响自然增长趋势。因此，即使处理组和对照组在处理前水平上存在差异，只要没有处理，两组的结果变化趋势仍然相同。
该场景的理论预期是：DID 和 PSM-DID 都应接近真实处理效应，而 PSM-DID 相比 DID 不一定具有明显优势。
###2.3 场景 B：趋势差异由可观测协变量解释
场景 B 用于模拟 PSM-DID 可能优于未匹配 DID 的情形。在该场景中，\(X_i\) 不仅影响处理分配，也影响未处理状态下的结果变化趋势。
处理分配仍然设定为：
\[
P(D_i=1|X_i)=logit^{-1}(\gamma_0+\gamma_1X_i)
\]
处理前结果为：
\[
Y_{i0}=\alpha_i+\beta X_i+\epsilon_{i0}
\]
处理后结果为：
\[
Y_{i1}=\alpha_i+\beta X_i+\lambda+\delta X_i+\tau D_i+\epsilon_{i1}
\]
其中，\(\delta X_i\) 表示 \(X_i\) 对结果自然变化趋势的影响。因此，在没有处理时：
\[
\Delta Y_i(0)=\lambda+\delta X_i+\epsilon_i
\]
如果处理组和对照组在 \(X_i\) 上分布不同，则两组即使没有处理，也会呈现不同的自然变化趋势。此时，未匹配 DID 的无条件平行趋势假设被破坏。
该场景的理论预期是：普通 DID 会产生偏误，而控制 \(X_i\) 的 DID 和 PSM-DID 应更接近真实处理效应。PSM-DID 可能有效的原因在于，它先通过倾向得分匹配改善处理组和对照组在 \(X_i\) 上的平衡性，再在匹配样本中比较结果变化，使条件平行趋势更接近成立。
###2.4 场景 C：存在不可观测时间变化混淆
场景 C 用于模拟 PSM-DID 也可能失败的情形。在该场景中，除可观测协变量 \(X_i\) 外，还存在不可观测变量 \(U_i\)。该变量同时影响处理分配和未处理状态下的结果变化趋势。
设定：
\[
U_i \sim N(0,1)
\]
处理分配由 \(X_i\) 和 \(U_i\) 共同决定：
\[
P(D_i=1|X_i,U_i)=logit^{-1}(\gamma_0+\gamma_1X_i+\gamma_2U_i)
\]
处理前结果为：
\[
Y_{i0}=\alpha_i+\beta X_i+\theta U_i+\epsilon_{i0}
\]
处理后结果为：
\[
Y_{i1}=\alpha_i+\beta X_i+\theta U_i+\lambda+\delta X_i+\rho U_i+\tau D_i+\epsilon_{i1}
\]
其中，\(\rho U_i\) 表示不可观测变量 \(U_i\) 对结果变化趋势的影响。需要强调的是，虽然 \(U_i\) 参与了真实数据生成过程，但在估计阶段研究者无法观察到 \(U_i\)，因此倾向得分匹配和控制变量回归都只能使用 \(X_i\)。
该场景的理论预期是：即使匹配后 \(X_i\) 达到平衡，PSM-DID 仍然可能有偏，因为不可观测变量 \(U_i\) 仍然同时影响处理分配和结果变化趋势，条件平行趋势不成立。

⸻

3. 估计方法与评价指标

##3. 估计方法与评价指标
本文在每个场景中比较四类估计方法：单独 PSM、单独 DID、控制 \(X_i\) 的 DID，以及 PSM-DID。
###3.1 单独 PSM
单独 PSM 使用处理前协变量 \(X_i\) 估计倾向得分：
\[
\hat e(X_i)=P(D_i=1|X_i)
\]
本文使用 Logistic 回归估计倾向得分，并采用 1:1 最近邻匹配。匹配后，单独 PSM 通过比较处理组和对照组的处理后结果 \(Y_{i1}\) 均值差来估计处理效应。
###3.2 单独 DID
DID 比较处理组和对照组处理前后的结果变化差异：
\[
\hat{\tau}_{DID}
=
(\bar{Y}_{T,post}-\bar{Y}_{T,pre})
-
(\bar{Y}_{C,post}-\bar{Y}_{C,pre})
\]
在两期数据中，也可以通过变化值回归实现：
\[
\Delta Y_i = \alpha + \tau D_i+\epsilon_i
\]
其中，\(D_i\) 的系数即为 DID 估计量。
###3.3 控制 \(X_i\) 的 DID
为了比较控制可观测趋势差异后的 DID 表现，本文进一步估计：
\[
\Delta Y_i = \alpha+\tau D_i+\gamma X_i+\epsilon_i
\]
该方法允许 \(X_i\) 影响处理前后的自然变化趋势，因此在场景 B 中应比普通 DID 表现更好。
###3.4 PSM-DID
PSM-DID 先使用处理前协变量 \(X_i\) 估计倾向得分并进行匹配，再在匹配样本中比较处理组和对照组的结果变化：
\[
\hat{\tau}_{PSM-DID}
=
E[\Delta Y_i|D_i=1, matched]
-
E[\Delta Y_i|D_i=0, matched]
\]
本文采用基于 Logistic 倾向得分的 1:1 最近邻匹配。匹配采用有放回方式，即同一个对照组个体可以被多个处理组个体重复匹配。为了避免坏匹配，本文设置卡尺约束，卡尺取 \(0.2\times SD(\hat e(X))\)。在匹配前，本文使用 min-max 规则处理共同支持域，只保留处理组和对照组倾向得分分布重叠区间内的样本。
因此，本文的 PSM-DID 估计对象更接近于“具有共同支持且成功匹配的处理组个体”的 ATT，而不是总体 ATE。
###3.5 评价指标
本文对每种方法在 500 次重复模拟中的估计值进行汇总，报告以下指标：
\[
Bias = \bar{\hat{\tau}}-\tau
\]
\[
RMSE = \sqrt{\frac{1}{R}\sum_{r=1}^{R}(\hat{\tau}_r-\tau)^2}
\]
其中，\(R=500\)，\(\tau=2\)。Bias 衡量估计值是否存在系统性偏误，RMSE 同时反映偏误和估计波动，Std 则衡量估计结果在重复模拟中的稳定性。

⸻

4. 模拟结果展示

这里你要插入表格和图。建议结果表直接写在 report.md 里，图用相对路径引用。

##4. 模拟结果展示
###4.1 总体结果
表 1 展示了三个场景下四种估计方法的 Monte Carlo 结果。
| 场景 | 方法 | Mean Estimate | Bias | RMSE | Std |
|---|---:|---:|---:|---:|---:|
| A | PSM | 2.010 | 0.010 | 0.127 | 0.127 |
| A | DID | 1.999 | -0.001 | 0.088 | 0.088 |
| A | DID_X | 1.999 | -0.001 | 0.094 | 0.094 |
| A | PSM-DID | 1.993 | -0.007 | 0.134 | 0.134 |
| B | PSM | 1.993 | -0.007 | 0.135 | 0.135 |
| B | DID | 2.697 | 0.697 | 0.706 | 0.112 |
| B | DID_X | 2.001 | 0.001 | 0.095 | 0.095 |
| B | PSM-DID | 1.997 | -0.003 | 0.130 | 0.130 |
| C | PSM | 3.691 | 1.691 | 1.704 | 0.210 |
| C | DID | 3.357 | 1.357 | 1.363 | 0.123 |
| C | DID_X | 2.929 | 0.929 | 0.936 | 0.114 |
| C | PSM-DID | 2.923 | 0.923 | 0.936 | 0.157 |
真实处理效应为 \(\tau=2\)。从总体结果看，三个场景分别对应了三种不同的理论情况：场景 A 中 DID 和 PSM-DID 均表现良好；场景 B 中普通 DID 明显有偏，而 PSM-DID 和控制 \(X\) 的 DID 接近真实效应；场景 C 中即使使用 PSM-DID，估计结果仍然存在明显偏误。

场景 A

###4.2 场景 A：无条件平行趋势成立
场景 A 中，DID 的平均估计值为 1.999，Bias 仅为 -0.001，几乎等于真实处理效应 \(\tau=2\)。PSM-DID 的平均估计值为 1.993，Bias 为 -0.007，也接近真实值。
这一结果说明，当 \(X_i\) 只影响结果水平而不影响结果变化趋势时，处理组和对照组满足无条件平行趋势。此时 DID 已经能够较好识别真实处理效应，PSM-DID 并没有明显额外优势。与此同时，DID 的标准差为 0.088，低于 PSM-DID 的 0.134，说明匹配带来的样本筛选可能增加估计波动。
![场景A估计值分布](figures/A_Unconditional_Parallel_Trend_distribution.png)

场景 B

###4.3 场景 B：趋势差异由可观测协变量解释
场景 B 中，普通 DID 的平均估计值为 2.697，Bias 达到 0.697，明显高估真实处理效应。这是因为 \(X_i\) 同时影响处理分配和未处理状态下的结果变化趋势，而处理组和对照组在 \(X_i\) 上分布不同，导致无条件平行趋势不成立。
相比之下，控制 \(X_i\) 的 DID 平均估计值为 2.001，Bias 仅为 0.001；PSM-DID 的平均估计值为 1.997，Bias 为 -0.003，也非常接近真实值。这说明当趋势差异主要由可观测协变量解释时，控制 \(X_i\) 或基于 \(X_i\) 匹配都能够显著降低偏误。
场景 B 是 PSM-DID 最能发挥作用的情形。PSM 先改善处理组和对照组在 \(X_i\) 上的平衡性，再在匹配样本上实施 DID，使条件平行趋势更接近成立。
![场景B估计值分布](figures/B_Conditional_Parallel_Trend_distribution.png)

场景 C

###4.4 场景 C：存在不可观测时间变化混淆
场景 C 中，PSM-DID 的平均估计值为 2.923，Bias 为 0.923，仍然明显偏离真实处理效应。DID 和控制 \(X_i\) 的 DID 也分别存在 1.357 和 0.929 的偏误。
这一结果说明，当不可观测变量 \(U_i\) 同时影响处理选择和未处理状态下的结果变化趋势时，仅仅匹配或控制可观测协变量 \(X_i\) 并不能消除偏误。虽然 PSM-DID 可以改善处理组和对照组在 \(X_i\) 上的可比性，但无法处理 \(U_i\) 造成的趋势差异。因此，条件平行趋势仍然不成立，PSM-DID 仍然会失败。
![场景C估计值分布](figures/C_Unobserved_Time_Varying_Confounding_distribution.png)

匹配平衡性与共同支持

###4.5 匹配平衡性与共同支持
为了检验 PSM 是否改善了处理组和对照组在可观测协变量上的平衡性，本文计算匹配前后 \(X_i\) 的标准化均值差异。结果显示，匹配前各场景中 \(X_i\) 的 SMD 均较大，而匹配后 SMD 基本下降至 0 附近。
以场景 B 为例，匹配前 \(X_i\) 的 SMD 为 0.827，说明处理组和对照组在处理前协变量上存在明显差异；匹配后 SMD 接近 0，说明倾向得分匹配显著改善了 \(X_i\) 的平衡性。这也解释了为什么 PSM-DID 在场景 B 中能够明显降低普通 DID 的偏误。
不过，场景 C 的结果也说明，良好的可观测协变量平衡并不等于因果识别一定成立。场景 C 中，匹配后 \(X_i\) 的 SMD 同样接近 0，但 PSM-DID 仍然存在明显偏误。这是因为不可观测变量 \(U_i\) 仍然同时影响处理分配和结果变化趋势，而平衡性检验无法检验不可观测变量是否平衡。
![匹配前后平衡性](figures/balance_before_after.png)
![场景B共同支持](figures/B_Conditional_Parallel_Trend_common_support.png)

⸻

5. 理论解释

##5. 理论解释
###5.1 PSM 为什么可能有效？
PSM 的有效性依赖于条件可忽略性假设：
\[
(Y_i(0),Y_i(1))\perp D_i|X_i
\]
也就是说，在给定处理前协变量 \(X_i\) 后，处理分配可以近似看作随机。在本文的模拟中，处理状态 \(D_i\) 与 \(X_i\) 有关，因此直接比较处理组和对照组可能受到选择偏误影响。PSM 通过估计倾向得分并进行匹配，使处理组和对照组在 \(X_i\) 上更接近。
从平衡性结果看，匹配后 \(X_i\) 的标准化均值差异明显下降，说明 PSM 确实改善了可观测协变量的平衡性。因此，当处理选择和潜在结果的差异主要由可观测变量 \(X_i\) 决定时，PSM 可能有效。
不过，PSM 只能平衡可观测变量。如果存在不可观测变量同时影响处理分配和潜在结果，PSM 本身无法消除这类偏误。
###5.2 DID 为什么可能有效？
DID 的优势在于利用处理前后的变化来消除时间不变的个体异质性。设个体存在固定特征 \(\alpha_i\)，若该特征不随时间变化，则前后差分可以将其消除。
DID 的核心识别假设是平行趋势：
\[
E[\Delta Y_i(0)|D_i=1]=E[\Delta Y_i(0)|D_i=0]
\]
也就是说，如果没有处理，处理组和对照组的结果变化趋势应当相同。
场景 A 的结果验证了这一点。虽然 \(X_i\) 影响处理选择和结果水平，但它不影响结果变化趋势，因此无条件平行趋势成立。DID 的平均估计值为 1.999，几乎等于真实处理效应 2，说明 DID 能够在该场景下恢复真实效应。
###5.3 为什么 PSM-DID 可能优于未匹配 DID？
当趋势差异依赖于可观测协变量 \(X_i\) 时，普通 DID 可能违反无条件平行趋势。场景 B 中，\(X_i\) 同时影响处理分配和未处理状态下的结果变化趋势。因此，处理组和对照组在 \(X_i\) 上的差异会转化为自然增长趋势的差异，导致普通 DID 高估处理效应。
PSM-DID 的作用在于，先使用 \(X_i\) 进行倾向得分匹配，使处理组和对照组在 \(X_i\) 上更加相似，再在匹配样本上进行 DID。此时，PSM-DID 依赖的是条件平行趋势：
\[
E[\Delta Y_i(0)|D_i=1,X_i]=E[\Delta Y_i(0)|D_i=0,X_i]
\]
场景 B 的结果显示，普通 DID 的 Bias 为 0.697，而 PSM-DID 的 Bias 仅为 -0.003。这说明当趋势差异主要由可观测协变量解释时，PSM-DID 能够显著改善未匹配 DID 的偏误。
###5.4 为什么 PSM-DID 仍然可能失败？
PSM-DID 的局限在于，它仍然依赖处理前可观测协变量能够充分解释处理组和对照组之间的趋势差异。如果存在不可观测变量 \(U_i\) 同时影响处理分配和未处理状态下的结果变化趋势，那么即使匹配后 \(X_i\) 实现平衡，条件平行趋势仍然可能不成立。
场景 C 正是这一情形。数据生成过程中，\(U_i\) 同时影响 \(D_i\) 和 \(\Delta Y_i(0)\)，但估计时研究者无法观察 \(U_i\)。因此，PSM 只能平衡 \(X_i\)，不能平衡 \(U_i\)。结果显示，PSM-DID 的 Bias 仍然达到 0.923，说明其无法消除不可观测时间变化混淆造成的系统性偏误。
这说明 PSM-DID 不是万能方法。良好的可观测协变量平衡只能说明处理组和对照组在 \(X_i\) 上更可比，不能证明不存在不可观测趋势差异。
###5.5 匹配是否改变估计对象？
匹配会改变估计对象。ATE 表示总体平均处理效应：
\[
ATE=E[Y_i(1)-Y_i(0)]
\]
ATT 表示处理组平均处理效应：
\[
ATT=E[Y_i(1)-Y_i(0)|D_i=1]
\]
本文的 PSM-DID 以处理组为基准，为每个处理组个体寻找相似的对照组个体，因此其估计对象更接近 ATT，而不是 ATE。
进一步地，本文还使用共同支持处理和卡尺约束。如果某些处理组个体缺乏相似的对照组，它们会被剔除。因此，本文实际估计的更准确地说是：
\[
ATT_{matched}=E[Y_i(1)-Y_i(0)|D_i=1, matched]
\]
也就是具有共同支持且成功匹配的处理组个体的平均处理效应。这说明匹配可以提高可比性，但代价是改变估计对象。
###5.6 共同支持不足时，为什么偏误和方差都可能恶化？
共同支持要求处理组和对照组在倾向得分分布上存在重叠。如果共同支持不足，部分处理组个体无法在对照组中找到相似对象。
如果研究者强行匹配这些样本，即使最近邻对照组也可能与处理组个体差异较大，从而产生坏匹配并增加偏误。如果研究者选择剔除这些缺乏共同支持的样本，则样本量下降，估计方差可能上升，同时估计对象也会从全体处理组转向可匹配处理组。
因此，共同支持不足会带来偏误与方差之间的权衡。本文通过 min-max 规则处理共同支持，并设置卡尺约束，目的正是避免坏匹配。但这种处理也意味着部分样本可能被剔除，估计对象发生变化。

⸻

你现在具体怎么写、写在哪里？

你现在就做这几步：

1. 在项目根目录新建：

report.md

2. 把上面这几部分放进去：

##1. 问题背景
##2. 模拟设计
##3. 估计方法与评价指标
##4. 模拟结果展示
##5. 理论解释

3. 图表文件保持在：

figures/

4. 结果 CSV 保持在：

outputs/

5. report.md 里引用图片时，用这种相对路径：

![场景B估计值分布](figures/B_Conditional_Parallel_Trend_distribution.png)

不要写电脑本地绝对路径，比如：

/Users/你的名字/Desktop/...

GitHub 上会打不开。

⸻

目前你这部分如果按这个结构写，评分要点里：

Monte Carlo 设计 25%
方法实现 20%
理论解释 25%
写作呈现 10%

基本都能覆盖到。剩下还缺的是论文评价 20%，后面单独做。


## 6. 文献选取与评价
**User**:检索要求：主流中文或英文学术期刊中选择一篇使用 PSM-DID 的论文。论文可以来自经济学、金融、管理、公共政策、劳动、产业组织、环境经济学等相关领域
关于上述领域的文献，你认为高质量的标准是什么？请依照这些标准帮我找几篇高质量文献
**AI**：可以。你这个作业要选一篇做“真实论文评价”，我建议优先选政策评估类、PSM-DID 使用比较清楚、能和你模拟 A/B/C 对上的论文。下面这些都比较适合。

优先推荐

推荐度	文献	领域	为什么适合你的作业
首选	刘瑞明、赵仁杰：《西部大开发：增长驱动还是政策陷阱——基于 PSM-DID 方法的研究》	区域经济 / 公共政策	中文高质量期刊《中国工业经济》，政策处理清楚，PSM-DID 是核心方法，适合评价共同趋势、匹配变量、共同支撑和政策选择偏误。
备选 1	Fan et al. “Transformation effect of resource-based cities based on PSM-DID model: An empirical analysis from China”	城市转型 / 资源型城市	英文期刊 Environmental Impact Assessment Review，研究中国资源型城市转型政策，PSM-DID 明确，和你“城市政策试点”的例子非常贴近。 ￼
备选 2	Li, Wu & Sun, “Will China’s audit of natural environmental resource promote green sustainable development? Evidence from PSM-DID analysis…”	环境政策 / 绿色发展	PLOS ONE 论文，开放获取，题目直接使用 PSM-DID，便于下载全文和写方法评价。 ￼
备选 3	Wang et al. “The Effect Evaluation of China’s Energy-Consuming Right Trading Policy: Empirical Analysis Based on PSM-DID”	环境经济 / 能源政策	Sustainability 论文，政策背景清晰，PSM-DID 写得直观，适合做方法评价。 ￼
备选 4	黄寰等：《低碳城市试点政策的碳减排效应》	环境经济 / 低碳城市	中文期刊《资源科学》，基于 2006—2019 年中国城市面板数据，使用 PSM-DID 分析低碳城市试点。 ￼
备选 5	郭沛等：《低碳试点政策是否提高了城市碳排放效率》	自然资源 / 环境政策	中文期刊《自然资源学报》，低碳城市政策评价，使用 PSM-DID，适合和你的“城市试点政策”模拟逻辑对应。 ￼

⸻

我最建议你选哪一篇？

我建议你选：

刘瑞明、赵仁杰：《西部大开发：增长驱动还是政策陷阱——基于 PSM-DID 方法的研究》，《中国工业经济》，2015年第6期，32–43页。

原因是：

这篇和你的模拟报告最容易对接。它研究的是区域政策冲击，处理组和对照组是不同地区，天然存在“被纳入政策的地区本来就不同”的选择偏误问题。也就是说，它很适合用你前面模拟里的逻辑来评价：

* PSM 试图解决：西部地区和非西部地区在处理前特征上不同；
* DID 试图解决：地区之间存在时间不变的差异；
* PSM-DID 的关键假设是：匹配后处理组和对照组满足条件平行趋势；
* 可批判点是：西部地区可能存在不可观测的发展趋势差异，例如资源禀赋、地理位置、制度环境、中央投资倾向等，这些不一定能被匹配变量完全控制。

这篇文献的题名、期刊和方法信息可以从南开大学中国区域政策研究中心页面核对到，该页面显示其使用 1994—2012 年中国 283 个地级市面板数据，并采用 PSM-DID 方法分析西部大开发政策效应；另有文献引用页也列出该文发表于《中国工业经济》2015年第6期，页码 32–43。 ￼

⸻

如果你想选英文论文

英文里我更建议选这篇：

Fan, F. et al. “Transformation effect of resource-based cities based on PSM-DID model: An empirical analysis from China.” Environmental Impact Assessment Review, 2021.

它的优点是：研究对象也是中国城市政策，PSM-DID 是核心方法，且摘要明确说明其选择中国 87 个资源型城市，并在其余 166 个城市中匹配控制样本，用 DID 和 PSM 评估规划对城市转型的影响。 ￼

这篇和你的模拟也很好对接：

* 处理组：资源型城市或政策规划城市；
* 对照组：其他城市；
* 匹配变量：城市处理前经济、产业、人口等特征；
* 核心风险：资源型城市可能本身就存在不同转型趋势；
* 批判点：即使匹配了可观测变量，也可能仍有不可观测的产业路径依赖、地方治理能力、资源枯竭程度差异。

⸻

不太建议优先选的类型

有些论文虽然也写 PSM-DID，但不一定适合你的作业：

* 只是在稳健性检验里顺手做 PSM-DID，而不是核心方法；
* 期刊质量一般、方法部分写得很短；
* 处理组定义不清楚；
* 没有报告匹配平衡、共同支撑或平行趋势；
* 结果变量太复杂，不方便你在短篇作业里评价。

所以从“好写、好评价、和你模拟能对应”的角度，我建议排序是：

刘瑞明、赵仁杰《西部大开发》 
> Fan et al. 资源型城市转型 
> 低碳城市试点相关中文论文 
> 能耗权交易 / 自然资源审计类英文论文

最稳的是第一篇。

**User**:[上传文献]阅读这篇文献，先给我列出一些这篇文献的基本信息
**AI**：可以。先不进入深度评价，先把这篇文献的基本信息整理出来，后面再围绕它的 PSM-DID 设定、识别假设、匹配变量、共同趋势、共同支撑等逐项分析。

文献基本信息

项目	内容
论文题目	《西部大开发：增长驱动还是政策陷阱——基于 PSM-DID 方法的研究》
作者	刘瑞明、赵仁杰
作者单位	西北大学经济管理学院
发表期刊	《中国工业经济》
发表时间	2015 年第 6 期
页码	第 32—43 页
DOI	10.19581/j.cnki.ciejournal.2015.06.004
研究领域	区域经济、政策评估、经济增长
核心方法	PSM-DID，即倾向得分匹配与双重差分结合
研究对象	西部大开发战略对西部地区经济增长的影响
数据范围	1994—2012 年中国 283 个地级市面板数据
处理组	位于西部大开发覆盖省份的地级市
控制组	其他非西部地区地级市
处理时间	2000 年以后，即西部大开发政策实施后
被解释变量	地区实际 GDP 对数值、地区实际人均 GDP 对数值
主要控制变量	政府规模、外商直接投资、固定资产投资、产业结构、工业化水平、教育水平、总储蓄率等
核心结论	西部大开发并未有效推动西部地区 GDP 及人均 GDP 的快速增长，政策过程中存在“政策陷阱”问题
机制解释	政策过度集中于固定资产投资和资源能源开发，忽视体制改革、软环境建设、人力资本积累和产业结构调整

这些信息主要来自论文首页摘要、模型与估计方法部分、数据变量部分和结果部分。论文摘要明确说明作者使用 1994—2012 年中国 283 个地级市面板数据，并采用 PSM-DID 方法研究西部大开发对地区经济发展的影响；模型部分说明其将西部大开发覆盖地区作为处理组，非西部地区作为控制组；数据部分列出了主要变量和数据来源；结果部分指出西部大开发并未显著推动西部地区 GDP 和人均 GDP 增长。 ￼

这篇文章为什么适合你的作业

这篇文献和你前面 Monte Carlo 模拟的主题高度对应。它本质上研究的是一个非随机政策试点：西部地区之所以成为政策对象，并不是随机分配的，而是因为它们在经济基础、产业结构、资源禀赋、教育水平、政府投入等方面与其他地区本来就不同。因此，直接比较西部和非西部地区，或者直接使用普通 DID，都可能受到选择偏误和趋势差异的影响。

作者使用 PSM-DID 的理由也比较清楚：先通过 PSM 为西部地级市寻找处理前特征相近的非西部地级市，再用 DID 比较政策实施前后的变化，从而试图让 DID 更接近共同趋势假设。这个逻辑正好可以和你模拟中的场景 B：趋势差异来自可观测协变量时，PSM-DID 可能优于普通 DID对应起来。

后面可以重点评价的方向

后续分析这篇论文时，建议围绕这几个问题展开：

1. 它的处理组和控制组如何定义；
2. PSM 使用了哪些匹配变量；
3. 作者为什么认为普通 DID 可能不满足共同趋势；
4. PSM-DID 是否真的解决了共同趋势问题；
5. 文章是否充分报告了匹配平衡和共同支撑；
6. 它的估计对象更接近 ATE 还是 ATT；
7. 这篇文章可能面临哪些不可观测时间变化混淆。

其中第 7 点最适合和你模拟中的场景 C 对应：即使匹配了经济基础、产业结构、教育水平等可观测变量，西部地区仍可能在资源禀赋、政策执行能力、地方政府激励、地理条件、中央投资倾向等不可观测或难以完全测量的因素上与其他地区不同，而这些因素又可能影响后续经济增长趋势。

**User**：你会依照什么样的逻辑组织以上八个方面的问题，列出关键点
**AI**：我会按一个由浅入深的识别逻辑来组织，而不是按图片 8 个问题机械分 8 段。核心顺序是：

先交代论文研究设计 → 再解释为什么需要 PSM-DID → 再检查 PSM-DID 的关键前提 → 最后结合你的模拟结果评价可信与脆弱条件。

可以组织成下面这条主线。

⸻

论文评价的组织逻辑

第一层：这篇论文到底在评估什么？

先回答最基础的问题：研究问题、处理变量、处理组、控制组、结果变量是什么。

关键点包括：

* 论文研究的是：西部大开发是否推动了西部地区经济增长。
* 处理变量是：西部大开发政策。
* 处理组是：西部大开发覆盖省份中的地级市。
* 控制组是：其他非西部地区地级市。
* 政策时间是：2000 年以后。
* 结果变量是：地区实际 GDP 对数值和实际人均 GDP 对数值。
* 数据是：1994—2012 年中国 283 个地级市面板数据。

这一层的作用是把论文的因果问题翻译成 PSM-DID 语言：

D = 是否属于西部大开发地区
Post = 是否处于政策实施后
Y = 地区 GDP 或人均 GDP
D × Post = 西部大开发的政策处理效应

论文明确把西部大开发视为政策试验，并将西部地区地级市作为处理组、其他地区地级市作为控制组；被解释变量用于度量地级市经济增长，包括实际 GDP 和实际人均 GDP 的对数值。 ￼

⸻

第二层：为什么作者不用单独 PSM 或普通 DID，而要用 PSM-DID？

这一层回答图片中的第二个问题。

这里的逻辑是：

普通 DID 的优势是可以控制不随时间变化的地区差异，但前提是处理组和控制组满足共同趋势。问题在于，西部地区和非西部地区在经济基础、产业结构、资源禀赋、政府投入、人力资本等方面差异很大，直接使用普通 DID 可能不满足共同趋势假设。

单独 PSM 的优势是可以让西部城市和非西部城市在可观测变量上更相似，但它不能利用政策实施前后的变化，也不能消除时间不变的未观测异质性。

所以作者使用 PSM-DID 的逻辑是：

PSM：先找可观测特征相近的非西部城市
DID：再比较政策前后的变化
PSM-DID：用匹配后的样本做 DID，试图让共同趋势更可信

论文自己也说明，已有 DID 研究可能没有建立在共同趋势假设上，而西部地区与其他地区存在巨大差异，可能不满足共同趋势，因此使用 PSM-DID，希望使 DID 的政策评价结果更符合共同趋势假设。 ￼

这一层可以和你模拟里的场景 B对应：如果趋势差异主要来自可观测协变量，那么 PSM-DID 比未匹配 DID 更合理。

⸻

第三层：PSM 这一步做得是否足够规范？

这一层集中讨论：匹配变量、是否为处理前变量、平衡性检验、共同支撑。

关键点包括：

1. 匹配变量是否合理？

论文使用的变量包括：

* 政府规模；
* 外商直接投资水平；
* 固定资产投资水平；
* 第三产业比重；
* 工业化水平；
* 教育水平；
* 总储蓄率。

这些变量确实与地区经济增长和是否被纳入西部大开发有关。论文在变量表中列出了这些变量及计算方法，并说明在 PSM-DID 中使用 Logit 模型估计倾向得分。 ￼

2. 匹配变量是否都是处理前变量？

这是一个重要评价点。

PSM-DID 中，匹配变量最好是政策实施前已经确定的变量，否则可能出现“后处理变量”或“坏控制”问题。比如，固定资产投资、政府支出、产业结构本身可能受到西部大开发影响。如果用政策实施后的这些变量做匹配，就可能把政策结果的一部分当成匹配条件。

所以评价时要写：

* 论文列出了控制变量；
* 但需要进一步确认匹配时是否使用政策前变量；
* 如果没有清楚说明这一点，识别策略会有一定风险。

3. 是否报告平衡性检验和共同支撑？

论文提到进行了 Logit 检验和协变量平衡性检验，并称匹配后协变量均值在处理组和控制组之间不存在显著差异，各变量分布变得均衡；但由于篇幅限制，相关结果没有在正文中展示，而是说明可向作者索取。 ￼

这个地方要评价：

* 优点：作者意识到 PSM-DID 需要平衡性检验；
* 不足：正文没有展示具体平衡性结果；
* 更规范的做法：报告匹配前后标准化差异、共同支撑图、匹配样本损失情况。

这一层对应你模拟里的匹配诊断：仅说“匹配后平衡”是不够的，最好像你报告里那样展示 balance 和 common support。

⸻

第四层：DID 的平行趋势是否可信？

这一层回答图片中的平行趋势问题。

关键点包括：

* 作者明确意识到普通 DID 的共同趋势假设可能不满足；
* 采用 PSM-DID 是为了改善共同趋势；
* 论文还做了动态效应检验，考察政策效应是否随时间变化；
* 但严格来说，动态效应检验不完全等于政策前平行趋势检验。

这里可以这样评价：

论文使用 PSM-DID 的主要理由之一，就是西部地区与其他地区经济增长趋势可能存在系统性差异。作者也进行了动态效应检验，讨论西部大开发政策是否存在随时间变化的影响。 ￼ 但从更严格的 DID 识别标准看，论文最好单独展示政策实施前处理组和控制组的趋势是否相似，或者使用事件研究图检验政策前的各期系数是否接近 0。否则，PSM-DID 虽然提高了可比性，但不能完全证明平行趋势成立。

这一层对应你的模拟：

* 场景 A：无条件平行趋势成立，DID 可信；
* 场景 B：匹配后条件平行趋势成立，PSM-DID 可信；
* 真实论文需要说明它更接近哪一种，而不是默认 PSM-DID 一定成立。

⸻

第五层：是否存在不可观测时间变化混淆或政策同时发生？

这一层是评价的核心，直接对应你模拟里的场景 C。

关键点包括：

即使匹配了 GDP、人均 GDP、产业结构、教育、政府规模等可观测变量，西部地区和非西部地区仍可能在一些不可观测或难以充分测量的因素上不同，例如：

* 地理区位；
* 资源禀赋；
* 地方政府执行能力；
* 中央投资倾向；
* 长期产业路径依赖；
* 基础设施历史欠账；
* 政策执行强度差异。

如果这些因素不仅影响一个城市是否属于西部大开发范围，也影响其后续增长趋势，那么 PSM-DID 仍可能有偏。

还要讨论政策同时发生问题。西部大开发不是单一政策，而是一揽子区域发展战略，包含财政投入、税收优惠、资源开发、基础设施建设、固定资产投资等多重机制。论文理论框架也把西部大开发拆分为增长驱动效应和政策陷阱效应，涉及财政投入、税收优惠、资源开发、政府激励、软环境建设等机制。 ￼

所以评价时要指出：

* 论文估计的是西部大开发这一整套战略的综合影响；
* 不能简单解释为某一个具体政策工具的因果效应；
* 如果同时期还有其他区域政策或宏观制度变化，也可能影响识别。

这一层对应你的模拟场景 C：不可观测、随时间变化、且与处理选择相关的因素，会让 PSM-DID 失效。

⸻

第六层：估计结果更接近 ATE 还是 ATT？

这一层放在靠后位置比较自然，因为它是在评价识别对象。

关键点：

这篇论文的 PSM-DID 结果更接近 ATT，不是 ATE。

原因是：

* 它是以西部地级市作为处理组；
* 在非西部城市中寻找可比控制样本；
* 估计的是西部大开发对实际接受政策的西部城市的影响；
* 不是估计“如果所有城市都实施西部大开发，平均会怎样”。

论文在介绍 PSM-DID 估计步骤时也明确说，匹配后将西部大开发省份地级市的变化减去非西部大开发省份地级市的变化，得到的是政策的平均处理效应 ATT，用来度量西部大开发对西部省份地级市的实际影响。 ￼

这和你前面模拟里的结论一致：匹配通常围绕处理组构造反事实，因此 PSM-DID 更接近 ATT。

⸻

第七层：基于模拟经验，给出最终评价

最后不要只罗列优缺点，而要回到你的模拟结论。

可以总结成：

这篇论文的识别策略在以下条件下比较可信：

1. 西部地区和非西部地区的差异主要能够由论文匹配变量解释；
2. 匹配变量均为政策实施前变量；
3. 匹配后处理组和控制组具有良好的共同支撑和平衡性；
4. 匹配后两组满足条件平行趋势；
5. 没有其他同时发生且只影响西部地区增长趋势的重要政策冲击。

它在以下条件下比较脆弱：

1. 匹配变量包含政策实施后的变量，产生后处理偏误；
2. 匹配后虽然可观测变量平衡，但不可观测因素仍影响增长趋势；
3. 西部地区存在独特资源禀赋、治理能力、中央投资倾向等时间变化趋势；
4. 共同支撑不足，导致匹配样本代表性下降；
5. 西部大开发与其他区域政策、基础设施投资、资源开发政策同时发生，难以区分单独政策效应。

这个结尾能直接对应你模拟的三类场景：

场景 A：如果平行趋势成立，DID 已经有效。
场景 B：如果趋势差异来自可观测变量，PSM-DID 有优势。
场景 C：如果趋势差异来自不可观测变量，PSM-DID 仍然脆弱。

⸻

最终建议的写作结构

不要拆成 8 个小节，建议写成 3 个小节：

##5  真实文献评价：以刘瑞明、赵仁杰（2015）为例
###5.1 研究设计与 PSM-DID 的使用逻辑
###5.2 匹配质量与识别假设检验
###5.3 基于模拟结果的识别策略评价

这 3 个小节里，8 个问题全部自然覆盖：

图片要求	放在哪一层
研究问题和处理变量	5.1
为什么用 PSM-DID	5.1
匹配变量是否都是处理前变量	5.2
共同支撑和平衡性检验	5.2
平行趋势是否可信、如何检验	5.2
不可观测时间变化混淆或政策同时发生	5.3
ATE 还是 ATT	5.3
哪些条件下可信、哪些条件下脆弱	5.3

这样写出来不会像教材复述，也不会漏掉图片里的要求。

## 7.报告检查与修改
**User**：【上传报告全文及作业要求】以上是我最终的初稿与任务要求，先检查报告逻辑、措辞与完整性，之后对照任务要求细则输出checklist，确保覆盖所有关键点

**AI**：总体判断：这版已经基本完整，任务要求的大项都覆盖了，逻辑也比前面更顺。
但还需要做几处小修，尤其是 3.5 表述、第五部分引用信息、5.3 个别措辞。下面按任务要求给你核对。

⸻

Checklist：对照任务要求

任务要求	当前完成度	检查意见
任务一：自行设计数据生成过程	✅ 已完成	已包含处理前协变量 $X_i$、处理前后两期、处理分配机制 $D_i$、真实处理效应 $\tau=2$、500 次模拟、三个场景 A/B/C。
场景 A：DID 表现较好的基准情形	✅ 已完成	场景 A 中 $X_i$ 影响处理选择和结果水平，但不影响趋势；4.2 又解释了 $\alpha_i$ 和 $X_i$ 在差分中被消去，和模拟设计对应得很好。
场景 B：PSM-DID 可能优于 DID	✅ 已完成	场景 B 中趋势差异来自 $\delta_xX_i$，4.3 解释了普通 DID 偏误来自 $X_i$ 分布差异，也解释了为什么匹配 $X_i$ 后 PSM-DID 更有效。
场景 C：PSM-DID 可能失败	✅ 已完成	场景 C 中 $U_i$ 同时影响处理选择和结果趋势，4.4 明确解释了为什么匹配 $X_i$ 不能解决 $U_i$ 带来的条件平行趋势失败。
任务二：比较 PSM、DID、PSM-DID	✅ 基本完成	表 1 比较了 PSM、DID、控制协变量 DID、PSM-DID，并报告 Mean、Bias、RMSE、标准差；图也展示了分布、Bias/RMSE、匹配平衡和共同支撑。
单独 PSM、单独 DID、PSM-DID 的实现逻辑	⚠️ 需小修	3.5 第一段现在写得不够准确，尤其“估计四类方法下处理组和对照组的结果变化差异”不适用于单独 PSM。需要改。
任务三：解释模拟结果背后的理论原因	✅ 已完成	4.2—4.5 已经逐一回答 PSM、DID、PSM-DID、不可观测混淆、ATT/ATE、共同支撑不足等问题，并且和 A/B/C 模拟设计对应。
不要写成泛泛教材复述	✅ 基本做到	解释基本都绑定到了 $X_i$、$\alpha_i$、$U_i$、$\rho_uU_i$ 和具体 Bias/RMSE 结果，不是单纯背理论。
任务四：评价一篇使用 PSM-DID 的论文	✅ 基本完成	第 5 部分覆盖了研究问题、处理变量、为什么用 PSM-DID、匹配变量、平衡性、共同支撑、平行趋势、不可观测混淆、ATT/ATE 和可信/脆弱条件。
完整列出论文引用信息	❌ 还缺	你现在第五部分正文没有在末尾列出完整参考文献信息，必须补。
文献评价是否依照图片要求	✅ 基本覆盖	图片里的 8 个点都有，但个别地方措辞需要更严谨，尤其“政策同时发生问题文中已有考虑”这句。

⸻

需要修改的关键点

1. 3.5 第一段要改

你现在这句：

在每一次Monte Carlo模拟中，本文分别估计四类方法下处理组和对照组的结果变化差异。

问题是：单独 PSM 并不是比较结果变化差异，而是比较匹配样本中的处理后结果。
建议改成：

在每一次 Monte Carlo 模拟中，本文分别估计四类方法：单独 PSM、普通 DID、控制协变量的 DID 和 PSM-DID。其中，单独 PSM 在匹配样本中比较处理组和对照组的处理后结果；普通 DID 不进行匹配，直接比较处理组和对照组的结果变化差异；控制协变量的 DID 在变化值回归中进一步加入 $X_i$；PSM-DID 则先基于 $X_i$ 进行匹配，再在匹配样本中比较结果变化差异。

这处必须改，否则任务二里“单独 PSM”的定义会不准。

⸻

2. 4.5 标题建议改

你现在的标题是：

###4.5 匹配平衡与共同支撑

但这一节还讲了 ATT/ATE，所以标题建议改成：

###4.5 匹配诊断、共同支撑与估计对象

更完整。

⸻

3. 第五部分整体逻辑是可以的

你的第五部分现在的组织逻辑是：

5.1 研究设计与 PSM-DID 的使用逻辑
5.2 匹配质量与识别假设检验
5.3 基于模拟结果的识别策略评价

这个结构是合理的。它没有机械拆成 8 个问题，但图片中的 8 个要求都自然嵌进去了。

具体对应如下：

图片要求	你现在的位置	是否覆盖
研究问题和处理变量是什么？	5.1 第一段	✅
作者为什么使用 PSM-DID，而不是只用 PSM 或 DID？	5.1 第三、四段	✅
匹配变量是否都是处理前变量？	5.2 第二段	✅
是否报告共同支撑和平衡性检验？	5.2 第三、四段	✅
DID 平行趋势是否可信？作者如何检验？	5.2 最后两段	✅
是否存在不可观测时间变化混淆或政策同时发生？	5.3 第二、三段	✅
估计结果更接近 ATE 还是 ATT？	5.3 第四段	✅
识别策略在哪些条件下可信、哪些条件下脆弱？	5.3 最后一段	✅

⸻

第五部分需要微调的几句话

1. 这句话太口语化

你写的是：

也即场景C中提到的，当不可观测变量同时影响处理选择和结果变化趋势时，PSM-DID仍然可能产生偏误。

建议改成：

这对应于本文模拟中的场景 C：当不可观测变量同时影响处理选择和结果变化趋势时，PSM-DID 即使完成了可观测变量匹配，也仍然可能产生偏误。

⸻

2. “政策同时发生问题文中已有考虑”这句话不够准确

你写的是：

政策同时发生问题文中已有考虑。

这句话容易被理解为“作者已经充分解决了政策同时发生问题”，但其实文章更多是在机制层面讨论西部大开发的复杂政策组合，不等于已经完全排除了识别风险。

建议改成：

政策同时发生问题是评价这篇论文时需要特别注意的一点。论文在机制层面讨论了西部大开发涉及的多种政策工具和执行机制，但这并不等于已经完全排除了多重政策同时发生带来的识别风险。

这样更严谨。

⸻

3. 最后一段可以再收紧

你现在最后一段列了很多条件，内容没错，但稍微有点清单化。可以改成下面这样，更像论文结尾：

总体来看，这篇论文使用 PSM-DID 的动机是清楚的：普通 DID 可能因为西部地区和非西部地区差异过大而不满足共同趋势，单独 PSM 又无法利用政策前后的变化，因此作者选择 PSM-DID 来提高处理组和控制组的可比性，并在此基础上估计政策效应。但这一识别策略能否成立，并不取决于是否使用了 PSM-DID 方法本身，而取决于匹配变量能否充分解释西部地区和非西部地区之间的趋势差异。若匹配变量均为政策实施前变量，且匹配后两组城市具有良好的共同支撑、协变量平衡和条件平行趋势，那么该策略较有说服力；反之，如果匹配变量包含政策实施后的变量，或者仍存在资源禀赋、地理区位、政府执行能力、中央投资倾向等不可观测的时间变化混淆，PSM-DID 仍然可能存在偏误。同时，西部大开发是一揽子区域发展战略，估计结果更适合被理解为综合政策效果，而不是某一单独政策工具的净效应。

这一版比原来的“可信条件（1）（2）（3）……”更自然。

⸻

引用信息必须补

任务明确要求：

请完整列出论文引用信息。

你现在还缺这一部分。建议在报告最后加：

##参考文献
刘瑞明、赵仁杰：《西部大开发：增长驱动还是政策陷阱——基于 PSM-DID 方法的研究》，《中国工业经济》，2015 年第 6 期，第 32—43 页。DOI：10.19581/j.cnki.ciejournal.2015.06.004。

此外，第五部分里涉及论文事实的地方，最好至少加几处文内引用，比如：

（刘瑞明、赵仁杰，2015）

例如：

* 5.1 第一段介绍论文研究对象、数据、处理组和控制组后；
* 5.2 介绍变量和匹配方法后；
* 5.2 介绍作者声称进行了平衡性检验后；
* 5.3 介绍 ATT 后。

论文中确实说明西部大开发覆盖 12 个省份，西部地级市为处理组，其他地级市为控制组，并使用 1994—2012 年 283 个地级市数据；也说明使用 PSM-DID 是因为西部地区与其他地区差异较大，普通 DID 的共同趋势假设可能不满足。 ￼  ￼

⸻

最终判断

这份文本目前已经达到“可提交初稿”的程度，主要问题不是结构，而是几处精修：

1. 改 3.5 第一段，准确区分 PSM、DID、PSM-DID；
2. 4.5 标题改成“匹配诊断、共同支撑与估计对象”；
3. 第五部分补完整参考文献；
4. 第五部分补文内引用；
5. 把“政策同时发生问题文中已有考虑”改得更严谨；
6. 最后一段从清单式改成判断式。

改完后，整体逻辑就比较完整：
模拟设计 → 方法比较 → 理论解释 → 文献评价 四个任务都能对应上。
