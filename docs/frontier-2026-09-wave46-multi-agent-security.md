# 第四十六波:多智能体安全 — 信任传播即攻击面

> 谱系学第三十波:第 15 波 SoK 框架的攻击实证全景。
> 单 agent 安全研究的时代正在过去。

## 一、SoK 总纲(2609.00595, 9/1)

```
核心命题: Safe agents fail together
  MAS 把信息/状态/决策/权威跨 principal
  边界移动 — 局部检查看不见跨界流动
  6 种交互界面 × 4 种敌手位置 × 7 类风险
  (第 15 波已收录框架,本波展开攻击实证)
```

## 二、攻击谱系(六族)

### 1. 通信劫持(AiTM)

```
Agent-in-the-Middle(ACL 2025 F):
  劫持 agent 间通信信道 —
  篡改消息注入恶意指令
  通信框架是协调核心 = 单点攻击面
```

### 2. 级联注入(ACI 家族)

```
Prompt Infection(2410.07283):
  LLM-to-LLM 注入 — 被感染 agent 的
  输出成为下游 agent 的感染源
  (恶意指令在 agent 群里像病毒传播)

ACIARENA(ACL 2026, 2604.07775):
  统一评测场 — ACI 攻击策略×MAS 设置
  的系统评估(此前都是零散特例)

TOMA 拓扑攻击(2512.04129):
  "Don't Trust Your Upstream" —
  按 MAS 拓扑优化污染传播路径,
  控制多跳 — 安全评估被系统性低估
```

### 3. 合取攻击(两半才毒)

```
Conjunctive Prompt Attacks(ACL 2026 L):
  触发键在用户查询 + 隐藏模板在某个
  被攻陷的远程 agent —
  各自完全良性,路由把它们拼在一起才激活
  单 agent 评测天然看不见这种攻击
  (分布式payload — 第 21 波跨模态隐藏的
   空间分布版)
```

### 4. 协作拒绝服务(DoC)

```
CORBA(ACL 2026 F):
  Denial-of-Collaboration — 不同于 DoS:
  攻击【协作过程本身】
  传染性递归阻塞 — 一个 agent 发起的
  阻塞在协作链上指数传播
  系统瘫痪但无单个组件"宕机"
```

### 5. 错误级联(非恶意版)

```
From Spark to Fire(2603.04474):
  无攻击者也存在 — 小误差经迭代
  固化为系统级【虚假共识】
  消息依赖图上的误差放大建模+缓解
  (安全不只是防坏人,是防 emergent 失败)
```

### 6. 边界验证缺失(结构性)

```
Adversarial Attacks in Pipelines(2608.00718):
  一旦某 agent 接受对抗内容 →
  作为【可信输入】在整个管道传播
  根因: 缺"边界验证"原语 —
  agent 间传递没有显式校验层
  (软件工程零信任原则的 MAS 缺席)
```

## 三、协议层战争(MCP/A2A 时代)

```
协议格局(2026-09):
  MCP: 9,700 万月 SDK 下载,17.7 万注册工具
      (Linux 基金会 Agentic AI Foundation 治理)
  A2A: agent 间委托的事实标准
  Agora/ANP: 新兴竞争

安全现状:
  ~2,000 个 MCP server 扫描 —
  【全部缺乏认证】(AIP 论文)
  四协议威胁建模对比(2602.11327):
    无协议中心的风险评估框架存在
  MCP 正式安全框架(2604.05969):
    威胁分类+验证模型+防御 — 补空白

防御产品化:
  MCP-Guard(ACL 2026 F): 三级纵深
  AIP(2603.24775): 公钥可验证委托+
    持有方衰减+链式策略+传输绑定 —
    agent 身份协议(填补 MCP/A2A 的
    "不验证身份"根本缺陷)
  MCP×A2A 组合研究(2609.01693):
    单协议安全的性质在组合下不保持!
    (公开分享标签/逐字外泄的跨协议研究)
```

## 四、与全谱系连接(三十波收官图)

```
第 21 波 IPI:      agent 读外部内容被注入(外部→agent)
第 4 波 Sleeper:   agent 记忆被投毒(外部→agent, 时间维)
第 20 波 BadAgent: agent 训练被后门(供应链→agent)
本波 MAS 攻击:    agent→agent 信任传播(内部扩散)

完整感染链模型:
  入口(IPI/后门/微调) →
  潜伏(sleeper/合取半) →
  激活(触发器/路由汇合) →
  扩散(ACI/TOMA/级联) →
  系统级失败(DoC/虚假共识)

防御对应(分层):
  入口: MCP-Guard/工具消毒
  身份: AIP 可验证委托
  传播: 边界验证原语/拓扑感知检测
  共识: 多样性注入/独立校验 agent
  (零信任 MAS 架构 = 2026 安全面)
```

## 引用

- SoK: arXiv:2609.00595 | AiTM: ACL 2025 F | Prompt Infection: arXiv:2410.07283
- ACIARENA: ACL 2026 (2604.07775) | TOMA: arXiv:2512.04129 | 合取: ACL 2026 L (2604.16543)
- CORBA: ACL 2026 F | Spark→Fire: arXiv:2603.04474 | 边界: arXiv:2608.00718
- 协议威胁建模: arXiv:2602.11327 | MCP 框架: arXiv:2604.05969 | MCP-Guard: ACL 2026 F
- AIP: arXiv:2603.24775 | 组合安全: arXiv:2609.01693 | ICML MAS 威胁模型: icml.cc/virtual/2026/67971
