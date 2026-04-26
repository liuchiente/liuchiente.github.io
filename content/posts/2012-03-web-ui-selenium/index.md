---
title: "簡單易用的Web UI測試工具-Selenium"
date: 2012-03-20T14:05:00Z
lastmod: 2020-03-13T15:10:12.803Z
tags: ['UI Test', 'Language', 'PHP', 'HTML', 'Selenium', 'FireFox']
aliases:
  - /2012/03/web-ui-selenium.html
draft: true
---

當我們開發一個網站時，總是要因為一個問題的修正，而重複的進行畫面操作和確認，尤其是當一個  
小問題，譬如js的修改，或是後端程式碼的修改，難免會擔心是否連動導致整個操作流程受到影響，  
當然，當我們的畫面流程只是簡單的幾個步驟，再動一次好像也沒關係，但是當今天我們要操作多個  
分頁，進行多個流程，卻因為你一個程式碼修正，又要全部重新來一次，當下的感覺，真是快樂似神  
仙。  
  
  
  
於是我們就會開始想了，要是有人可以取代我操作這些畫面的時間不知道有多好，撇開勞心勞力不講  
，如果把這些重複性的動作重複再重複，自動執行，然後把這些時間拿來開發其他東西，或是看看球  
賽，說真的，好像會開心點。  
  
於是Web UI測試就因應而生了，你可以不用把這工作交給看似無所事事的行政小姐，也不用擔心是  
不是因為小姐對於系統概念不清楚，而測得不夠仔細，最後的測試工作跑到了End-User身上，然後  
業務忙著賠不是，抓你去浸豬籠。  
  
市面上的Web UI百百款，有的專注於對UI作壓力測試，有的則是模擬使用者操作動作，實實在在的  
跟著跑一次流程，Selenium就是這樣的產品，程式的功能不用多介紹，但是當有需求的人看到會露  
出雞姐看到牛丸的表情，那就成功一半了。  
  
Selenium，念起來有點饒口，是一個專注於Web UI測試的軟體，他最大的特色就是在於，真實的  
模擬使用者動作，包含點選，輸入，送出，並可以觀察想預知的的資訊，使用上，只要點選錄製功能  
，然後用瀏覽器操作一次畫面，它就會記住你的動作，並且照你的走法跑一次流程，假定今天你所  
設定的資料沒呈現出來，或是該點選的按鈕不見了，它就會顯示錯誤訊息，並且終止測試，聽起來  
是不是很不錯呢？  
  
於是我們就可以開始透過FireFox，來進行我們的測試之旅了。  
  
首先請先安裝FireFox之後，並且到Selenium的官方網站，下載[Seleniumm IDE](http://seleniumhq.org/projects/ide/)，在這裡，官方網  
站會整帶包裝好你所需要的套件，製作成xpi檔案供FireFox進行安裝，待安裝完畢後，你就可以看到  
，你的FireFox瀏覽器上，在工具選項，已經出現一個Selenium IDE供你呼叫了。  
[![IDE1](/posts/2012-03-web-ui-selenium/images/2012-03-web-ui-selenium-4756774115462187215.png)](http://onez.pixnet.net/album/photo/172304133)  
  
是的，接下來請點選它之後，你會發現，你已經呼叫出Selenium IDE了。  
  
[![IDE2](/posts/2012-03-web-ui-selenium/images/2012-03-web-ui-selenium-8118532313468567170.png)](http://onez.pixnet.net/album/photo/172304149)  
在這裡，可以看到最上面分別有紅色錄影鍵，綠色播放，還有暫停，播放有分為整個單元測試的播放，和單一測試案例的播放。  
  
左邊視窗則是測試案例的管理，你可以看到有一個未命名的測試案例。  
  
當然，你可以在右邊的測試案例管理上，增加或減少，甚至用add Case加入已經存在的測試案例。  
  
也可以在你指定的案例上點右鍵，從屬性去變更你的測試案例名字跟檔名。  
  
當你按下紅色鍵，就是開始錄影了，請微笑。  
  
[![ide4](/posts/2012-03-web-ui-selenium/images/2012-03-web-ui-selenium-227958401986116845.png)](http://onez.pixnet.net/album/photo/172304167)  
  
錄影完畢後，可以看到中間的指令窗，已經有我們剛剛的動作記錄了，這裡區分為三種型態，一是指令種類，一是頁面上的目標，另一則是給定的值。  
  
從畫面上可能看不到所謂給定的值是啥意思，但是當我們的動作是輸入，這時候就會有給定的值了，譬如你輸入Hello，Value就是Hello。  
  
指令則區分很多種，這裡我們看到的都是ClickAndWait，代表點選後等待回應，這些指令，都是Selenium所定義的[Selenese](http://seleniumhq.org/docs/02\_selenium\_ide.html#selenium-commands-selenese)  
  
這指令的定義真得很有趣，也很直覺，指令呈現方式則如同以下。  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  


| onez | | |
| --- | --- | --- |
| open | /blog | |
| clickAndWait | link=World Wild Web (1) | |
| clickAndWait | link=故鄉人 (1) | |
| clickAndWait | link=垃圾話 (2) | |
| clickAndWait | link=PHP (2) | |
| clickAndWait | link=死亡筆記本 (1) | |
| clickAndWait | link=打字機 (7) | |


  
它是使用HTML表格呈現的。  
  
當然我們可以透過[官方說明](http://seleniumhq.org/docs/02\_selenium\_ide.html#selenium-commands-selenese)，取得更多相關指令。  
  
[![ide4](/posts/2012-03-web-ui-selenium/images/2012-03-web-ui-selenium-227958401986116845.png)](http://onez.pixnet.net/album/photo/172304167)  
  
如果今天你要執行一整個單元測試，記得點選左邊的播放按鈕。  
  
如果是單一案例，則點選案例播放按鈕，程式會忠實的呈現測試結果。  
  
[![ide5](/posts/2012-03-web-ui-selenium/images/2012-03-web-ui-selenium-5404624541483923572.png)](http://onez.pixnet.net/album/photo/172304181)  
  
最後我要說的是，以上的這些測試動作，都可以儲存，當然這是廢話，不過測試結果並不能自動儲存。  
  
有一點很重要，我們可以在這裡點選export，把指令轉成java或是python等，然後透過它們的執行，來做測試的工作。  
  
原先有支援匯出為phpunit，但是本版先拿掉了，但是實際上我們還是可以用phpunit來做測試。  
  
由於瀏覽器限制，以上的動作只能作在FireFox上，但是實際上來說，Web UI的測試，是可以跨瀏覽器，跨語言的，意思是說，只要您是Web頁面，都可以透過這樣的動作來執行測試。  
  
當然有人會說，那我就用錄的就好了，但是實際運用上，我們其實可以有更多選擇，來定義我們的動作，也包含了事件的預知跟觀察，因此優劣之間，還是看個人判定吧。