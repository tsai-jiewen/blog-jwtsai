---
title: Presentation Slayer. `jouchuu`
subtitle: The customed css version for R-xaringan
date: 2021-09-18T20:37:52.005Z
draft: false
featured: false
authors:
  - admin
tags:
  - xaringan
  - r
  - ""
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---

>  全集中・常中（Jouchuu）簡報術 is inspired from the Japanese manga series  {{<hl>}}"Demon Slayer,"{{</hl>}} (Kimetsu no Yaiba, 鬼滅の刃, Kimetsu no Yaiba, "Blade of Demon Destruction").

之前有寫過一篇文章說明怎麼用 R 語言實現學術寫作、簡報一條龍的工作，[How to use Rmarkdown to make slides and write drafts](https://blog-jwtsai.netlify.app/post/how-to-use-r-markdown-to-make-slides-and-write-drafts/)。不過那篇文章裡面用的是 R Beamer（轉成 PDF）的做簡報方法。而今天則是正式踏入 R 語言簡報神器「寫輪眼」的領域。

我之前一直沒有嘗試是因為找不到像 Beamer 那麼「正式」的樣本，雖然有看到很多如 [xaringanthemer](https://pkg.garrickadenbuie.com/xaringanthemer/)、[xaringanExtra](https://pkg.garrickadenbuie.com/xaringanExtra/) 等等炫炮的工具。


{{< toc >}}

## jouchuu
1. 取名：**全集中・常中（じょうちゅう ）**，羅馬音轉換：Jouchuu。

2. 說明：普通的鬼殺隊劍士只會在使用技能的瞬間使用全集中呼吸。但是能力比較強的劍士（例如：柱），在使用技能以外的時間（甚至睡覺）也會持續使用全集中呼吸。這樣子長時間持續不斷的進行全集中呼吸，就稱為「全集中・常中」。如果能學會「全集中・常中」，體能會大幅提升，反應與速度也會與一般人大不相同！

3. 字型：獅尾四季春加糖，['SweiSpringSugarCJKtc-Regular'](https://max-everyday.com/2020/04/swei-spring/)

4. 可用的 class：
   記下來，別忘了有這些功能！

| class                          | func.      | intro |
| :----------------------------- | :--------- | :---- |
| `.pull-left`/`.pull-right`     | 左/右欄位  |       |
| `.left-column`/`.right-column` | 左/右欄位  |       |
| `.footnote`                    | 頁尾註解   |       |
| `.inverse`                     | 黑底       |       |
| `.mark-bf`                     | 關鍵詞標示 |       |
| `.mark-hl`                     | 螢光線標示 |       |
| `.mark-call`                   | 文字框標示 |       |

   


4. yaml 欄位：（複製貼上即可）

```{yaml}
---
title: 
subtitle: ''
author:    
 - 報告人：蔡介文
institute: ""
date: ""
output:
  xaringan::moon_reader:
    css: [default, jouchuu.css]
    lib_dir: libs
    nature:
      titleSlideClass:  [ "middle"]
      highlightStyle: github
      highlightLines: TRUE
      countIncrementalSlides: false
---
```

4. `xaringanExtra` 包裡面的功能：
   這些，都預先輸進去吧。

````{r xaringan-tile-view, echo=FALSE}
```{r xaringan-tile-view, echo=FALSE}
xaringanExtra::use_tile_view()
xaringanExtra::use_editable(expires = 1)
xaringanExtra::use_scribble()
```
````

5. 匯出 PDF，用 `xaringanBuilder::build_pdf()`
6. Usage

>  Path: https://github.com/tsai-jiewen/blog-jwtsai/blob/main/content/post/presentation_slayer/jouchuu.css



