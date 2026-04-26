---
title: "查資料庫連線Lock"
date: 2019-11-22T16:13:00Z
lastmod: 2020-07-25T01:42:35.821Z
tags: ['Lock.Kill Session', '資料庫', '工程師', 'Oracle', 'SQL']
aliases:
  - /2020/03/lock.html
draft: true
---

  


```
SELECT sys.v_$session.osuser,sys.v_$session.machine,v$lock.SID,
sys.v_$session.serial#,
DECODE(v$lock.TYPE,
'MR', 'Media Recovery',
'RT','Redo Thread',
'UN','User Name',
'TX', 'Transaction',
'TM', 'DML',
'UL', 'PL/SQL User Lock',
'DX', 'Distributed Xaction',
'CF', 'Control File',
'IS', 'Instance State',
'FS', 'File Set',
'IR', 'Instance Recovery',
'ST', 'Disk Space Transaction',
'TS', 'Temp Segment',
'IV', 'Library Cache Invalida-tion',
'LS', 'Log Start or Switch',
'RW', 'Row Wait',
'SQ', 'Sequence Number',
'TE', 'Extend Table',
'TT', 'Temp Table',
'Unknown') LockType,
RTRIM(object_type) || ' ' || RTRIM(owner) || '.' || object_name object_name,
DECODE(lmode, 0, 'None',
1, 'Null',
2, 'Row-S',
3, 'Row-X',
4, 'Share',
5, 'S/Row-X',
6, 'Exclusive', 'Unknown') LockMode,
DECODE(request, 0, 'None',
1, 'Null',
2, 'Row-S',
3, 'Row-X',
4, 'Share',
5, 'S/Row-X',
6, 'Exclusive', 'Unknown') RequestMode,
ctime, BLOCK b
FROM v$lock, all_objects, sys.v_$session
WHERE v$Lock.SID > 6
AND sys.v_$session.SID = v$lock.SID
AND v$lock.id1 = all_objects.object_id;

```


```
ALTER SYSTEM KILL SESSION '787,8659' ;

```
