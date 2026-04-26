---
title: "Twig template engine 新一代的PHP樣板引擎之區塊與巨集"
date: 2012-10-31T14:08:00Z
lastmod: 2020-03-13T15:10:11.992Z
tags: ['Twig', 'PHP']
aliases:
  - /2012/10/twig-template-engine-php.html
draft: true
---

繼之前的樣板描述語言解釋完畢之後，這裡要介紹一下一般使用上會比較少用到的樣板相關技術，當然這些技術，並不是專指Twig獨有的，但是仍有些許概念，在Twig上的使用較為方便，不過實際上還是看使用者在運用上的習慣度。  
  
有些人會說，樣板引擎本身就是擔任呈現的工作，那對於呈現的角度來說，理當是越簡單越好，盡量不要有程式的邏輯，以免後續變更頁面呈現時，所需要耗費的功夫變得更為複雜，理想上，商業邏輯的運算，應該包含在後端程式即可，而不需要耗費在頁面進行任何處理；這樣說也許是對，如果是單純把樣板引擎的功能視為變數呈現，的確是如此沒錯，但今天如果是把樣板引擎視為View端的一個重要媒介，單純的變數呈現，就太大材小用了。  
  
於是乎，首先講到的是區塊的規劃，在開始之前，必須強調的是，Smarty也有所謂的區塊規劃功能，如果是Smarty愛用者，也可以考慮用區塊規劃來變化一下呈現思維。  
  
如同DIV一樣，區塊的概念是將頁面邏輯劃分成一塊又一塊的區域，並且可以針對這些區塊進行操作，但是要切記一點是，區塊規劃好後，是呈現縱向的規則，如果要變更區塊呈現順序，則只能在本身頁面所規劃的區塊進行操作。  
  
區塊的語法如下；  
  
 `{% block testblock %}  
  
do something...  
  
  
{% endblock %}`  
  
如同上面規劃，我們規劃了一個區塊名叫testblock，當然你可以在裡頭的do something加上任何有效操作，一個表格、一個變數、一個表單或一個頁面，也就所以在一個完整的頁面規劃中，也許我們可以看到以下的作法。  
  
 `{% block head %}  
  
title  
  
{% endlbolock %}  
  
{% block body %}  
  
describes  
  
{% endlbolock %}  
  
{% block tail %}  
  
foot  
  
{% endlbolock %}`  
  
當然這樣的作法並不足為奇，對區塊來說，還需要搭配上繼承的功能，英雄才略顯有用武之地，回顧一下先前的規劃。  
  
 `#parent.html  
  
{% block head %}  
  
title  
  
{% endlbolock %}  
  
{% block body %}  
  
describes  
  
{% endlbolock %}  
  
{% block tail %}  
  
foot  
  
{% endlbolock %}`  
  
 `#child.html  
{% extends "parent.html" %}`  
  
從上述的例子可以看到，父樣板宣告了一堆區塊後，在子樣版只要經由extends繼承，br />就可以得到父樣板的相關排版。  
  
如此一來的好處是什麼呢？我們可以規劃一個完整的架構當作父樣板，也就是類似基底  
類別，而且可以透過控制區塊裡面的來決定是否呼叫父類別的區塊內容，用法如下；  
  
`#child.html  
{% extends "parent.html" %}`  
  
`{% block tail %}  
  
{{ parent()}}  
  
{% endlbolock %}`  
  
又或者是呼叫父區塊後，繼續我們的其他覆寫。   
  
`#child.html  
{% extends "parent.html" %}   
{% block tail %}  
  
{{ parent()}}  
 do something...  
  
  
{% endlbolock %}`  
也就所以，我們可以透過繼承和覆寫的動作，來有效管控前端畫面的呈現，在這樣的作法之下，我們可以減少把頁面切割後又進行FETCH取回的複雜手續，因為透過區塊控制，我們就可以處理好頁面上不同元素的呈現了，當然哪個好用，就隨個人喜好了，因為在使用繼承的過程中，也是可以把頁面進行抽象化的動作，然後進行擴充。