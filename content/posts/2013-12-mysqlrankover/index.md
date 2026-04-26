---
title: "MySQL中實現Rank　Over"
date: 2013-12-18T15:28:00Z
lastmod: 2020-03-21T00:16:27.124Z
tags: []
aliases:
  - /2013/12/mysqlrankover.html
draft: true
---

習慣使用Oracle統計資料的人大多知道rank over這個函式，rank over這個函式的好處是可以把你抓出來的 資料集進行分組，然後排序進行分組排名，譬如以下資料好了。 no number 1 28 2 33 3 33 4 89 5 99 我們可能需要他呈現以下的效果。 no number rank 5 99 1 4 89 2 2 33 3 3 33 4 1 28 5 但是MySQL並沒有所謂的rank over，所以當我習慣下這語法時，系統只有默默的跳出錯誤給我，但是為了達 到部份功能，還是需要使用者變數來實現。  

`SELECT no, number, rank FROM (SELECT tmp.no, tmp.number, @rank := @rank + 1 AS rank FROM (SELECT no, number FROM table ORDER BY score desc) tmp, (SELECT @rank := 0) count) RESULT;` 