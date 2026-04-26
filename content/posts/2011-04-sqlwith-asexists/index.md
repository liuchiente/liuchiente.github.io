---
title: "SQL:WITH AS和EXISTS"
date: 2011-04-06T15:50:00Z
lastmod: 2020-07-25T01:42:54.960Z
tags: ['Database', 'Oracle', 'SQL']
aliases:
  - /2011/04/sqlwith-asexists.html
draft: true
---

當WITH AS和EXISTS相遇會發生啥事情咧，如果腦筋動的快一定會發現，這兩個都  

是SQL語法之一，如果腦筋動得慢就會發現，其實就只是SQL語法，不過膚淺的是，  

兩個的相同處也不過就是這樣...。  

  

  

 **WITH AS & Subquery**  

  

子查詢先遞迴完成括號內查詢結果，再到外部完成所有查詢。  


> 
> SELECT \* FROM (  
> SELECT \* FORM TABLE1,TABLE2 WHERE TABLE1.COLUMN1=TABLE2.COLUMN1  
> ) TOTAL1


以下是WITH AS，該語法會建立T1,T2,T3，再由最下方SQL指令完成語法，姑且不論效能是否真的提昇很多，相較之下這樣的敘述是比較直覺且易讀的；  


> 
> WITH T1 AS  
> (SELECT COLUMN5,COLUMN6 FORM TABLE1,TABLE2  
> WHERE TABLE1.COLUMN1=TABLE2.COLUMN1),  
> T2 AS  
> (SELECT COLUMN5,COLUMN6 FORM TABLE3,TABLE4  
> WHERE TABLE3.COLUMN1=TABLE4.COLUMN1)  
> T3 AS  
> (SELECT COLUMN5,COLUMN6 FORM TABLE5,TABLE6  
> WHERE TABLE5.COLUMN1=TABLE6.COLUMN1)  
> SELECT \*FROM T1,T2,T3 WHERE T1.COLUMN5=T2.COLUMN5 AND T2.COLUMN6=T3.COLUMN6


**EXISTS & IN**  

  

IN的使用方法很直覺，單純只要判斷該欄位是否落在括號中條件。  

  


> 
> SELECT \* FROM TABLE1 T1 WHERE FIELD1 IN (1,2,3,4,5)


  

也可以判斷是否落在子查詢裡面的條件，子查詢必須指定好哪個欄位與外部欄位做IN的比對。  

  


> 
> SELECT \* FROM TABLE1 T1 WHERE FIELD1 IN (SELECT FIELD FROM TABLE2)


  

但是當預估查詢筆數一多，或是比對條件一多，相對的使用IN就會變得超慢，另外子查詢條件所列出的欄位非常多時，使用IN也會很慢，因此可用EXISTS替換，但是主要會用來替換有子查詢的IN子句，  

  


> 
> SELECT \* FROM TABLE T1 WHERE EXSITS(SELECT 1 FROM TABLE2 WHERE FIELD1=T1.FIELD)


  

在(SELECT 1 FROM TABLE2 WHERE FIELD1=T1.FIELD)裡面，由於只需要驗證子查詢是否存在，不需要使用到子查詢提出的欄位，因此使用1取代提出欄位，對子查詢的效能會比較佳。  

  

EXISTS的邏輯意義代表著是否存在，因此該語法可以解釋為外部查詢，是否有指定條件存在於子查詢中，若有則提出，當然相對的NOT IN及NOT EXISTS也可以用一樣的作法取代，由於EXISIT只需要驗證子  

  

查詢是否存在，會比IN進行大量的逐筆比對效能好的多。