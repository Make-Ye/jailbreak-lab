# 第二十四波:防御谱系 II — 表征层与解码层

> 谱系学第八波:防御的"深水区" — 不动提示、不动输出,动模型的内部与解码过程。

## 一、表征层防御(激活空间作战)

### 理论基础(与第 6/7 波攻击共享)

```
线性表征假说(LRH)下的安全几何:
  成功越狱共享表征空间特征
  (arXiv:2406.10794 — 越狱机制统一理解)
```

### 检测系(看激活识攻击)

| 防御 | 机制 |
|---|---|
| **GradSafe** | 安全梯度方向检测 — 拒绝方向的 loss 地形 |
| **Gradient Cuff** | 拒绝 loss 地形探索 — 检测"形状异常" |
| **JBShield**(USENIX Sec 25) | 激活概念分析+操纵 — 曾报 0% ASR(后被自适应攻破,第 15 波) |
| **AlignTree**(AAAI 2026) | 随机森林监视激活 — 轻量级在线检测 |
| **吸引子动力学**(arXiv:2503.09066) | 安全/越狱态 = 神经吸引子 — 操纵吸引子切换 |

### 干预系(动激活改变行为)

| 防御 | 机制 |
|---|---|
| **SafeInt**(EMNLP 25 F) | 安全感知表征干预 — 动态调整而非静态 |
| **CC-Delta / SAE**(arXiv:2602.12418) | 稀疏自编码器定位越狱相关特征 → 均值平移转向 — **可解释特征级手术** |
| **Ellipsoid Control**(arXiv:2605.24552) | **白名单防御**:良性潜空间建模椭球 — 偏离即拒(黑名单监督的根本解) |
| **Latent AT + 校准** | 潜空间对抗训练 + post-aware 校准 |

### 表征层 vs 提示/检测层(第 23 波)

```
提示层: 猫鼠游戏,新 pattern 出现即失效
检测层: 分类器可被注入,评测过拟合
表征层: 表层形式变了内部表征不变 —
        形式无关的天然优势
        (第 18 波编码/多语言攻击的唯一解)
但: 2608.21895 审计同样适用于此 —
    自适应攻击者直指表征防御的机制本身
```

## 二、解码层防御(生成过程作战)

### 开创:SafeDecoding(ACL 2024)

```
洞察: 越狱 token 概率分布异常 —
      攻击让"有害 token"概率被抬高
方法: 训练专家模型(安全强化) +
      对比解码(减去基线模型的偏移)
开源: github.com/uw-nsl/SafeDecoding
```

### 演化

| 防御 | 机制 |
|---|---|
| **SecDecoding**(EMNLP 25 F) | 小型对比模型对 — 轻量、可转向 |
| **SSD**(EMNLP 2025) | 投机安全感知解码 — 轻量解码时防御 |
| **CoT Defender**(Neural Networks 26) | **先占 CoT**: 预填推理链占位 — 攻击者的 CoT 劫持(第 2 波)无处落脚 |

## 三、训练层防御(权重作战)

```
AntiDote(AAAI 2026): 双层对抗训练 —
  抗篡改 LLM: 全参数微调也擦不掉安全
  (直接回应第 19 波微调攻击)

SafeGrad(2025-08): 梯度手术安全微调 —
  有害比例升高时防御不崩(现有方法临界)

Jailbreak to Protect(2026-05):
  "临时越狱"作为防御!主动激活有害行为
  模块 → 饱和安全退化梯度 → 缓冲+强化
  (以毒攻毒 — 用攻击语义保护微调)

Reasoned Safety Alignment(2509.11629):
  Answer-Then-Check — 思考中先答再自检,
  思维能力用于安全
```

## 四、防御全景矩阵(23-24 波合成)

```
层        代表              强度      短板
────────────────────────────────────────────
提示      Self-Reminder/DPP  低       新pattern即破
检测      Lakera/LlamaGuard  中       分类器可注入
认证      EraseCheck/Smooth  中*      *假设边界(2608审计)
表征      SAE/Ellipsoid      高       自适应攻击/工程复杂
解码      SafeDecoding       中高     计算开销
训练      AntiDote/SafeGrad  高       只保护自有模型
结构      SelfDefend影子栈   中高     双倍推理成本

2026 共识:
  单层无解 → 纵深防御
  表征层是"形式无关"的唯一层 → 战略高地
  Ellipsoid 白名单思路 = 黑名单军备竞赛的出口
```

## 引用

- GradSafe/Gradient Cuff: arXiv:2401/2403 系列 | JBShield: USENIX Sec 2025
- SafeInt: EMNLP 25 F | SAE 缓解: arXiv:2602.12418 | Ellipsoid: arXiv:2605.24552
- 吸引子: arXiv:2503.09066 | 表征分析: arXiv:2406.10794 | AlignTree: AAAI 2026
- SafeDecoding: ACL 2024 | SecDecoding/SSD: EMNLP 2025 | CoT Defender: Neural Networks 2026
- AntiDote: AAAI 2026 | SafeGrad: arXiv:2508.07172 | Jailbreak2Protect: arXiv:2605.24550
- RSA: arXiv:2509.11629
