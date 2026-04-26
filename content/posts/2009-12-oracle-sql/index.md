---
title: "Oracle SQL"
date: 2009-12-25T10:00:00Z
lastmod: 2020-03-13T15:10:15.742Z
tags: ['RDBMS', 'Database', 'Oracle', 'SQL']
aliases:
  - /2009/12/oracle-sql.html
draft: true
---

找出schema裡面，所有的物件資訊，如函式、資料表等，也就是所謂Function、Package、View等，就像MS SQL的SYSOBJECTS一樣。  
  
  
  

> **SELECT \* FROM USER\_OBJECTS**

  
當然，也可以很簡單計算一下各種物件數量  

> **SELECT COUNT(OBJECT\_TYPE),OBJECT\_TYPE FROM USER\_OBJECTS GROUP BY OBJECT\_TYPE**

  
或是只過濾某種類型，譬如TABLE  

> **SELECT \* FROM USER\_OBJECTS WHERE OBJECT\_TYPE='TABLE'**

  
以上指令只能抓出本OWNER底下的相關訊息，如果要抓出資料庫機器上所有OWNER的所也物件，用以下語法，會傳回三個欄位，OWNER，TABLE\_NAME，TABLE\_TYPE。  

> **Select \* from ALL\_CATALOG**

  
取出OWNER的所有資料表。  

> **Select \* From User\_Tables**

  
取出所有資料表，但取出的資料太多了，必須考慮效能的話，就要取捨了。  
  

> **Select \* From All\_Tables  
> Select \* From Dba\_Tables**

