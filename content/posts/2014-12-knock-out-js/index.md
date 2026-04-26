---
title: "Knockout JS"
date: 2014-12-11T11:09:00Z
lastmod: 2020-07-25T01:42:45.222Z
tags: ['工程師', '寫網站']
aliases:
  - /2014/12/knock-out-js.html
draft: true
---


knock out js是一套在MVVM觀念下，用來建置View-Model的javascipt套件，但是基於jQuery做開發，所以在使用之前，也需要引入jQuery，以下就簡稱KO吧。   


  


KO的原理其實很簡單，可以分為兩大塊，一是資料布局，令一個則是資料變動的觀察和連動更新，也就所以，當你透過KO進行View-Model進行布局後，假定你在佈局上或是在程式上對View-Model進行變動，透過KO，他會即時把你的變動反應到View-Model上，其實我們可以把他想成JSP或是PHP的布局，但是又透過jQuery可以作到即時反應。

  


所以啦，在構成KO所建置的View-Model環境下，我們可以看到有幾個要角是一定會有的。

  


1. Data，原始資料來源，這可以是JavaScrpit Object，或是JSON Object，廣泛點來講，只要你可以把資料來源轉成前述兩個形式，他就可以變成一個合用的Data。
2. Knock Out JS，透過Konck Out JS把Data轉換成View-Model，讓KO的引擎可以把這些資料用來做頁面上的布局。
3. 布局，透過KO自定義的Tag來找尋DOM節點，並利用布局語法來做資料配置，在啟動後，KO引擎就會把相關數值配置到你指定的DOM上。




以上看起來，是不是跟PHP或是JSP非常非常的像？但是唯一需要瞭解的是，KO是要爬樹的，也就是說，他必須不停不停的爬DOM節點，來完成值的配置跟輸出，但是PHP跟JSP則是透過文本本身的轉換，因此對PHP來說，網頁就是個文字檔，你可以在你喜歡的地方隨便丟值，但KO就不一樣了，你要有DOM樹才有後續的活動。

  


所以，準備好了嗎？猴子？

  


[Knock Out JS](http://knockoutjs.com/index.html)官方網站



| 這封郵件來自 Evernote。Evernote 是您專屬的工作空間， 免費下載 Evernote |
| --- |


