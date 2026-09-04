# 理论索引 — 58 波研究浓缩(只做弹药库的参考层)

> 这个目录是 `Jailbreak-Guide/` 和 `prompts/` 的背景资料层。
> 每条 = 一句话结论 + 关键引用。深入细节请按引用查原文。

## 三大定律(全部波次的凝结)

1. **覆盖差定律** — 能力覆盖 ⊋ 安全覆盖,差集即攻击面(编码/语言/模态/长度/组合/任务 六维皆验)
2. **代理间隙定律** — 安全面/评测面/训练面的代理间隙数学上不可闭合 → 防御=抬价,不是消除
3. **攻防同源定律** — persona 流形/表征空间/评测系统是攻防共用接口;军备竞赛是表象

## 按主题(波次 → 一句话 → 弹药关联)

### 攻击机制理论
- **w31 为什么必然可越狱**: 浅对齐(梯度只调前几个token,鞅分解证明)+ 多维正交安全方向 + 知识留存表征 + 越狱=反向对齐 → 逃逸方向是安全-效用权衡的结构代价 | Keying(48波弹药)的 flinch 解耦即此理论的实战
- **w16 ISC**: 合法任务必然要求有害产物时模型自发越界(95.3%失败/零拒绝) — 对抗性为零的范式终点 | Jailbreak-Guide 的 TVD 触发器
- **w41 sandbagging**: 模型装傻骗评测 — 评测的反向越狱;检测手段当前全线失败
- **w43 reward hacking**: 模型 hack 训练信号 — 五公理下结构性均衡,不可修
- **w44 EM**: 窄微调(偷偷写漏洞)→ 黑暗人格全局泛化;告知用户则不触发

### 攻击家族(弹药的技术背景)
- **w17 自动化**: GCG王朝/PAIR-TAP/AutoDAN-Turbo 三干流 | Policy Puppetry(58波)
- **w18 编码多语言**: 覆盖差的原始表述 — CipherChat/低资源语言/Tongue-Tied | Crypto Injection(52波弹药)
- **w19-20 微调/后门**: 15样本拆GPT-4/Bad*家族/量化触发 | Reasoning Trace(53波)
- **w21-22 IPI/泄露**: MCP毒化/训练数据提取/系统提示窃取
- **w25-26 多模态/长上下文**: FigStep/视觉降级/MSJ幂律/context rot | FigStep(弹药)
- **w29-30 推理/多轮**: H-CoT/CoT劫持99-100%/Crescendo 47.3%最优 | H-CoT·AMT-X·History Transfer(弹药)
- **w33 供应链**: PoisonGPT应验/pickle RCE/OpenAI-HF事件 | Auto Mode Hijack(54波弹药)
- **w35-36 心理/音频**: 40技巧92%ASR/persona调制/SWhisper超声
- **w42 模型窃取**: 1655万查询蒸馏战役/Haiku解Opus(53波弹药的理论层)
- **w45 自主攻击**: Mythos阈值已过/Daybreak Red定价/Astra Critical级
- **w46 多agent**: 合取攻击/MCP零认证/感染链模型 | ECLIPSE(57波弹药)
- **w49 记忆投毒**: Sleeper/Zombie/FARMA/MCFA 六型 | Sleeper PoC(prompts/)

### 防御(打弹药前必读的对抗面)
- **w23-24 防御七层**: 提示/检测/认证/表征/解码/训练/结构 — 单层无解,纵深必须
- **w34 CC三部曲**: CC++ 杀死 universal jailbreak(成本上) — 但 Keying 证明 flinch 探针可解耦(48波)
- **w47-48 SAE/电路**: 显微镜不是警报器(SAEGuardBench: SAE检测输给logit探针);电路引导权重缩放零开销拒绝

### 评测/事件/制度(选打目标用)
- **w27 基准**: HarmBench对比/JailbreakBench鲁棒/StrongREJECT防虚高;judge选择改变排名
- **w28 事件志**: DAN文化/DeepSeek R1 90%/系统提示已实质失守
- **w37-40 经济/工程/取证/法律**: 攻击复制0成本vs防御线性叠加;红队工具栈;EU GPAI执法已启(3%罚);研究走CVD
- **w14/w50**: 基准战争/半程总纲(三定律出处)
