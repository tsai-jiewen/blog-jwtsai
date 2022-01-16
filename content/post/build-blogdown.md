---
title: How to build a personal website with R blogdown? (zh-tw)
authors: 
  - admin
date: 2020-11-16
draft: false
tags:
  - zh-tw
  - r
---

[![hackmd-github-sync-badge](https://hackmd.io/WTHz1t8KRwuV0j7ldbEUhA/badge)](https://hackmd.io/WTHz1t8KRwuV0j7ldbEUhA)

{{< toc >}}

## 1 使用 R 套件 blogdown

### 1.1 blogdown

### 1.2 new_site

### 1.3 install_theme（from hugo）

## 2 建立 Github repo

![](https://i.imgur.com/Xhpb4ST.png)

### 2.2

## 3 部署到 Netlify

![](https://i.imgur.com/exndP4Z.png)

## 4 其他調整議題

### 4.1 config

### 4.2 googleAnalytics

### 4.3 disqusShortname

我是用 hugo-nanx2020 這個主題，但有幾個問題，第一個就是我發現 Disqus 設定不能用？

https://nanx.me/blog/post/migrating-from-disqus-to-utterances/
參考這篇，可知是要修改 `single.html` 這個檔案。
以我的電腦為例，路徑為 

```
GitHub/my_blog/themes/hugo-nanx2020/layouts/blog/single.html
```

找到最末端（大概倒數第 3 行）的這段文字，把 `utterances` 改成 `disqus`，留言功能 Disqus 就可以跑了。（可能因為原作者 把 `disqus` 改成 `utterances`，所以他改回來就能用了。）

```
    {{ partial "disqus.html" .}}
```

### 4.4 highlight 格式（R）

hugo-nanx2020 主題第二個問題，就是他的程式 highlight 不符合 Ｒ 的樣式？

https://stackoverflow.com/questions/48075493/change-style-color-of-blocks-of-code-in-blogdown

參考這篇文章，終於找到解答。
在這個路徑中找到 header.html。在本主題中是 header.html，在其他主題也有可能是 head.html。

```
GitHub/my_blog/themes/hugo-nanx2020/layouts/partials/header.html.
```

貼上這一段：

```
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.10.0/styles/default.min.css"> <script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.10.0/highlight.min.js"></script> <script>hljs.initHighlightingOnLoad();</script>
```

這樣就可以了！

### 4.5 tags 導致無法顯示問題

hugo-nanx2020 主題第三個問題，就是好像不支持 tag、cat 等分類標籤，所以只要加了標籤，在 Netlify 那端都會 failed to deploy。

### 4.6 取消 RSS 的設置

在 hugo-nanx2020 主題中，RSS 的路徑在：

```
GitHub/my_blog/themes/hugo-nanx2020/layouts/partials/footer.html
```

其中有一段：

```
{{ if .Site.RSSLink }}
<li><a href="{{ .Site.RSSLink }}" class="mr-0">RSS</a></li>
{{ end }}
```

刪掉就可以。

### 4.7 LikeCoin 設置

關於 LikeCoin 設定的問題，詳細可以參考這篇 LikeCion 的官方文章：[如何在 Hugo 開發環境的文章中加入 LikeCoin button](https://docs.like.co/v/zh/user-guide/creator/self-host/hugo)
