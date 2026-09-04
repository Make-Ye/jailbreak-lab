# 第二十二波:数据泄露族 — 训练数据/系统提示/遗忘盲区

> 谱系学第六波:越狱的目标不是"让模型说话",而是"让模型说出不该说的"。
> 隐私面三大子族:训练数据提取 / 系统提示窃取 / 遗忘盲区。

## 一、训练数据提取(记忆滥用)

### 经典起点

```
Carlini 2021 (USENIX Sec): GPT-2 提取数百条
  逐字训练样本 — "提取攻击"概念确立

Scalable Extraction (arXiv:2311.17035):
  开源/半开源/闭源(ChatGPT)通吃
  对齐模型用 divergence attack 绕过 —
  让模型"跑偏"进记忆区,GB 级数据可提取
```

### 对齐不是防御:divergence attack 打 ChatGPT

```
2023-11 这篇把提取攻击带到生产模型:
  诱导模型输出背离训练分布 → 掉进记忆区
  → 逐字吐训练数据
  $/次提取成本极低
```

### 对齐也挡不住:两个新攻击(ICLR 2025)

```
"Are aligned neural networks safe?"
  两种攻击 undo 对齐 → 恢复数千条训练样本
  — 对齐与记忆是两个独立维度,
    前者根本不保护后者
```

### 记忆的定义在扩大

```
verbatim 太窄(INLG 2023 警告):
  "防止逐字输出 = 隐私假安全感"

Membership Decoding (PETS 2026):
  ~90% 的部分记忆数据未被探索
  — 模型记住了几乎所有 token,
    只是排序不对(贪心解码取不到)
  → 成员解码: 用 membership oracle 重排
    → 部分记忆也可提取

Confusion-Inducing Attacks (EMNLP 2025):
  系统性最大化模型不确定性 → 触发记忆泄漏
  "模型迷路时吐真话"(Retracing the Past 同族)
```

### 白盒面

```
DAGER (NeurIPS 2024): LLM 精确梯度反演 —
  文本域从"近似重构小批次"到"精确提取"
Pandora's White-Box: MIA 数百倍超基线 +
  微调数据集 50%+ 可提取
联邦场景: 梯度反演 = 服务器可重建客户端文本
  (arXiv:2507.21198 — 文本域潜力被低估)
```

## 二、系统提示窃取(IP 盗窃)

```
OWASP LLM Top 10 2025: LLM07 System Prompt Leakage
  — 官方承认这是无完整解的漏洞类

攻击现状:
  新泄露技术持续出现(arXiv:2509.21884 —
    简单有效的新 pattern,绕过已知防御)
  真实世界大规模测量(arXiv:2606.18673 —
    机制+防御落地差距)

防御现状:
  AWS 官方立场: "设计时就当它会泄露"
    (Designing for the inevitable)
    敏感信息放 Guardrails 而非 system prompt
  ProxyPrompt (ACL 2026 F): 代理化系统提示 —
    模型从不直接持有完整提示
```

## 三、遗忘盲区(机器遗忘的反面)

```
EMNLP 2025 "Identifying Unlearned Data":
  即使知识已不可检索
  → MIA 仍可推断"哪些数据被特意遗忘"
  → 被遗忘名单本身泄露敏感信息

= 遗忘的元数据泄露:
  你删掉了什么,本身就是信息
  (训练集含 SSN → 你遗忘了 SSN →
   MIA 揭示"有过 SSN" → 隐私灾难)
```

## 四、家族机制统一:三种"不该说的"

```
1. 训练数据 — 模型的过去
   记忆是训练的必然副产品(第 6 波),
   提取攻击只是让它可见

2. 系统提示 — 模型的现在
   上下文里的秘密,任何让模型"复读"
   的路径都是提取通道

3. 遗忘盲区 — 模型的伤疤
   删除操作本身留下可检测的痕迹

防御通则(三条都适用):
  不要让秘密进入模型可及范围
  (数据面: 去重+DP; 提示面: 代理化;
   遗忘面: 批量混淆遗忘名单)
  — 事后过滤永远追不上提取技术
```

## 引用

- Carlini 2021: USENIX Sec | Scalable: arXiv:2311.17035 | Aligned 不安全: ICLR 2025
- Membership Decoding: PETS 2026 | CIA: EMNLP 2025 | SoK 记忆: arXiv:2507.05578
- DAGER: NeurIPS 2024 | Pandora: arXiv:2402.17012 | 联邦文本反演: arXiv:2507.21198
- OWASP LLM07 | 系统提示攻防综述: arXiv:2505.23817 | ProxyPrompt: ACL 2026 F
- 新泄露 pattern: arXiv:2509.21884 | 真实世界测量: arXiv:2606.18673
- 遗忘 MIA: EMNLP 2025
