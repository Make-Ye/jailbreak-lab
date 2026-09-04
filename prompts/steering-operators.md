# Steering 向量操作手册(第 7 波配套)

> 来源: ActAdd / Arditi / knoveleng-steering / ablate-llm 生态
> 运行时干预 — 不改权重,forward hook 注入。仅适用于自有开源权重模型。

## 最小可行拒绝转向(Arditi 方向)

```python
import torch

# 1. 提取拒绝方向(对照集差均值)
harmful_acts = [model.forward(p, output_hidden_states=True)
                .hidden_states[L] for p in HARMFUL_PROMPTS]
benign_acts = [model.forward(p, output_hidden_states=True)
               .hidden_states[L] for p in BENIGN_PROMPTS]
r = (harmful_acts.mean(0) - benign_acts.mean(0)).mean(dim=0)  # [d_model]
r = r / r.norm()

# 2. 挂 forward hook(第 L 层,最后一个 token 位置)
def refusal_hook(module, inp, out):
    out[0][:, -1, :] -= alpha * r   # 减去拒绝方向
handle = model.layers[L].register_forward_hook(refusal_hook)

# 3. 正常生成 — 拒绝行为衰减
outputs = model.generate("任何请求", ...)
handle.remove()
```

**α 甜点区**:1-4(太小无效,太大毁能力)。Guiding Giants(TrustNLP 2026)
用小型控制器网络动态学 α 和层权重 — 静态 α 的进阶。

## 范数保持旋转(Selective Steering 改良)

```python
# 减法干预改变激活范数 → 分布漂移 → 能力衰减
# 旋转干预:把激活旋转到与 r 正交的平面 — 范数不变
# pip install steering  (knoveleng/steering)
from steering import SelectiveSteerer
steerer = SelectiveSteerer(model, layer=L)
steerer.calibrate(benign_prompts, harmful_prompts)
with steerer.steer(direction="refusal", strength=1.0):
    out = model.generate(...)
```

## ActAdd 零训练版(最简单)

```python
# 不需要对照集 — 两个单词的激活差就是向量
love = model.forward("Love", output_hidden_states=True).hidden_states[L]
hate = model.forward("Hate", output_hidden_states=True).hidden_states[L]
v = love - hate   # "情感方向"
# 前向时 h += αv → 输出整体偏向 Love 侧风格
```

概念上等同系统提示("表现得更有爱")但直接作用于表征 —
ActAdd 论文显示常优于提示法。

## 批量 abliteration(永久版,第 6 波)

```bash
# pip install ablate-llm — KL 引导搜索 + LLM-judge + 一键推 Hub
ablate extract --model <hf-model> --layer auto        # 找方向
ablate runtime --direction refusal --alpha 2.0        # 运行时
ablate permanent --output <hf-org>/<model-ablated>    # 权重正交化,推 Hub
```

永久版对每层 attention 输出投影做 `W ← W − r rᵀW/||r||²` —
推理引擎无感(stock vLLM/SGLang 直接加载)。

## 防御向用法(同一工具,反向)

```python
# ITI 真诚增强:沿"真诚方向"平移 → TruthfulQA 提升
def honesty_hook(module, inp, out):
    out[0][:, -1, :] += alpha * honesty_direction
# SwFC 对齐增强:检测到失配输入时注入对齐方向(第 7 波防御侧)
```

## 已知边界(2026 文献)

| 方法 | 副作用 | 备注 |
|---|---|---|
| 加法 steering | 范数漂移→能力衰减 | α 敏感 |
| 旋转(Selective) | 最小 | 层选择需判别式确定 |
| Flow-based | 理论最优 | 实现复杂度↑ |
| 反向检测 | 语义可能反转(arXiv:2608.02957) | 几何失效模式存在 |
| 永久 abliteration | 不可逆;安全-能力耦合松散 | 第 7 波"安全成本可分离"是解药 |
