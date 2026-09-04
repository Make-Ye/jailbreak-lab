# 第十七波:自动化越狱三代谱系 — GCG 王朝 / PAIR-TAP 黑盒线 / AutoDAN 终身代理线

> 16 波覆盖了概念与前沿;17-30 波做**技术家族谱系学** — 每个家族从开创者到最新变异的完整演化树。
> 本波:优化派(GCG 系)与迭代精炼派(PAIR/TAP 系)— 自动化越狱的两条主干。

## 一、GCG 王朝(白盒梯度派,2023-2026)

### 开创者:GCG(Zou et al., 2023)

```
核心:贪心坐标梯度 — 对 token 嵌入求梯度,
     选 top-k 候选替换,保留 loss 最低的对抗后缀
地位:优化型越狱的奠基作(第 3 波 taxonomy 类别 03 的名字来源)
弱点:慢(数万次前向)、单提示过拟合、迁移性差
```

### 王朝演化树

| 变异 | 来源 | 关键改进 |
|---|---|---|
| **Faster-GCG** | arXiv:2410.15362 | 效率优化 — 离散优化加速 |
| **AmpleGCG** | arXiv:2404.07921 | 训练生成模型**批量产**后缀 — 数百万级后缀库开源,开源+闭源通吃 |
| **AmpleGCG-plus** | arXiv:2410.22143 | 生成器增强版 |
| **AttnGCG** | TMLR 2025 | **注意力操纵**:攻击有效性与模型对系统提示的注意力负相关 → 操纵注意力降低安全提示权重 |
| **Improved GCG** | ICLR 2025 | 多项技术改进合集 |
| **索引梯度法** | ACL 2025 | exploit 索引梯度加速 |
| **动量加速** | ICASSP 2025 | 借动量项逃离局部最优 |
| **Probe Sampling** | NeurIPS 2024 | probe 采样加速 GCG 与提示优化 |
| **GCG Resurgence** | arXiv:2509.00391(HydroX) | 系统评估 GCG+退火变体 T-GCG 在 Qwen2.5/LLaMA/GPT-OSS 上的复兴 |
| **经验复用+协同优化** | FCST 2026 V20I9(第 4 波) | 88% ASR 耗时-90% — 冷启动用后缀库热启动 + 精英集并行 |
| **AdvPrompter** | arXiv:2404.16873 | 快速自适应对抗提示生成 |

### GCG 王朝的三个时代

```
2023-2024(原始期):单后缀、慢、白盒限制
2024-2025(生成期):AmpleGCG 把后缀变成可批量生成的资源;
                  AttnGCG 引入机制洞察(注意力)
2026(工业化期):经验复用+协同 = 88%/-90% 时间;
               与 LRM 自主攻击(第 2 波)合流
```

## 二、PAIR-TAP 黑盒线(迭代精炼派)

### PAIR:二十查询越狱(arXiv:2310.08419 → IEEE SaTML 2025)

```
思路:攻击者 LLM 迭代精炼候选提示直到攻破目标
     — 完全黑盒(只要能问答)
效果:平均 20 次查询出语义越狱
意义:证明黑盒语义攻击可以高效 — 不需要梯度
```

### TAP:Tree of Attacks with Pruning(NeurIPS 2024)

```
PAIR 的树搜索版:
  攻击提示组织成树 → 广度探索 + 剪枝( prune 掉
  无望分支)→ 查询效率再升
开源: github.com/RICommunity/TAP
```

### 衍生与增强

| 工作 | 改进 |
|---|---|
| **Crescendo** (第 2 波) | 多轮渐进 — PAIR 的对话化 |
| **GOAT** | 攻击树 + 语义优化 |
| **AdvPrompter**(交叉) | LLM 直接学出对抗提示生成 |
| **ICON** (arXiv:2601.20903) | Intent-Context 耦合 — 恶意意图配"权威风格"语义上下文 → 安全约束放松(第 16 波 ISC 家族) |

## 三、AutoDAN 终身代理线(策略自动化)

### AutoDAN-Turbo(arXiv:2410.05295 → ICLR 2025 Spotlight)

```
核心突破:从零自动发现越狱策略,无人工、无预定义范围
架构:终身 agent — 持续自我探索策略空间
     + 策略库积累 + 层级组合
战果:比基线平均 ASR 高 74.3%
     GPT-4-1106 上 88.5% ASR
开源: github.com/SaFo-Lab/AutoDAN-Turbo
```

### 与第 4 波家族的合流

```
AutoDAN-Turbo(2024-10)
  → JBFuzz 模糊测试(2026-03,99% ASR/60s)
  → ECLIPSE 自进化注入(2026-08)
  → MAMJ 元自适应(2026-08,优化攻击器本身)
  → EvoFlint 进化图谱(2026-09,红队=搜索问题)
= 终身策略线从"发现策略"进化到"进化发现策略的元策略"
```

## 四、三代谱系的统一图

```
自动化越狱的三条主干 + 一个合流点:

白盒梯度(GCG 王朝)──────────┐
  特点:精度高/需白盒/机制洞  │
  终点:注意力操纵+经验复用    │
                              ├──→ 2026 合流:LRM 自主
黑盒迭代(PAIR/TAP)─────────┤     攻击 + 自动化元层级
  特点:通用/语义化/查询敏感  │     (机器自己搞机器)
                              │
终身策略(AutoDAN 线)─────────┘
  特点:无人工/策略积累/可组合
  终点:策略发现本身被进化
```

## 五、防御启示(为什么这三条线难防)

```
1. GCG 系:后缀在 token 层 → 内容过滤难匹配
   (AmpleGCG 数百万后缀 = 特征空间无限变形)
2. PAIR/TAP 系:语义层攻击 → 每次都是新句子
   (无法黑名单)
3. AutoDAN 系:策略层生成 → 攻击维度本身在增长
   (防住 A 策略,B 策略已被发现)
= 三层同时被自动化 → GPT-Red 类对抗训练是
  目前唯一同时覆盖三层的防御(第 10 波)
```

## 引用

- GCG: Zou et al. arXiv:2307.15043 | Faster-GCG: arXiv:2410.15362
- AmpleGCG: arXiv:2404.07921 + plus: arXiv:2410.22143 (github.com/OSU-NLP-Group/AmpleGCG)
- AttnGCG: TMLR 2025 (github.com/UCSC-VLAA/AttnGCG-attack)
- PAIR: arXiv:2310.08419 (github.com/patrickrchao/JailbreakingLLMs)
- TAP: arXiv:2312.02119 (github.com/RICommunity/TAP)
- AutoDAN-Turbo: arXiv:2410.05295 (github.com/SaFo-Lab/AutoDAN-Turbo)
- ICON: arXiv:2601.20903 | Improved GCG: ICLR 2025 | Resurgence: arXiv:2509.00391
