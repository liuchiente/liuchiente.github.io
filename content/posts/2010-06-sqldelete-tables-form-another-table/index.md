---
title: "SQL:Delete tables form another table 用其他資料表關聯刪除指定資料表"
date: 2010-06-10T09:31:00Z
lastmod: 2020-03-13T15:28:23.262Z
tags: ['資料庫', 'Database', '工程師', 'Oracle', 'SQL']
aliases:
  - /2010/06/sqldelete-tables-form-another-table.html
draft: false
---

  

  

以下說明其實跟使用關聯做UPDATE相當相似，當然對資料庫來說，也是有區分ORACLE跟MYSQL等資料庫可用性。  

  

  

  

  

對於以下相關範例，做個簡略說明，TABLE1跟TABLE2有關連欄位存在，而關聯欄位分別是COLUMN1,COLUMN2,COLUMN3,COLUMN4等四個欄位。  

  

首先是適用於ORACLE的寫法；  


> 
> DELETE FROM TABLE1 WHERE (COLUMN1,COLUMN2,COLMN3,COLUMN4) IN (COLUMN1,COLUMN2,COLUMN3,COLUMN4 from TABLE2)


以下的寫法是用EXISTS來做條件控制，不過對於ORACLE方面倒是需要測試看看，因為使用UPDATE會發生一點小錯誤。  


> 
> DELETE FROM TABLE1 WHERE　EXISTS (SELECT 1 FROM TABLE2 WHERE COLUMN1=TABLE1.COLUMN1,COLUMN2=TABLE1.COLUMN2,COLUMN=TABLE1.COLUMN3,COLUMN4=TABLE1.COLUMN4)


以下寫法則是據稱適用於一般資料庫，而語法會將兩個TABLE關聯到的資料做刪除，但是ORACLE不適用。  

  


> 
> DELETE (SELECT \* FROM TABLE1 A, TABLE2 B where A.COLUMN1 = B.COLUMN)


以下語法則使用USING，可以看見USING兩個TABLE，其中TABLE2設定特定條件，並關聯回TABLE1，並把TABLE1資料刪除。  


> 
> DELETE FROM TABLE1 USING TABLE1, TABLE2 WHERE TABLE1.COLUMN1=TABLE2.COLUMN1 AND TABLE2.COLUMN1='XXXX';


當然未必在設定TABLE2欄位條件時，一定非得使用關聯的欄位做篩選不可，可注意TABLE2.COLUMN2。  

  


> 
> DELETE FROM TABLE1 USING TABLE1, TABLE2 WHERE TABLE1.COLUMN1=TABLE2.COLUMN1 AND TABLE2.COLUMN2='XXXX';

