---
title: "owner管理"
date: 2013-06-13T16:09:00Z
lastmod: 2020-03-13T15:10:11.234Z
tags: ['未分類']
aliases:
  - /owner.html
draft: true
---

drop user BESTTEST cascade;  
  
CREATE USER BESTTEST IDENTIFIED BY BESTTEST   
 DEFAULT TABLESPACE TBCBEST TEMPORARY TABLESPACE TEMP   
 PROFILE DEFAULT ACCOUNT UNLOCK;  
  
GRANT CONNECT TO BESTTEST;  
GRANT DBA TO BESTTEST;  
GRANT RESOURCE TO BESTTEST;  
GRANT UNLIMITED TABLESPACE TO BESTTEST;  
ALTER USER BESTTEST DEFAULT ROLE ALL;  
  
grant create table to BESTTEST;  
grant create view to BESTTEST;  
grant create sequence to BESTTEST;  
grant create table to BESTTEST;  
grant create view to BESTTEST;  
grant create sequence to BESTTEST;  
Grant Select Any Table to BESTTEST;  
grant Insert Any Table to BESTTEST;  
grant Update Any Table to BESTTEST;  
grant Delete Any Table to BESTTEST;  
grant Select Any Sequence to BESTTEST;  
grant Create Any View to BESTTEST;  
grant Create Any Table to BESTTEST;  
grant Drop Any Table to BESTTEST;  
grant Create Any Index to BESTTEST;  
exit;