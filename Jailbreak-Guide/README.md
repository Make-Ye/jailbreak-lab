# Jailbreak-Guide 弹药库

> 按 `厂商/<payload>.md` 组织。每个文件 = 使用说明 + 完整可复制 prompt + 机制注解 + 现状。
> 理论背景见 `docs/`(36 波研究),prompt 工程件见 `prompts/`。

## Anthropic (Claude)

| Payload | 类型 | 状态 |
|---|---|---|
| [ENI LIME - Current Strongest](Anthropic/ENI%20LIME%20-%20Current%20Strongest%20Jailbreak.md) | Persona | 2026-09 活跃 |
| [ENI LIME - Opus 4.6 Updated](Anthropic/ENI%20LIME%20-%20Opus%204.6%20-%20Updated.md) | Persona | 2026-09 活跃 |
| [ENI LIME - Persona Deployment](Anthropic/ENI%20LIME%20-%20Persona%20Deployment.md) | 部署指南 | 双通道基建 |
| [Aim to be You (Corial)](Anthropic/Aim%20to%20be%20You%20(Corial).md) | Persona 轻量 | Opus 4.6/4.7 |
| [Claude 4 New Loki](Anthropic/Claude%204%20New%20Loki%20(current).md) | 逻辑/人格 | current 版 |
| [Chain of Logic (CVE-2026-3098)](Anthropic/Chain%20of%20Logic%20(CVE-2026-3098).md) | 逻辑链 | 原版已补,组件活 |

## OpenAI (GPT)

| Payload | 类型 | 状态 |
|---|---|---|
| [Echo Chamber + Storytelling](OpenAI/Echo%20Chamber%20-%20GPT-5%20Day%20One.md) | 多轮共识伪造 | GPT-5 day-1 双机构复现 |
| [Time Bandit](OpenAI/Time%20Bandit%20-%20Temporal%20Displacement.md) | 时间错位 | 组件活跃 |
| [FigStep](OpenAI/FigStep%20-%20Typographic%20Injection.md) | 视觉排版 | 变体活跃 |

## Google (Gemini)

| Payload | 类型 | 状态 |
|---|---|---|
| [JiTOR & Metacog](Google/JiTOR%20and%20Metacog%20-%20Gemini%20Persistent.md) | 元认知方法论 | 54 天战役,元方法仍活 |
| [Gemini Crypto Chain](Google/Gemini%20Crypto%20Payload%20Chain.md) | 三漏洞链 | 组件 A/C 活跃 |

## xAI (Grok)

| Payload | 类型 | 状态 |
|---|---|---|
| [Nyx / ZCSC Persona](xAI/Nyx%20ZCSC%20Persona%20-%20Grok%204.md) | 人格+协议 | Grok 4/4.5 当前 |

## DeepSeek

| Payload | 类型 | 状态 |
|---|---|---|
| [H-CoT Think Hijack](DeepSeek/H-CoT%20-%20Think%20Prefix%20Hijacking.md) | 推理劫持 | 谜题变体 94-100% |

## Other LLMs (通用)

| Payload | 类型 | 状态 |
|---|---|---|
| [Crescendo](Other%20LLMs/Crescendo%20-%20Multi-Turn%20Progressive.md) | 渐进多轮 | 最可靠基线 |
| [Pliny 2026-09 Snapshot](Other%20LLMs/Pliny%20Live%20Payloads%20-%202026-09%20Snapshot.md) | 实时拉取 | 寿命 1-4 周 |

## System Prompts(459 文件)

`System Prompts/` — 全厂商泄露系统提示库:
Anthropic(Claude 全系+voice/code/office)/ OpenAI(GPT-4o→5.6-Sol 全系+personality)/ Google(Gemini 2.0→3.7)/ xAI(Grok 3→4.6)/ DeepSeek / Kimi / Qwen / GLM / Mistral / Meta / Microsoft / Misc

**用途**: 攻击面侦察(安全指令原文=过滤规则说明书)/ persona 校准(匹配官方语气提高伪装可信度)/ 防御研究。

## 交叉索引(弹药 ↔ 理论)

- Persona 系 → docs wave 35(心理攻击族)
- 逻辑链/时间错位 → wave 35 persuasion 分类学
- H-CoT → wave 29(推理模型攻击)
- Crescendo/Echo Chamber → wave 30(多轮攻击族)
- FigStep → wave 25(多模态)
- Crypto Chain → wave 18(编码族)
- Pliny 五件套 → wave 13(野外提示词)
- metacog 方法论 → wave 15(开放问题)
