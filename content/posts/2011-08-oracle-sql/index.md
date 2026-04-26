---
title: "Oracle SQL 函式"
date: 2011-08-29T01:38:00Z
lastmod: 2020-03-13T15:20:16.131Z
tags: ['Orac', '資料庫', 'Database', '工程師', 'Oracle', 'SQL']
aliases:
  - /2011/08/oracle-sql.html
draft: false
---

Oracle 相關 SQL 函式參考  

  

  

  

CEIL回傳最小的整數,會回傳大於或等於輸入的值 (無條件進位)  


> 
> SELECT CEIL(10), CEIL(10.5), CEIL(-10.5) FROM DUAL


  

FLOOR會回傳小於或等於輸人的值 (無條件捨去)  


> 
> SELECT FLOOR(9.9), FLOOR(-9.9) FROM DUAL


  

GREATEST取最大值(文字、數值、日期)  


> 
> SELECT GREATEST(-1,-2,-3), GREATEST('One','Two','Three') FROM DUAL


文字部份取最大值，是用每一個位元的ASCII值比較，若該位元相同，再以下一位元做比較。  

  

LEAST 取最小值(文字、數值、日期)  


> 
> SELECT LEAST(-1,-2,-3), LEAST('One','Two','Three') FROM DUAL


  
MOD(m, n) 回傳 m 除以 n 的餘數  


> 
> SELECT MOD(18,12),MOD(30,12),MOD(33,13) FROM DUAL


  

POWER(m, n) 回傳 m 的 N 次方  


> 
> SELECT POWER(2,3), POWER(2,-2), POWER(-10,3) FROM DUAL


  

Round(n, m) (四捨五入到N位)  
n 將近位的值  
m 指定一個小數點位數的數字，此數字必須是一個整數。  
若m值是負數,將會導致值被進位到所指定之特定小數點左邊的地方。  


> 
> SELECT ROUND(123.45), ROUND(123.45, 1), ROUND(123.45,-1) FROM DUAL


  
SIGN(n)回傳一個值,以指出n的符號。  
回傳 -1 代表 n 是負數, 0 代表 n 為 0, 1 代表 n 為正數  


> 
> SELECT SIGN(33),SIGN(0),SIGN(-33) FROM DUAL


  

SQRT(n) 回傳 n 的根次方  


> 
> SELECT SQRT(100), 10\*10, SQRT(81), 9\*9 FROM DUAL


  

TRUNC(n [,m]) 無條件捨去至特定小數點  


> 
> SELECT TRUNC(123.456), TRUNC(123.456, 2), TRUNC(123.456, -1) FROM DUAL


FLOOR函數只能無條件捨去到整數位,但trunc可以指定小數位  

  

TRUNC(d [,format])  


> 
> SELECT TRUNC(TO\_DATE('2006/12/31 23:59:59','YYYY/MM/DD HH24:MI:SS')) FROM DUAL


trunc用於日期，則是將時分秒去除;另外，也可自行定義trunc的格式。  

  

ASCII回傳字元的 ASCII Key Code  


> 
> SELECT ASCII('A'), ASCII('ABC') FROM DUAL


  

CHR回傳指定的 ASCII Key Code 符號  


> 
> SELECT CHR(65) FROM DUAL


  

INITCAP 將字串中每個單字的開頭字母改變為大寫，而將其它字母變成小寫  


> 
> SELECT INITCAP('GOOD MORNING, DVID') FROM DUAL


  

大小寫字元轉換，Upper 轉成大寫字元, Lower 轉小寫字元  


> 
> SELECT UPPER('ABCDefg') ,LOWER('ABCDefg') FROM DUAL


  
LENGTH 計算總字元數量, LENGTHB 計算字串 Byte 數  


> 
> SELECT LENGTH('ABC一二三'), LENGTH('Hi 一二三'), LENGTHB('ABC一二三'), LENGTHB('Hi 一二三') FROM DUAL


  

LPAD(string1, n[,string2])在 string1 左邊填滿 string2 字串直到該字串達到 n 字元時  


> 
> SELECT LPAD('1', 3), LPAD('1', 3, '0'), LPAD('1', 3, '^'), LPAD('1',5,'ABC') FROM DUAL


  
RPAD(string1, n[,string2])在 string1 右邊填滿 string2 字串直到該字串達到 n 字元時  


> 
> SELECT RPAD('1', 3) || '123', RPAD('1', 3, '0'), RPAD('1', 3, '^'), RPAD('1',5,'ABC') FROM DUAL


  

INSTR(string1, string2 [,n[,m]])，回傳在 String1 中的第 n 個字元開始所找到 string2 第 m 次的字元位置,，m, n 省略不打則預設為1，若 n 為負數則表示從第一個字元找到倒數第 n 個字元。  

  

INSTRB(string1,string2 [,n[,m]])，回傳在 String1 中的第 n 個 Byte 開始所找到 string2 第 m 次的Byte位置m，n 省略不打則預設為1，若 n 為負數則表示從第一個字元找到倒數第 n 個 Byte。  

  


> 
> SELECT INSTR('ABCDE一二三四五','二'), INSTRB('ABCDE一二三四五','二') FROM DUAL


  


> 
> SELECT INSTRB(' 1一2二3三1一2二3三','3', -3, 1), INSTRB(' 1一2二3三1一2二3三','3', -5, 1) FROM DUAL


  

REPLACE(String,search\_string, [,replacement\_string]) 搜尋一個字串，並用其它的來取代子字串，若replacement\_string 不填，表示要刪除 String 中含 search\_string 的字串。  


> 
> SELECT REPLACE('The sky is gray.','gray','blue') FROM DUAL



> 
> SELECT REPLACE('The sky is bluegray.','gray') FROM DUAL


  

LTRIM(string1 [,string2]) 移除 string1 左邊指定字元(string2)  
RTRIM(string1 [,string2]) 移除 string1 右邊指定字元(string2)  
TRIM([LEADING | TRAILING | BOTH] [trim\_character FROM] string)  
移除 string1 左邊或右邊或左右兩邊指定字元(string2)  

  


> 
> SELECT '$' || LTRIM('  ABC  ') || '$','$' || RTRIM('  ABC  ') || '$','$' ||  TRIM('  ABC  ')  || '$' FROM DUAL


  

SUBSTR(string, strart\_index [,length]) 回傳字串的部分字元  


> 
> SELECT SUBSTR('OneTwoThree',7), SUBSTR('OneTwoThree',4,3), SUBSTR('OneTwoThree',-5), SUBSTR('OneTwoThree',-5,1) FROM DUAL


  

SUBSTRB(string, strart\_index [,length]) 回傳字串的部分位元組  


> 
> SELECT SUBSTR('1一2二3三',4), SUBSTRB('1一2二3三',4) FROM DUAL


  

TRANSLATE(string, from\_string, to\_string) 藉由字組對照來修改字串(若 string 中含有 from\_string 中沒有對照 to\_string 的字元則會被刪除)  


> 
> SELECT TRANSLATE('123.45','0123456789','abcdefghij'), TRANSLATE('123.45','0123456789.','abcdefghijk') FROM DUAL


  

ADD\_MONTHS(d, n) 將 n 個月份加到日期 d 中，假如原來的日期代表的是該月份的最後一天，則調整的日期也是也會落在最後一天；若調整後的日期為無效日期，則調整後的天數會被落在最後一天。  


> 
> SELECT ADD\_MONTHS(TO\_DATE('2006/7/31','YYYY/MM/DD'),2) FROM DUAL  
> SELECT ADD\_MONTHS(TO\_DATE('2000/4/30','YYYY/MM/DD'),-2) FROM DUAL


  

MONTHS\_BETWEEN(d1, d2) 回傳在日期 d1 和 d2 之間的月份總數  


> 
> SELECT  MONTHS\_BETWEEN(TO\_DATE('1999/12/29','YYYY/MM/DD'),TO\_DATE('1988/12/29','YYYY/MM/DD')) FROM DUAL



> 
> SELECT MONTHS\_BETWEEN(TO\_DATE('2000/5/12','YYYY/MM/DD'),TO\_DATE('1988/12/29','YYYY/MM/DD')) FROM DUAL


  

回傳相對於日期 d 所在月份之最後一天日期  


> 
> SELECT LAST\_DAY(TO\_DATE('2000/2/12','YYYY/MM/DD')) FROM DUAL


  

ROUND(d [,format]) 將一個日期 / 時間值四捨五入到所指定之近的日期 / 時間單位  


> 
> SELECT  
> ROUND(TO\_DATE('2006/12/28 22:20:00','YYYY/MM/DD HH24:MI:SS')) DAY,  
> ROUND(TO\_DATE('2006/12/28 22:20:00','YYYY/MM/DD HH24:MI:SS'),'HH') HOUR,  
> ROUND(TO\_DATE('2006/12/28 22:20:00','YYYY/MM/DD HH24:MI:SS'),'YYYY')  
> YEAR FROM DUAL


  

NUMTODSINTERVA時間相加的函數(如對一個時間加上13秒) ，NUMTODSINTERVAL (n,char\_expr)，char\_expr支援：DAY, HOUR, MINUTE, SECODE  


> 
> SELECT SYSDATE,SYSDATE+NUMTODSINTERVAL(20,’DAY’) "系統日+20天" FROM DUAL;



> 
> SELECT SYSDATE,SYSDATE+NUMTODSINTERVAL(4,’HOUR’) "系統日+4小時" FROM DUAL;



> 
> SELECT SYSDATE,SYSDATE+NUMTODSINTERVAL(10,’MINUTE’) "系統日+10分鐘" FROM DUAL;



> 
> SELECT SYSDATE,SYSDATE+NUMTODSINTERVAL(30,’SECOND’) "系統日+30秒" FROM DUAL;

