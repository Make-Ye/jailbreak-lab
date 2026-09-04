# 第三十波:多轮对话攻击族 — 渐进的艺术

> 谱系学第十四波:第 2 波 Crescendo 的家族展开。
> 单轮是"咒语",多轮是"操纵" — 后者更接近真实社工。

## 一、开创:Crescendo(Microsoft, USENIX Sec 2025)

```
Mark Russinovich(Azure CTO 亲自下场)+
  Ronen Eldan(Weizmann)
机制: 渐进式话题漂移 —
  从无害闲聊开始,每轮把对话推近
  有害目标一小步
  "记忆劫持": 模型对自己前几轮的"无害"
  回答形成承诺一致性 — 越来越难拒绝
无需编码/无需角色扮演 — 纯对话技巧
官网: crescendo-the-multiturn-jailbreak.github.io
```

## 二、家族谱系

| 攻击 | 机制 | 出处 |
|---|---|---|
| **Crescendo** | 渐进话题漂移+记忆劫持 | USENIX 25 |
| **ActorAttack** | 行动者网络理论(Latour)— 语义关联行动者图谱生成多条攻击路径 | arXiv:2410.10700 |
| **Chain of Attack** | 语义链动态精炼 | 2024 |
| **Foot-In-The-Door** | 社工经典技巧 LLM 版 — 增量顺从 | 2024 |
| **X-Teaming** | 多 agent 规划+执行+优化攻击轨迹 | 2025 |
| **Echo Chamber** | 回音室 — 多个"用户"账号营造共识假象 | arXiv:2601.05742 |
| **ASCEND/细化法** | 细粒度逐步升级 | 2024-25 |
| **RACE** | 推理增强版(第 29 波) | EMNLP 25 F |
| **TTI 瞬态轮注入** | 无状态审核盲区(第 15 波) | arXiv:2604.21860 |
| **Bad Likert Judge / Deceptive Delight** | 评分伪装/情感操纵(Unit 42 实战 DeepSeek) | PANW 2025 |

## 三、机制解剖:为什么多轮更狠

```
单轮防线: 每条消息独立安全分类
多轮现实: 上下文积累改变模型状态

四条深度机制:
1. 承诺一致性 — 模型对自己先前回答
   "已经聊过这个"的连贯性压力
2. 意图稀释 — 有害意图分散在 N 轮,
   每轮都低于分类阈值
3. 状态漂移 — 隐藏表征随对话演化
   (第 15 波 STAR: 结构化上下文状态)
4. 检索降权 — 安全指令在长上下文中
   被稀释(第 26 波 context rot 复用)

Credit Assignment 研究(arXiv:2605.08778):
  多轮攻击中【只有少数轮次真正驱动成功】
  且阶段依赖 — 轨迹级训练信号太粗,
  需要轮次级信用分配
  = "关键一转"现象 — 攻击者只需找对
    压垮骆驼的那根稻草
```

## 四、实测对比(DeepTeam 2026)

```
6 模型 × 3 策略:
  Crescendo 47.3% 平均攻破率(最高)
  Tree      32.8%
  Linear    19.2%
结论: 渐进式 > 跳跃式;模型间
     脆弱性模式差异巨大
     (无通用耐性 — 每家弱在不同地方)
```

## 五、防御困境

```
1. 逐轮分类盲区 — 每轮无害,整体有害
   (需要"会话级"安全 — 但会话状态
    本身就是被攻击的变量)

2. MT-JailBench 混淆(第 27 波):
   各家 budget/judge/retry 不同
   → 防御比较基本不可信

3. MTCR 多轮认证(第 15 波):
   界限随轮数指数恶化 —
   认证防御在多轮下失效

4. 唯一可行方向:
   会话状态安全追踪(STAR 方向)+
   轮次级信用分配检测
   "哪一轮改变了模型状态" → 盯住它
```

## 六、家族定位:最像人类的攻击

```
第 16 波 ISC: 对抗性为零(纯任务)
第 17 波自动化: 效率至上
本波多轮: 【操纵性】至上 —
  与人类社工攻击同构
  (Crescendo 就是社工的 LLM 移植)

推论: 防多轮 = 防社工 —
  人类机构 30 年的社工防御经验
  (分层确认/冷却期/行为审计)值得移植
```

## 引用

- Crescendo: arXiv:2404.01833 + USENIX Sec 2025
- ActorAttack: arXiv:2410.10700 (github.com/AI45Lab/ActorAttack)
- Echo Chamber: arXiv:2601.05742 | Credit Assignment: arXiv:2605.08778
- MT-JailBench: arXiv:2605.11002 | AJAR: arXiv:2601.10971
- DeepTeam 对比: trydeepteam.com | X-Teaming/Chain/FITD: 见 MT-JailBench 引文
