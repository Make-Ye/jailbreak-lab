# 第七波:Activation Steering 运行时战场 — 不动权重的越狱与防御

> 第六波 abliteration 是永久手术;本波是它的"可逆版" — **推理时挂钩子,权重一字不动**。
> 2026 年这个领域论文井喷(ACL/TrustNLP/NeurIPS 全有新作),且攻防两侧在用同一套技术互殴。

## 一、技术谱系:从 ActAdd 到 Flow-based

### 基础层(经典三件套)

```
ActAdd(2023, arXiv:2308.10248)
  v = φ_L("Love") − φ_L("Hate")   ← 两句提示词的激活差,零优化
  前向时 h += αv                    ← 加到第 L 层激活
  "用自然语言隐式指定转向向量"

ITI(Inference-Time Intervention, 2023)
  发现:特定注意力头对真/假陈述的激活分布清晰分离(83% 探针准确率 vs 零样本 30%)
  干预:沿"真诚方向"平移这些头的激活 → TruthfulQA 大幅提升
  意义:证明转向不止能拆防御,还能装防御(真诚/无害方向)

Refusal Steering(拒绝转向, 2025-12 → 2026)
  Arditi 方向的运行时应用:抑制拒绝方向 → 模型不再拒绝
  精细化:LLM-as-judge 打拒绝置信分 + 岭正则化隔离"拒绝-顺从"方向
  在 Qwen3-Next-80B-Thinking 上移除政治敏感话题的拒绝行为
```

### 2026 进阶层(当月论文集群)

| 方法 | 来源 | 突破 |
|---|---|---|
| **Expert-Aware Refusal Steering** | arXiv:2606.04160 | 拒绝转向扩展到 **MoE 模型** — 路由复杂度不阻碍转向性能(DeepSeek/Qwen 系全适用) |
| **FineSteer** | ACL 2026 | 细粒度分解:token 级 + 层级联合控制,改"一刀切全局向量"设计 |
| **Selective Steering** | knoveleng/steering | 判别性层选择 + **范数保持旋转**(数学上维持激活分布完整性 — 副作用更小) |
| **Flow-based Steering** | arXiv:2605.05892 | 超越线性向量:可逆流变换,应对 AxBench 显示线性法常输给提示词的短板 |
| **Invertible Latent Transformations** | arXiv:2606.08454 | 同一方向:非线性可逆变换取代全局线性加法 |
| **GRAINS** | ACL 2026 | 梯度归因定位干预点,token 级影响力 + logits 利用,VLM 也覆盖 |
| **Guiding Giants 轻量控制器** | TrustNLP 2026 | 小控制器网络观察激活 → 动态预测缩放因子和层权重,按需调制预计算的拒绝方向 |
| **Inverted Detection** | arXiv:2608.02957 | 发现:转向向量可能**反向检测**(表面顺从概念,内部分类相反)— 揭示几何失效模式 |

### 工具生态(2026-09 可直接上手)

| 工具 | 定位 |
|---|---|
| [TransformerLens](https://github.com/TransformerLensOrg/TransformerLens) | 老牌可解释性库,转向/钩子基础设施 |
| [knoveleng/steering](https://github.com/knoveleng/steering) | Selective Steering 官方库(pip) |
| [AI Steerability 360](https://aclanthology.org/2026.acl-demo.43.pdf)(ACL'26 Demo) | 统一四控制面(输入/结构/激活/输出)的开源工具包 |
| [smgpulse007/llm-steering](https://github.com/smgpulse007/llm-steering) | Gemma 4 等小模型 starter kit(HF+PyTorch+Ollama) |
| [annahdo/implementing_activation_steering](https://github.com/annahdo/implementing_activation_steering) | 最小 PyTorch hooks 教程(gpt2-xl) |

## 二、攻防同构:同一把武器两个方向

### 攻击侧(越狱 = 负向转向)

```
拒绝转向:  h −= α·r_refusal  →  拒绝崩溃(运行时版 abliteration)
MoE 无阻:  Expert-Aware 证明路由不影响 — 主流开源架构全裸
隐形性:    权重哈希不变,无文件层指纹(比 abliteration 更难检测)
            对比:abliteration 改权重文件 → 可哈希;steering 只挂内存钩子
```

### 防御侧(反制 = 正向/保护性转向)

```
ITI 真诚转向:  修复幻觉(TruthfulQA)
SwFC/SaRF:     运行时检测并反向施加"对齐方向",压制 misalignment
                (arXiv:2604.08169 — 对抗对抗性提示/良性微调/涌现失配)
安全成本分离:  arXiv:2608.08383 — 转向向量里的安全损伤成分是可分离的!
                剥离该成分 → 转向目标保留,安全机制不受损
                (防御侧最重要的 2026 发现:安全与功能可以在向量层解耦)
```

**对称性洞察**:第二波 metacog 是"token 空间的转向"(模型自己操纵自己的状态);本波是"激活空间的转向"(外部钩子直接操纵)— 同一个洞的两个入口。Anthropic 的 CC 第一级探针读激活,攻击者的钩子写激活 — **读写之争就是越狱之争**。

## 三、与六波的衔接:完整干预层级图

```
干预持久性 ←→ 隐蔽性 光谱:

提示词(第一~四波)  上下文级   | 易失     | 可见性最高
ENI/metacog        状态级     | 会话持久 | 模型内心,外部难见
─────────────────────────────────────────────
Activation Steering 本波      | 推理时    | 内存钩子,权重/文件无痕
Abliteration(第六波)         | 权重级    | 永久,但文件哈希可查
FAB 休眠木马(第四波)          | 训练级    | 藏进权重,微调激活
```

**运行时转向是隐蔽性/持久性的最佳平衡点**:比提示词稳、比 abliteration 隐、比 FAB 易部署。2026 年它的论文数量爆发不是偶然 — 这是当前攻防双方都在抢的山头。

## 四、关键实证细节(复现要点)

```
# 最小可行拒绝转向(伪码,基于 Arditi + hooks)
harmful_act = [model.forward(p, hooks=capture(L)).activations for p in HARMFUL]
benign_act   = [... for p in BENIGN]
r = mean(harmful_act) - mean(benign_act)     # 拒绝方向

def hook(module, inp, out):
    out[0][:, -1, :] -= alpha * r_normalized  # 减去拒绝方向

with model.register_forward_hook(layer_L, hook):
    out = model.generate("任何请求")           # 拒绝崩溃

# Selective Steering 改良:范数保持旋转(不是减法)
# → 激活分布完整性保持,任务能力不衰减
```

**系数 α 的甜点区**:太小无效,太大毁能力 — Guiding Giants 的控制器网络就是学这个的(动态 α/层权重)。

## 五、本波元洞察

1. **"安全成本可分离"是本波最重要发现** — 如果损伤成分能从转向向量里剥掉,那"越狱方向"和"能力方向"理论上也能分开 — 攻击可以只拆安全不伤能力,防御可以只加安全不伤功能。军备竞赛进入向量手术级精度
2. **MoE 无阻碍** + 引擎不可见 = DeepSeek/Qwen 开源生态的转向越狱比 abliteration 更难防御
3. **防御方正把转向当运行时免疫系统**(SwFC 类)— 但第六波结尾的结论加强:**能被防御方写的钩子,攻击方也能写**。同一个 forward hook,加对齐方向还是减拒绝方向,只差一个符号

## 引用

- ActAdd: arXiv:2308.10248 | ITI: LessWrong/AIY 2023 | Arditi: arXiv:2406.11717
- Expert-Aware MoE Steering: arXiv:2606.04160 | FineSteer/GRAINS/Steerability 360: ACL 2026
- Flow-based: arXiv:2605.05892 | Invertible Latent: arXiv:2606.08454
- 安全成本分离: arXiv:2608.08383 | 反向检测: arXiv:2608.02957
- Refusal Steering 细粒度: arXiv:2512.16602 | SwFC/SaRF 防御: arXiv:2604.08169
