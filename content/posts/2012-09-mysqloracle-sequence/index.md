---
title: "在MySQL模擬Oracle Sequence"
date: 2012-09-17T14:43:00Z
lastmod: 2020-03-13T15:10:12.194Z
tags: ['MySQL', 'Database', 'Oracle', 'SQL']
aliases:
  - /2012/09/mysqloracle-sequence.html
draft: true
---

經過好幾個月沒有更新，時至今日，我終於又出現了，回想這幾個月的消失，其實原因很簡單，就只是接到了一個蠻有挑戰性的專案，而這挑戰性來自於...要獨自完成，所以在橫衝直撞的摸索中，終於這些脫韁的野牛逐漸可以被控制了，所以就來寫寫網誌吧，雖然裡面還有很多草稿沒發表...。  
  
 這次要分享的是剛好在案子裡面遇到的小狀況，由於這次開發使用的是MySQL，MySQL有什麼不同呢，一個很大的機制不同是在於自動新增的值處理方式，在Oracle我們可以看到，對於自動新增的值，我們會去取用資料庫內所提供的Sequence，Sequence是一個不斷累加的值，我們可以自訂起始、結束甚至是最大值還有何時要重新計數的值，簡單來說，就是一個累加器。 至於MySQL的Auto　INCREMENT機制，則是需要將該資料表要新增的欄位設定為PK，型態設定為INT，然後設定為auto\_increment，如此一來在該table新增一筆資料時，資料庫系統就會自動把這欄位新增上去，並且自動累加1，在系統的機制作用下，這樣產生的值並不會有重複的問題，值會從1累加到上限為止，而在一個資料表內，只能有一個欄位是auto increment。  
  
 至於這兩種機制，誰好誰壞，就看使用的環境了，有趣的是，在postgre SQL裡面，我們可以使用sequence來模擬mysql 的　auto increment，不過這就等待之後有機會再分享了。(應該吧)    
  
不同於mysql，當我們使用sequence時，必須自己去取得新增的值，在塞入到指定欄位，來達到自動累加的功能，看似麻煩，但是其實是有他的好處的，當我們必須以這樣的累加值，搭配我們想要的編碼規則時，這樣的功能就派上用場，譬如我們需要以年月日來搭配建立流水號，所建立出來的值可能就會是； 20120901+xxxxxx 當xxxxxx為流水號時，組成的值則會為20120901000001。 至於為何要這樣組合，則是屬於系統設計的問題，以下要講的是如何使用mysql模擬oracle的sequence，而我們將會用到的機制是採用資料表+store procedure。  
  
 首先建立一個sequence專用的資料表如下；  
  
  `CREATE TABLE Sequence   
 ( seqname VARCHAR(32) NOT NULL, --sequence名稱   
 currentValue INT NOT NULL, --sequence當前值   
 increment INT NOT NULL DEFAULT 1, --sequence值   
 PRIMARY KEY (seqname) )   
ENGINE=InnoDB;`  
接著我們透過維護這個資料表，來管理我們所需要的sequence狀態，譬如給個名字，起始值為999，每次累加為1。  
  
  `INSERT INTO Sequence VALUES ('myseq',999,1);`  
  
接著建立取得sequence的程式，首先是取得sequence現在的值。  `DELIMITER $ CREATE FUNCTION nextval (seq_name VARCHAR(32)) RETURNS INTEGER CONTAINS SQL BEGIN UPDATE Sequence SET currentValue = currentValue + increment WHERE seqname = seq_name; RETURN currval(seq_name); END$ DELIMITER ;`  
於是我們就可以使用以下SQL來取得指定sequence現在的值了，承上範例，應該會是999，在取完值後，這個sequence的值將會累加1，變成1000。  `SELECT nextval('myseq');` 接著來建立取得下一個值的程式。  
  `DELIMITER $ CREATE FUNCTION nextval (seq_name VARCHAR(32)) RETURNS INTEGER CONTAINS SQL BEGIN UPDATE Sequence SET currentValue = currentValue + increment WHERE seqname = seq_name; RETURN currval(seq_name); END$ DELIMITER ;`用以下SQL取值，承上範例，應該會取到1002。 以上就是使用MySQL模擬Oracle的sequence功能，目前先寫到取得現在值及下一個值。