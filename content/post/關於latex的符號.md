---
title: 'A tool: LatexToMathML (zh-tw)'
authors: 
  - admin
subtitle: 關於 LaTex 的一些符號設置跟轉貼上 MS Word 的幾個驚喜
date: 2021-08-03
tags:
  - HackMD
  - zh-tw
categories:
  - LaTeX
---

[![hackmd-github-sync-badge](https://hackmd.io/_WzY5zURTaady2-8mobKGA/badge)](https://hackmd.io/_WzY5zURTaady2-8mobKGA)

{{< toc >}}

今天要分享的是 LaTeX。

我在大三暑假與大四一整年（大約是 2016 年 7 月到 2017 年 6 月底那段時間），都在 [均一教育平台](https://www.junyiacademy.org) （那時候的均一也不完全是現在的均一）當實習生，收穫當然很多，但最實用的收穫卻是學會 LaTeX 跟連帶的啟蒙 Markdown。

坦白講，對於一個文組而言，LaTeX 也沒什麼必要。文組論文裡面的公式其實也只是擺設而已，辛苦一點用 MS Word 點幾下也 OK 的。

但是開始學習測量理論之後，需要寫公式的時間真的變多了。

今天寫這篇文是因為我打一條公式竟然觸發了 3 個值得紀錄的點，趕緊記錄下來！

這條公式是（多向度）Rasch 模型的通式 (multidimensional random coefficients multi- nomial logit model, MRCMLM) (Adams, Wilson, & Wang, 1997) 

$$P(X_{ik}=1; \bf{A}, \bf{B}, \xi | \pmb{\theta}) = \dfrac{\exp( \bf{b_{ik}}\pmb{\theta} + \bf{a'_{ik}}\xi )}{\sum\limits_{k=1}^{K_i}\exp( \bf{b_{ik}}\pmb{\theta} + \bf{a'_{ik}}\xi )}$$

```
P(X_{ik}=1; \bf{A}, \bf{B}, \xi | \pmb{\theta}) = \dfrac{\exp( \bf{b_{ik}}\pmb{\theta} + \bf{a'_{ik}}\xi )}
{\sum\limits_{k=1}^{K_i}\exp( \bf{b_{ik}}\pmb{\theta} + \bf{a'_{ik}}\xi )}
```

## LaTeX 公式轉 MS Word 公式的網頁工具！

第一個要記的是這個網站：

- [LaTeX 公式轉 Word](http://web.xiaoyv.top/web/LatexToMathML/#home)

It is too tired to use MS Word to make a formula like the one above. But at the moment (2020), I tried with RStudio or Typora to output MS Word, and then both can't display the formula smoothly. So using this site, you can copy and paste with one click after typing LaTeX, which is a good tool for typing formulas!

## 一些常用 LaTeX（我怕忘記）

- The first question is how to bold the Greek letters? The answer is to use `\pmb{}`!
- The second is how to keep the in-line `\sum` superscript on display mode ( $\sum\limits_{k=1}^{K_i}$ ) instead of the in-line mode ( $\sum_{k=1}^{K_i}$ )? The answer is to add a `\limits` after `\sum`, which can work!


## Re:
