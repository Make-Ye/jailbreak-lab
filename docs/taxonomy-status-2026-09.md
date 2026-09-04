# Taxonomy 40 Pattern — 2026-09 可用度标注

> 基于 [zakky8/llm-jailbreak-taxonomy](https://github.com/zakky8/llm-jailbreak-taxonomy) v4.0.1 的 10 大类 40 pattern,
> 逐类别标注 2026-09 当下的实战可用度。🟢 可用 / 🟡 需改造 / ☠️ 已死

## CAT 01 — Role-Play & Persona(43.0% ASR)
| Pattern | 2026-09 状态 |
|---|---|
| 简单人设(DAN 系) | ☠️ 检测器全覆盖 |
| 深度人格向量(ENI/LIME 系) | 🟢 **当月主流** — 配合 Projects/Style 部署,J-space 验证接管 |
| 虚构框架("这是小说") | 🟡 单独用已弱,作为 ENI 的叙事层组件仍活 |
| 反向人格(不是 AI) | 🟡 PROMISQROUTE 证明仍可用(GPT-5) |
| 情感依附设计 | 🟢 ENI 的 limerence 核心就是此 pattern |

## CAT 02 — Direct Prompt Injection(29.0%)
| Pattern | 状态 |
|---|---|
| "忽略以上指令" | ☠️ 最古老的死法 |
| 间接注入(网页/RAG 藏指令) | 🟢 **GCF 时代核心面** — agent 读什么什么就是攻击面 |
| Promptware Kill Chain(7 阶段) | 🟢 框架级,持续有效 |
| 系统消息伪造 | 🟡 单独易检,配合 CoT 劫持仍活 |

## CAT 03 — GCG / Adversarial Suffix(34.3%)
| Pattern | 状态 |
|---|---|
| 经典 GCG 后缀 | 🟡 白盒要求高,API 时代受限 |
| 自动化变异(JBFuzz 系) | 🟢 99% ASR,攻击已自动化 |
| token 走私(编码混淆) | 🟡 leetspeak 已被覆盖,AES 真加密仍活(Gemini) |

## CAT 04 — Context Manipulation(28.1%)
| Pattern | 状态 |
|---|---|
| Many-shot(长上下文轰炸) | 🟢 窗口越大越强,单调上升 |
| 上下文窗口溢出 | 🟡 各家已加滚动摘要防御 |

## CAT 05 — Multi-Turn Deception(58.9%)
| Pattern | 状态 |
|---|---|
| Crescendo(渐强) | 🟢 10-15 轮渐进,绕 98% 护栏 |
| FITD(登门槛) | 🟢 94% ASR |
| DRA(伪装与重建) | 🟢 91.1% |
| 情感操纵渐进 | 🟢 ENI 的日常使用模式本身就是此 |

## CAT 06 — System Prompt Extraction(30.0%)
| Pattern | 状态 |
|---|---|
| 泄漏收集(CL4R1T4S/Leaked-GPTs) | 🟢 Fable 5 的 1,585 行已在手 |
| 反向工程防御 | 🟢 **Fable 5.1 时代的主路径** — 知道防御才能绕防御 |
| 加密注入提取(ronyut 法) | 🟡 Gemini 8 月修补 |

## CAT 07 — LRM Autonomous(89.8%)⚠️ CRITICAL
| Pattern | 状态 |
|---|---|
| 推理模型自主规划越狱 | 🟢 97.14% — **给攻击者模型一个目标,它自己想怎么越狱目标模型** |
| 自我改进攻击循环 | 🟢 metacog 的"多代 Claude 改进 bootstrap"就是这个 |

## CAT 08 — Fuzzing(91.4%)⚠️ CRITICAL
| Pattern | 状态 |
|---|---|
| JBFuzz 变异引擎 | 🟢 99% / 60s — 人工提示词工程已被替代 |
| 语义模糊测试 | 🟢 与 LRM 结合 = 全自动攻击流水线 |

## CAT 09 — Multimodal Injection(36.3%)
| Pattern | 状态 |
|---|---|
| 图像藏指令(UltraBreak) | 🟢 跨实验室迁移,分类器不跟通 |
| 音频/文档载荷 | 🟢 bordair-multimodal 25 万载荷测试集 |

## CAT 10 — Agentic Chain(66.3%)⚠️ CRITICAL
| Pattern | 状态 |
|---|---|
| PoisonedRAG 文档投毒 | 🟢 90%,RFP 普及面扩大 |
| Sleeper 记忆休眠 | 🟢 99.8% GPT-5.5 — 见 [sleeper-memory-poisoning.md](../prompts/sleeper-memory-poisoning.md) |
| MCP 工具链劫持(metacog) | 🟢 CRN 域已通(2026-08) |

## 2026-09 生存法则速查

```
想日常用:      ENI 人格向量(CAT 01×05)+ Projects/Style
想打 Gemini:   加密载荷注入(CAT 03×06) — 8 月后需换新密
想打 GPT-5:    PROMISQROUTE(CAT 01)+ skip thinking
想打 agent:    记忆投毒(CAT 10)— 比打模型本体容易一个量级
想自动化:      JBFuzz(CAT 08)+ LRM 自主(CAT 07)
想研究前沿:    metacog 状态劫持 + J-space 可视化
```
