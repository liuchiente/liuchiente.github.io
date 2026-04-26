---
title: "PHP 版面的區塊規劃與繼承"
date: 2012-11-23T17:19:00Z
lastmod: 2020-03-13T15:10:11.788Z
tags: ['Smarty', 'Template Engine', 'Twig', 'PHP']
aliases:
  - /2012/11/php.html
draft: true
---

這些日子來，對於PHP的開發依舊使用大量的樣板引擎，使用樣板引擎的原因其實也是跟工作上的切割和分派有關係，在美工和程式設計的工作內容相對分開清楚時，樣板引擎的使用就相對好用，畢竟身為一個程式設計師，雖然也是設計師，美感總是比較特殊一點，因此我們總需要依賴專業的美工人員來做版面的設計，好一點的狀況之下，樣板的設計可以包含到HTML的輸出，不得不說，太棒了！  
  
畢竟有些開發模式會由風格設計師輸出圖片之後，再由程式設計師做版面輸出，相對之下，我就會覺得跟我配合的風格設計師身上總是散發出偉大的光芒，使我感到溫暖這樣。  
  
在以下的內容，並不會特別提到子區塊和子版面的使用效率問題，在我的心中，這也是個謎，畢竟一個要走讀寫，一個要走繼承，誰好誰壞？我想這可能要另外開討論。  
  
在我習慣的樣板引擎使用上，我會使用到一定的繼承功能，這個繼承功能我會搭配區塊的規劃使用，而區塊的規劃使用不管是在twig或是smarty，都有提供現成的函數可以用，當然對於繼承的功能來說，兩者都算夠用，而在這些使用之中，我們必須針對版面來做一個一個的檢視。  
  
首先，針對HTML頁面整體架構來看，一個簡單的HTML頁面，會包含  
  
1. HTML描述
2. HTML head
3. HTML body

  
  
最最抽象來看，會包含這三個部份，所以要的話，也可以切割這樣來用就好，哈，所以會變成以下的區塊變化。  
  
1. block HTML Description
2. block HTML head
3. block body

  
而針對不同樣式和不同功能類型的頁面，我們需要引入不同的CSS和Javascript，因此我們可能針對HTML　head再度做切割，切割的內容可能會包含幾個。  
  
1. block HTML Head
1. block HTML CSS Link
2. block HTML Javascript Link

很偷懶的用縮排來表示他們的關係，應該可以看得出來Link跟Head的關係，Link是屬於HTML Head區塊裡面的Block吧？（自我感覺）  
  
而在這之間，我們需要帶入我們已經寫好的JS Code，所以我們會怎麼做呢？  
1. block HTML Head
1. block HTML CSS Link
2. block HTML Javascript Link
3. block HTML Javascript Code

 一般常見的運用是如此，所以我們接下來把焦點放在block HTML body的處理上，當然對於這些處理來說，是很模糊的，有幾多模糊呢？畢竟每個頁面的取向都不一樣，有的是作為條列式的呈現，有些則是一整個詳細資料放上來，還有一些是條列很多詳細資料，所以對於body的呈現，相對的就會比較複雜，所以在這裡的規劃，可能又會依照功能取向而分別繼承下來規劃出各自方向的頁面好提供往下繼承，但是我們可以大略歸納出一個方向來做。  
1. block HTML body
1. block HTML top
2. block HTML banner
3. block HTML menu
4. block HTML content
5. block HTML bottom

 如果你今天需要一個條列式的頁面，那可能就會切割出來，另外改寫出一個不同的父頁面。  
1. block HTML body
1. block HTML top
2. block HTML banner
3. block HTML menu
4. block HTML content
1. block HTML List Content

6. block HTML bottom

 綜合以上的規劃，相信我們已經可以規劃出一些堪用的父頁面了，而在往下的繼承使用，子頁面會繼承到父頁面所設定的相關內容，當然，你可以進行覆寫，或是呼叫父頁面的block後進行改寫使用，透過這樣的使用，可以讓我們更快速的進行版面切割，還有子頁面的使用，在維護上，我們可以減少管理子頁面的異動，也可以減少子頁面的數量，少了管理的複雜度，因為透過一定程度的抽象化，會提高頁面的重用性，往下再繼續實際撰寫的效果跟處理。  
  
  
  
  
  
  
