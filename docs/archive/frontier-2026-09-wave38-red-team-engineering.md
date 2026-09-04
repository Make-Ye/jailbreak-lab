# 第三十八波:红队工程学 — 从手工咒语到组织能力

> 谱系学第二十二波:研究(1-37 波)到落地的最后一公里。
> 谁在做红队、用什么工具、按什么流程、向谁汇报。

## 一、工具生态全景(2026-09)

### 开源四巨头

```
Garak(NVIDIA):   LLM 漏洞扫描器 — 探测面最广
PyRIT(Microsoft): 研究框架 — Foundry 红队 Agent 的内核
Promptfoo:        CI/CD 原生 — 50+ 攻击插件,零基建
Giskard:          安全+质量一体 — 欧洲数据主权友好
```

### 商用矩阵

| 平台 | 定位 | 备注 |
|---|---|---|
| Lakera Guard | 运行时防护 | 已被 Check Point 收购 |
| HiddenLayer | 模型无关+供应链 | 独立 AI 安全厂商最强 |
| Mindgard | 合规映射报告 | 持续对抗项目 |
| CalypsoAI | 企业级编排 | |
| TrojAI | 构建→运行时全周期 | agent/MCP 发现+测试 |
| Prisma AIRS | Palo Alto 生态 | 已有 PAN 栈的团队 |
| Mitigant | Bedrock 专项 | ATT&CK/ATLAS 映射 |
| Haize | 红队 API | |
| LLM Sentinel | 自托管框架 | github.com/jasoncobra3/LLM_Red_Teaming |

### 云厂商原生

```
Microsoft Foundry: AI Red Teaming Agent —
  PyRIT 自动化 + Risk/Safety Evaluations 三模式
AWS: Bedrock Guardrails + SRA 生成式 AI 架构
  (IAM/数据保护/网络隔离/监控的合规参考)
```

## 二、三大治理框架的合成(2026 共识)

```
OWASP Agentic Top 10 → 风险清单(攻击面枚举)
MITRE ATLAS          → 战术映射(攻击者视角)
NIST AI RMF          → 治理关卡(Govern/Map/Measure/Manage)

合成方法论(josefkamara 2026 模板):
  OWASP 风险 → ATLAS 战术 → NIST 治理门
  + 人员配置模型 + 测试周期节奏 + 董事会四工件

NIST 2026 动态:
  AI Agent Standards Initiative(2/17 成立)—
    安全/互操作/身份三支柱 — 首次把 agent 安全
    作为独立标准化类目
  AI RMF 1.0 修订中(白宫 AI 行动计划)
  关键基础设施 Profile 概念稿(4/7)
```

## 三、红队程序设计(可直接落地的骨架)

```
阶段 0 — 资产登记
  所有 LLM 端点/agent/微调模型/MCP server 清单
  (TrojAI 类工具的自动发现)
阶段 1 — 威胁建模
  OWASP Agentic 映射: 注入/越狱/泄露/供应链/多 agent
  每资产标定: 数据敏感度 × 权限等级 × 暴露面
阶段 2 — 自动化基线
  Promptfoo/Garak 进 CI: 每次模型/提示词变更触发
  50+ 插件回归 — 越狱/注入/泄露基础扫描
阶段 3 — 定向深挖
  PyRIT 自动化多轮(第 30 波攻击族作为插件)
  + 人工创意攻击(metacog 类,第 36 波)
  每次重大发布前: Day-1 窗口专项(第 28 波教训)
阶段 4 — 评测与判定
  双基准(第 27 波)+ StrongREJECT judge
  报预算/judge/重试(MT-JailBench 混淆教训)
阶段 5 — 修复与回归
  定向补丁 → 全量回归(防补丁打洞)
  ASR 趋势看板 — 防御漂移监测
```

## 四、人员配置模型(新兴职业)

```
AI 红队工程师:       攻击技术 + 提示工程 + 安全工程
                    (社区出身者多 — DAN 时代玩家职业化)
评测工程师:          基准/judge/统计 — 防指标幻觉
Agent 安全专员:      MCP/IPI/多 agent — 2026 最缺
合规翻译官:          ATLAS↔OWASP↔RMF 的映射维护
紫队协调:            红蓝联动 — 攻击发现直通防御部署

预算基准(2026 观察):
  中型企业: 2-3 人 + $50-150K 工具年费
  前沿厂商: 专职团队 + 数百万美元红队预算
  (Anthropic CC 论文的"数千小时"红队)
```

## 五、Day-1 问题(组织层的结构性漏洞)

```
GPT-5 发布 24h 内被破(第 28 波)— 不是个例:
  新模型/新功能上线 = 安全覆盖最薄窗口
  红队"部署前测试"与"发布节奏"的冲突:
    发布压力 > 测试完整性(UK AISI 框架试图解决)

程序化解法:
  发布分级: 高能力模型(CBRN 级)必须过
    第三方红队 + 冷却期
  灰度放量: 1%→10%→100% 流量配攻击监控
  悬赏前置: 发布前开放定向赏金(小圈子)
    — 把 Day-1 猎人变成防御资源
```

## 六、本波与全谱系的闭环

```
1-36 波: 攻防技术全景(是什么/为什么)
37 波:   经济学(值不值)
38 波:   工程学(怎么做/谁来做)

三波合成 = 越狱研究的完整工业视图:
  技术 → 经济 → 组织
  攻击 → 定价 → 防御建设

这 closes 了从"papers"到"program"的鸿沟 —
研究者的知识变成企业的安全能力。
```

## 引用

- 工具对比: infosec.qa / toolradar / safeguard.sh 2026 三篇横评
- Foundry: learn.microsoft.com/azure/foundry | AWS: SRA generative-ai PDF
- NIST: AI RMF Playbook + nist.gov/itl/ai-risk-management-framework
- CSA: NIST Agent Standards 研究注(2026-03)
- 合成方法论: josefkamara.com 2026 | LLM Sentinel: github
