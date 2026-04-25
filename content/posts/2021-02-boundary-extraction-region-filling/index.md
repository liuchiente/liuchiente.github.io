---
title: "影像處理 | Boundary Extraction &amp; Region Filling"
date: 2021-02-04T18:57:00Z
lastmod: 2025-01-22T02:41:33.865Z
tags: ['影像處理與深度學習', 'Dilation', '中興大學資工所', 'Boundary Extraction', '學習紀錄', 'Region Filling', 'Erosion', 'Python']
aliases:
  - /2021/02/boundary-extraction-region-filling.html
draft: false
---

這是我在中興大學資工系修習影像處理 Digital Image Processing這門課程所做的課後作業，在這個作業中，我將會使用到數學形態學(**[Mathematical morphology](https://zh.wikipedia.org/wiki/%E6%95%B0%E5%AD%A6%E5%BD%A2%E6%80%81%E5%AD%A6)**)中的一些技巧，包含侵蝕(**Erosion**)、膨脹(**Dilation**)，來實現如標題所需要的邊界抽取(**Boundary Extraction**)及區域填充(**Region Filling**)。

本作業的原理是這樣，我們可以在二值化影像中利用形態學原理來改變影像內容，假想一下，所所謂的**[二值化影像](https://zh.wikipedia.org/wiki/二值化)**，就是影像的像素簡單表現為0和1(嚴格來說應該是255)，就是全亮和全暗，以下我則描述為全黑和全白，這些像素將構成完整的圖片。

想像一下，你拿著一隻立可白塗黑板，黑板上就只有白色線條和黑色版面。

[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-2937420103425359984.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEghXUGccyaAzATN-E-P-K\_tk9g8iMRbNOs6bwXeu2UV56bkjMxBE0Bb41swM9VBS4jAzzZCyEkyYjo5Tjv1-DeZFNi1yf9ipGZvpWMWjwC8w\_0kmpM6ypkFSdyFcfiTZxJ0-fo\_rvArFp4Y3AOcmF9FbmVtE9951fWhV687eWhT91mBJrGM6JWtEzN1HNk/s220/Region%20Filling18.png)  
一張簡單的二值化影像，影像中只有全黑、全白兩種像素 [![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-4934726797766642128.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEho49UJ2SFvACyb38BxEiHNWkf6e2E7Qj4wBAb3-6LON\_XkflmARxwAPIOxbru2MRlOytc\_LoLaf-FLicuZxWiPE7nEHMM4QFBwFMx9k8oKCkY65Ql0bLDfFND0R0uz1HEVhPT3AweLdapZV1Yh8r9lgYbBhVXBJdTZOlB8PQ7LbSmVv0gEwQy4PG4Mt2k/s329/Region%20Filling8.png)  
二值化影像的像素分佈情況往下描述如何透過**Erosion**和**Dilation**來分別實現**Boundary Extraction**、**Region Filling**。

**Boundary Extraction**

只需用到**Erosion**，再搭配像素加減運算，謹記著幾個要點即可。

**Erosion**是將影像中白色區域做減肥，運算完的結果會比原圖的白色區域更小，讓該物體瘦一圈，這一圈的寬度是由捲積Kernel 的大小所決定的， Kernel沿著影像滑動並計算，如果Kernel中m \* n 範圍內所有像素值都是1，那麼新的像素值就保持原來的值，否則新的像素值為0，Kernel掃過的所有像素都會被腐蝕或侵蝕掉，影像的白色區域會變少。

在我們執行完**Erosion**後，可以發現1的部位被縮小了，就視覺上未必會發現亮的部位變小，因為他的變化著實很細微，有些原圖亮的像素是1，他在**Erosion**結果中是0，如果把他們相減，就可以得到找到亮部的相差，如此就會拿到亮部的輪廓。

先把圖片**Ａ**做Erosion後，高亮的地方變瘦了，接著用原圖減去Erosion結果**Ｂ**，對比原圖，**Ｂ**(**Erosion**)亮的地方比較瘦，原圖減去Erosion結果**Ａ-B**，可以得到在Erosion結果中對比原圖沒有的亮部位，就可以得到邊緣了。

看一下**Boundary Extraction**的處理步驟：特別提醒一點，這邊說的相減或其他運算，都是指向素值處理，可以想像成把圖片轉成向量後做計算。

  
 １.Boundary Extraction測試原圖[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-4953068446934987404.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjIquq7RWiSizvARYE3TNrq2NMnIhMUrYHCcG2TxKlHMPsZ0S5D142xDXR9qKrCW3iR3-Zt1jrlsSxfftWrC45J9CVvMEtWrvONaa3-lxkI-VhZPNmlf0LC2DRDnD9QdHz4yFNaM14kBo2PBCU9qSMCX5VoJdNCm1BCP9N\_Dpv6eMEaceG2CKRsBs7f-YE/s308/Region%20Filling9.png)  
 2.Erosion結果，肉眼看不出，但亮部已經變小了。 [![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-2218362944917187230.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhQxA5gf05oocz5wGJ2j\_d3B5OzVj8Q80LRq\_pTs1v6hpJS4dSESRURBwrlo4kh0k6pZo2wsjntqxdstVHzKLYmhm4z571BKa6443YNAEvIRlMm5myzq2GILwoGXV74caY9SkLgTo6w6ZwFhF8yEB6slK6sh-yNW-G0-KHm-p2kvP3TP6vviXn6rrDzfbA/s329/Region%20Filling8.png)3.相減之後可以得到邊緣[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-2705310860940084995.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhaQluwPhJEX9wr8O4ImcDeCoL2m3e2v48Jt9Z-DiA-hv83VsBv77RWHz2\_Jnh6p-WF23NSr9FWkMAuhCsbN6RbU\_dikTZXj\_29vm3JceK4Lm2qF3I0lhFjZzj65jPzqyfd9MJiR3NUHXHMLYInf-rNlOfDcjjNlpLtOv6IqGMvVZ0z4PNSBzw5Q4yfnbI/s350/Region%20Filling10.png)**Region Filling**

這裡需要用到Dilation再搭配影像的其他運算。

Dilation的概念就是將影像中白色區域增肥，運算完的結果圖比原圖的白色區域更大，這一圈的寬度是由捲積 Kernel 的大小所決定的，Kernel沿著影像滑動並計算，如果捲積 Kernel m \* n 範圍內只要有一個像素值是1，那麼新的像素值就為1，否則新的像素值保持原來的像素值，這表示捲積 Kernel 掃過的所有像素都會被擴張或膨脹，整張影像的白色區域會變多。

Region Filling區域填充，我先把原圖A用NOT取得相反(補數)的圖C，接著我先在原圖A要擴充的黑色部位加上白點，再來我將原圖A做Dilation後，亮度會變高得到圖B，但為了限縮白色不要超出邊緣無限增長，必須把B和C做AND運算，則得到一個結果Ａ。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/02/image-7.png?w=525&ssl=1)此時可以看到我撒上的白點以已經被放大，但是其他部位因為和補數C做過AND，所以會被限縮著，重複做50次後，可以發現我的點都被白色填滿了，是圖D。

接著把填滿的圖D和原圖做OR運算，可以看到原圖中的其他部位被保留了，但是我指定的點變成白色了，就可以看到一個完整的填充效果。

看一下****Region Filling****的處理步驟：特別提醒一點，這邊說的相減或其他運算，都是指向素值處理，可以想像成把圖片轉成向量後做計算。

  
 Region Filling測試原圖[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-6140869818833831458.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgxZMWA\_48fgLdoS\_q8qupi0dfzZGY1LUryEovBDWRaKIpFT5YMH0MrRhUgzGuMsU9ZoFr-mZ97VqZJ8xhVYlFXydhLoVfuDeAdHydT1nVlUrau1CQFKCy6XEc\_mPoGaSFt2Br3m3e2Arkr5soqg2EiXd8EUvmedCHXzb7iBxY-jP-7gEs4hFeiPcMjTPE/s420/Region%20Filling11.png)  
NOT後得到補數圖[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-5000025061347924167.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhdRTgrG8gnEOsVa-9VcWE4-StlKLrEUWGpAgy8744NrLEShQfOuYNLDyaCGy0jebLi0N\_rqus7RZlSsZqk6fL2SQ2fayRrHv2P-7JHfyJrv9-U7K5lISuV6f1fDygwpBkMLQM-ykifgQCI\_Zq3nn0ET0aWCS3Pv5ikSDvijUZoXG8b7h3JPNB02o0Kpo0/s410/Region%20Filling12.png) 每次Dilate後白色膨脹，跟補數做AND，做完數次後得到以下。[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-7905739167554830244.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhrHZBEfsMTU1d6BIibvOXhIMLu729gOFW\_peZ6zIwyJXY0XrZ0JXubHQ-Iv\_V2H2rkzmSoX8tGHRYuhp5Rg-mOrqGLtChDbmLDqkv-zI71ozFEoa9FCgtlPr69BHibY-SPSYRCNb5t-YjdmWHNUDhZXYeg8pYnmm0U65aQ7v6BlnJ2ybnNudfXgiZTFq0/s343/Region%20Filling13.png) 最後把原圖和Dialte後的圖做OR得到填充的圖。[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-6848413098474797101.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgrzZ4HpxITCx-x0DRLeJTgF57Mb5XjKiyjZ5IHpgZSNlJc1mqp53qY\_LCtOLAHw0J2DXyngI0CEYr7BrVXgJK1vCtY\_Iynt1HA\_Mr2w9zUF2o4EEkfuxp8JeAJgwTy9OPtXOewdz2-VtofxhJBW2mJrzdffU3I-DuhizGOnbwDEWXQwAp1um\_HTWk2dtk/s370/Region%20Filling14.png)預先撒上白點才能正確做到填充效果，其實也只是把白點放大再放大，然後再用原始圖去限縮他的邊界，避免失真。我曾嘗試用Erode後的圖取補數再去填充，但是會發現沒法控制邊緣一直變大，後來才改回原圖的補數去填充，就可以應付各種形狀的填充，跟小畫家的效果一樣，即便是用不規則的圖也可以被補數解決掉，還原出原本的形狀。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/02/image-10.png?w=525&ssl=1)[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-7380478764248436247.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgcrhIDvf9xxrdw31LMLnYI6wPmtB9kkvfvFAh1jRyQFedgcWz-\_y3Qak5KXCUb8fCKxgmNpLcf0BXW8TGHtoEnJ\_StX6dY9wyWeu8W\_mB7hgv2qaii2GBxa1MJEyamkiIpM6fvTorxWF7xuPO1yq8If3qP1mxY1vgMX0YneW3nqlMxV6JSb9SrWu3m\_ek/s220/Region%20Filling18.png)  
[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-3852758280438208498.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhGVnVLG\_TBkbeY2qwcqcBdQIS6bGGtMpeMlc9YZC3wnBo5o5pB4v5Vt6s5I5ba-pD6jrFxpFEHFRB6\_S2qubbbKsZPmJs9c8iWWUb0b6bpU2-9ACSeotmCTm1lPX\_Xgsm9KZFUiYWrTvaAVuJVVw3x3wSi31n0r3H41uFng0-dBCp5SrJyyzucKXemXI0/s214/Region%20Filling17.png)  
[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-5567504018759246916.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhIDm8KwD0KM1h8ijfV9pSoZtObd6-JuI3dQdVevz8\_9LjWdHdScwnRVSMdipWEupZeUc3CayGIdMbxGGAhubllwNQ1ibPs5-BSbItgAY1FB3\_lwkMceODNxcULKYBNQooImWUdZk\_ym\_USfkSTJdZJg8FiZVDlDsxQphqvfzSUpaTUclf9KXiSSVwvedg/s214/Region%20Filling16.png)  
[![](/posts/2021-02-boundary-extraction-region-filling/images/2021-02-boundary-extraction-region-filling-6959416743416047278.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh2XO5bUIsgJHn0o9bn8hfgyHYa6mma3GMRFNGaCxU1r\_wLcos5aYtxJDueCkrGvgTRf8sJ4fDg5wi13otcSf7Q032qOtpJbkXL7SNM2Rj-R9gTYf3SryzHxdLHoH9sWVaAmhrdEWK7QqHB34iptu4bgDncACIbdYu2rHC9DrmES3U32QYdsmluFVajbq0/s220/Region%20Filling15.png)  
  
![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/02/image-12.png?w=525&ssl=1)![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/02/image-13.png?w=525&ssl=1)如果你需要更進一步了解Source Code的話，[可以參考我寫的Python版本 on Colab](https://github.com/liuchiente/DigitalImageProcessAtNCHU/blob/main/Boundray\_Extraction\_%26\_Region\_Filling.ipynb)，希望對你有幫助。

