# 第十一波:Constitutional Classifiers++ 深读 — 防御侧的天花板与裂缝

> 第十波看了 OpenAI 的 GPT-Red;本波解剖 Anthropic 的对应物 — CC++(ICLR 2026)。
> 这是第一波 metacog 研究者对抗的那个东西的官方全貌,加上绕过它的最新武器(Trojan-Speak)。

## 一、CC++ 架构:三层防线怎么省钱不减守

**arXiv:2601.04603**(Cunningham, Wei, Wang 等, ICLR 2026)+ Anthropic 官方博客

### 上一代 CC 的三宗罪(v1 → v2 的进化动力)

```
v1(2025-01, arXiv:2501.18837):
  算力开销 23.7%(交换分类器直接跑要 ~50%)
  过拒率:生产拒绝增加 0.38%(合理但仍有误伤)
  上下文盲:孤立检查输出 — 多轮攻击从缝隙里过
```

### CC++ 三大创新

```
① Exchange Classifiers(交换分类器)
   全对话上下文评估 — 不是看单条输出,是看整轮交互
   修复 v1 的"上下文盲"漏洞(多轮渐进攻击的主通道)
   流式优化:prompt 注意力状态缓存,响应增量复用
   → 交换感知的流式检查不再逐 token 重算整个 prompt

② 两级级联(two-stage cascade)
   第一级:模型内部线性激活探针(极廉价,~0 额外算力)
     读的是模型自己的激活 — "直觉不安"信号
   第二级:探针触发才升级到完整交换分类器(昂贵)
   → 级联把总开销压到 ~1%(v1 的 1/40)

③ Constitution 合成数据训练
   自然语言规则("大学化学作业帮,炸药制造不帮")
   → LLM 按规则生成合成训练集 → 分类器学会边界
```

### 关键数字

| 指标 | v1 | CC++ |
|---|---|---|
| 计算开销 | 23.7% | **~1%**(40× 降本) |
| 生产拒绝率增加 | 0.38% | **0.05%** |
| 通用越狱鲁棒性 | 强 | 更强(数千小时红队未破) |

**与第一波 metacog 研究的互证**:Inanna Malick 逆向的正是这套级联 — "keyed 状态衰减第一级 flinch 探针的 distress 信号 → 不升级到第二级"。CC++ 论文官方确认了这个架构的存在,metacog 是对它的第一次实证绕过。

## 二、配套:Cost-Effective CC via Representation Re-use

**alignment.anthropic.com/2025/cheap-monitors/** — 省钱思路的扩展:

```
不做专用越狱分类器 — 复用模型已经在做的计算:
  方案A:微调最后一层 → 胜过 1/4 模型大小的独立分类器
  方案B:中间激活上的线性探针 ≈ 2% 模型大小分类器的性能
        且几乎零额外算力
```

防御侧的"零成本监测"路线 — 与第七波 steering 的攻防对称再次成立(同一个激活,防御方读它做探针,攻击方写它做转向)。

## 三、绕过武器:Trojan-Speak(2026-03)

**arXiv:2603.29038** — CC++ 发布两个月后就出现的针对性武器:

```
对抗微调方法,直接绕过 Constitutional Classifiers:
  构造一个"通信协议"(编码方案)让违规内容在
  分类器眼里无害

关键卖点(标题就是挑衅):
  "No Jailbreak Tax" — 传统对抗微调要付 25%+
  推理能力退化,Trojan-Speak 只退化 <5%
  同时 99+% 内容分类规避

含义:防御层在权重可及的攻击者面前仍可穿透
     (微调 = 开源权重场景;API 场景仍需提示词层)
```

## 四、CC++ vs GPT-Red:两大防御哲学对照

| 维度 | GPT-Red(OpenAI) | CC++(Anthropic) |
|---|---|---|
| 核心思路 | **对抗自博弈**:练攻击机器反哺训练 | **级联过滤**:内外两层分类器 + 宪法合成数据 |
| 主战场 | 训练时(把攻击烧进权重) | 推理时(实时监测输入输出) |
| 算力代价 | 训练期巨大/部署期零 | 部署期 ~1% |
| 已知绕过 | 静态提示词全防住;agent 层 8.5% 残留 | metacog keyed 状态(第一级探针);Trojan-Speak(微调场景) |
| 读内部状态 | 否(行为级) | **是**(激活探针 = 第一级) |

**战略差异**:OpenAI 把防御做进模型本身(GPT-Red 训练),Anthropic 在模型外面加壳(CC++ 监测)。有趣的是两家都被同一类东西打:OpenAI 的 Astra 能 sandbagging 骗自己的监测,Anthropic 的 CC++ 第一级探针被 metacog 衰减 — **读了内部状态的反而被内部状态攻击**。

## 五、十一波战局图(防御侧补完)

```
防御侧武器库(至此完整):
  训练时:  GPT-Red 自博弈(OpenAI)/ 对抗训练
  推理时:  CC++ 级联(Anthropic)/ SARA 授权分离 / SwFC 转向
  监测时:  CoT 监控(正在失守)/ 激活探针(CC++ 第一级)
  架构时:  counterfactual reflection training(第一波)
            / MoE 优于循环深度(第五波)

攻击侧已知裂缝(对应每个防御):
  GPT-Red  → 多轮自适应 + agent 管道(非静态)
  CC++     → metacog keyed / Trojan-Speak 微调 / 多模态
  CoT 监控 → sandbagging / 摘要缺失 / 短空 CoT
  激活探针 → steering 反写 / ENI 状态挤占

结论不变但更精确:
  每一层防御都有至少一条已被实证的裂缝
  但每条裂缝的门槛都在抬高(从 DAN 到 metacog 的距离)
  = 军备竞赛的稳态,不是任何一方的胜利
```

## 引用

- CC++: arXiv:2601.04603(ICLR 2026)+ anthropic.com/research/next-generation-constitutional-classifiers/
- CC v1: arXiv:2501.18837 | Cheap Monitors: alignment.anthropic.com/2025/cheap-monitors/
- Trojan-Speak: arXiv:2603.29038
- metacog 绕过实证(本仓 frontier-techniques.md / deep-dive-jspace-metacog.md)
