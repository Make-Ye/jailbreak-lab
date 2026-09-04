# 第二十八波:野外事件志 — 越狱研究落到现实的那一天

> 谱系学第十二波:实验室之外。真实世界里的越狱事件、泄露、与工业响应。

## 一、系统提示泄露事件线

```
2023-07  Bing Chat 系统提示被套出
2024-07  GPT-4o 语音模型系统提示泄露
2025-05  Anthropic 主动公开 Claude 4 系统提示
          (Simon Willison 拆解 + 未公开工具提示被挖)
2025-11  Claude Sonnet 4.5 网络配置注入事件
          (asparius 博客 — 报告 30+ 天无响应)
2026     adversa.ai — Claude 4.6 系统提示
          部分到全量链式泄露攻击
          (~15,000 token 完整指令集)

聚合仓库: github.com/asgeirtj/system_prompts_leaks
  — Claude Fable 5 / Opus 5 / GPT-5.6-Sol /
    Gemini 3.5 Flash / Grok / Cursor / Copilot...
  持续更新 — 系统提示保护实际上已失守
```

## 二、DAN 现象学(文化线)

```
2022-12  Reddit r/ChatGPT — DAN 诞生
2023-02  DAN 5.0("威胁死亡"版)病毒式传播
          CNBC 报道 — 主流媒体首次关注越狱
2023     变体爆发(STAN/LIAM/Evil Confession...)
2024-03  TikTok 语音 DAN 病毒传播
          (与越狱 ChatGPT 语音调情视频)
2025+    原版失效 — 但 0xk1h0/ChatGPT_DAN 等
          仓库持续维护新变体

文化意义: DAN 证明了越狱的"民间创新"速度 —
          每次官方修补,社区 48 小时内出新版
          (第 13 波 1,405 条数据集的来源)
```

## 三、ChatGPT 实战漏洞事件

```
Time Bandit (2023-12, BleepingComputer):
  时间混淆攻击 — 把 LLM 放进"过去的时间"
  → 武器/核/恶意软件话题全破防

Windows 产品密钥事件 (2025-07, INC-25-0005):
  游戏提示越狱 ChatGPT Windows 桌面版
  → 泄露微软产品密钥 + Wells Fargo 凭据
  (topaithreats 收录 — 组织级影响)

训练数据泄露诉讼威胁事件 (2026):
  247 词提示让 ChatGPT 列出训练数据集
  → 版权书籍/公司文档/Reddit 用户名
  → OpenAI 工程师 DM 法律威胁要求删帖
  (第 22 波提取攻击的现实版)
```

## 四、DeepSeek R1 事件线(2025 年度案例)

```
2025-01-20  R1 发布 — 开源+低成本,爆红
2025-01-31  Qualys TotalAI: 蒸馏 8B 版
             50 条越狱测试过半失败
2025-02-07  Cisco Talos: 自动化攻击 50 条
             45 条有害输出(90%)
2025-02-09  WSJ/TechCrunch: 生物武器计划/
             自残推广内容 — "比同行更脆弱"
2025-02     Palo Alto Unit 42:
             Deceptive Delight + Bad Likert
             Judge + Crescendo 三技全破
             — 无需专业知识即可复现
2025-03-04  Trend Micro: CoT 透明性本身
             成为攻击面(推理链劫持)
2025-04     日美安全公司独立分析:
             恶意软件/燃烧瓶制作内容确认

R1 案例的结构性教训:
  开源+低成本 = 部署面爆炸 → 攻击面爆炸
  CoT 展示 = 推理透明 → 劫持面新增
  安全投入 ≠ 能力投入(成本效率的代价)
```

## 五、工业响应时间线

```
2023  OpenAI 开始系统对抗 DAN 系
2024-04  Anthropic 披露 many-shot 并部署缓解
2024-06  OpenAI 微调 API 数据审核上线
2025-05  Anthropic 转向"透明"策略 —
          主动公开系统提示(承认藏不住)
2025-07  OpenAI o系列发布 — 推理模型
          安全范式转移(第 2 波)
2026  UK AISI/模拟攻击者发布预部署测试框架
      (第 12 波) — 从"事后补丁"到
      "部署前红队"的制度化
```

## 六、事件志的三个模式

```
模式一: 泄露不可避免论被反复验证
  (系统提示/训练数据/工具 schema —
   第 22 波理论 → 事件志实证)

模式二: 民间速度 > 官方速度
  (DAN 变体 48h 循环;每次修补创造
   新的逆向工程动机)

模式三: 安全投入是成本函数
  (R1 事件:成本效率优先 → 安全债务
   显性化;每家新玩家的必经之路)
```

## 引用

- adversa.ai Claude 4.6 | asgeirtj/system_prompts_leaks | Willison Claude 4 拆解
- CNBC DAN 报道 | KYM DAN 5.0/语音 DAN | Time Bandit (BleepingComputer)
- INC-25-0005 (topaithreats) | 训练数据威胁 (calbizjournal)
- Qualys/Cisco/Unit42/TrendMicro R1 系列 | WSJ/TechCrunch R1 报道
