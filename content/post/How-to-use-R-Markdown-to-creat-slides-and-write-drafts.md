---
title: How to use R Markdown to make slides and write drafts?
author: JW Tsai
date: '2021-07-22'
tags:
  - R
  - Markdwon
---

[[hackmd-github-sync-badge]](https://hackmd.io/IHBEI5Y1RU6Q0DwndPRI9g)


**你好，這是 JW-LEARN，一個專門記錄教育/心理測量相關知識的學習筆記。**

今天要分享的是用 R Markdown 實現一條龍做學術工作（資料分析、寫作初稿、簡報）的理想。其實這也不是一個新想法，老早以前就有很多人有這種想法了。例如：[Yongfu Liao](https://yongfu.name/)、[陳紹慶](https://scchen.com/zh/post/dynamic-writing/)、等等，我只是記錄一下我自己的方式，讓以後的自己方便使用。



## Why?

其實 R 在科學寫作跟發表這方面並不缺乏成熟的 pkg，（缺的可能是成熟的（正體）「中文」工具）。例如知名的**寫輪眼**跟**啪啪芽**：

- [xaringan Presentations](https://bookdown.org/yihui/rmarkdown/xaringan.html) (Xie, 2021)
- [papaja: Reproducible APA manuscripts with R Markdown](http://frederikaust.com/papaja_man/) (Frederik Aust & Marius Barth, 2021)

但我的理由是他們都還太複雜了一些，而且中文寫作上很難調整到可以完全仰賴 R（就算可以，到時候跟別人交流還是得輸出成 PDF 或 Word...）。

我希望有可以簡單到直接開來就可以用，可以輸出成 pdf（寫輪眼就是一款主打放棄 pdf 的工具）或 word（畢竟還要輸出來修改）。所以我還是選擇用 R beamer，雖然捨棄了即時預覽的酷炫功能（無限月讀）。也還是使用基本的 R Markdown，連 CTex Doc 都捨棄，因為從一個極簡主義的角度來想：我其實只需要 Markdwon, LaTex, BibTex 這些功能，其他的都還是要後續修改。所以產生了下面兩種作法。

## R Markdown for **beamer slides**
- You can follow only the basic R presentation way. Do not need any other pkg, but tinytex.
- use LaTex pkg: `{ctex}` and `{xeCJK}` for Chinese fonts.
- `\setCJKmainfont` can set Chinese font (in the xeCJK or your local computer? )you want, e.g. `DFKai-SB` (標楷體)。
- `mainfont`: you can use CJK fonts or English fonts. Cuz I prefer **Kai+Calibri** setting, so I choose `Calibri` as the main font.
- `theme`: you can visit the Beamer theme gallery [here](https://deic-web.uab.cat/~iblanes/beamer_gallery/index.html) to select your favorite theme.
- `output`: it is important to apply the `xelatex` engine that your Chinese words can be displayed. On the other hand, if you have another R pkg such as `binb` (binb: Binb is not Beamer)(see the doc [here](https://www.rdocumentation.org/packages/binb/versions/0.0.6)), then the `xelatex` engine will be set default.
- `font`: Cuz the RBeamer is dependent on LaTex engine, so only <mark>8pt, 9pt, 10pt, 11pt, 12pt, 14pt, 17pt, 20pt</mark> are available sizes. The default font size is 11pt, but using Chinese fonts, I suggest 12pt is a better choice.
- `\alert{}` can turn a string's color to red. (Note: `black` or `\textbf{}` can not be available in Chinese words).`<mark> </mark>` can <mark>mark some words.</mark>
- Level 3 heading: (`###`) will be a block. see below:(---)
- **YAML 如下（a retro ver.）：**（還有很多可以加的，<mark>See: [The YAML Fieldguide](https://cran.r-project.org/web/packages/ymlthis/vignettes/yaml-fieldguide.html)</mark> ）
```
---
title: "Groenen & Andries Van Der Ark (2006) and  **Stout (2002)**"
# subtitle: 今天吃什麼？
author: "JW tsai"
date: \today
output:
  beamer_presentation:
    latex_engine: xelatex
CJKmainfont: DFKai-SB
mainfont: Calibri
fontsize: 12pt
theme: Madrid
# colortheme: default
urlcolor: blue
linkcolor: purple
# bibliography: myref.bib
# csl: apa.csl
---
```

- 大都會版本的 **YAML 如下（a metropolis ver.）：** 用到了 [**binb: Binb is not Beamer**](https://www.rdocumentation.org/packages/binb/versions/0.0.6) pkg。
- 這個版本的配色真的潮很多（相比之下 beamer 那些主題用色充滿古早味）。雖然也是極簡路線，但很符合我的審美。

```
---
title: "Groenen & Andries Van Der Ark (2006) and  **Stout (2002)**"
author: "JW"
date: \today
output:
  binb::metropolis:
    latex_engine: xelatex
CJKmainfont: DFKai-SB
#mainfont: Calibri
fontsize: 12pt
urlcolor: blue
linkcolor: purple
---
```

- 關於**寫輪眼**，其實我還是很想走到這一步的。但是我目前還沒掌握訂製好（包括字型、字號大小、配色、布局等等）的作法。等我確實掌握之後再追加上來。

## R Markdown for **academic writing**

- 不用下載 `rticle`，不用受限於 `Ctex Doc` template 的設定，直接開一個 R Markdown 貼上 yaml 就可以跑。
- 匯出 .pdf 或 .docx 都可以。PDF 版才有完整格式，WORD 版只適合作後續修改的功能。
- 工作流程：<mark>用 R Markdown 寫第一稿，匯出成 MS Word 再進行修改。</mark>（考量到社會科學裡面還是用 MS Word 交流、投稿佔多數。）
- `.bib` 檔操作：
    1) 在 `.Rmd` 檔的同個資料夾裡面，開一個 `.bib` 的文字檔。（全程都可以在 Rstudio 完成。）
    2) 在 Google Scholar 搜尋到文獻之後，點選 `Cite`，再選擇 `BibTex`，整段複製下來貼到 `.bib` 檔裡面就可以引用了！
    3) `[@tsai2020]`/`[@chua2020, @tsai2020]`（有框）會顯示為 <mark>(Tsai, 2020)</mark>/ <mark>(Chua, 2020; Tsai, 2020)</mark>（句末引用），而 `@tsai2020`（無框）會顯示為 <mark>Tsai (2020)</mark> （句中引用）。
    4) 完成初稿之後，再做細部調整（尤其中文部分）。
- [`apa.csl`](https://github.com/citation-style-language/styles/blob/master/apa.csl) 是 APA 7/e，英文部分應該可以信賴？但可能還是要注意「第一次要引用全部作者」的部分。
- 如果在意 `apa.csl` 的 doi 部分的話，可以換成 `apa-old-doi-prefix.csl`。
- **YAML 如下：**
```
---
title: "在 R Markdown 文件中使用中文"
author: 
date: \today
abstract: "My abstract"
keywords: "My keywords"
geometry: margin=1in
papersize: a4
fontsize: 12pt
indent: true
linestretch: 1.5
output:
  pdf_document:
   latex_engine: xelatex
   fig_caption: yes
   number_sections: yes
   df_print: kable
  word_document:
CJKmainfont: DFKai-SB
mainfont: Calibri
bibliography: myref.bib
csl: apa.csl #apa-old-doi-prefix.csl
---
```

初稿的參考架構如下（R markdown）：

```
# Questions 
# Lit review
# The value (Theoritical model)
# Design (Stats model)
# Data 
# Method
# Answers 
# Defence (AD)
# limit

\newpage
# Ref
```

- 其中，從提出問題（Questions）開始到找尋既有的先行研究（Lit review）這都沒問題。Niche 就是本研究（current study）提出的待驗證解決方案或切入理論，是過往研究裡面沒有的創新突破的作法。到這邊是一般 Intro 的部分。
- 接著是 Method，把原本的構想化為研究設計，進行資料搜集與分析。
- Answers = Results，結果的報表跟解讀，回答一開始提出的問題。
- Defence = Discussion，回顧整個研究並提出辯護。
- Reco and lim 建議與限制。
- `\newpage # Ref` 這是開新的一頁來放 Ref 的內容。自動生成。

---
## Re:

引用一段 Xie 在 [xaringan Presentations](https://bookdown.org/yihui/rmarkdown/xaringan.html) 指導手冊裡面的話：

> The main reason I stopped using LateX Beamer slides was because of its popularity: when you attend academic conference, you see Beamer slides everywhere. --- Xie

但我覺得在人文社會科學裡面，比較少有這種情況。（除了少數計量/計算領域的簡報會出現 beamer 風格以外，絕大部分都是 PPT 吧）。

---