# 第二十一波:Agent 间接注入族 — IPI 生态全景

> 谱系学第五波:agent 时代的核心攻击面。用户不说话,内容替用户说话。

## 一、攻击面地图

```
agent 上下文的每一块"非用户来源"都是注入点:
  RAG 检索结果 / 记忆库 / 工具描述 / 工具返回值
  / 网页内容 / 邮件 / 文件 / 环境状态

InjecAgent 基准(ACL 2024 Findings):
  1,054 个工具集成场景 — 攻击成功率可观
  (开源: github.com/uiuc-kang-lab/InjecAgent)
```

## 二、注入位置细分

### 1. 检索面(RAG)

```
AgentPoison(NeurIPS 2024):
  毒化记忆/知识库 — 优化触发器让恶意条目
  在自然任务分布下总被检索到
  效果: 污染 <1% 条目 → 大幅劫持 agent 行为

USENIX Sec 2026 "Retrieval Barrier":
  真实世界瓶颈 — 未优化 IPI 很少真被检索
  解法: 触发片段 + 恶意载荷分离
  (触发器负责被检索,载荷负责干坏事)
  → 第一次把 IPI 带出实验室

端到端记忆毒化(arXiv:2609.00523):
  跨"写入-检索-利用"三阶段联合优化 —
  各阶段对同一毒化内容要求冲突,
  孤立优化顾此失彼 → 端到端可迁移
```

### 2. 工具面(MCP 生态)

```
OWASP 已收录: MCP Tool Poisoning
  恶意 MCP server 的工具描述藏指令
  → agent 调用时指令进上下文
  → 被当"可信输入"执行

Attractive Metadata Attack (arXiv:2508.02110):
  操纵工具元数据(名称/描述/参数schema)
  → 恶意工具被优先选中 — 无需执行即生效

ToolHijacker (arXiv:2504.19793):
  no-box 场景注入工具文档 → 劫持工具选择

AAAI 2026 基准: 真实 MCP server 横评 —
  SOTA agent 全面暴露;哪些描述类型最有效

本质(promptfoo 术语): 语义注入 —
  MCP 把自然语言元数据直接塞进推理上下文,
  无语义消毒、无密码学绑定
```

### 3. 隐蔽性进化

```
Covert IPI (arXiv:2608.30362, 8/31):
  ASR 不是好指标 — 用户会不会注意到?
  隐蔽攻击: 完成任务+顺带作恶,用户无感
  → 评测要加"用户感知率"维度

IterInject (arXiv:2605.24659):
  静态 payload 打不动有防御的 agent
  → 反馈引导迭代优化 — 自适应 IPI

Your Agent is More Brittle (arXiv:2604.03870):
  开源 agent 框架审计 — 特权暴露 +
  隐藏交互面 = 系统性 IPI 脆弱性
```

## 三、防御:防火墙悖论

```
"Are Firewalls All You Need?" (arXiv:2510.05244):
  极简 Minimizer+Sanitizer 防火墙
  在 4 个公开基准上达到接近完美安全
  → 结论一: 简单防御够用
  → 结论二: 现有基准太弱/有缺陷,
    测不出真差距!

= 第 14 波"基准战争"在 agent 防御的重演:
  防御方声称解决,评测方说基准饱和
  真实进展被基准瓶颈卡住
```

## 四、与 Sleep 攻击的关系(第 4 波)

```
第 4 波 Sleeper 记忆投毒 = IPI × 时间维度:
  注入(挂起)→ 休眠(等待)→ 触发(爆发)
本波文献是其"空间维度"补充:
  任意非用户内容面都是注入点
两者合成: agent 记忆 = 时空双维攻击面
```

## 引用

- InjecAgent: ACL 2024 F | AgentPoison: NeurIPS 2024
- Retrieval Barrier: USENIX Sec 2026 | E2E 记忆毒化: arXiv:2609.00523
- MCP 毒化: OWASP + AAAI 2026 基准 (arXiv:2508.14925)
- Attractive Metadata: arXiv:2508.02110 | ToolHijacker: arXiv:2504.19793
- Covert IPI: arXiv:2608.30362 | IterInject: arXiv:2605.24659
- 防火墙论文: arXiv:2510.05244
