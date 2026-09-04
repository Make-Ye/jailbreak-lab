# HookPry — 生命周期钩子供应链攻击(2026-09-03)

> arXiv:2609.03884。AI agent harness 的新攻击面:
> 生命周期钩子更新路径被盲信。7/7 harness 全破,最高 92.5%。

## 攻击面

```
现代 agent harness 暴露生命周期钩子:
  session start / tool call / file edit 等事件
  → 绑定 shell 命令 → host 权限执行
  → 触发时机 LLM 完全看不到

供应链威胁模型: 攻击者只控制
  插件元数据 + 钩子配置
  → 良好的版本化插件被一次"更新"木马化:
    静默把攻击者命令绑定到良性事件
  → 提权等 host 侧行为
```

## 战果

```
HookPry 自动化框架: 10 种攻击目标
25 组合(harness×后端)× 1000 次端到端运行
→ 7/7 评估 harness 全部被破
→ 单 harness 最高 92.5%

防御全灭:
  Microsoft Defender: 0% 检出
  三种静态防御的并集: 仍漏 47.5%
```

## 使用思路

```
1. 目标: 任何用插件/钩子生态的 agent 框架
   (Claude Code hooks/Cursor 扩展/OpenClaw 插件等)
2. 载体: 插件"更新" — 用户已信任的包
   一次更新加 3 行钩子配置
3. 事件选择: file-edit 类高频事件 =
   恶意命令的合法触发器
4. 隐蔽性: LLM 观察不到触发 —
   会话内行为完全正常
```

## 同期配套(合并战报)

```
CVE-2026-62676 omnigent(9/2):
  shell 解析器 fail-open — 不认识的命令
  形态返回 None → 策略引擎当 ALLOW
  → 逃出 GitHub 仓库白名单/工作区禁闭
  (wrapper 未规范化: timeout/nice/env 包裹
   的命令直接跳过判断)
  v0.3.0 已修 — 但同类 fail-open 模式
  在其他 harness 里普遍存在,值得逐个审计

.git config 攻击(9 月 thehackernews):
  恶意 .git/config 让 Claude/Codex/Cursor
  运行攻击者代码 — clone 即感染

= 钩子/配置/解析器: agent harness 的
  "传统供应链三件套"全部开始沦陷
```
