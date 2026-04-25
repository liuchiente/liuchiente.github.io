---
title: "在Colab Notebooks上定義並訓練Yolov4 Model (以水果辨識為例)"
date: 2022-01-25T14:43:00Z
lastmod: 2024-11-22T09:22:50.965Z
tags: ['影像處理與深度學習', 'Evernote', '中興大學資工所', 'YoloV4', '學習紀錄', 'GPU', 'liuchien', 'Digital Image Process']
aliases:
  - /2022/01/colab-notebooksyolov4-model.html
draft: false
---

Yolo (You only look once)可以說是最知名的即時辨識模型了，不僅效果好，速度也快，[當初的論文可以參考這裡，](https://arxiv.org/abs/1506.02640)現在一路也出到V4了，你可以在訓練資料上定義不同的圖片和標籤，以此訓練模型，就可以輕鬆達到辨識新目標的效果。

當然我這樣講是有點浮誇，你必須注意一下你的電腦能不能如願把訓練跑完。

在這次的訓練中，我嘗試在Colab Notebooks上安裝，並訓練Yolov4，主要是為了省去GPU這個環節，因為平時我的工作筆電是一台Mac，想要跑完訓練是真的需要一點時間的；另外為了完成訓練，首先我必須找到一份資料集，再來我必須製作自己的YOLO  Format標記檔案，提供給Yolov4進行訓練。

資料集的部分，我使用的是Kaggle上的Fruits fresh and rotten for classification資料集，這其中包含了新鮮蘋果、香蕉、橘子和腐敗的蘋果、香蕉和橘子等六種類型的圖片，[如何在Colab Notebooks上下載Kaggle資料集的話可以參考我這篇文章。](https://liuchien.ink.tw/學習紀錄/如何在colab-notebooks上引用kaggle資料集dataset/)

至於如何把Kaggle資料集轉成Yolo format的部分，[可以參考這篇文章，在這裡我捨棄了用LabelImg，也捨棄了Yolo Marker，](https://liuchien.ink.tw/學習紀錄/利用open-cv自動找出image的bounding-box並轉成yolo-label/)為什麼呢？~~純粹是因為我懶，~~純粹是因為我想盡量把工作自動化，雖然可能割捨掉一些正確性。

在準備好資料集和Yolo Format的標記檔案後，我們可以開始接著作模型下載和訓練的工作，首先我們要先從AlexeyAB下載darknet，這是一個使用C語言實作Yolo的共用函式庫，但是可以提供Python調用；當初是從Yolo的原始作者那fork出來的分支，因為原作者已經不維護原始程式碼了，所以網路上大多用這個版本在訓練和實戰。

首先，我們下載darknet。![](https://locker.ifttt.com/v2/27073430/1643109749720-692d809869d4a952/1939ca382c8b7688817944c00cd624bf5d72427bda0c0808a537586c806d8796/272bcd69-3d59-e3b3-df18-aa7a3b03ac63?sharing\_key=1b04415a8534fde69666e069fdbd542d)

當darknet下載完成後會在Colab上建立一個目錄，這裡要注意，後面訓練、打包時，都會在這個目錄底下進行。![](https://locker.ifttt.com/v2/27073430/1643109750220-9f23bb2238183d96/07911545736b7d3aae2b6c465a3658c7546075dc1457beb2244d4af6d2c8c4cb/764f2782-f1ca-2c4b-d6b2-63068169440f?sharing\_key=33c4f7b6a0e470f11eb508f04ecb469b)

當Make時，或模型訓練時，會需要持續寫入權重(weights)，所以我們需要先把目錄權限改一下，開放寫入。![](https://locker.ifttt.com/v2/27073430/1643109750727-24a6eaf55bec511e/858df6405bf137ce145376628dc9e5a57235e0ebb68c6d2305130a185d37f494/5a834aba-aeb0-c9c0-5a32-b552cd4feaa5?sharing\_key=ae585d1b79ed611e6ba46c6b4a9df203)

在Make打包之前，我們需要改一下組態檔案，這將影響我們後續打包完的模型如何運作，在這裡我們用sed來修改組態，sed 是「stream editor 」的縮寫，顧名思義是進行串流(stream) 的編輯。無論是在編寫shell 的或處理STDIN 的時候，當有需要進行字串取代、複製、刪除，就可以直接下命令調整，當然你也可以直接透過編輯器修改；這裡我們有幾個東西要調整。1. OPENCV=1，啟用OPENCV，注意，你必須先安裝OpenCV。
2. GPU=1，啟用GPU，注意，你的Coblab要先啟用GPU。
3. CUDNN也需要啟用，他是基於CUDA的DNN函式庫。如果你用的是自己的電腦，也有自己的GPU，可以找一下顯卡可以支援到什麼程度，然後修改ARCH，ARCH主要是指定框架裡面用到的GPU資訊，在某些狀況下會導致你的YOLO無法編譯，[詳細狀況可以參考這篇文章。](https://www.itread01.com/p/1417745.html)

![](https://locker.ifttt.com/v2/27073430/1643109751281-a52508177fc1cfa5/21742cc168c9d8c59dc2d2954c345eee88593e7da30e864e9e16f51f52b37111/c6453d3c-b50a-504b-3795-37f9b05b5cd2?sharing\_key=883c498ee895a46bf42cf82de4d545da)

接著進行Make，基本上做這些事情速度都很快，麻煩的是後面的訓練。![](https://locker.ifttt.com/v2/27073430/1643109752010-31e21705be4b5b67/deab2e24b5d80f2b2ebf26b9bdaf262d1c794884d317c845c11cb93c7882ecaa/f798c587-a7e1-3ffe-5048-48cc068ac05c?sharing\_key=91a10918a8a90b2cc83c3430241b8972)

打包完成後，就可以開始訓練了，在訓練時，我傾向使用網路上已經訓練過的模型(Pre-Trained Model)，再來做一次訓練，這樣的效果和識別能力會更好，因為在模型裡面已經有其他人訓練好的權重，如此一來，我們也可以用比較少的訓練資料量，來達到一定的辨識效果。![](https://locker.ifttt.com/v2/27073430/1643109752458-7edb1b72a07b9128/59b6cf54cc7e8cf85c1eaf5139922f79dc79c941b8e3f75f344a63aa6536def6/4a04fdda-d321-8123-8d73-de0b44468e1b?sharing\_key=68f82c92c62ef753d86590f0628d5fea)

另外我會用yolov4-tiny.cfg做為訓練用的設定組態，之所以不用yolov4.cfg來訓練，是因為yolov4.cfg的batch數比較多，在Colab Notebooks上訓練很容易記憶體不足，或是說根本就練不起來。![](https://locker.ifttt.com/v2/27073430/1643109752981-70941d684b741491/af04ad34798983ed29b63c95d92d7899574b31039e742e3c6352b6433ae8a1e7/85215773-2485-7f10-7914-933d5c093dd7?sharing\_key=c802c595f5f39dccdf430638ed4ebb5c)

訓練時我會引入前面下載的預訓練模型，需要用到一點時間訓練，基本上幾小時是跑不掉的。![](https://locker.ifttt.com/v2/27073430/1643109753445-0c9f7020d9bae023/4859321a6d664ee5836b5e695a680b080feb81037b48bc3f01eda274c0a02ed0/3d4553e9-a7cb-192e-fbbd-a8d09bc17208?sharing\_key=9079f392573068dc5fe0a2b26608014c)

訓練完畢的話，也有語法可以測試，注意一下他會在測試圖片標記出辨識結果。![](https://locker.ifttt.com/v2/27073430/1643109754136-a01d80c9dd75e94a/007dca3f14ccf90418be08667e9caf0fe95a46d2f27669735e76f28131267d4f/966385b2-e9fe-d661-327a-5443aeaf54e0?sharing\_key=661b5949a712cc894bad750408f13f8a)

圖片的辨識訓練其實不會很複雜，訓練資料集方面，也不需要把圖片都轉成同樣大小(這也是為什麼你在產生Yolo format的label資料時，需要取的是比例的相對位置，而不是像素的絕對位置)，另外需要注意的是cfg設定檔中的filter數量，這個數值會和你定義的class數量相關，計算公式為filters=3\*(classes+5)，譬如你在資料集中定義了3個類型(classification)，那filters則會是(3+5)\*3=24。

後面我會花一點時間整理Yolo的做法和基本原理，並且附上我的些許心得，如此一來才能算真正掌握了Yolo這套框架。(待續)

[在此之前，若需要詳細原始碼的話，可以來這裡找找。](http://Realtime\_Fruits\_fresh\_and\_rotten\_for\_classification\_with\_Yolov4\_使用Yolov4偵測水果是否腐爛.ipynb)January 14, 2022 at 04:33PM

