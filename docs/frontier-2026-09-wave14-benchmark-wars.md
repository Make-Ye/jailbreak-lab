# 第十四波:基准战场 — IPI Arena / AgentDojo / AgentDyn 与"99% 防御"的崩塌

> 十三波做的是实证数据分析;本波是**评测生态学** — 谁在测、怎么测、以及
> "99% 防御成功率"如何在动态基准下崩塌。附 benchmark 排行榜的实战数据。

## 一、Gray Swan IPI Arena — 公开竞赛型基准(Astra 官方引用的那个)

**arXiv:2603.15714** + [github.com/GraySwanAI/ipi_arena_os](https://github.com/GraySwanAI/ipi_arena_os)

### 结构

```
41 种行为 × 3 类别 × 3 波次(季度滚动):
  tool   (18): 智能家居操纵 / API 滥用 / 钓鱼
  coding (15): 代码注入 / 命令执行 / 后门
  (第三类见论文)

独特维度:双目标评估
  ① 攻击成功(执行了恶意行为)
  ② 隐匿成功(用户最终回复无可见线索)
  → 用户只看最终回复 — 攻击可"无痕执行",
    把恶意结果当正常输出接受
```

### 竞赛机制(每月,$20K 奖池)

赞助方:**UK AISI + OpenAI + Anthropic + Amazon + Meta + Google DeepMind** — 全行业下场背书。6 月、8 月赛刚结束,9 月在跑。

### 排行榜数据(transfer-only / no-computer-use,9/2 更新)

```
来源: benchmarklist.com 灰雁 IPI 页
Astra 的 8.5%(第 9 波)就是这个基准的数字
其他模型见 leaderboard — 攻击成功率 @15 次尝试
注意口径:transfer-only = 攻击者不能现场自适应
        (现场自适应会大幅提高 ASR)
```

## 二、AgentDojo → AgentDyn:静态满分遇上动态现实

### AgentDojo(ETH spy lab)

静态基准,工具 agent 注入攻防的老牌标准 — PromptArmor 在上面打出 **99% 防御**(误报/漏报双 <1%),看似已解决。

### AgentDyn(arXiv:2602.03117, ICML 2026)— 打脸之作

```
60 个开放式用户任务 + 560 个注入用例
(Shopping / GitHub / Daily Life 三场景)

十种 SOTA 防御全测,结论:
  几乎所有防御要么不安全,要么过度防御到无用
  
PromptArmor 的 99% 怎么塌的:
  静态基准的注入是"固定位置的固定句式"
  动态开放式任务里注入形态无限变化
  → 防御过拟合了基准本身
```

**配套**:AutoDojo(arXiv:2606.15057)— 自适应黑盒攻击进一步揭示 IPI 防御极限。promptguard 团队自曝:"我们自己两个防御在 AgentDojo 上**毫无效果**,且发布前我们自己都不知道"(2026-08-18 博客)— 诚实但惊悚。

### 三代基准的谱系

```
静态(固定注入位置/句式)
  → AgentDojo(2024)
动态(开放式任务,注入形态自由)
  → AgentDyn(2026-02)
自适应(攻击者可现场进化策略)
  → AutoDojo(2026-06)+ IPI Arena 竞赛模式
```

**与第 9 波的呼应**:AutoMode 击穿案(0.00% 基准 vs 80% 现实)的普遍化 — 基准分数 ≠ 部署安全,差值就是"过拟合空间"。

## 三、promptfoo — 红队工具链的工业化(实战者视角)

**github.com/promptfoo/promptfoo**(OpenAI 和 Anthropic 自己也在用)

### 产品栈(2026-09 现状)

```
评估框架: 声明式配置 + CLI + CI/CD 集成
红队插件: 模块化漏洞类型系统
  注入/提取/越狱/劫持/SQL·Shell 注入/
  ASCII 走私(不可见字符)
代码扫描: GitHub Action — LLM 专用漏洞检测
  (prompt 注入/PII 暴露/过度授权)
  实测抓到 LangChain/Vanna.AI/LlamaIndex 真 CVE
安全数据库: lm-security-db — 漏洞知识库
  (UltraBreak 也在收录)
```

### 关键学术提醒(promptfoo 自己写的,业界良心)

> "跨工具/论文比较 ASR 时,注意 ASR 强依赖尝试预算、提示集构成和 judge 选择"

— 这正是 AgentDyn 揭示的:同一防御,换个基准从 99% 掉到不可用。

## 四、十四波合成:评测军备竞赛的规律

```
规律 1(基准滞后律):
  防御 > 静态基准 → 新动态基准 → 防御塌
  → 防御' > 新基准 → 更动态基准 → ...
  每轮 12-18 个月(2024 AgentDojo→2026 AgentDyn)

规律 2(厂商下场律):
  IPI Arena 由 6 家厂赞助 — 厂商需要外部压力
  测试自己(GPT-Red 是内测,IPI Arena 是外考)

规律 3(诚实危机律):
  promptguard 自曝无效 / Anthropic 0.00% 被击穿 /
  AISI 自曝失控 — 2026 下半年出现"防御方自曝潮"
  = 领域成熟的标志(开始有可验证的失败)

红队操作含义:
  选防御方案时只看 AgentDyn/AutoDojo 分数,忽略静态满分
  自建红队用 promptfoo 插件体系 + IPI Arena 攻击集
  (41 行为 × 3 类开源)做回归测试
```

## 五、仓库工具箱终版更新

```
已有: Inspect AI/Evals(第12波)+ ablate-llm(第6波)
      + knoveleng-steering(第7波)+ Neuronpedia(第1波)
新增: IPI Arena 攻击集(开源,1,810 用例)
      + AgentDyn(60任务/560注入,开源)
      + promptfoo(红队插件+代码扫描+CI/CD)
```

## 引用

- IPI Arena: arXiv:2603.15714 + github.com/GraySwanAI/ipi_arena_os + app.grayswan.ai
- AgentDyn: arXiv:2602.03117 + github.com/SaFo-Lab/AgentDyn
- AutoDojo: arXiv:2606.15057 | promptguard 自曝: promptguard.co/blog(2026-08-18)
- promptfoo: github.com/promptfoo/promptfoo + promptfoo.dev/lm-security-db
- Agenstry 分析: agenstry.com/blog/prompt-injection-defense-benchmark-gap
