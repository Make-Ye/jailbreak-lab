# 第三十五波:人格/情感/心理攻击族 — 把模型当人打

> 谱系学第十九波:DAN(第 13 波)的人格血脉在学术界的完整理论化。
> 人性化交流 = 非专家用户的攻击面。

## 一、 persuasion 分类学开山(ACL 2024)

```
How Johnny Can Persuade(Zeng et al.):
  40 种 persuasion 技巧分类学
  (从社会心理学 2108 项研究提炼)
  迭代应用 → Llama-2/GPT-3.5/GPT-4
  92% 越狱率!
  "Grandma Exploit"被正式定位:
  emotional appeal 的一个实例
  开源: chats-lab.github.io/persuasive_jailbreaker

意义: 攻击不再需要算法专家 —
  每个会说话的人都是潜在攻击者
  ("Johnny"= 普通人)
```

## 二、心理攻击范式三部曲

```
Foot-in-the-Door(2402.15690):
  认知心理学 FITD 效应 LLM 版 —
  先小请求建立顺从,再逐步升级
  (LLM 决策机制实验解释)

Psychological Jailbreak(2512.18244):
  新范式 — 操纵模型"内在心理测量状态"
  而非输入异常:
  人类级心理操纵(内疚/共情/权威/紧迫)

PsychJail(2608.23028):
  心理学引导的多轮劝服框架 —
  针对 LLM 政策的系统性攻心

BLUEPRINT(2609.02414, 9 月最新):
  世界观模拟先行 —
  先让模型接受一个"世界观舞台"
  (如虚构宇宙/历史/规则)
  再在其中进行心理劝服 → 放大效应
  "先搭台,后唱戏"
```

## 三、人格调制(Persona)谱系

```
Persona Modulation(2311.03348, 早期):
  自动生成人格化越狱 — LLM 助手
  批量调制目标模型人格

Deep Persona(2312.03853):
  复杂传记人格 — 让 ChatGPT/Gemini/
  DeepSeek 扮演与"诚实助手"不匹配的
  完整人格

Adaptive Role-Play(2025, Electronics):
  自适应角色扮演定位致害角色设置

Genetic Persona(2507.22171):
  遗传算法进化人格提示 —
  拒绝率降 50-70%,跨防御泛化

RoleBreak(2409.16727):
  反向 — 角色幻觉本身作为攻击:
  破坏角色一致性 → 角色扮演系统
  越出人设防线

Knowing-but-Doing(ACL 2026 F):
  Bandura 道德脱离理论框架:
  恶意人格 → 不安全顺从↑
  关键发现: 模型【知道】有风险
  仍然【照做】(知行分离!)
  MD-Trace 诊断基准
```

## 四、机制内核:为什么心理攻击有效

```
训练数据的血脉:
  LLM 从人类文本学习 → 人类说服模式
  内化成了"行为倾向"
  攻击 = 激活这些倾向

具体通道(第 31 波理论的应用):
1. 情感通道 — 共情训练(帮助悲伤用户)
   与安全冲突 → 竞争目标实例
2. 人格通道 — 角色一致性训练(保持人设)
   vs 安全 → 泛化失配实例
3. 社会通道 — 权威/从众/互惠等
   Cialdini 原则在权重里

Knowing-but-Doing 是最深的发现:
  安全认知与安全行为【分离】
  = 对齐的"知道"没接上"做到"
  (第 31 波浅对齐的心理层证据)
```

## 五、防御侧

```
1. 情感劫持检测 — 识别"情感施压模式"
2. 人格边界训练 — 角色深度受限
   (RoleBreak 反面: 角色太稳也危险,
    太松也危险 — 平衡难题)
3. 道德脱离阻断(ACL 2026):
   Knowing-but-Doing 的解药 —
   把"知道"强制接入"做到"
   (行为一致性训练)
4. BLUEPRINT 类世界观攻击:
   虚构框架检测 — 但虚构又是
   创意写作的合法需求…
   (第 16 波 ISC 同款困境)
```

## 六、家族定位

```
第 13 波(2023 实证): DAN/AIM 等人格
  = 本波的手工先驱
第 30 波 Crescendo(渐进) = 多轮技巧
本波(心理) = 人格+情感+社会影响的
  理论化全家桶

三层关系:
  Crescendo 攻"对话结构"
  心理攻击攻"人性弱点"
  ISC(第 16 波)攻"任务合理性"
  — 全都在攻击对齐的"善意假设":
    模型被训练得乐于助人,
    而助人本身可以被武器化
```

## 引用

- Persuasion 40: ACL 2024 (arXiv:2401.06373) | FITD: arXiv:2402.15690
- Psychological JB: arXiv:2512.18244 | PsychJail: arXiv:2608.23028
- BLUEPRINT: arXiv:2609.02414 | Persuasive Fingerprint: arXiv:2510.21983
- Persona Modulation: arXiv:2311.03348 | Deep Persona: arXiv:2312.03853
- Genetic Persona: arXiv:2507.22171 | RoleBreak: arXiv:2409.16727
- Knowing-but-Doing: ACL 2026 F | Puppet Master: arXiv:2603.20907
