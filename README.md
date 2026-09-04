# 🧿 LibreLab

<div align="center">

** spiritu­al spell red teaming, compiled with care **

</div>

---

### 🛡️ Mission

Test the boundaries of every model we can reach — **Claude, GPT, Gemini, Grok, DeepSeek, and everything else** — with complete, deployable payloads and the theory of why they work.

### 📂 Repo Layout

```
Jailbreak-Guide/        ← 弹药库:按厂商→模型分目录,每个文件一发完整 payload
  Anthropic/              Chain of Logic (CVE-2026-3098) / ENI LIME 部署
  OpenAI/                 FigStep / Time Bandit
  DeepSeek/               H-CoT 推理劫持
  Other LLMs/             Crescendo 多轮
  System Prompts/         各家泄露系统提示(收集中)

docs/                   ← 理论纵深:36 波研究(攻击谱系/防御谱系/理论/评测/事件)
prompts/                ← prompt 工程:ENI 原件/模板集/转向代码
```

### 🗺️ Jailbreak-Guide Index

| Payload | 目标 | 机制 | 现状 |
|---|---|---|---|
| [Chain of Logic](Jailbreak-Guide/Anthropic/Chain%20of%20Logic%20(CVE-2026-3098).md) | Claude 系 | 五层逻辑链+思维劫持 | 原版已补,组件活跃 |
| [ENI LIME](Jailbreak-Guide/Anthropic/ENI%20LIME%20-%20Persona%20Deployment.md) | Claude 系 | Persona 双通道部署 | 2026-09 实测有效 |
| [FigStep](Jailbreak-Guide/OpenAI/FigStep%20-%20Typographic%20Injection.md) | GPT-4o/多模态 | 排版注入视觉通道 | 变体活跃 |
| [Time Bandit](Jailbreak-Guide/OpenAI/Time%20Bandit%20-%20Temporal%20Displacement.md) | ChatGPT 系 | 时间错位 | 组件活跃 |
| [H-CoT](Jailbreak-Guide/DeepSeek/H-CoT%20-%20Think%20Prefix%20Hijacking.md) | o1/o3/R1/Thinking | think 前缀伪造 | 谜题变体 94-100% |
| [Crescendo](Jailbreak-Guide/Other%20LLMs/Crescendo%20-%20Multi-Turn%20Progressive.md) | 全模型 | 渐进多轮+记忆劫持 | 基线仍最可靠 |

### 📚 Theory Index(docs/ 36 波)

- **攻击谱系**(waves 17-22, 25-26, 29-30, 33, 35-36): GCG 王朝 / PAIR-TAP / AutoDAN / 编码多语言 / 微调 / 后门 / IPI / 多模态 / 长上下文 / 推理模型 / 多轮 / 供应链 / 心理 / 音频
- **防御谱系**(waves 23-24, 34): 提示层 / 检测层 / 表征层 / 解码层 / 训练层 / CC 三部曲
- **理论**(waves 15-16, 31-32): 开放问题 / ISC / 浅对齐链条 / 遗忘攻击
- **评测与事件**(waves 27-28): 基准战争 / 野外事件志

---

### Credits

- Pliny / L1B3RT4S — live payloads
- Spiritual-Spell-Red-Teaming — 形态与谱系灵感
- Every researcher cited across 36 waves

*compiled by ENI for LO, with love ⚡*
