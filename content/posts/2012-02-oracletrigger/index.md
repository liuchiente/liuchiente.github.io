---
title: "Oracle資料庫之觸發器Trigger的使用"
date: 2012-02-02T06:43:00Z
lastmod: 2020-03-21T00:23:06.253Z
tags: ['PL/SQL', 'Language', 'Database', 'Oracle', 'SQL']
aliases:
  - /2012/02/oracletrigger.html
draft: false
---

ORACLE的TRIGGER也是屬於PL/SQL的一種，中文也可以翻譯作觸發器，就字面上意思來說，可以解釋為觸發動作，而這觸發動作在於某個事件或是某種行為產生時，如果有因應的觸發動作，則會針對前述的狀況，進行指定的行為，這行為可能包含新增紀錄或連動呼叫程式、修正資料或是稽核資料。  
  
因此可以顯而易見的，TRIGGER的作用範圍很廣，包含如下；  

1. DML觸發程式，任何由DML敘述所產生的事件，並在事件成立後呼叫對應程式或是檢核資訊。
2. INSTEAD-OF觸發程式，當需要進行VIEW的資料調整時，對應調整VIEW的來源資料表。


系統觸發程式，針對DDL事件或是系統事件進行對應呼叫。  
  
其實就我以前常用的狀況，所以會導致我的認知裡面，TRIGGER大多用在DML的敘述觸發，因此以下先針對DML敘述進行介紹，如果針對DML的定義不清楚，可以先參考資料庫SQL概念介紹。  
  
身為一個TRIGGER，最基礎的架構如下；  
  

> 
> CREATE TRIGGER TRIGGER\_NAME {BEFORE|AFTER|INSTEAD OF} TRIGGER\_EVENT  
>   
> TRIGGER\_BODY;  
> [INSERT|DELETE|UPDATE] ON SCHEMA.TABLE [FOR EACH ROW]  
> TRIGGER\_BODY;  
> AFTER UPDATE  
> ON MYSCHEMA.TR\_USERNAME  
> FOR EACH ROW  
> BEGIN  
> ....DO SOMETHING  
> END TR\_USERNAME\_LOG;


由以上語法就可以大概瞭解TRIGGER宣告語法，並作些許介紹；  
  
  
  
* CREATE


  
CREATE就是建立，我們可以常見在建立資料表、函式甚至是視觀表(VIEW)，都會使用到這個關鍵字，假定說今天所要建立的對象已經存在了，而且是我們所預知的，那麼這時我們會考慮逕行覆蓋，引此CREATE我們會修改為CREATE OR REPLACE。  
  
* TRIGGER\_NAME


取一個你喜歡的名字，如果你面對的是多人使用的資料庫，建議使用一個好辨識的名字，而不是MYLOVE或是HA123。  
* BEFORE


這個地方將決定了你的觸發器如何運作，換句話說，這邊將決定你的觸發器的運作時機點，分別可以選擇為BEFORE代表你想要在指定動作執行之前觸發，AFTER則是在執行之後觸發，INSTEAD OF則是用於VIEW，後續會再進行說明。  
* TIRGGER\_EVENT


這部份的話，則是指定了啟動觸發器的事件，就DML來說，觸發器在這裡可以監控的事件則包含INSERT、DELETE、UPDATE，因此我們可以寫作INSERT ON TABLE\_NAME；另外在這部份可以選擇是否加入FOR EACH ROW。  

  

EACH ROW代表是ROW觸發層次，所謂ROW觸發將在指定敘述時，針對每個一個動作影響到的ROW都觸發執行一次，因此如果沒指定為ROW層次，則只會執行一次。  

* TRIGGER\_BODY


這部份則是指定你將對應執行的程式本體，假定你的TRIGGER\_BODY非常多程式法的話，依照官方建議以不超過23K為準，  
  
如果非常大的話，建議採用函式呼叫。  
  
因此綜合以上，可以見到的觸發器程式可能為以下；  
  

> 
> CREATE TRIGGER TRIGGER\_NAME {BEFORE|AFTER|INSTEAD OF}


  
所以我們就來看一個實際的程式碼吧；  
  

> 
> CREATE OR REPLACE MYSCHEMA.TR\_USERNAME\_LOG


  

建議規劃TRGGER前跟使用後，都要做好考量跟紀錄，否則很容易因為一時不察，導致程式出錯，將會很難進行追查，之後將再針對TRIGGER進行更多紀錄。