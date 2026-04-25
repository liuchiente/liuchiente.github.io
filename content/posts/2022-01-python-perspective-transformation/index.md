---
title: "用Python實作 Perspective Transformation 透視變換"
date: 2022-01-27T08:56:00Z
lastmod: 2025-01-15T02:12:32.636Z
tags: ['影像處理與深度學習', 'Perspective Transformation', 'OpenCV', '中興大學資工所', '工作管理', '學習紀錄', 'Digital Image Process', '上課學習心得', 'Python']
aliases:
  - /2022/01/python-perspective-transformation.html
draft: false
---

* 主要演算法
	1. 通常我們在自然背景拍攝一張照片時，或多或少都會有一些[透視變形](https://zh.wikipedia.org/zh-tw/透视变形)存在，而透視變形是由拍攝和觀看圖像的相對距離決定的，導致遠近特徵的相對比例有變化，產生了彎曲或變形，當然在本例子，我是刻意拍了一張比例歪斜的圖，想要驗證依照公式轉換會得到什麼效果。
	2. 依照解決方法，這樣的變形我們可以下列的聯立方程式來表示：


> ​x = ax’ + by’ + cx’y’ + d
> 
> y = ex’ + fy’  + gx’y’ + h
> 
> ​其中x、y為你原始影像的座標系，x’、y’為校正後的座標系，
> 
> a、b、c、d、e、f、g、h為常數，表示變形關係。
> 
> 

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-6857140727985475800.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgZVtlp1SLZgwoOC1oxy2t2ODxKP0YgTJliqSwhwjLYYLiRRaSLblhs03LLqDW5fRWof5ttOIYFC6jY3dit5JSIbtqEZU3pKm75LJZ1U4atBN-g9ij\_Tgh4qhh4r81lWzH5pOR9e\_5qOru3qE4XITOQwfFCLvcOWhYIZHMuv8LVtdJk-5iBOIwaZDk0HbU/s866/Perspective%20Transformation1.png)* 程式片段
1. 定義[高斯消去](https://zh.wikipedia.org/zh-tw/高斯消去法)

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-1114696523862495389.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhzaJ8bGki71JWxDx9mTp4dz2ihSSDz18wzeuWDgnReqPFZ\_JMBTrlw6WJcY9m-EezEg4qFHf2ROk49iyspzRvOBh1LZ\_AROCMaRySVf9vDhDabgRe70bZ9xcvFqKbkfti56reYkip4cRc4UGCMRF8fbeGDqZRNyvIUITLxc726yGeYCTn2F8SL\_hVlpC8/s660/Perspective%20Transformation2.png)2. 先定義4個點，找出矩陣

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-5809550902250250096.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhrtZMpixros49cWHtDc1EtoaRLL6QKu2Ro1FLMRgtqGwzVqf1HCbiwUp1Ob2to0DjOAsusKDFi\_Li5nTzJtZGh3mlE2sRDJ14Cweby-\_I-eJcHdHkjj1177ga757jPjALbNGK5mVmNeNRfn7MeOeMiv2G1eY8Rqo2b7R7ooHdfa1k-vfzB0QmOpbPSrfw/s614/Perspective%20Transformation3.png)3. 照公式進行矩陣轉換，解出方程式，輸出結果就得到圖片了。

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-2108812322683336029.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg52Gc59B0qxWomXBEEMx3hJRpfV3uFljRYEgHoYfZYIPptEx5rpT2t1Z6WuMgh2ek8GqORT7pKaKgnFGHLeTYePwkpfZUiEbhLYSm6yn0h4jmQdjj8KhO1wvlULkX2DMyqBX-s2NDuR6h\_eUfcx07dg6I41fc0iTCSRiQkCd22Mt4S\_pyeL72eMqQvZ1k/s675/Perspective%20Transformation4.png)4. 另外我也參考網路範例，仿造OpenCV寫了一個轉置方法，後續比較一下兩種做法的差異。

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-7740932738572326158.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiXtMLlj7lrugoh-P\_VyKkNtJyjmYkZrjB-1LioDzjGLogQwMXISpuu9ESDrliXmuQK8-Dn3xdB3OlxSwZ-T0sMpbDwGwxvo4drXH9eixAldFORT36BmobXW2eahBe7veHob5\_FPvoLH4YeImghdwK32WTHTujAe2ZhSuPngWQtZxEyzu2UzDrT4YqUQmI/s639/Perspective%20Transformation5.png)[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-6712269857023954101.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEioyFWHRNyUkp3cnzcwdBN-g\_DP2FoeQKo-pQbrLRSNEy2E5MGme2m1tVR4C2MY5O2KbzdnnmvMZdVssKnfRAwDMDuI-N9JKYNGNC8Hk05Sv65vFrFnW3pdyiT5doLHT4XD-1h\_kWACimm93L\_GIw3UDmOCCB3J21p06Lk9fqAfUuZlHSVGxyQc7Q85-i4/s650/Perspective%20Transformation12.png)   
[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-7652708448841208194.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiyMspqr93li1TwJYG0TEQokIMYRBDdHIxXQYDHHTbI4WOCQmc3v2EYspTRsX7b8uz56MPtIrLiKblXOze2h4Io2cGbLW2DDAaWCU2qeXHQ5NpW6YkCXc9o0Df8zuD0mhoWUR81GKOOm9\_k8MBLSp\_QUWfoyw7zW5OhtTiafyiOk6792uzbpzd0U6X0Frw/s643/Perspective%20Transformation6.png)* 測試資料與結果
1. 原圖是張刻意拍歪斜的照片，我標出四個白點，可以看到原始轉換區塊變形到接近菱形了。

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-8859272821961614282.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjj81oKPmaUnX8EG58BATZ8Bp-tiY0IvhVlyX9UWNdi9UtMqnmThF-oMi1ltSJ7Qd4oBpOrJ-\_WEvL7ANurye8X-NUlYbGZCOW4iQ2TqSAc9WZtS1kRvlFDMvvq6JiOzHnJmVKwyrd7WOHZ3ls\_Jd0xJMiWSkh9GZKR5D8RThCvUA19aiqc1TeFeE26hWg/s327/Perspective%20Transformation7.png)2. 轉換後得到的結果如下，或許是因為變形幅度高，沒達到完整的轉換。

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-3533516829175729947.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhxCzf9Z-SLcmUK0AiUxTvsDPaQ59GwkhxPuOpBq0hIIQFXDItFwgpgNMJHjqRtIkedkWbKkZOE8TvjSisgdg11LonVsPCMuOf2dXhR3gERCxS6j0Da1PnRB4No9GAgx3m9SYOozuxd73Eby\_0J3RmaoyFA4qYT6gQqjmA\_q1f8EKYxIXVTqG7NUnvZZtY/s362/Perspective%20Transformation8.png)3. 參考網路實作[Perspective Transformation](https://blog.csdn.net/u010925447/article/details/77947398)做出來的效果，因為先做3D後轉2D，校正效果很好，只是程式寫得不好，有失真。

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-8783781151610757542.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEijwBQZ6RFOIwe5ohPZCNrXNb\_DbPs6ozZyUP8HytRkJRkGJbTM9Xj\_xQTYq0MGgtoYOW4WZ5pmRWGOZk8atfalAEMsnxNVpTz4FbFmg\_7991io23mdZPbRjZEHR556AUs23S4lnCxtpP\_W\_7QEWereldmAgj-ZNc5C\_O3IEAYZa\_gVeMOXvbMisjHgKgY/s368/Perspective%20Transformation9.png)  
  
4. 使用Open CV做的效果

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-4698018068069709098.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqdo1dlWqXDQVClywfT\_Hm5J94B6hPvx5j3P2FTGrSg5Z4EqSY6ELphyphenhyphenbW9JSsiN-YLLv2imPNS9l1mtjUGFnghhyphenhyphenuxbcz6S9jBphlu0RG9OzCquZvE4GJVg4HFfRE\_R0FCv2GjiG0nkDNlRIKKamuXyHqJvxthbpsQhccHo31Pgy\_N6\_7Z09kuXCcXBs/s404/Perspective%20Transformation10.png)  
  
* 討論

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-7489857298800613.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiLggeYuBlrOe0v02KWF0CoXpZWqaEvtpxkfZOpzM9GuD3EJfv-Cm0O74GOwK0\_PyC5pdI7Mjzx7b8gMHbRndJUvwQ\_l3VqOQyQbCoGPDIXCxk7mMA6DMIknbHCHKZnZSUziPqqTy-hzUH\_9SXD4Ozk0-v5Y739TXR1uUXqx-F50nw0Cwg8iWskYhXxuLE/s510/Perspective%20Transformation11.png)1. 原本課堂講解的校正來源是一個平行四邊形，校正效果相當好，但在本次作用我使用一個不規則的多邊形來做轉換，雖然轉換結果不比原本的好，但還算可以得出資訊。
2. 參考Perspective Transformation實做出來的程式做轉換，對於不規則多邊形轉換的效果很好，只是有失真，若用OpenCV轉出來的話就趨近完美了。
3. 此類的校正可以應用在機器人自駕時的資訊辨認，或是在OCR應用上先把讀入的圖片做校正，在讀取資訊，可以減少判斷失誤。
4. 在Deepfake影片偵測的研究中，許多論文的原理是把影像的Frame讀入後分析他是不是假造影像；如果在深度學習時，我先把人臉或是指定的擷取特徵做校正，可以把每個Frame的特徵變成一樣的構圖，就有機會分辨出他是不是造假的Frame：因為Frame造假是基於原本扭曲變形的畫面做變更，在校正後應該可以看出他和一般正常角度Frame的不同之處。

[![](/posts/2022-01-python-perspective-transformation/images/2022-01-python-perspective-transformation-5182669792010959897.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjfebBygmq9cQaDMg\_JEnqCj6oKLePPQbJlfxb7ApBWMxafqQf-5gWuPTTxednWrlnleF3euTsSt2KztH\_ZKDA6ufJQ0Arhsyybr3bgSnXDW2lyKoiOYukjb3fsMrkQQVGGFFXZ18zDctfl5J2amnu52epitaR8z7OGAvl0Brpq3jdZfryuFbiJ9TL55xE/s558/Perspective%20Transformation13.png)  
  
  
