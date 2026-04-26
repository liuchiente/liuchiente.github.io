---
title: "REST Service"
date: 2013-06-27T16:29:00Z
lastmod: 2020-03-13T15:10:10.895Z
tags: ['RESTFull', 'REST', 'PHP', 'Web Service', 'RESTLike']
aliases:
  - /2013/06/rest-service.html
draft: true
---

在實現資料交換的過程中，我們常會需要去制定一些交換規則，一切的一切都是為了實現異質系 統的資料交換，畢竟今天一個系統PHP，另一個系統JSP，系統就是不同，總不會要用彼此的陣列 丟來丟去吧？  
  
在基於HTTP之上，舊有的作法我們會使用GET跟POST，由於FORM的關係，大眾對這兩 個METHOD會相對比較有所認識，當然也常用在AJAX的傳遞。 資料交換的方式有很多，有人使用SOAP協定，有人用XML-RPC或是JSON-RPC，在Server to Server 的情況下，也許有時候RPC會有點問題，但是這些方法還是依舊常被用來解決資料交換的議題，但 是當遇到APP的需求時，就很難講了，畢竟還要看app這個client端是否有足夠的資源來處理web s ervice資料剖析跟組合的問題，對於這個需求我是採用REST架構來解決。  
  
 不同於XML-RPC或是SOAP，REST充其量只是一個概念或一個架構，而非一個協定，當然也有人說傳 統的網址用來GET跟POST就很Happy了，搞那麼多幹麼，這種話就不用多說了，人生爽就好。  
  
 Representational State Transfer (REST)，在這概念底下，我們應該是以資源為概念來觀察整個 網路資源，而每個資源都用URI來表達，而每個URI都有自己的狀態，使用者不斷地切換操作各個URI 會造成資源的狀態改變，所以我們稱為表徵狀態轉移，對於REST的幾個特色，簡單講一下；   
* *資源以URI呈現。*
* *對資源操作需要有CURD(建立、修改、讀取、刪除)，正好對應HTTP的GET、POST、PUT、DELETE。*
* *透過操作資源的表現形式來操作資源。*
* *資源表現以XML或是HTML，這方面很自由，json也可。*

  
 對於REST的資源表現，其實實做起來我會有點想偷懶，哈，但是若作到RESTful，則資源的可研讀 性相對會好很多例如以下；  
  
 http://jdliu.example.com/resources/ 這是一組資源，  
  
就這呈現來說，GET或POST這個URI，我們可以對這路徑底下的所有資源進行取得或 是加入資料。  
  
 http://jdliu.example.com/resources/123456/ 這是一個資源，  
  
就這呈現來說，GET或POST這個URI，我們可以對這資源進行取得或是加入資料。 也就所以說，當你GET　http://jdliu.example.com/resources/123456/，你應該可以取得這個資源 的相關資料，當你下Delete http://jdliu.example.com/resources/123456/，你應該可以刪除這個 資源。  
  
 而GET或是DELETE，應該取決於你所下的HTTP request method，他會放在你所發出的request中， Server對於接收的資源操作命令，應該取決於Http request中，這些相關資訊，而不是取決於這個 URL的進入點。 當然...為了更好讀，更方便組合，我會把它作成是RESTlike形式而已，例如以下；  
  
 http://jdliu.example.com/resources/get/123456/?param=98765 一切只是因為...我想要好看一點，而param只是因為我不想做太多層，但不失是一個好方法就是。