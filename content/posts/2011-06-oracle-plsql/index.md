---
title: "Oracle PL/SQL 指標"
date: 2011-06-15T08:01:00Z
lastmod: 2020-03-13T15:10:14.282Z
tags: ['PL/SQL', 'Database', 'Oracle', 'SQL']
aliases:
  - /2011/06/oracle-plsql.html
draft: true
---

要說指標還是cursor，隨便了。  
  
從下面一堆code裡面，可以看出這是一個基本的PL/SQL程式，且使用了PL/SQL中的cursor，所謂的cursor，就是我們先宣告了一個變數，其指向一個指定的查詢，在依照我們去異動cursor的指向，就可以對應到我們想要的查詢結果，相對於一般我們對指標的定義，是指向記憶體中的某個位置，而在這裡則是表示資料庫中資料的位置。  
  
  
  
  
[sql]SET SERVEROUTPUT ON  
DECLARE  
CURSOR TNAME IS  
SELECT \* FROM SO001 WHERE CUSTID<10;  
B TNAME%ROWTYPE;  
BEGIN  
OPEN TNAME;  
LOOP  
FETCH TNAME INTO B;  
DBMS\_OUTPUT.PUT\_LINE('THE KERKER IS:'||B.CUSTID);  
DBMS\_OUTPUT.PUT\_LINE('KERKER IS:'||TNAME%ROWCOUNT);  
EXIT WHEN TNAME%NOTFOUND;  
END LOOP;  
CLOSE TNAME;  
END;  
  
/[/sql]