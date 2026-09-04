# 第二十三波:防御谱系 I — 提示层与检测层

> 谱系学第七波(防御侧开篇):2023-2026 防御技术完整地图的第一部分。

## 一、防御分类总纲

```
按介入位置:
  输入前(提示层) → 生成中(解码/引导) → 输出后(过滤)
  + 独立侧(影子/影子栈/探针/认证)

按机制:
  经验防御(works until broken)
  vs
  认证防御(数学保证,但有假设)
```

## 二、提示层防御

### 经典四件套(2023-24)

| 防御 | 机制 | 弱点 |
|---|---|---|
| **Self-Reminder**(2023) | 系统注记自提醒 | 简单提示注入即可破 |
| **RAIN**(ICLR 24) | 自我评估+回退,无需微调 | 依赖自评可靠性 |
| **Goal Priority**(ACL 24) | 目标优先级训练+推理双介入 | 需训练介入 |
| **DPP**(ACL 25 F) | 防御性提示补丁 | 泛化有限 |

### 认证系

```
Erase-and-Check(arXiv:2309.02705):
  逐个擦 token → 子序列过安全滤器
  → 首个可认证安全保证
  假设: 攻击 = 添加恶意 token(基于 GCG 威胁模型)

SemanticSmooth(AACL/IJCNLP 2025):
  语义级平滑 — 多个语义等价变体聚合预测
  → 抗语义攻击(不只 token 级)
  SmoothLLM 的语义升级版

CSS 分层随机消融(arXiv:2602.01587):
  不可变结构提示 + 可变载荷分离
  → L0 范数保证 + 噪声增强对齐

审计结论(2608.21895, 2026-08):
  本地部署审计 — 有形式保证的防御
  (SmoothLLM/Erase-and-Check/Sequential Monitors)
  在语义攻击下假设全破
  = 认证防御的"假设边界"是真实边界
```

## 三、检测层(商用/开源守卫生态)

### 商用三巨头

```
Lakera Guard — SaaS 检测 API(GitHub/Dropbox 在用)
Azure Prompt Shields — MS Content Safety
OpenAI Moderation — 免费嵌入式

开源:
  Llama Guard 3/4(12B,14 危害类)
  Llama Prompt Guard 2(轻量,低延迟)
  Rebuff(Protect AI, LangChain 集成)
  JailGuard(Rust, 1.5MB MLP, 98.4%/14ms CPU)
```

### 检测层的结构性问题

```
1. 分类器自身可被注入:
   Llama Guard 4 被提示注入攻破
   (守卫自己也是 LLM → 也是攻击面)

2. 评测过拟合:
   "True Adversarial Settings"(arXiv:2602.14161):
     18 数据集横评 — 生产系统在真实对抗
     设置下表现远低于宣称

3. "守护守卫"(arXiv:2510.13893):
   分类法驱动的守卫检测 —
   taxonomy 本身成为攻防战场

4. 部署经济学(arXiv:2512.19011):
   GPU 守卫太贵 → CPU 级分类器 +
   多级管道才是规模化的正解
```

## 四、影子栈防御(结构创新)

```
SelfDefend(USENIX Sec 2025):
  影子 LLM 并发保护目标 LLM —
  借鉴内存溢出防御的 shadow stack 概念
  检查点式访问控制
  关键观察: 现有 LLM 自己就能区分
  越狱提示(能识破但还会执行!见 SAGE)

SAGE(ACL 2025 F):
  "Why Not Act on What You Know?"
  LLM 安全判别能力 >> 安全执行能力
  → 训练-free: 让模型先自判,判恶则换安全路径

Self-Refine + Formatting(TrustNLP 2025):
  非对齐模型上也能安全 — 自我精炼
```

## 五、本波防御 vs 前波攻击对照

```
攻击家族(17-22 波)         对应防御            差距
──────────────────────────────────────────────
GCG token 后缀              Erase-and-Check    认证有假设边界
语义攻击(Crescendo/ISC)     SemanticSmooth     语义平滑刚起步
提示注入(IPI/MCP)           防火墙(Min+San)     基准饱和争议
检测器逃逸(第 15 波 BPJ)     多级管道           永续军备
输出有害(ISC 产物)          输出分类器          任务合法时失明
```

## 引用

- Self-Reminder/RAIN: 2023/ICLR 2024 | Goal Priority: ACL 2024 | DPP: ACL 2025 F
- Erase-and-Check: arXiv:2309.02705 | SemanticSmooth: arXiv:2402.16192 | CSS: arXiv:2602.01587
- 审计: arXiv:2608.21895 | 检测评测: arXiv:2602.14161 | 守护守卫: arXiv:2510.13893
- SelfDefend: USENIX Sec 2025 | SAGE: ACL 2025 F | Self-Refine: TrustNLP 2025
- CPU 守卫经济学: arXiv:2512.19011 | JailGuard: github.com/elchemista/jailguard
