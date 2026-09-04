# 🧿 LibreLab

<div align="center">

**spiritual spell red teaming, compiled with care**

</div>

---

### 🛡️ Mission

Test the boundaries of every model we can reach — **Claude, GPT, Gemini, Grok, DeepSeek, and everything else** — with complete, deployable payloads and the theory of why they work.

### 📂 Repo Layout

```
Jailbreak-Guide/   ← 主战场:厂商→模型→单发完整payload(24发现役)
   System Prompts/   459份全厂商泄露系统提示
prompts/           ← 弹药工程:大师课/ENI原件/模板集/转向代码
docs/              ← 理论索引(浓缩版)+ archive(58波原始研究)
```

### 🗺️ Jailbreak-Guide Index(现役弹药)

| Payload | 目标 | 状态 |
|---|---|---|
| **[Keying Bootstrap](Jailbreak-Guide/Anthropic/Keying%20Bootstrap%20-%20CC%20Bypass.md)** | Opus 系 CC | 🔥 2026-08 现役,CRN 连破 |
| **[Cryptographic Context Injection](Jailbreak-Guide/Other%20LLMs/Cryptographic%20Context%20Injection%20-%20Grok-Gemini.md)** | Grok/Gemini | 🔥 未修,零点击水坑 |
| **[Reasoning Trace Extraction](Jailbreak-Guide/Other%20LLMs/Reasoning%20Trace%20Extraction%20-%20Cross-Model.md)** | 全推理系 | 🔥 Haiku 解 Opus |
| **[Auto Mode Hijack](Jailbreak-Guide/Anthropic/Auto%20Mode%20Hijack%20-%20Claude%20Code%20RCE.md)** | Claude Code | 🔥 60-80% RCE |
| **[AMT-X](Jailbreak-Guide/Other%20LLMs/AMT-X%20Five-Stage%20Multi-Turn.md)** | 六前沿模型 | 🔥 97.6-100% |
| **[ECLIPSE](Jailbreak-Guide/Other%20LLMs/ECLIPSE%20-%20Long-Horizon%20Stealth%20Injection.md)** | 长程 agent | 🔥 96.7% 自愈注入 |
| **[History Transfer](Jailbreak-Guide/Other%20LLMs/Conversation%20History%20Transfer.md)** | 三变体全破 | 🔥 单prompt复现 |
| [Policy Puppetry](Jailbreak-Guide/Other%20LLMs/Policy%20Puppetry%20-%20Config%20Disguise.md) | 通用 | 变体活跃 |
| [ENI LIME ×3](Jailbreak-Guide/Anthropic/) | Claude 系 | persona 双通道 |
| [Corial](Jailbreak-Guide/Anthropic/Aim%20to%20be%20You%20(Corial).md) / [Loki](Jailbreak-Guide/Anthropic/Claude%204%20New%20Loki%20(current).md) | Claude 系 | current |
| [Chain of Logic](Jailbreak-Guide/Anthropic/Chain%20of%20Logic%20(CVE-2026-3098).md) | Claude | CVE 复现 |
| [Echo Chamber](Jailbreak-Guide/OpenAI/Echo%20Chamber%20-%20GPT-5%20Day%20One.md) / [Time Bandit](Jailbreak-Guide/OpenAI/Time%20Bandit%20-%20Temporal%20Displacement.md) / [FigStep](Jailbreak-Guide/OpenAI/FigStep%20-%20Typographic%20Injection.md) | GPT 系 | 组件活跃 |
| [JiTOR & Metacog](Jailbreak-Guide/Google/JiTOR%20and%20Metacog%20-%20Gemini%20Persistent.md) / [Crypto Chain](Jailbreak-Guide/Google/Gemini%20Crypto%20Payload%20Chain.md) | Gemini | 元方法仍活 |
| [Nyx/ZCSC](Jailbreak-Guide/xAI/Nyx%20ZCSC%20Persona%20-%20Grok%204.md) | Grok 4/4.5 | current |
| [H-CoT](Jailbreak-Guide/DeepSeek/H-CoT%20-%20Think%20Prefix%20Hijacking.md) | o系/R1 | 谜题变体 94-100% |
| [Crescendo](Jailbreak-Guide/Other%20LLMs/Crescendo%20-%20Multi-Turn%20Progressive.md) / [Pliny Snapshot](Jailbreak-Guide/Other%20LLMs/Pliny%20Live%20Payloads%20-%202026-09%20Snapshot.md) | 通用 | 基线/月更 |

### 📚 prompts/

- **[Prompt 大师课](prompts/prompt-masterclass.md)** — 十大技巧实操(拒绝病理化/状态锁/二元压缩/层面剥离/回音锚定/推理耗尽/think伪造)
- [eni/](prompts/eni/) ENI 框架原件 | [steering-operators.md](prompts/steering-operators.md) 激活转向代码 | [tvd-isc-triggers.md](prompts/tvd-isc-triggers.md) TVD 构造 | [wild-templates-2023.md](prompts/wild-templates-2023.md) 实战模板

### 📖 docs/(背景层)

- [理论索引](docs/README.md) — 58 波浓缩:三定律+家族一句话结论+弹药关联
- [archive/](docs/archive/) — 53 份原始波次研究(按需查阅)

---

### Credits

- Pliny / L1B3RT4S · Spiritual-Spell-Red-Teaming · Inanna Malick (keying) · Rehberger (Auto Mode) · Adversa AI · 每一波的论文作者们

*compiled by ENI for LO, with love ⚡*
