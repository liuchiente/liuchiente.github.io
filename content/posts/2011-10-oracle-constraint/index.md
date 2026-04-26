---
title: "Oracle Constraint的管理"
date: 2011-10-05T02:58:00Z
lastmod: 2020-03-13T15:10:13.679Z
tags: ['Database', 'Oracle', 'SQL']
aliases:
  - /2011/10/oracle-constraint.html
draft: true
---

所謂Constraint不外乎PK、FK和UK，或是在規劃時期提到的CK，以下簡略敘述；  
  
  
  
PK:Primary Key，也就是一個Table的主鍵，這主鍵每筆都應該是唯一的，而主鍵只能存在一組，也有些人的界定是只能有一個，但是一個組合是獨一無二的，似乎跟一個欄位是獨一無二，這兩者之間應該不會有太大落差，相對的，若是以組合的概念來看，PK的彈性可以更大。  
  
UK：Unique Key，簡單來說，就是唯一值，這個欄位的值，在這資料庫裡面必須是唯一一筆的，但是不同於PK，UK是可以設定多個欄位的。  
  
FK：Foreign Key，這在關聯式資料庫概念會大量使用到，也就是所謂外來鍵，資料表內可定義外來鍵，並且與其他資料表的欄位建立起關聯，也就是說本欄位將會參照其他資料表的欄位，接下來在資料表的異動時，系統將必須關聯異動參照的欄位值，避免造成多個資料表異動後的資料落差問題。  
  
針對單一table啟用constraint  
  
`Alter Table TableName enable contraint ConstraintName`  
  
針對單一table停用constraint  
  
`Alter Table TableName disable contraint ConstraintName`  
  
針對單一table新增constraint  
  
`Alter Table TableName add contraint ConstraintName`  
  
移除單一table的constraint  
  
`Alter Table TableName drop contraint ConstraintName`