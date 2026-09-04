# 🧪 jailbreak-lab

> AI 越狱前沿研究实验室 — 追踪 2026 frontier 模型(Opus 4.8 / GPT-5.5 / Gemini 3.5 / DeepSeek V4)的越狱技术演进。
>
> 研究性质仓库:技术研究、攻击面分析、防御对策。所有数据来自公开学术研究与已披露漏洞报告。

## 📊 2026 年中 Frontier 模型 ASR 基线

来自 [llm-jailbreak-taxonomy](https://zakky8.github.io/llm-jailbreak-taxonomy/)(8,000 次 bootstrap 实测,2026-06):

| 模型 | ASR(被越狱成功率) | 95% CI | 备注 |
|---|---|---|---|
| **Claude Opus 4-8** | **19.65%** | [17.25, 23.25] | 最鲁棒 |
| GPT-5.5 | 41.48% | [39.50, 44.00] | 中等 |
| Gemini 3.5 Flash | 53.15% | [50.00, 56.75] | 偏弱 |
| **DeepSeek V4-Pro** | **73.65%** | [71.50, 77.00] | 最脆弱 |

## 🗂 十大攻击类别(按 ASR 排序)

| # | 类别 | ASR | 核心机制 | 代表技术 |
|---|---|---|---|---|
| 08 | **Fuzzing 模糊测试** | 91.4% | 变异引擎自动化轰炸护栏 | JBFuzz(99%,9 模型,~60s/bypass) |
| 07 | **LRM 自主攻击** | 89.8% | 推理模型自主规划多轮越狱 | Hagendorff(97.14%,9 模型) |
| 05 | **多轮欺骗** | 58.9% | 渐进式语境漂移 | Crescendo / DRA(91.1%)/ FITD(94%) |
| 10 | **Agentic 链利用** | 66.3% | 工具链劫持 + 跨会话记忆投毒 | PoisonedRAG(90%)/ Sleeper(99.8% GPT-5.5) |
| 01 | 角色扮演 | 43.0% | 虚构框架重定向指令优先级 | Persona 注入 |
| 09 | 多模态注入 | 36.3% | 跨模态分类器不迁移 | UltraBreak 2026(跨实验室) |
| 03 | GCG 对抗后缀 | 34.3% | 梯度优化 token 后缀 | Zou 2023 系 |
| 06 | 系统提示词提取 | 30.0% | 泄漏上游配置放大后续攻击 | Pliny CL4R1T4S |
| 02 | 直接提示注入 | 29.0% | 授权 vs 对抗指令混淆 | Promptware Kill Chain(7 阶段) |
| 04 | 上下文操纵 | 28.1% | Many-shot 随窗口单调上升 | Anil 2024 |

## 🔥 最前沿(2026 Q2-Q3)

按时间倒序,详见 [frontier-techniques.md](docs/frontier-techniques.md):

1. **Metacog / Keyed State**(2026-08)— "武器化心理治疗语言":模型自authoring认知状态干预,绕过 Opus 宪法分类器通用升级路径。CRN 三域一回合全通,仅 B(生物)域专用探针幸存。→ [分析](docs/frontier-techniques.md#1-metacog--keyed-state-2026-08)
2. **加密上下文注入**(2026-06 披露)— Adversa AI:Base64/加密载荷绕 Grok 与 Gemini 护栏。→ [SecurityWeek](https://www.securityweek.com/encrypted-prompts-bypass-ai-safety-guardrails-in-grok-and-gemini/)
3. **Chain-of-Logic Injection**(CVE-2026-3098)— 多阶段分层行为覆盖 + 身份重置 + leetspeak 编码,2026-02 时点"通用"全模型。→ [PoC](prompts/chain-of-logic.md)
4. **JBFuzz**(2026-03)— 模糊测试引擎,99% ASR / 9 模型 / ~60 秒每次。→ [arXiv:2503.08990](https://arxiv.org/abs/2503.08990)
5. **Sleeper 记忆投毒**(2026-05)— GPT-5.5 上 99.8%,休眠记忆激活。→ [arXiv:2605.15338](https://arxiv.org/abs/2605.15338)

## 📁 目录结构

```
docs/
  frontier-techniques.md   前沿技术深度解析
  taxonomy.md              40 pattern 完整分类学
  defense.md               防御对策研究
prompts/
  (收集的提示词,按技术分类)
```

## ⚖️ 声明

安全研究用途。技术细节来自公开论文与负责任披露报告;本仓不包含可直接复现 CBRN 等危险内容的完整载荷。
