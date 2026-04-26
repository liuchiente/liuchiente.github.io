---
title: "untitled"
date: 2014-10-07T05:45:00Z
lastmod: 2020-03-13T15:10:09.895Z
tags: []
aliases:
  - /untitled.html
draft: true
---

`GO IF OBJECT_ID('nextValue', 'P') IS NOT NULL DROP PROC nextValue; GO IF OBJECT_ID(N'NextSequence',N'U') IS NOT NULL DROP TABLE NextSequence; BEGIN CREATE TABLE NextSequence( SequenceName VARCHAR(8), SequenceNow INT, SequenceMax INT ); END GO CREATE PROCEDURE nextValue @VA INT OUTPUT, @ValType VARCHAR(8), @ValMin INT=1, @ValMax INT=99999 AS SET NOCOUNT ON BEGIN DECLARE @RowCheck INT=0; SELECT @RowCheck=(SELECT count(1) FROM NextSequence WHERE SequenceName=@ValType); IF @RowCheck=0 INSERT INTO NextSequence(SequenceName,SequenceNow,SequenceMax) VALUES(@ValType,@ValMin,@ValMax); ELSE UPDATE NextSequence SET SequenceNow=SequenceNow+1 WHERE SequenceName=@ValType; SELECT @VA=SequenceNow FROM NextSequence WHERE SequenceName=@ValType; END RETURN; GO` 