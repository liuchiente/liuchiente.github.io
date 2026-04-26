---
title: "SQL:PL/SQL基礎"
date: 2011-05-21T05:38:00Z
lastmod: 2020-03-13T15:10:14.877Z
tags: ['PL/SQL', 'Database', 'Oracle', 'SQL']
aliases:
  - /2011/05/sqlplsql.html
draft: true
---

對於SQL來說，我們所做的動作很簡單，就是一進一出，換句話說，就是下一個  
  
指令出去，然後出來給我們一個結果，不管是insert、update、delete，因此  
  
當需要針對進入的指令多做複雜的判斷，或是對產出的結果多做處理，相較之下  
  
，有些SQL語法並不能完美達到我們要的結果，這時候就需要PL/SQL這一類的  
  
程式出現了，而SQL Server用戶有所謂的Store Procedure，也是類似的道理  
  
。  
  
  
  
PL/SQL程式的架構；  
  
Set Serveroutput on;   //其實這行跟一般在寫這程式沒太大關聯，只是當你需要看輸出結果，就必須把這輸出參數打開。  
  
Declare  
v\_number    number(3); //宣告你要使用的變數、物件、紀錄、表格，這地方是可以自己決定是否宣告的。  
Begin  
//do somethimg....就是你的程式主體。  
exception  
//任何你預期的變化,必須所作的例外處理，當然也可以自己決定是否處理這塊。  
End;  
/  
  
雖說宣告跟例外處理是可以自己決定是不是所以嚴格來講，但實際上來說，例外  
  
處理還是建議要做好，不然老是等系統彈一個錯誤給使用者看，實在有失嚴謹性  
  
，扣除掉自選是否處理的，程式就剩下如下所示；  
  
Begin  
//do somethimg....就是你的程式主體。  
End;  
/  
  
就只有開始跟結束，不過可以稍稍解釋一下巢狀架構。  
  
Begin  
//do somethimg....就是你的程式主體。  
Begin  
//do somethimg....就是你的程式主體。  
End;  
End;  
/  
  
當然也可以宣告變數。  
  
Declare  
v\_number\_outside    number(3);  
Begin  
Declare  
v\_number\_inside    number(3);  
Begin  
//do somethimg....就是你的程式主體。  
End;  
End;  
/  
  
相信這樣的結構應該很好明白，最外層已經宣告了v\_number\_outside  
  
，當你內部巢狀還需要額外的變數時，就可再宣告內部巢狀區塊變數，但是需要注意到  
  
，當宣告內部變數v\_number\_inside，外部是用不到的，這應該是很好理解的概念。