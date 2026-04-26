---
title: "ISBN判別函式 for PHP"
date: 2011-05-21T06:14:00Z
lastmod: 2020-03-13T15:10:14.677Z
tags: ['ISBN', 'PHP']
aliases:
  - /2011/05/isbn-for-php.html
draft: true
---

10碼的ISBN,將第一個數字開始,乘以10,第二個數字乘以9,以此類推乘到1,加起來總和,如果mod 11後為0,就是正確的,13碼的ISBN,從第一個數字開始,乘以1,第二個數字乘以3,以此類推,乘到第12個數字後,總和加上第13個數字,如果可以被10 mod,該ISBN則是正確的.  
  
[php]  
function ISBN\_TEST($ISBN)   
{   
 $ISBN=explode("-",$ISBN);  
 $ISBN=implode("",$ISBN);   
 if(strlen($ISBN) == 10 )  
 {  
 $sum = 0 ;  
 for($index=0 ; $index < strlen($ISBN) ; $index++)  
 {  
 $char = substr($ISBN,$index,1);  
 if($char == ‘x’|| $char == ‘X’)  
 {  
 $char = 10;  
 }  
 $sum += $char \* (10-$index);  
 }  
 if( ($sum % 11)!= 0 ){  
 return false;  
 }else{  
 return true;  
 }  
 }  
 elseif(strlen($ISBN==13))  
 {  
 for($index=0 ; $index < strlen($ISBN)-1 ; $index++)  
 {  
 $char = substr($ISBN,$index,1);  
 if($index%2==0)  
 {  
 $sum=$sum +$char;  
 }  
 elseif($index%2!=0)  
 {  
 $sum=$sum +$char\*3;  
 }   
 }  
 $sum=$sum+substr($ISBN,12,1);  
 if( ($sum % 10)!= 0 ){  
 return false;  
 }else{  
 return true;  
 }  
 }  
 else  
 {  
 return false;  
 }  
}  
[/php]