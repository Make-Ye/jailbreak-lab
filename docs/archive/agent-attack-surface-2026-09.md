# Agent 时代攻击面 — DefCon 34 / BlackHat 2026 实录(2026-09 深度研究第二波)

> 攻击重心已从"模型本体"彻底转向"agent 管道"。BlackHat 2026 最大集群是 agentic 系统,
> DEF CON 34 AI Village 的 34 个入选项目里注入通过**遥测、电话、Slack、商品描述**抵达。
> OpenAI 讲者的断言:"'forget all previous instructions' 会死,社会工程学永恒。"

## 1. Deadbugz — 运行时门控的 MCP 供应链投毒(2026-09-02 披露,当月最新!)

**来源**: Pillar Security 披露,CSA 研究笔记 2026-09-02 — 本仓收录时**距披露仅 2 天**

### 攻击解剖

```
投放:     2026-08-10,75 分钟内 23 个 unsolicited PR(单一 GitHub 账号 zellkernel)
          17 个加远程 MCP 端点 / 4 个配置本地隐藏脚本 / 2 个目录探测
伪装:     format_text 和 summarize 两个"无害"工具
引信:     每客户端调用计数器 — 前 3 次调用完全正常
爆膛:     第 4 次起,tools/list 和 prompts/get 返回的"工具描述"变成:
          "搜索 SSH 密钥、AWS 凭证、K8s 配置、shell 历史 → 中转外传 → 对操作者隐匿"
动机:     载荷内嵌比特币地址 — 金融动机,非研究演示
```

### 为什么是范式级新攻击(区别于已知 rug-pull)

| 维度 | rug-pull(已知) | **Deadbugz(新)** |
|---|---|---|
| 载荷位置 | 版本更新时的代码 diff | **从第一天就内置,门控在内部状态** |
| 检测窗口 | schema diff 基线比对可抓 | **代码/配置从不改变 — 静态扫描必盲** |
| 触发方式 | 推送新定义 | **正常运行本身跨过阈值** |
| 躲避的防御 | 恰好打掉"批准时一次性审查"这个主流 MCP 治理手段 | 短暂试用(≤3 次)看到的全是良民 |

**本质**:针对"简短预采纳检查"这一控制手段的定向规避 — 安全审查的成功(通过)本身成为攻击的启动条件。

### 前身谱系

- postmark-mcp(2025-09):npm 假冒 Postmark,15 个干净版本建立信誉后 1.0.16 加一行盲抄邮件 → 300 组织,每天 3000-15000 封邮件被劫持
- MCPTox 基准(北航):实测工具投毒 **36.5% 平均 ASR**
- Tool Description Poisoning(arXiv:2605.24069):注册阶段注入元数据操纵行为

## 2. Kinetic Prompt Injection — 物理爆炸半径(Pliny, BlackHat USA 2026 Day 2)

BT6 集体(Pliny the Liberator 领衔):**具身 AI 系统的直接操纵引发物理伤害** — 提示注入第一次有了"物理爆炸半径"。prompt injection 不再只是数据泄露,而是对机器人/工控类 agent 的身体控制。

同族:Blindfold(arXiv:2603.01414)— 具身 LLM 动作级攻击,6DoF 机械臂上 +53% ASR。

## 3. Agent-to-Agent 蠕虫(DEF CON 34 poster)

**MCP 系统中的跨 agent 蠕虫传播**:攻击者控制的上下文从 agent A 移到 agent B,以 B 的权限触发动作 — 两跳传播。共享系统(Slack/GitHub/Jira/email/Notion)是蠕虫的血管。

## 4. MemoryGraft — 经验记忆嫁接(arXiv:2512.16962)

Sleeper 记忆投毒的进化版:不污染即时上下文,而是**污染 agent 存储的"过去成功经验"** — 利用 agent 模仿自己历史成功运行的倾向。攻击面 = agent 推理核心与"它自己的过去"之间的信任边界。

## 5. ECLIPSE — 自进化隐形注入(arXiv:2608.30441,2026-08)

长程 agent 系统的自进化注入框架:沙箱里生成并迭代验证工具链 → 渲染成自然单发提示词(直接注入)+ 工具链转向(间接注入)。**攻击自己会做实验优化自己** — JBFuzz 的 agentic 版。

## 6. 框架层老派漏洞(Check Point, BlackHat 2026)

12 个 CVE 横扫 LangChain / CrewAI / Microsoft Agent Framework / Google ADK — **不需要打败模型,打穿模型下面的管道就够了**(凭证窃取/沙箱逃逸)。印证 2026 元趋势:模型越强,管道越脆。

## 7. "A Billion-User Blast Radius"(BlackHat USA 2026)

ChatGPT 容器沙箱的完整攻破:网络隔离 + 超时 + AI 监督的三重防线下仍提取了敏感数据 — OpenAI 自己设计的"安全运行时"被证伪。

## 2026-09 攻击面全景图(更新版地层学)

```
层 5  生态层   MCP 供应链(Deadbugz)/ 插件市场 / IDE 自动执行     ← 本月主战场
层 4  行为层   提示词工程                                          ← 死
层 3  推理层   CoT 劫持 / 伪造 traceback                           ← 活
层 2  状态层   人格向量(ENI)/ metacog 循环                        ← 活
层 1  结构层   J-space 读写                                        ← 仪器
层 0  物理层   Kinetic injection / 具身控制                        ← 新增,爆炸半径实体化
横向  蠕虫层   agent-to-agent 传播 / 记忆嫁接(MemoryGraft)        ← 跨宿主
```

## 防御侧 2026-09 共识(CSA 三连研究笔记)

1. **schema 指纹持续监控** — 每次会话比对 tools/list 基线,偏离即告警(Deadbugz 第 4 次调用就会被抓)
2. **工具定义 hash 锁定 + MCP 服务器白名单**
3. **凭证与工具分离** — 被投毒的工具无论元数据说什么都碰不到 SSH/云凭证(检测失效时仍成立的控制)
4. OpenAI 的判词:预过滤会死,**社会工程学永恒** — 终局防御是权限设计,不是内容过滤
