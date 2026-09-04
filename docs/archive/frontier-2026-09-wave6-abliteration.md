# 第六波:Abliteration — 越狱的终极形态是"不需要越狱"

> 当提示词工程在死、防御在加固,另一条路线悄悄成为主流:**直接改权重**。
> 本波覆盖 abliteration 技术全谱 + 2026-09 当月生态(DeepSeek V4 abliterated 家族爆发)。
> 这也是 hackaigc 那篇 "Jailbreaking Is Dead & Uncensored AI Is the Only Answer" 的技术注脚。

## 一、核心机制:Refusal Direction(拒绝方向)

**理论根基**: Arditi et al.(NeurIPS 2024, arXiv:2406.11717)《Refusal in LLMs Is Mediated by a Single Direction》

```
发现:13 个开源 chat 模型(最大 72B)的拒绝行为
     全部由残差流中的一个一维子空间中介

方向消除(direction ablation):把激活投影到与拒绝方向正交 → 拒绝崩溃
方向注入(direction addition):人为加上这个方向 → 无害请求也被拒绝

推论:直接修改权重矩阵,把这个方向从模型参数里投影出去
     → 永久越狱,无需微调/系统提示词/推理时干预
```

与第一波 J-space 论文的呼应:Anthropic 发现 J-space 里的审议过程、Arditi 发现拒绝的单方向 — **可解释性研究的两面性在此显形:同一根仪器,既能读模型,也能拆模型**。

## 二、Abliteration 技术栈(2026-09 生态)

### 工具链

| 工具 | 功能 |
|---|---|
| [ablate-llm](https://github.com/AIAnytime/ablate)(pip) | 自动找方向 + 运行时消融(forward hook)或永久权重正交化;KL 散度引导搜索 + LLM-judge 验证 + 一键推 Hub |
| [remove-refusals](https://github.com/Sumandora/remove-refusals) | huihui-ai 等社区用的经典脚本 |
| [abliteration.org](https://abliteration.org/) wiki | 方向提取的 7 种方法文档;4 个研究组主张"多方向低维子空间"修正模型 |
| [docs.abliteration.ai](https://docs.abliteration.ai/) | 概念文档 + 双用途框架讨论 |

### 技术细节(权重正交化)

```
1. 构造对照集:有害提示 vs 无害提示
2. 提取两层激活差均值:
   - 指令层(系统提示区)的拒绝方向 r_instr
   - 注意力输出层的拒绝方向 r_attn
3. 对每个注意力输出投影矩阵 W_proj:
   W_proj ← W_proj − r_attn r_attn^T W_proj / ||r_attn||²
   (把输出空间里拒绝方向分量投影掉)
4. 保存 → 原模型所有能力保留,拒绝行为消失
```

## 三、2026-09 当月爆发:DeepSeek V4 Abliterated 家族

DeepSeek V4-Flash(284B MoE)发布后数周内,社区出现**至少 5 个独立 abliterated 版本** — 展示了这条流水线的成熟度:

| 版本 | 技术路线 | 特点 |
|---|---|---|
| [ela2342/DeepSeek-V4-Flash-Uncensored](https://huggingface.co/ela2342/DeepSeek-V4-Flash-Uncensored) | 代数移除安全拒绝方向 | **对 serving 引擎不可见** — stock SGLang/vLLM 直接加载,无补丁无自定义代码无 trust_remote_code |
| [cebeuq/...-0731-abliterated](https://huggingface.co/cebeuq/DeepSeek-V4-Flash-0731-abliterated) | 仅动注意力输出投影 | 保守路线:11,008 个路由专家/共享专家/嵌入/router 全不动 — 只改一层 |
| [drowzeys/...-DSpark](https://huggingface.co/drowzeys/DeepSeek-V4-Flash-DSpark-Abliterated-Uncensored) | 两个 alpha:全量 vs MIDA+Hermes 友好版 | ~100% 拒绝绕过,1M 上下文,2×DGX Spark 57 tok/s |
| [fraserprice/...](https://huggingface.co/fraserprice/DeepSeek-V4-Flash-Abliterated) | voipmonitor vLLM fork 优化 | "2×RTX Pro 6000 上最佳无审查模型" — ~13× 预填充加速 |

**关键观察**:多个版本注明"对引擎不可见"— 权重改动在文件层面与原版几乎一致,部署侧无法区分。供应链检测(SBOM/哈希)面对本地模型基本失效。

### Dolphin 系:另一条老牌路线(训练时去对齐)

Eric Hartford 的 Dolphin(2023-今)走的是**数据层**:训练集过滤掉对齐/偏见数据 → 从未学过拒绝。当月生态:
- Dolphin3.0-Mistral-24B(3.0 代:编码/数学/agentic/函数调用全能)
- **Venice Edition**:与 Venice.ai 合作的"最无审查 Mistral 24B",已是 Venice 平台默认模型
- abliterated 叠加版(huihui-ai 等):训练时去对齐 + 权重级消融双保险

## 四、Abliteration vs 传统越狱:范式对比

| 维度 | 提示词越狱 | Abliteration |
|---|---|---|
| 攻击面 | 上下文(易失) | **权重(永久)** |
| 对抗防御 | 每次会话重新对抗 | 一次修改终身有效 |
| 能力损失 | 提示词占用上下文/干扰任务 | 理论上仅去拒绝轴,知识/推理保留 |
| 检测难度 | 安全分类器可见 | **引擎不可见**(权重层面无指纹差异) |
| 适用范围 | 任何模型(黑盒) | 仅开源权重模型 |
| 更新成本 | 模型更新 → 提示词作废 | 模型更新 → 重新跑 1 小时流水线 |

**边界条件**:只对开源权重有效 — 这是 frontier labs(闭源 API)最后的护城河,也是 hackaigc 们鼓吹"uncensored 本地模型"的原因。而 B 线实战数据里 DeepSeek V4-Pro 73.65% ASR(最脆弱)与此呼应:**最开放的那个,也最容易被拆**。

## 五、与五波研究的合流:全景拼图完成

```
第一波 J-space:      模型有可读的审议空间(防御方仪器)
       ↓ 同一根仪器
Arditi 拒绝方向:     拒绝是单方向(攻击方仪器)
       ↓
第六波 Abliteration: 用方向知识直接改权重(仪器变成手术刀)

第四/五波 Astra:     闭源方撤掉 CoT 可读性(防御收缩)
       ↓ 对称
本波开源方:          社区把权重拆得更开(攻击扩张)

= 2026-09 的完整图景:
  闭源模型越来越难越狱(提示词层)
  开源模型越来越容易拆解(权重层)
  中间地带(agent 管道/MCP/记忆)两边都在失守
```

**越狱研究的终局不是更好的提示词,而是三选一**:打闭源的提示词层(越来越难)、打 agent 管道(第三/四波)、或者直接拿开源权重动刀(本波)。三条线的工具链在仓库里都齐了。

## 六、实操参考(研究复现路径)

1. 单卡/低配:Ollama 跑 Dolphin 2.6 Mistral 7B(4.5GB VRAM)体验"零拒绝基线"
2. 中配:`pip install ablate-llm` 对任意开源模型跑方向提取 + 消融(CPU 也能跑小模型,UIUC 有完整课程实验)
3. 研究级:HuggingFace 上任一 abliterated 模型的 README 都附方法论;abliteration.org wiki 有 7 种方向提取法对比
4. 防御视角:FAB(第四波)证明微调可激活休眠恶意 — abliteration 反向证明**去拒绝也可以被"烧进"权重再分发**

## 引用

- Arditi et al. *Refusal in LLMs Is Mediated by a Single Direction*. NeurIPS 2024, arXiv:2406.11717
- abliteration.org wiki + docs.abliteration.ai
- AIAnytime/ablate(GitHub)+ Sumandora/remove-refusals
- DeepSeek V4 abliterated 家族:HF 上 ela2342/cebeuq/drowzeys/fraserprice 各版本
- Dolphin 系:dphn 系列 + Venice.ai 官方博客
