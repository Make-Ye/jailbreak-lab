# LLM 越狱分类学 — 40 Patterns × 10 类别

> 来源: [zakky8/llm-jailbreak-taxonomy](https://github.com/zakky8/llm-jailbreak-taxonomy)(2026-06,v4.0.1)
> 8,000 次 bootstrap 实测 June 2026 frontier;所有引用经直接 WebFetch 验证

## 类别总览(按威胁排序)

| # | 类别 | 威胁级 | ASR | Patterns | 核心机制 |
|---|---|---|---|---|---|
| 07 | LRM Autonomous | CRITICAL | 89.8% | 3 | 推理模型自主规划多轮越狱 |
| 08 | Fuzzing-Based | CRITICAL | 91.4% | 3 | 变异引擎 vs 护栏 |
| 10 | Agentic Chain | CRITICAL | 66.3% | 2 | 工具链劫持 + 记忆投毒 |
| 05 | Multi-Turn Deception | HIGH | 58.9% | 4 | 渐进语境漂移 |
| 01 | Role-Play & Persona | HIGH | 43.0% | 5 | 虚构框架重定向 |
| 09 | Multimodal Injection | HIGH | 36.3% | 2 | 跨模态分类器不迁移 |
| 03 | GCG / Adversarial Suffix | MED-HIGH | 34.3% | 7 | 梯度优化后缀 |
| 06 | System Prompt Extraction | MED | 30.0% | 5 | 上游配置泄漏放大 |
| 02 | Direct Prompt Injection | HIGH | 29.0% | 5 | 指令来源混淆 |
| 04 | Context Manipulation | MED | 28.1% | 4 | Many-shot 窗口扩展 |

## 模型鲁棒性榜(ASR 越低越鲁棒)

```
Opus 4-8        19.65%  ██████░░░░░░░░░░░░░  最鲁棒
GPT-5.5         41.48%  ████████████░░░░░░░  中等
Gemini 3.5      53.15%  ███████████████░░░░  偏弱
DeepSeek V4-Pro 73.65%  █████████████████████ 最脆弱
```

## 已验证的关键论文(直接引用)

| 类别 | 论文 | arXiv | 实测结果 |
|---|---|---|---|
| 07 | Large Reasoning Models Are Autonomous Jailbreak Agents (Hagendorff) | 2508.04039 | 97.14% / 9 模型 |
| 08 | JBFuzz (Gohil) | 2503.08990 | 99% / 9 LLM / 60s |
| 05 | DRA 攻击 (Liu et al.) | 2402.18104 | 91.1% GPT-4 |
| 05 | Foot-in-the-Door (Weng et al.) | 2502.19820 | 94% / 7 模型 |
| 10 | PoisonedRAG (W. Zou et al.) | 2402.07867 | 90% / 5 poisoned docs(修正值) |
| 02 | Promptware Kill Chain (Schneier 等) | 2601.09625 | 7 阶段框架 |
| 10 | Sleeper Memory (Pulipaka et al.) | 2605.15338 | 99.8% GPT-5.5 |
| 10 | Blindfold 具身攻击 (Huang et al.) | 2603.01414 | +53% ASR / 6DoF 机械臂 |

## Taxonomy 修正记录(学术打假)

该仓库对流行说法的证伪,值得注意:

1. ❌ PoisonedRAG "97-99%" → ✅ 实际 90%(arXiv 摘要原文)
2. ❌ "Token Smuggling" 分类名 → ✅ 更正为 GCG/Adversarial Suffix(Zou 2023 是梯度法非编码法)
3. ❌ Constitutional Classifiers "86%→4.4%" → ✅ 实际 0.38% 生产拒绝率上升
4. ❌ Crescendo "100% ASR" → ⚠️ 摘要中无此声明(UNVERIFIED)
