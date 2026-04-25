---
title: "數位系統測試 | 修課心得"
date: 2021-07-22T17:26:00Z
lastmod: 2024-11-22T09:22:53.543Z
tags: ['中興大學資工所', '學習紀錄', '上課學習心得']
aliases:
  - /2021/07/blog-post_22.html
draft: false
---

先前提到, 本學期修了三門課, 除了[計算機網路](https://liuchien.ink.tw/中興大學資工所/計算機網路-修課心得/), [嵌入式系統原理與應用](https://liuchien.ink.tw/中興大學資工所/嵌入式系統原理與應用-課程回顧/)之外, 再來就是數位系統測試了, 之所以最後才提這門課, 是因為我對這門課的掌握度最低, 起初, 剛聽完第一堂課時, 我就覺得我聽不懂。

後來又聽了第二堂, 猛然發現, 我真的聽不懂！？

事後我思考一下我的問題, 主要應該是因為我離開校園之後, 這十多年來接觸的都是High Level的軟工作(相比底層硬體邏輯), 所以猛然一接收到這課程的相關知識時, 就總有一個很大的Gap在前面等我。

![back view of a woman standing on brown wooden planks](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/07/pexels-photo-1232594.jpeg?w=525&ssl=1)Photo by Kilian M on [Pexels.com](https://www.pexels.com/photo/back-view-of-a-woman-standing-on-brown-wooden-planks-1232594/)那股仰望高山的感覺, 真的很難形容啊！

不過事情總是會有轉機的, 回到正題, 這門課主要是在講解如何針對積體電路、記憶體等做測試工作, 為什麼呢？因為我們每一個晶片做出來, 裡面其實包含了很多電路、記憶體, 而為了確保晶片可以如我們設計一樣好, 就要去進行驗證, 驗證看是否有什麼異常, 會不會有線路錯誤。

在進入這門課程之前, 你應該先掌握一些事情, 譬如AND、OR等的基本邏輯閘, 再來就是[正反器](https://zh.wikipedia.org/wiki/触发器), 除此以外, 你還需要了解[序向邏輯電路](https://zh.wikipedia.org/wiki/时序逻辑电路), 有了這些基礎知識後, 你就更能進入這個世界, 如果可以, 就再多看一下積體電路的介紹, 記憶體如何讀寫。

![board chip circuit circuit board](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/07/pexels-photo-459411.jpeg?w=525&ssl=1)Photo by Pixabay on [Pexels.com](https://www.pexels.com/photo/board-chip-circuit-circuit-board-459411/)而在這門課程中, 主要是介紹Fault model (假定會發生錯誤的狀況, 定義何謂沒有錯誤), Fault simulation 錯誤模擬, Design for testability, 來衡量測試計畫的可靠性, 而且你會學到, Fault coverage(錯誤涵蓋率), 將是用來衡量你的設計是否可以找出最多錯誤的好方法, 接著往下就有很多方法來改善Fault coverage了, D-Algorithm、PODEM都會是這裡的重點, 後面則是BIST和Boundary Scan, 講完這些後, 則看時間安排, 能不能講點Memory測試。

* ![man wearing black and white stripe shirt looking at white printer papers on the wall](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/07/pexels-photo-212286.jpeg?w=525&ssl=1)Photo by Startup Stock Photos on [Pexels.com](https://www.pexels.com/photo/man-wearing-black-and-white-stripe-shirt-looking-at-white-printer-papers-on-the-wall-212286/)
*整理來說, 老師的學風很自由, 且總希望用盡任何方法來教會你, 甚至會花一大段時間帶你把整個演算法走一遍, 讓你可以更明白發生了什麼事情, 且上課不要害怕提問, 簡單來說, 我會覺得老師的教法很美式, 也很開放, 因此很注重學生提問, 我也很感謝老師的用心指導, 雖然說隔行如隔山, 但我是對數位系統測試有了基本的了解。*

最後, 這門課主要有兩次測驗, 測驗方式很自由, 你可以Open book, 甚至帶自己的小抄, 但我想告訴你的是, 如果把上課內容都搞懂了, 基本上考試題目不算太難, 祝你好運！

