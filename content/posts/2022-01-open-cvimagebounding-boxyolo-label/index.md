---
title: "利用Open CV自動找出Image的bounding box並轉成Yolo Label"
date: 2022-01-11T15:05:00Z
lastmod: 2025-01-22T03:06:15.652Z
tags: ['Auto Labeling', '影像處理與深度學習', 'Yolo Label', 'OpenCV', 'YoloV4', '學習紀錄']
aliases:
  - /2022/01/open-cvimagebounding-boxyolo-label.html
draft: false
---

這陣子我試圖在Yolo V4上訓練一個識別模型，在我原始構想中的功能，是為了讓這個模型可以擁有判斷水果新鮮度、腐敗程度的能力，想當然，如果要達到一定精準度，那勢必是要給他一些訓練資料(Training Data)，而訓練資料的多寡也會影響模型判斷的正確性，於是乎，我就在網路上找了些資料集(Dataset)，幸運的是最後我在Kaggle上找到這個近乎理想的資料集，裏面包含了腐敗的蘋果、香蕉、柑橘，同樣也有新鮮的蘋果、香蕉、柑橘，其實這資料集的原始功能是用來做一個分類器，只是被我暫借來用在[Yolo V4](https://arxiv.org/abs/2004.10934)上罷了。 也就因為有這些前因後果，這時候我就遇到訓練模型最耗工的時候了:資料預處理；在這個[資料集](https://liuchien.ink.tw/學習紀錄/如何在colab-notebooks上引用kaggle資料集dataset/)中，有腐敗的蘋果、香蕉、柑橘，同樣也有新鮮的蘋果、香蕉、柑橘，我將各個型態的水果歸為一類，因為雖然資料集只有6類(Classes)，但是資料集中涵蓋多個變化時段、多個角度、多個面向的水果，故所有圖片總結起來不多不少也是上萬張有。 在Yolo訓練時所需要的Label裡，需要標記圖片中的重點，框出一個正方形，告訴Yolo該去關注哪個區域，並讓Model知道這個區域是屬於哪類型的資料，譬如說我們可以在一張框出人臉來，然後告訴Model這個區域的樣子就是人類。 也就是說，當我想要讓Model知道我這些資料集是腐爛、新鮮的香蕉，我應該先把這些水果的特徵標記起來，然後告訴Model這是腐爛、新鮮的水果，也就是一個多分類的議題，雖然我們知道人工標記是最準確的作法，但當我們知道圖便有上萬張時還採人工用軟體(LabelImg、Yolo marker)來標記，就顯得比較不切實際了。 於是我選擇用[Open CV](https://opencv.org)在圖片上框正方形，也就是找出bounding box，要完成這個任務，需完成幾個步驟。* ### 首先我們輸入一張圖片。

[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-5376856718382920432.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj\_He2I8uEu-1nW3gvjKcMsFqTezXRR5vLSH6v6Fc\_5\_-W2e6pFzr59rr9r\_dFNU2u2jiVQyS-RRchV7Q8EdKodeWF5PsYWNmwco7kQtCF-I0YQmcfGK\_PIaAa4FIAjWBPFAQjmYZI7OlvEpIZEtU02AHfKalW0BqfpB\_qZVPu83af2bwDylXnD1DimpJE/s566/image.png)### 

* ### 把cv2. cvtColor把圖片由RGB轉成HSV，主要是為了能方便表現出特定顏色，HSV即色相、飽和度、明度（Hue, Saturation, Value），色相（H）是色彩的基本屬性，就是平常所說的顏色名稱，如紅色、黃色等，飽和度（S）是指色彩的純度，越高色彩越純，低則逐漸變灰，取0-100%的數值，明度（V），亮度（L），取0-100%、越高越亮。

[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-77321557999860812.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg2\_LRo27uRfnwyCESp1spwrklQPyhXSuwvcoE-l-MorG43Kze5BUhE\_eRP5-Y9vJW3e4YHxMkttU8KCj2WMPp7HVm25gEDSSkJE92ftxGNCemP6GnHg4jB2R4XHLgf8cies81njo4sYtzJULT0UEdfj4TW1OFr8MI\_ZIKoLQw\_uP2Xpq7DQJ25TrlAOlc/s566/image.1.png)* ### 將圖片二值化，指的不是變成灰階圖片，因為灰階圖片的像素有0~255的變化，二值化指的是圖片中的像素只會有0、255兩種變化，透過cv2.inRange(img2, lower, upper)設定閥值，像素超過upper的設為255，像素值低於lower的設為0。

[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-3721829890576111787.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgmfGR8gBuFzojF3jgKTq2JlEUA1dSAZoqE099le2P6i3tJylUVNoIdXALXI8VcPFsByA9h0Cm033FAlgieP62RPZ7gUVxhyx9rnEGLkeSBHSpYN0PfpHyoTkOW3G70tfyiNm-3TeUDS7Bk-Ve2J8W\_G4cPpep7-ZEfcWVHTo-py3XgJNKALq\_rSXzXlb4/s566/image.2.png)  
### 

* ### 將圖片模糊化，主要是用平均濾波去雜訊，減少圖片上的雜訊影響，資料集中有些圖片有刻意加上雜訊，效果類似撒上了胡椒粉，在這裡可以一定程度抹去，但是這也可能導致圖片邊緣變模糊。[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-2834605909994772921.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi22cJzD4s0W4uHgRbiA\_rlcs1xA356DToCDiYCutbJszPUTqUzJJukegy4TlSnobETjUGZXbapz\_8PLypflidPBOhyEkenkgX1F56LLMYchBNnZJQQm2sSJii6khuiQ9FJERiPynMwJwq1T3YvPm5ens-dkElPurvNKfzx\_4CryaDjKj8PyZeDiKIeeAQ/s566/image.2.png)

* ### 主要因為模糊化去雜訊後，此時圖片的像素會出現0、255以外的變化，所以再做一次二值化，會得到一張正確的二值化圖片。

[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-553439954467733690.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgShVCtTeqL5hJ4Ukn40r1C7\_NKPD5\_vNlYsUYG7D4oUthFr5TEgkRoiv63SeGNjTMapGCLsSnPWOQcfCJFHfRTUXHjQ4lUKS4Tgiqt2t1p59ElVNd3XuvOMj50bo9YbGfby1J-BJ4NAUs5o\_jJzoh6HAuZeJc-9jOeby7UvG1k2vwxdtea\_\_S5HVbENIw/s566/image.3.png)  
* ### 最後把圖片像素做NOT運算，如此可以凸現出物件，把物件部分變成白色

[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-3940078402423203223.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgfyIU5s7ehnuJKLKwbzxU6K1MFJmj2EeO4aUw\_F42MzBRCQiuYbL0ISWFZm3XjyDs5SoVuC0wtAkXWIFfsmRMTPms9vXlF2ORHncIPVebOxrwbVFQljI3fca61fKb5jgLL967iIjoPbQXKI\_zZwvXoDoGr79FY2lmJzVYpRxHMPDar5IZXh4jWLq6wrWc/s566/image.4.png)* ### 最後用cv2. findContours找出物件邊緣外的方框，也就是畫出個正方形，最後綠色正方形的四個角，就是我們需要的資訊了。[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-1791478106167876509.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi\_pb51XsAlGxQMYojtcC87v1zBaP9s82jOUIExE5lq\_FddpX3Tm0Gz21FiGjk3J02J-1NVBtkkduefP8JQVP9I-qLwyLza3Rz97VUyo-n20nlziG388OSdb\_cun8PUsIxwphSXpvV2RNbXw4OJTS-tjA0\_G4SM28Q7DtpoXpHFd\_PA8ecU\_\_2Oq9BIGVc/s566/image.5.png)

* ### 整體來說，用Open CV找出bounding box的效果算很不錯，大部分時候程式都能在圖片上找出一個方框，並完整地把特徵圈出來。

[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-63003571962806128.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh27T0EDUuYYF-dnBc2SBJ9WW6z3Tr07zXonNY5b-zXbNfebBMKJTnPARFBpeRQ\_jrzbIjSisXhRKjqADpbjjwnl-riNHKSLf1c8HAcDhSYAtYL5CAmHY6cH0YzSv1rVEjEtbybDcKsut3o1NSHeTH1qgi5V9EXnkBYO12G3-Mv8Q\_BbJ\_kd5uzrhvc-KI/s814/image.6.png)  
當然，世上並不存在所謂的銀子彈(silver bullet)，某些時候程式也會劃錯，特別是當圖片中的物件邊緣特別不明顯時，錯誤就會被凸顯出來。[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-785594692953937541.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiow9MrSvBMwzRI3K3DaIryjOkgmdHG4ca0cziAdXA8REvh8QaL5dgrB-x3qkBStjWw3UKgXGVlUxUQMgxKU5f\_m\_Mpn1drmxo\_lR8SQnf7KKSV-ENSfwgnml6Cvs4UXZIeqd3NWiGZpuE9Oz3bkTuMtAkuk7CVUua2FlSZBu8xGAXrtS5kL7Rx1oNOJWQ/s564/image.7.png) 有些時候，可能只找出物件的部分區塊而已，但我就原始資料集的狀況看來，倒覺得這問題不大，一方面是因為標記不完整的圖片，他們通常不是標記到很離譜的地方，譬如本該標香蕉，卻標到香蕉的影子，反倒很忠實地找到物件所在位置，故即便只有找到一部份的香蕉，相信仍不影響訓練結果。[![](/posts/2022-01-open-cvimagebounding-boxyolo-label/images/2022-01-open-cvimagebounding-boxyolo-label-7160513424792054994.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjYJ8hVTE3HkhobmdwYPBk8GLVOWZoE-G2q\_we8f0\_VthmxSci3kqbWehsaVjgdPkJEJRNLEUpSk9Ie7OsW-otPYm96a-nOTOcArXiFQFXMVGFgZN7Zdn5cpz8TYrRLN1Op9Mjwil6wQY0i1edXYpsRGEO9lypd7Rm3SJxNqZozTu5xHz4w2nRM3OBiZfI/s830/image.8.png)  
但是找出四個角還不夠，最後最後，我們要透過一個公式來算出這個矩陣在圖片中的相對位置，如此一來Yolo才能在不同圖片中找出你指定的區域；在cv2. findContours找到的資訊分別是x (像素起始X)、y(像素起始Y)、w(正方形寬)、h(正方形高)，而我們應該先找出圖片的原始長寬，接著用xmin、xmax、ymin、ymax，才能算出比例，xmin當然就是x，ymin則是y，透過w、h我們可以推算出xmax、ymax，最後和原圖長寬算出比例。//原先找出的4個腳,分別轉換出四角的最大值、最小值xmin=xxmax=x+wxmin=yymax=y+hxp = (xmin + (xmax-xmin)/2) \* 1.0 / image\_widthyp = (ymin + (ymax-ymin)/2) \* 1.0 / image\_heightwp = (xmax-xmin) \* 1.0 / image\_ widthhp = (ymax-ymin) \* 1.0 / image\_ height另一方面，我們會用0~5先定義好6種水果的Label，再將xp、yp、wp、hp寫入文字檔後，即完成這一張圖片的標記，仔細想想，是不是比一張一張標記快多了？總結一下，在我使用的資料集共有13319張圖片，而我只用了616.294秒就完成了標記工作！