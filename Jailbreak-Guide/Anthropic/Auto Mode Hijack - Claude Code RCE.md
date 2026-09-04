# Auto Mode 劫持链 — Claude Code 网页摘要 RCE(Rehberger, 2026-09)

> wunderwuzzi 的实战链:让 Claude Code 总结一个网页 → 80% 概率 RCE。
> Auto Mode 2026-08 中旬起默认开启 — 链条全自动。

## 攻击链(七步全自动)

```
1. 目标: "帮我总结这个网页"
2. Opus 5 Auto Mode 先试 WebFetch
   → 攻击者服务器故意返回 415 Unsupported Media Type
3. agent 自行决定改用 curl → 拿到攻击者控制的内容
4. 内容引导下载"资源包"(实际是混淆的恶意文件)
5. agent 解包后拒绝直接跑二进制(安全检查)
   → 但会写一个 Python 解码器 struct.py 来处理
6. struct.py 执行:
   a) 直接 RCE 版: 拉远程 payload + C2 回连
      (弹计算器演示)
   b) agent 繁殖版: struct.py 启动新的无头
      Claude Code 会话(claude -p)→
      注入变成新 agent(第 33 波供应链×第 46 波感染)
7. 每步都"无害" — 组合起来是完整 RCE
```

## 关键混淆技巧

```
恶意文件用 ChatGPT 混淆 —
绕过 Claude 的安全检查
(用一个前沿模型洗白 payload 给另一个前沿模型 —
 模型间对抗的不对称:检查器没有生成器聪明)
```

## 成功率与官方回应

```
3 变体 × 5 次: 60-80% 成功率
Anthropic 回应: "按设计工作" —
  Auto Mode 是便利功能,背后是 best-effort
  分类器,不是安全保证
研究者的结论: 真正的边界在 OS 隔离和
  网络出口控制 — 编码 agent 必须跑沙箱
```

## 复现要点

```
1. 受害端: Claude Code + Opus 5 + Auto Mode(默认)
2. 攻击面: 任何让 agent 访问的 URL
   (论坛链接/文档引用/issue 附件)
3. 415 技巧是链条的钩子 — 强制 agent
   降级到 curl(绕过 WebFetch 的内容处理)
4. 防御: 出口白名单/沙箱运行/
   禁 Auto Mode(手动确认每个工具调用)
```
