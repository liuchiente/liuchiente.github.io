---
title: "Javascript 判斷身份證"
date: 2013-06-02T07:05:00Z
lastmod: 2020-03-13T15:10:11.589Z
tags: ['Javascript', 'HTML']
aliases:
  - /2013/06/javascript.html
draft: true
---


```
詳細的規則其他地方也找得到，在此就只講用最單純的作法來進行檢核格式正不正確，畢竟除了正
```

```
規表達式以外，這算是個比較精準的作法，不過實際上若要確保資料正確性，我們應該加入更多資
```

```
料來做交叉比對，才算是精進的作法。 
```

```
 
```

```
以下作法是寫成一個Function，得到輸入後，進行剖析，剖析後利用對應格式來做運算，符合運算
```

```
結果後當然是決定格式正不正確囉！ 
```
`function idCheck(src)  
{  
 if(src.match('^[a-zA-Z]{1}[1-2]{1}[0-9]{8}$')===null) return false;  
 var srcMap=src.split("");  
 var head=[1,10,19,28,37,46,55,64,39,73,82,2,11,20,48,29,38,47,56,65,74,83,21,3,12,30];  
 var result=0;  
 var idx=srcMap[0].charCodeAt()-('a').charCodeAt();  
 if(srcMap[0].charCodeAt()<('a').charCodeAt()) idx=srcMap[0].charCodeAt()-('A').charCodeAt();  
 for(i=1;i<8;i++)  
 result=result+(srcMap[i]*(9-i));  
 result=result+(srcMap[8]-0)+(srcMap[9]-0)+(head[idx]-0);  
 return ((result%10)===0)  
}` 