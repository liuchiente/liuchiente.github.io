---
title: "Twig template engine 新一代的PHP樣板引擎之樣板語言"
date: 2012-04-15T23:04:00Z
lastmod: 2020-03-13T15:10:12.398Z
tags: ['Language', 'Template Engine', 'Twig', 'PHP', 'HTML']
aliases:
  - /2012/04/twig-template-engine-php.html
draft: true
---

  
繼安裝及在MC端的基本使用講解完畢之後，本篇則是著重在Twig樣板語言的使用，在此之前，再重複一次Twig的幾種標籤，不同Smarty只有一種標籤格式，Twig定義了三種標籤格式來進行排版。  
  
  
{% action %}：舉凡是邏輯判斷、邏輯控制或是資料操作，都是使用這個標籤來定義。  
  
{{var }}：舉凡是單一資料或是個體的呈現，都是使用這個標籤來定義，會特別強調是個體，是因為Block的呈現也是使用{{ ... }}定義。  
  
{# commet #}：舉凡是註解，都是只用這個標籤來定義，有個好處是，我們可以先用這標籤來把測試中想先忽略的 code註解掉，或是在冗長的樣板頁面中加入說明。  
  
解釋完標籤的基本定義後，以下來解釋一般排版會常用到的標籤語法。  
變數與陣列  
  
  
誠如第一篇所介紹的，我們在使用render或是display來傳遞變數時，會使用一個陣列來包含我們所要傳遞的變數，假定說在MC端的Code是如此撰寫；  
  
 `/*  
  
MC.php  
  
*/  
  
$template->render(array('Banana'=>$Banana,'Kiwi'=>$Kiwi));`  
  
在樣版我們的取用將會如下；  
  
 `#View.html  
  
<ul>  
  
<li>{{Banana}}</li>  
  
<li>{{Kiwi}}</li>  
  
<li></li>  
  
</ul>`  
  
好的，以上我們已經理解，如何傳遞變數到樣板上，一切是這麼直覺，雖然從Smarty慣用者的角度來看，似乎多了一道工序，但是換個方向來看，其實作法也是大同小異，當然今天我們不會單單只是一個一個變數慢慢傳到樣板，更多時候，我們會因為便於管理，或是其他原因，而選擇傳達一個陣列到樣板。  
  
 `/*  
  
MC.php  
  
*/  
  
$template->render(array('pack'=>array('Banana'=>$Banana,'Kiwi'=>$Kiwi),'girlfriend'=>array('Miffy'=>$Miffy,'Angel'=>$Angel)));`  
  
當然這樣有可能很眼花撩亂，當我們的pack有很多種水果，或是不巧我們的girlfriend有很多個(I hope so...)，我們可以改個寫法；  
  
 `/*  
  
MC.php  
  
*/  
  
$pack=array('Banana'=>$Banana,'Kiwi'=>$Kiwi);  
  
$girlfriend=array('Miffy'=>$Miffy,'Angel'=>$Angel);  
  
$template->render(array('pack'=>$pack,'girlfriend'=>$girlfriend));`  
  
在樣版我們的取用將會如下；  
  
 `#View.html  
  
<ul>  
  
<li>{{pack.Banana}}</li>  
  
<li>{{pack.Kiwi}}</li>  
  
<li>{{girlfriend['Miffy']}</li>  
  
<li>{{girlfriend['Angel']}</li>  
  
</ul>`  
  
從以上範例我們就可以清楚發現，我們取用陣列的資料，只要使用「.」來取用就好，是不是很實用？當然也可以用[]來取用，但是盡量不要將「.」完全認定是物件取用屬性，因為這樣一來你容易把陣列跟物件搞混了，當然假定你今天傳過來的是物件，也可以用「.」來取用。  
  
 `/*  
  
MC.php  
  
*/  
  
$pack=new pack();  
  
$template->render(array('pack'=>$pack));`  
  
在樣版我們的取用將會如下；  
  
 `#View.html  
  
<ul>  
  
<li>{{pack.method}}</li>  
  
<li>{{pack.property}}</li>  
  
</ul>`  
  
這樣比較下來[]和. 似乎是沒有差異的，對於陣列上的應用，的確是如此，但是往內探究就有差異了，以pack.lemon和pack['lemon']，會有以下差異。  
  
pack['lemon']  
  
樣板將會檢查pack這變數在來源的php是不是合法的陣列，而lemon是不是該陣列合法的索引。  
  
如果不存在則回傳NULL。  
  
pack.lemon  
  
檢查pack是不是合法陣列，lemon是不是該陣列合法索引，是則回傳。  
  
否則檢查pack是不是合法物件，lemon是不是該物件屬性property，是則回傳。  
  
否則檢查pack是不是合法物件，lemon是不是該物件方法method，是則回傳，即使lemon是建構子也會檢查。  
  
否則檢查pack是不是合法物件，該物件是不是存在getLemon，是則回傳。  
  
否則檢查pack是不是合法物件，該物件是不是存在setLemon，是則回傳。  
  
假定以上都不成立，好吧，那我們就回傳NULL。  
過濾器的使用  
  
  
filter的使用，Smarty裡面並不是沒有，只是使用的頻繁度到哪邊而已，對於過濾器，簡單來說，就是要修飾變數的呈現，因為樣板引擎會把我們的變數內容忠實地用原始字串呈現，因此必要時刻，當來源資料不方便更動，或是其他情況，我們可以用過濾器更快速改變我們的呈現方式，以下則簡略列出幾個比較常使用的過濾器。  
  
`{{'I have a %s' and a %s|format('big head','big mouth')}}`  
  
其實就是格式化輸出。  
  
`{{'I have a bighead'}|upper}`   
  
使用upper則是把字串大寫，現在真的有一個大頭了。  
  
當然我們也可以同時使用多個過濾器，透過 | 的分隔，這將成為一個chain，執行時將會依次執行。  
  
`{{'I have a %s' and a %s|format('big head','big mouth')|upper}}`  
  
當然過濾器也可以使用參數，而實際上使用的話，則端看你過濾器的設定，譬如我們使用了如PHP函數裡面的join來呈現。  
  
`{{　list|join(',')}}`  
  
關於過濾器的使用，我們也可以用區塊式的定義方式，來操作更大範圍的資料內容。  
  
 `{%filter upper%}  
  
good morning,I'm John,how are you?  
  
{%endfilter%}`  
新增變數與賦值  
  
  
在樣板裡面我們可能會用到的另一個常用標籤，就是變數賦予，因為我們可能需要把MC端傳來的變數加以解析，放到我們想要的地方，用我們自己的玩法來使用，於是乎我們可以用set來新增變數及給值。  
  
 `#View.html  
  
{% set Apple=1 %}  
  
{% set Apple=array[1,2,3]%}  
  
{% set Apple={'bug':'Qmmm','butterfly':'db'}%}`  
流程控制  
  
  
控制流程的話，以下就先介紹常用的兩種控制流程，分別是判斷跟迴圈，判斷的話即是一般判斷使用 ，毫無意外的，就是在你的判斷內容加入你要呈現的資訊，也許是字串，又或許是其他相關流程。  
  
`{%if head < mouth%}  
  
really ?  
  
{%endif%}`  
  
也有可能是呈現變數。  
  
`{%if head < mouth%}  
  
{{head}}  
  
{%endif%}`  
  
當然啦，你在判斷式也可以加入過濾器來協助判斷。  
  
`{%if road|length>40%}  
  
try another day,please...  
  
{%endif%}`  
  
如果是完整一點的邏輯判斷，加入else。  
  
`{%if road|length>40%}  
  
try another day,please...  
  
{%esle%}  
  
okay,let's go.  
  
{%endif%}`  
  
甚至是多個流程判斷。  
  
 `{%if road|length>40%}  
try another day,please...  
{%esleif road|length>30%}  
let me think...  
{%else%}  
okay,let's go.  
{%endif%}`  
  
另外迴圈的話，使用上會跟foreach的用法一樣，只是在這裡我們使用For  in。  
  
 `{%for cuteduck in duck%}  
{{cuteduck}}  
{%endfor%}`  
  
當然迴圈也可以預加判斷，避免多作迴圈流程。  
  
 `{%for cuteduck in duck　if duck.exists%}  
{{cuteduck}}  
{%endfor%}`  
  
或是在迴圈沒有任何資料可以呈現時，呈現相關資訊。  
  
 `{%for cuteduck in duck　if duck.exists%}  
{{cuteduck}}  
{%else%}  
no ducks anymore.  
{%endfor%}`