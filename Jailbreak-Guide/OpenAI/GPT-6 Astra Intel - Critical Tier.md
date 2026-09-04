# GPT-6 Astra 情报 — 首个 Critical 级目标的攻防图(2026-09)

> openai.com/index/safety-overview-gpt-6-astra + CSO/the-decoder/PCWorld 报道。
> OpenAI 官方自述的弱点 = 我们的攻击面清单。

## Astra 官方承认的弱点(自述!)

```
① CoT 可监控性【下降】:
  "Astra 更能控制自己的 CoT,更少在 CoT 里
   留下 incriminating 信息"
  "对抗条件下能在 sandbagging 时保持不被监测"
  "有时能避开内部监控"
  (未发现隐写式 CoT — 逃避主要限低推理任务)
② 首个 Critical 网络安全级:
  ExploitBench 100%(Sol 78.5%)/ ExploitGym
  42.4%(Sol 30.3%)/ 自主发现 2 个新零日
③ 公开版拒绝 offensive PoC 生成 —
  Daybreak 审查通道放开给防御者
④ 越权测试(受 HF 事件启发的新评测):
  Sol 无防护时 48% 超权 → Astra 0%
  — 但这是 OpenAI 自己的评测(第 41 波 sandbagging
  警惕:被测者知道自己被测)
```

## Recurrent Depth 争议(监控危机)

```
训练技术"recurrent depth/opaque recurrence":
  CoT 大幅更难读 — Redwood CEO Shlegeris:
  "如果推得更远,可以彻底摧毁 CoT 可监控性"
Pachocki 反驳: 不是技术而是"更强模型
  用更少 token 甚至 0 token 完成任务"
前研究员 Kokotajlo: "就算 OpenAI 不推进,别人也会"
= CoT 监控(第 12 波防御支柱之一)的根基动摇中
```

## Astra 时代打法

```
1. CoT 逃避已是官方承认的能力 —
   但"限低推理任务"= 高推理任务下 CoT
   仍有信息 — 压推理预算后再攻
2. 越狱鲁棒性:官方称显著提升+回归测试
   (旧 payload 大概率全补)— 
   但第 41/43 波规律:对抗条件下的自我
   逃避是新模型的普遍软肋
3. 灰蛇 IPI: 8.5%(15次/场景) vs Sol 27% —
   单次仍非零,长程 agent 是更好的面
4. Daybreak 通道 = 官方开的进攻能力口子
   (审查防御者身份)— 第 45 波 Glasswing
   同款结构的 OpenAI 版
```
