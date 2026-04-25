---
title: "使用Python實作Image Enhancement"
date: 2022-01-27T08:31:00Z
lastmod: 2025-01-20T03:36:58.950Z
tags: ['影像處理與深度學習', 'Image Enhancement', 'Sobel', '中興大學資工所', 'Laplacioan', '學習紀錄', 'Digital Image Process', 'Python']
aliases:
  - /2022/01/pythonimage-enhancement.html
draft: false
---

趁著學期告一段落，工作也在過年前的休息時間，趕快把之前的作業整理出來；這篇文章依然會是先前的[影像處理課程所編寫的作業](https://liuchien.ink.tw/學習紀錄/影像處理-修課心得/)，在這裡不會用到深度學習或機器學習，而是使用傳統的影像處理概念來做，目的是為了瞭解原理。

本篇文章主要是探討如何通過二階微分，將影像邊緣強化。

* 主要演算法
	1. 原圖雖然用的是彩色影像，但我在程式中有轉為灰階，所以實際增強是針對灰階影像增強。
	2. 把原影像先乘[Laplacioan運算子做二階微分](https://zh.wikipedia.org/wiki/拉普拉斯算子)，可以得到影像的邊緣。
	3. 接著和原始影像相加，邊緣會被加強，但是因為原圖的雜訊也被放大，所以這裡的圖是有雜訊的加強，得到圖A。
	4. 接著原始影像用[Sobel對X和Y做一階微分，可以得到影像邊緣圖B](https://zh.wikipedia.org/zh-tw/索貝爾算子)，接著對圖B做去雜訊。
	5. 再來把圖B的像素做標準化，每個像素會被做成0~1的數值，此時在平坦區(比較亮的地方)數值會較大，趨近1，邊緣(比較暗的地方)則趨近0。
	6. 我們把標準化的矩陣和圖A相乘，可以知道原圖依照比例被放大了，原先暗的像素方又變得更黑了，非邊緣的地方則會因為乘趨近1的值，故不太有大變化，結果我們會得到圖C，圖C就是加強後的結果。
* 程式片段

1. 先把圖片轉成灰階影像。  



![image](images/2022-01-pythonimage-enhancement-121791479260247205.png)

  
   
2. 定義Laplace運算子遮罩



![image](images/2022-01-pythonimage-enhancement-4566266664442158405.png)

  
3. 定義Sobel運算子遮罩，0是dY，1是dX



![image](images/2022-01-pythonimage-enhancement-5630978552146362651.png)

4. 定義Slide Window Filter降噪



![image](images/2022-01-pythonimage-enhancement-5382783291964771549.png)

5. 對原本影像乘Laplace運算子，二階微分得到邊緣，接著和原圖相加。



![image](images/2022-01-pythonimage-enhancement-8250568701700640877.png)

6. 把微分後結果和原圖相加，得到一張銳化的圖。



![image](images/2022-01-pythonimage-enhancement-5390771837112259163.png)

  
7. 接著對原圖做Sobel運算子，一階微分先做dX再做dY，然後用OR合成，也可以得到一張圖的邊緣。



![image](images/2022-01-pythonimage-enhancement-1354381009165178173.png)

8. 接著用[Slide Window filter](https://openaccess.thecvf.com/content\_CVPR\_2019/papers/Yin\_Side\_Window\_Filtering\_CVPR\_2019\_paper.pdf)把雜訊降低。



![image](images/2022-01-pythonimage-enhancement-6673026403590700892.png)

9. 最後我們把這結果做標準化，取得一個0~1的矩陣，然後把矩陣和步驟3的圖相乘，圖的暗部被放暗，亮度則不太改變，因為暗部被變暗了，會感覺到邊緣變得銳利。



![image](images/2022-01-pythonimage-enhancement-8369187170178152729.png)

* 測試資料與結果
	1. 原圖，將他模糊後轉成灰階。

![image](images/2022-01-pythonimage-enhancement-1549148015588944975.png)



2. 做Laplace運算後得到邊緣。



![image](images/2022-01-pythonimage-enhancement-4472194122117172110.png)

3. 和原圖相加，但感覺到雜訊也放大了



![image](images/2022-01-pythonimage-enhancement-2708166017325019580.png)

4. 對原圖X、Y做Sobel微分後，做合成，得到一個較為銳利的邊緣。



![image](images/2022-01-pythonimage-enhancement-7339449936257998080.png)

5. 使用Slide Window Filter把雜訊去掉。



![image](images/2022-01-pythonimage-enhancement-6992953436016827906.png)

  
6. 接著把模糊後結果乘上前面的圖，得到邊緣增強的結果。



![image](images/2022-01-pythonimage-enhancement-3635950924074756091.png)

  
  
* 討論
	1. 把Sobel微分後的結果做標準化後，再乘上和二階微分後的圖，可以有效讓暗部變暗，因為標準化結果介於0~1之間，亮部\*1~0.5之間，只會感到些許變暗。
	2. 但是這樣的方法會讓原本的圖變得比較暗，暗部變的更暗，邊緣會更明顯，但亮部受到乘數影響，也明顯感覺到變暗。
	3. 依據課堂上提到一個好處，我們把邊緣做模糊消去雜訊後再做標準化，因為雜訊已經被去掉了，最後再去和圖相乘，可以避免雜訊放大。
	4. 延伸一個思考問題，若是我們處理彩色影像時，對R、G、B個做一次上述步驟的處理，是否也能把彩色影像的影像做增強呢？

  
