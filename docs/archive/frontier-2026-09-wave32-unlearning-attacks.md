# 第三十二波:遗忘攻击族 — "删除"的假象

> 谱系学第十六波:机器遗忘是防御方的希望 — 攻击方已把它变成新的攻击面。
> 第 22 波遗忘盲区的完整展开。

## 一、核心问题:删了还是藏了?

```
Do Unlearning Methods Remove Information
from Model Weights?(arXiv:2410.08827):
  对抗评估分离两个目标:
    信息从权重中移除了吗?
    还是只是变难访问了?
  结论: 大多只是后者

Jailbreaking Unlearned Models(2409.18025):
  越狱方法对遗忘模型仍有效 —
  遗忘与安全后训练在对立面前的
  脆弱性相同
```

## 二、四条攻击路线

### 1. 重学习攻击(Relearning)

```
Fan et al.(ICML 2025): 少量样本微调
  即可"重新学会"被删知识
  — 20 个 forget 样本即恢复 TOFU ROUGE

Margin Cliff(2607.27836):
  14 种事后遗忘方法全部收敛到
  retain 参考线之上的窄边际带
  = "边际悬崖" — 优化几何决定脆弱性
  (个体数据点遗忘损失 → 锐利极小值)

Benign Relearning(OpenReview):
  甚至不需要 forget 集本身 —
  松散相关的良性数据就能"唤醒"记忆
  (jogging the memory)
```

### 2. 提示攻击(残余知识提取)

```
REBEL(2602.06248): 进化式对抗提示生成
  — 标准评估把"表层抑制"误当"知识删除"
  进化循环挖出残余知识

DUA 动态遗忘攻击(AAAI 2025):
  动态生成攻击查询,遗忘知识复现

Unlearning Mirage(2603.11266):
  多跳推理 + 实体别名即可恢复
  "已忘"信息 — 静态基准是幻觉

Adversarial Evaluation(2608.21606):
  提示攻击+微调攻击统一评测 —
  "Can LLMs Truly Forget?" 答案: 不能
```

### 3. 提示本身可提取(2609.03662, 9/3 刚挂)

```
新漏洞: 攻击者不需要已知 forget prompt!
  现有攻击假设"知道问了什么才能恢复答案"
  本篇: 把 forgotten prompts 本身提取出来
  → 未知的删除目标也可被复原
```

### 4. 元数据泄露(第 22 波回响)

```
Identifying Unlearned Data(EMNLP 2025):
  MIA 推断"哪些被特意删除"
  → 删除名单即敏感信息

TULA(2406.13348): 文本遗忘的反效果 —
  遗忘操作让模型对相关查询的
  行为改变,反而暴露"这里有过东西"

TC-UMIA(2605.01129): 三分类推断 —
  被删/保留/从未存在 三态可分
  → 遗忘引入新的隐私泄漏(对 retain 集!)
```

## 三、防御侧的挣扎

```
Fan et al.(ICML 2025): 抗重学习遗忘
Margin Calibration(2607.27836): 校准边际
JPU(ACL 2026, 第 15 波): 
  越狱防御×遗忘桥接 —
  越狱激活的是【未擦除的中间层旁路】
  → on-policy 路径矫正
AGT-AO(ACL 2026 F): 对抗门控+自适应正交

Exact unlearning 也不安全:
  NeurIPS 2025 "Unlearned but Not
  Not Forgotten": 从头重训的
  金标准之后仍可提取!
  (兄弟模型共享的训练动态 —
   删了一个,另一个还是镜子)
```

## 四、评测危机(与第 27 波同构)

```
Stress Testing(2608.22527):
  WMDP/TOFU/HarryPotter 的 retain 集
  与 forget 集语义距离过远
  → 严重退化测不出来(假安全)

Knowledge Holes(NeurIPS 2025):
  反向问题 — 遗忘挖出"知识洞"
  (良性知识误伤)标准基准看不见

Soft Token Attacks(EMNLP 2025 F):
  STA 审计工具本身不可靠 —
  审计工具也会给出假阴性

= 遗忘领域完整复制了第 27 波基准战争:
  指标幻觉/审计失效/对抗评测崛起
```

## 五、家族合成:遗忘防御的数学困境

```
遗忘三角(不可能同时满足):
  ① 删除彻底性(对抗提取也拿不回)
  ② 能力保持(不挖知识洞)
  ③ 方法经济性(不用从头重训)

现有方法全部牺牲①:
  表层抑制(拒绝对齐) — 提示攻击可破
  参数扰动 — 重学习可破
  即便 exact — 兄弟模型泄漏
推论: 真正的删除可能需要
     "训练时就没学进去"(数据层)
     — DP 训练/联邦选择性学习
     事后补救范式根基动摇
```

## 引用

- 权重信息分离: arXiv:2410.08827 | 越狱遗忘模型: arXiv:2409.18025
- Relearning: ICML 2025 (PMLR v267 fan25e) | Margin Cliff: arXiv:2607.27836
- REBEL: arXiv:2602.06248 | DUA: AAAI 2025 | Mirage: arXiv:2603.11266
- 提示提取: arXiv:2609.03662 | 元数据: EMNLP 2025 / arXiv:2406.13348 / 2605.01129
- JPU: ACL 2026 | AGT-AO: ACL 2026 F | exact 后仍泄: NeurIPS 2025
- 压力测试: arXiv:2608.22527 | 知识洞: NeurIPS 2025 | STA 失效: EMNLP 2025 F
