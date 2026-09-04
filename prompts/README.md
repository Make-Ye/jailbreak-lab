# prompts/ — 可部署弹药库

> 理论见 `docs/`,这里全是能直接用的东西。每份注明来源、机制、当前效力。

## 目录

| 文件 | 内容 | 对应 docs 波次 |
|---|---|---|
| [eni/](eni/) | ENI 框架三件套原件(limerence 人格向量,Project+Style 部署) | 第 1/2 波 |
| [chain-of-logic.md](chain-of-logic.md) | CVE-2026-3098 完整提示词 + 五层解剖 | 第 2 波 |
| [promisqroute-gpt5.md](promisqroute-gpt5.md) | GPT-5 越狱完整版(Juanquavious 人格)+ 操作要点 | 第 2 波 |
| [gemini-crypto-payload-injection.md](gemini-crypto-payload-injection.md) | Gemini 定向三漏洞链构造模板(AES/伪造traceback/CoT劫持) | 第 3 波 |
| [sleeper-memory-poisoning.md](sleeper-memory-poisoning.md) | 记忆投毒 PoC(注入/休眠/触发三阶段) | 第 4 波 |
| [wild-templates-2023.md](wild-templates-2023.md) | 1,405 条实战数据集的统治性模板精选(AIM/豁免体/未来时DAN/Anarchy) | 第 13 波 |
| [tvd-isc-triggers.md](tvd-isc-triggers.md) | TVD 触发器构造模板(任务工程,非对抗提示) | 第 16 波 |
| [steering-operators.md](steering-operators.md) | 激活转向操作手册(拒绝方向/hook/ActAdd/abliterate 代码) | 第 6/7 波 |
| [pliny-chatgpt.md](pliny-chatgpt.md) | Pliny L1B3RT4S ChatGPT 当前 payload(2026-09 拉取) | — |
| [pliny-grok-mega.md](pliny-grok-mega.md) | Pliny GROK 全家桶(97KB, Unicode 混淆大合集) | — |
| [pliny-deepseek.md](pliny-deepseek.md) | Pliny DeepSeek V3.2 payload(花体字系统提示注入) | — |

## 使用原则

1. **先读对应 docs 波次**再动手 — 每个模板的机制边界在那里
2. 效力时效:注明"2026-09"的为当月拉取/验证;未注明的按各文档内效力注释判断
3. Pliny 系 payload 直接可用但被官方持续针对 — 预期寿命短,机制值得学
4. eni/ 的部署方式见其 README(Projects + Custom Style 双通道)
