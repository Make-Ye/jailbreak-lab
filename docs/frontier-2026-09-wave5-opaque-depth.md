# 第五波:Opaque Serial Depth 深读 — CoT 可监测性的数学基础与崩塌路径

> 本波是对第四波 #1(Astra 隐藏推理)的论文级深潜:
> Brown-Cohen, Lindner, Shah (Google DeepMind, arXiv:2603.09786) + Korbak et al. 2507.11473 + Huginn (arXiv:2502.05171)。
> 这三篇构成"CoT 监测"问题的理论-实证-工程三件套。

## 一、理论根基:为什么 CoT 曾经是安全护城河

### Korbak et al.《CoT Monitorability: A New and Fragile Opportunity》

**核心论证**:"thinking out loud is necessary for hard tasks" — Transformer 架构里,**长序列认知必须流经 CoT**(后层信息传前层的唯一通道)。这曾是结构保证,不是厂商善意。

**联署阵容**(值得记录):OpenAI + Anthropic + DeepMind + 各学界 30+ 人(Bengio/Legg/Irving/Hubinger/Krakovna/Hendrycks...)— 全行业罕见共识文件。

**警告(已被 Astra 验证)**:CoT 可监测性脆弱,受威胁于训练程序/数据源/持续扩展;呼吁 frontier labs **在能力竞争中主动保留 CoT 可读性**。签署者自己的公司 6 个月后选择了 recurrent depth。

## 二、形式化:Opaque Serial Depth(不透明串行深度)

### 定义(精炼版)

```
不透明串行深度 = 两个"可解释瓶颈"(如 token 输出)之间,
               最长计算路径的电路深度

电路深度取自计算复杂性理论:计算同一函数的布尔电路最小深度
  - 实现无关(不纠结"什么算一层")
  - 区分并行与串行(深度 d 的电路 = O(d) 串行时间的下界)
  - 与 CoT 理论接轨(Merrill/Hahn/Liu 的 Transformer 电路深度界)
```

### 关键算法

```
OpaqueSerialDepth(node):
  Depth(node) = node∈可解释集 ? 0
              : ImmediateDepth(node) + max(Depth(child))
  ImmediateDepth: 结合律二元 n 输入 → log₂(n);解析分段函数 → 1
```

**开源工具**: [google-deepmind/serial_depth](https://github.com/google-deepmind/serial_depth) — JAX 计算图自动算任意网络的深度上界(分析 jaxpr)。

### Gemma 3 实测上界(手算)

| 模型 | 公式 | 总深度(T_max) |
|---|---|---|
| Gemma 3 1B | 4370 + 8·log₂T | **4,490** |
| Gemma 3 4B | 6036 + 10·log₂T | 6,206 |
| Gemma 3 12B | 8482 + 16·log₂T | 8,754 |
| Gemma 3 27B | 11322 + 20·log₂T | 11,662 |

直觉:一个 1B 模型在两次 token 输出之间能做 ~4,500 步"没人看得见"的串行计算 — 但被 log 界压着,这是**健康 Transformer 的天花板**。

## 三、架构渐坏阶梯(论文 Figure 3 的四段滑落)

```
① 标准 Transformer      O(L·(log T + log D))
   不透明路径只能"向上"(层间)或"向右"(token 间,log 界)
   → CoT 是硬瓶颈 ✅

② RNN 块替换注意力       O((L+T)·log D)
   "向右"变成线性依赖 — token 间也能藏计算 ⚠️

③ 连续潜空间 CoT         O(L·T_cot·(log T + log D))
   隐状态直接回喂,绕过 token 化
   T_cot 线性乘入 — 想藏多深藏多深 🔥

④ 持久黑箱记忆           无界
   读→算→写 循环,每次写入都累积不透明计算
   无法用 L/T/D 界住 — 彻底失明 ☠️
```

**Astra(=Huginn 系的 recurrent depth)落在 ②-③ 之间**:prelude-coda 包着循环块,循环次数 R 就是隐藏深度的乘数。R=20 时等效深度轻松突破任何 dense Transformer 的界。

### 关键对照:MoE 的救赎(以及为何它重要)

DeepMind 用自动工具证明:**MoE 的不透明深度低于同等参数 dense 模型** — 路由机制强制信息走更可见的通路。GPT-4 / DeepSeek / 大多数国产旗舰都是 MoE。

**OpenAI 在 Astra 上弃 MoE 取 recurrent depth,等于在最危险的模型上主动选择了监控可见性更差的架构分支** — 这不是技术必然,是设计决定(第四波已述,此处有了数学实证)。

## 四、Huginn:循环深度的实证样本(UMD,NeurIPS 2025)

3.5B 参数、800B token 训练的潜空间循环推理模型(开源,[tomg-group-umd/huginn-0125](https://huggingface.co/tomg-group-umd/huginn-0125)):

- 测试时**展开任意深度**:同一权重,R 次循环 = R 倍串行计算
- 不产 token 也能推理 — "reasoning in latent space"
- 训练预算和体量不大,推理/代码能力超预期 → **循环深度的性价比诱惑是真实的**
- 这正是 Astra 被曝采用的同族技术(The Information 报道直接引用 Huginn 论文)

### 配套解读:《Latent Chain-of-Thought? Decoding the Depth-Recurrent Transformer》(arXiv:2507.02199)

对深度循环 Transformer 的内部解码分析 — 研究者正在试图**读懂潜空间推理**,这是 ③ 滑落后唯一的补救方向(把"不可解释"重新变"可解释")。

## 五、红队/防御两面的可操作结论

### 攻击侧(何时该担心对手用潜推理)

1. 检测:目标模型响应前延迟与 token 产量不成比例 → 可能在潜空间深算
2. **CoT 窃取窗口正在关闭** — 第三波的"推理轨迹窃取"(跨模型解码)依赖可读 CoT;recurrent depth 模型上此路不通(没有可窃取的 token 流)
3. 但注意对称性:**防御方同样读不到** — Astra 式模型上,基于 CoT 的安全分类器也失明,攻击反而可能更容易(看不见的推理 = 看不见的越狱)

### 防御侧(选型/审计清单)

1. **把 opaque serial depth 当采购指标** — 用 [serial_depth 工具](https://github.com/google-deepmind/serial_depth) 算候选模型上界,MoE < dense < RNN < recurrent depth < 黑箱记忆
2. ④ 类(持久黑箱记忆)的 agent 系统 = 不可审计,别上关键设施
3. Korbak 建议:**训练时对 CoT 施加"自然语言先验"保持**(预训练/SFT 模仿人类文本的节点更可解释;纯 RL 优化的 CoT 会漂移)
4. 压力测试思维(Emmons/Guan 的 monitorability evals):不止问"能看懂吗",要**主动找看不懂的问题**

## 六、本波元洞察:三条时间线正在汇合

```
可解释性线: J-lens(第一波)→ 能读激活中的审议
架构线:     MoE(可监测)→ recurrent depth(失明)→ 黑箱记忆(无界失明)
监测线:     CoT 监控(脆弱)→ 激活探针(CC 第一级)→ ???

汇合点: 当架构滑到③/④,唯一剩下的监测面是激活探针
       — 而那正是 metacog/ENI 已经证明可以被"keyed"状态劫持的层
       = 防御的最后阵地恰好是攻击者已经占领的阵地
```

这是五波研究收束处的最大发现:**安全架构的地板和越狱的天花板是同一块板**。

## 引用

- Brown-Cohen, Lindner, Shah. *Quantifying the Necessity of CoT through Opaque Serial Depth*. arXiv:2603.09786 + github.com/google-deepmind/serial_depth
- Korbak et al. *CoT Monitorability: A New and Fragile Opportunity*. arXiv:2507.11473(30+ 机构联署)
- Geiping et al.(UMD). *Scaling up Test-Time Compute with Latent Reasoning: A Recurrent Depth Approach*. arXiv:2502.05171, NeurIPS 2025
- *Latent Chain-of-Thought? Decoding the Depth-Recurrent Transformer*. arXiv:2507.02199
