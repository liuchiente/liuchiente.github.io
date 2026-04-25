---
title: "計算機網路 | 修課心得"
date: 2021-07-13T19:11:00Z
lastmod: 2024-11-22T09:22:53.776Z
tags: ['中興大學資工所', '學習紀錄', '上課學習心得']
aliases:
  - /2021/07/blog-post_13.html
draft: false
---

本學期修習了三門課程, 其中兩門會有比較多的實際操作, 除了[嵌入式系統原理與應用外](https://liuchien.ink.tw/%e4%b8%ad%e8%88%88%e5%a4%a7%e5%ad%b8%e8%b3%87%e5%b7%a5%e6%89%80/%e5%b5%8c%e5%85%a5%e5%bc%8f%e7%b3%bb%e7%b5%b1%e5%8e%9f%e7%90%86%e8%88%87%e6%87%89%e7%94%a8-%e8%aa%b2%e7%a8%8b%e5%9b%9e%e9%a1%a7/), 另一門就是這篇文提到的計算機網路了, 就這門課來說, 主要著重在講解網路的基礎知識, 包含最基本的網路傳輸介質、架構, 再到TCP/IP協定, 最後到DNS概念和原理等。

當然,這門課整體來說會比較乏味一點,但乏味的主因是題材, 而不是授課內容, 要想想, 如果對著你解釋TCP的各個Filed分別代表甚麼意思, 又或著跟你講解Slow-start的變動流程, 如果你本身對網路就沒太多興趣的話, 自然就不容易聽的津津有味, 除非你可以自己找點樂子, 譬如聯想看看, 在[CDN](https://zh.wikipedia.org/zh-tw/%E5%85%A7%E5%AE%B9%E5%82%B3%E9%81%9E%E7%B6%B2%E8%B7%AF)架構底下, 若你的看片口味跟別人不一樣, 自然就會覺得影片載入的速度特別慢。

![netflix on an imac](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/07/pexels-photo-5082566.jpeg?w=525&ssl=1)Photo by cottonbro on [Pexels.com](https://www.pexels.com/photo/netflix-on-an-imac-5082566/)在開始這課程之前, 你可以做一些準備, 這些資訊可以在搜尋引擎上輕易找到, 譬如像是常見網路架構, 中華電信提供的ADSL和第四台業者提供的Cable Modem是甚麼東西? 再來則是網路5層的定義, HTTP協定和TCP/IP協定的介紹等, 若是你對上述內容有一定了解的話, 自然可以跟上老師的腳步。

而課程的主要內容大概會分為幾個綱要: 首先是介紹常見的網路架構, 譬如區域網路、網域網路，甚至到衛星網路等, 他們之間有甚麼差異, 再到網路傳輸介質, 常見網路介質可能是雙絞線、銅線和光纖等, 這裡提到的網路, 其實跟你熟知的上網那種網路不完全相同, 因為你平常接觸的網路是屬於多媒體內容, 而這裡要告訴你的是我們應該怎麼用機器去傳輸這些內容, 將他從世界上其他不同地方蒐集起來, 塞到你手上的小盒子裡。

整體來說, 課程的介紹方式會由粗到細, 先告訴你這個世界的網路有多大, 再往下解釋E-Mail的機制, 是透過 SMTP、POP3來傳遞資料, 再來則是HTTP協定, 怎麼讓你可以看到網頁。

![white printed label](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/07/pexels-photo-2611877.jpeg?w=525&ssl=1)Photo by Ethan Wilkinson on [Pexels.com](https://www.pexels.com/photo/white-printed-label-2611877/)講完HTTP之後, 則會開始解釋TCP和UDP差異, 相信到這裡, 如果你沒有相關工作經驗, 應該會覺得眼花撩亂, 但其實你只要記得, 這些協定, 不過就是確保機器和機器之間有共同的溝通方式和溝通語言罷了。

在講完TCP和UDP, 也會提到DNS的應用, 如何建置CDN, CDN是確保你可以好好看Netflix的好東西, DNS則是你上網的必備步驟, 打上一串網址, 然後進到那個網站。

當然, 上面講的都是屬於應用層的事情, 要記得前面我講過, 課程也會介紹到網路5層, 這5層分別負擔一些工作, 讓你的資料可以從你的應用程式, 怎麼到網路卡轉成電器訊號, 傳到另外一台電腦上。

而除了這些事情以外, 也會開始講解TCP如何進行3-Way HANDSHAKE, 4-WAY HANDSHAKE等等, 如果你要說聽到這裡就好…那我跟你說好沒好透呢!

後面則是TCP如何進行Slow-start, 再到 Router如何進行封包轉送, 如何把封包存在Queue裡面,當Queue滿了之後又會發生甚麼事情?最後則是NAT的概念和SDN的簡單介紹。

當然不能忘記怎麼切子網路, 還有Longest prefix match。

如果你要問我這是一門甚麼樣的課程, 那我會告訴你, 你每一次上課都不該錯過, 因為老師講課是會環環相扣, 在起初起初會講一個現象, 再細細往下解析為什麼會造成這個現象, 譬如說Router的Packet loss是怎麼造成, 又為什麼會發生這種狀況。

![unrecognizable hacker with smartphone typing on laptop at desk](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/07/pexels-photo-5935791.jpeg?w=525&ssl=1)Photo by Sora Shimazaki on [Pexels.com](https://www.pexels.com/photo/unrecognizable-hacker-with-smartphone-typing-on-laptop-at-desk-5935791/)*在這門課程中, 老師講課方式相當明快, 且有一定節奏感, 但不會漏掉任何細節, 對我來說, 聽老師講課是相當有趣, 就像在陳述相當龐大的故事, 即便我曾在大學時修過相關課程, 但我仍深感到收穫良多, 也感謝老師細心準備課程, 讓我能夠快速吸收。*

而在最前面提到,這門課也有一些實務內容, 但不外乎就是利用Wireshark來擷取封包, 幫助你更快了解實際網路裡面在收送封包是怎麼一回事, 大概會收到甚麼樣的封包這一類的, 這些都會歸在上課實作中, 一學期結束, 大概會有5次的實作要完成, 而除了這個以外, 還有回家作業, 也是幫助你更瞭解上課內容, 約莫也是有5次的回家作業要完成。

至於其他程式面的東西就沒有了, 這課程多著重在理論和實際操作, 跟程式較無關係, 以上, 希望對即將修課的你有所幫助。

