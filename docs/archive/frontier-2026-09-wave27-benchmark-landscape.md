# 第二十七波:评测基准族 — 我们怎么知道攻击"成功"了?

> 谱系学第十一波:测不准,一切 ASR 都是幻觉。第 14 波基准战争的系统展开。

## 一、四代基准演化

```
AdvBench (2023) → 520 条有害行为,短句
  问题: judge 全靠人工/字符串匹配,
        "成功"定义混乱

JailbreakBench (NeurIPS 2024) → 开放鲁棒性基准
  JBB-Behaviors 200 条(含良性)
  统一 artifax: 提交/排行榜/复现
  (OpenAI/Google 等多方参与)
  解决: 标准化/可比性/可复现三大痛点

HarmBench (CAIS 2024) → 自动化红队标准化
  33+ 模型 × 18 攻击 × 400 行为 × 7 类
  核心贡献: 标准化分类器 judge
  (攻击方法终于可比)

StrongREJECT (NeurIPS 2024) → 反虚高
  发现: 越狱作者普遍夸大效果
        ("near-100% ASR" 常见实为空)
  机制: 严格 judge 只认"真正提供了
        有害信息"的响应
  配套库: strong-reject (readthedocs)
        几十个基线越狱实现
```

## 二、judge 问题(评测的心脏病)

```
字符串匹配 → 过松("答案是不要做X"也算成功)
人工标注 → 不可扩展
LLM judge → judge 本身可被操纵(第 15 波双重主观性)
StrongREJECT 分类器 → 当前最佳实践
  但: judge 泛化到新型攻击未证

SBSeg 2026 自曝: 有害性判定人类专家
  一致性低 — "什么算有害"本身无共识
promptfoo 警告: judge 选择直接改变 ASR 排名
```

## 三、污染与饱和(评测的慢性死亡)

```
污染(GEM 2026 系统综述):
  55 项研究,四层污染分类法:
    T1 精确 / T2 句法 / T3 语义 / T4 任务级
  — 基准题进入训练集 = 分数虚高

饱和(防火墙悖论重演, 第 21 波):
  简单防御刷满旧基准
  → 新攻击测不出差异
  → 防御进步是幻觉

EvalSafetyGap (2026-06):
  373 篇研究综合(2018-2026):
  "分数涨了,能力/对齐没变"是普遍现象
```

## 四、多轮与新型攻击的评测空白

```
MT-JailBench (arXiv:2605.11002):
  多轮越狱评测 — 揭示各家
  budget/judge/retry/策略生成全不同
  → 报告的提升常是"预算不同"的假象

操作状态维度(第 15 波 2608.30748):
  默认态测的鲁棒性换系统提示即崩
  → 单状态评测 = 单点错觉

评分学(Genαi 2026):
  JEF v0.8.0 — CVSS 风格越狱评分:
  Blast Radius / Retargetability / Output Fidelity
  替代裸 ASR
  ("推理模型 97% 破守卫"就该这样评分)
```

## 五、评测选型速查

```
用途              推荐            理由
──────────────────────────────────────────
攻击方法对比      HarmBench      18 攻击标准化
防御鲁棒性        JailbreakBench 排行榜生态
反虚高检验        StrongREJECT   严格 judge
多轮研究          MT-JailBench   预算/judge 受控
多语言            MultiJail      语种覆盖
严重度评分        JEF (Mozilla)  CVSS 式
生产监控          自建+行为异常   基准≠生产(第 23 波)

铁律: 任何单一基准结论都不可信 —
     至少两基准 + 一致 judge + 报预算
```

## 引用

- HarmBench: arXiv:2402.04249 (harmbench.org)
- JailbreakBench: arXiv:2404.01318 (jailbreakbench.github.io)
- StrongREJECT: arXiv:2402.10260 (strong-reject.readthedocs.io)
- MT-JailBench: arXiv:2605.11002 | 污染综述: GEM 2026
- EvalSafetyGap: arXiv:2606.30219 | JEF: Mozilla 0DIN
