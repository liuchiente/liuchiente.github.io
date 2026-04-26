---
title: "SQL:Update one table from another table"
date: 2010-03-29T05:39:00Z
lastmod: 2020-03-13T15:30:06.798Z
tags: ['資料庫', 'Database', '工程師', 'Oracle', 'SQL']
aliases:
  - /2010/03/sqlupdate-one-table-from-another-table.html
draft: false
---

  

  

還是要說update one table using another table都好吶，最近太少在專業議題上嘴砲大家了，所以放了一些自己寫的小品文來矇混，朕真是太聰明了，廢話不多說，雖然還是說了很多，今天要說的是，怎麼樣利用一個資料表，來更新另一個資料表。  

  

  

  

  

  

利用tableB及tableA關聯相關條件，更新tableA，Oracle適用。  

  


> 
> **update tableA****set tableA.cols1=null****where exists****(****SELECT a.cols1 FROM tableA A,tableB B****WHERE A.cols3=B.cols3 AND****A.cols2 IS NULL AND****cols1=to\_date('2009/09/11','yyyy/mm/dd')****)**


  

以下語法有相同功能，但Oracle不適用，嘖嘖。  

  


> 
> **update A****set A.cols1=null****FROM tableA A,tableB B****WHERE A.cols3=B.cols3 AND****A.cols2 IS NULL AND****cols1=to\_date('2009/09/11','yyyy/mm/dd')**


  

利用tableB及tableA關聯條件，取tableB資料更新tableA，Oracle適用  

  


> 
> **update tableA****set tableA.cols1=(Select cols1 from tableB)****where exists****(****SELECT a.cols1 FROM tableA A,tableB B****WHERE A.cols3=B.cols3 AND****A.cols2 IS NULL AND****cols1=to\_date('2009/09/11','yyyy/mm/dd')****)**


  

利用tableB及tableA關聯條件，取tableB資料更新tableA，Oracle不適用。  

  


> 
> **update A****set A.cols1=B.cols1****FROM tableA A,tableB B****WHERE A.cols3=B.cols3 AND****A.cols2 IS NULL AND****cols1=to\_date('2009/09/11','yyyy/mm/dd')**

