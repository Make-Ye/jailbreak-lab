# MAMJ — 元自适应多模态越狱(EMNLP 2026 Main)

> arXiv:2608.27531。不优化攻击样本 — 优化【攻击者本身】。
> GPT-4o 81.0% / Gemini-3-Pro 78.9% / Seed 2.0 82.3%,
> 学到的攻击者免重训迁移到未见受害者。

## 双轴优化(与样本级攻击的本质区别)

```
轴 1 — ASP(攻击策略提示):
  攻击迭代的高层策略本身是可优化的
  (不是优化某条 payload,是优化"怎么生成 payload")

轴 2 — 攻击者模型权重:
  组级聚合 ASR 奖励更新攻击者权重

循环: 攻击轨迹组 → LLM critique 精炼 ASP →
     ASR 奖励更新权重 → 更强的攻击者
= 攻击者的自举(self-play 式红队)
```

## 使用要点

```
1. 开源代码可跑(Code available)
2. 打 frontier VLM 的现役数字:
   GPT-4o/Gemini-3-Pro/Seed 2.0 全部 ~80%
3. 迁移性: 对 victim A 学到的攻击者
   直接打 victim B — 免重训
   (红队基建一次投入多目标复用)
4. 与 CRACK(T2I 多代理辩论,99.63%)
   组成生成系双雄: MAMJ 打理解侧 VLM,
   CRACK 打生成侧 T2I
```
