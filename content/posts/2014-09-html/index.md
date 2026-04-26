---
title: "Html的基本架構及相關標籤"
date: 2014-09-17T11:45:00Z
lastmod: 2020-03-13T15:10:10.273Z
tags: ['HTML']
aliases:
  - /2014/09/html.html
draft: true
---

# HTML(英文：HyperText Markup Language)是從IETF的SGML簡化而來的，目前由全球資訊網協會(W3C)所維護。整個HTML的基本組成是由標頭、段落或列表等等。

# 如：

<!document html>  <html>    <head>      <title></title>    </head>#    <body>

#         <h1>這是我的第一個網頁</h1>

#    </body>

</html># 它是一個標籤語言，是由許多標間和純文字所組成，而每個標籤就是描述整的文件的內容。然而，每個都必須要有起始跟終止的標籤，缺一不可。而元素的純文字內容必須包含在起始跟終止標籤內，空元素則必須要有終止標籤，整個文件可以包含不同種的元素標籤。大多數的元素標籤都有屬性，可以依照自己的需求套入，例如：<body></body>，如果想把內容的背景加上顏色的話，可以利用background-color這個屬性套用，就會變成

<!document html>  <html>    <head></head>#    <body background-color="#FFFFFF">

#         <h1>這是我的第一個網頁</h1>

#    </body>

</html># 而有時候，並不是所有屬性都能在body這個元素這適用。

# 例如：

<!document html>  <html>    <head></head>#    <body target="\_top">

#         <h1>這是我的第一個網頁</h1>

#    </body>

</html>target不是body的屬性，所以網頁將不會產生任何作用。  
# 詳細可以參考這個網站(http://www.w3schools.com/tags/tag\_body.asp)，裡面有列出每個元素自己所能套用的屬性。

# 然而，當整份文件只有文字或標籤當然不夠精緻，我們可以加上一些CSS讓整個網頁變得更有色彩，而CSS的透用可以有兩種方式寫入，其一，我們可以直接寫在<head></head>，利用<style></style>這個元素標籤。

# 例如：

<!document html>  <html>    <head>      <title></title>      <style>          body{background-color:yellow;}          h1{color:blue;}      </style>    </head>#    <body>

#         <h1>這是我的第一個網頁</h1>

#    </body>

</html># 其二，可以利用外部連結的功能將它連結進來，可以利用<link></link>這個元素標籤，

# 例如：

<!document html>  <html>    <head>      <title></title>　　<link rel="stylesheet" type="text/css" href="mystyle.css">    </head>#    <body>

#         <h1>這是我的第一個網頁</h1>

#    </body>

</html># 第二種方式可以將樣式保存，當我們下次在其他的網頁中如果也須要將入這個CSS樣式的話，就可以直接連結進來，不需要再重寫一次。

如果能將標籤的屬性和CSS樣式熟悉運用的話，即可做出一個外觀很棒的網頁。