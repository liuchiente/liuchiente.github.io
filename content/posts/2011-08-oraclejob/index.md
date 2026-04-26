---
title: "Oracle處理JOB之語法"
date: 2011-08-22T13:46:00Z
lastmod: 2020-03-13T15:10:14.082Z
tags: ['PL/SQL', 'Database', 'JOB', 'Oracle', 'SQL']
aliases:
  - /2011/08/oraclejob.html
draft: true
---

關於JOB相信大家都對他有相當的認知，所謂的JOB呢，就是工作，透過Oracle提供  
  
的JOB功能，我們可以針對資料庫內之資料，進行固定性的處理，因為JOB主要就是  
  
做好排程後，再依照設定的時段，來進行所指定的處理，一般來說，我們可以透過套裝  
  
工具，可能TOAD或是其他不同的軟體來進行JOB的新增修改，當然我們也可以透過語法  
  
來做這樣的處理。  
  
  
  
在一般命令模式，我們可以透過dbms\_job.submit這個函式來做job的新增。  

```
**delcare job\_num number;begin dbms\_job.submit( job\_num　該部份函式會自動給值 , '內容;' 預定要排程處理的內容，記得結尾加上分號 , SYSDATE + n 第一次執行該job的時間, 一分鐘為 1 / 1440，一小時為1/24 , 'SYSDATE + m' 下一次執行時間, 'NULL' 表示只執行一次，請特別注意該參數必須為字串 , FALSE );end;**
```
  
如果你要確認JOB是否新增完畢，可以直接去看dba\_jobs這個資料表，記得過濾owner。  
  
**select \* from dba\_jobs;**  
  
如果要移除JOB，請先找出該JOB的編號，並且引用函式來進行處理。  

```
**begin dbms\_job.remove(job\_num );end;**
```
  
雖然10G已經有了新玩意，但其實這些函式還是很實用的。