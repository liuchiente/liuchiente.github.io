---
title: "(閱讀心得)Analyzing Political Parody in Social Media"
date: 2022-03-25T15:08:00Z
lastmod: 2024-11-22T09:22:48.990Z
tags: ['自然語言研究趨勢和論文', '中興大學資工所']
aliases:
  - /2022/03/analyzing-political-parody-in-social.html
draft: false
---

這篇文章收錄於[ACL](https://2021.emnlp.org), 作者是**Antonis Maronikolakis, Danae Sanchez Villegas, Nikolaos Aletras** , 第一作者**Antonis Maronikolakis**於[Center for Information and Language Processing, LMU Munich, Germany](https://www.cis.uni-muenchen.de/ueber\_uns/index.html)服務, 其餘第二作者, 服務於[Computer Science Department, University of Sheffield, UK](https://www.sheffield.ac.uk/dcs), 第三作者是**Daniel Preotiuc-Pietro,** 服務於[Bloomberg](https://www.bloomberg.com/asia), 本文僅為個人閱讀後分享, 並加上個人看法, 若有侵權請告知。

原論文詳見[https://arxiv.org/abs/2004.13878](https://arxiv.org/abs/2004.13878)。

本文發表的比較早, 詳細時間應該是2020年, 對比2021、2022提出的應用, 難免較顯微簡單, 而本文的啟發點, 應該來自於[Bloomberg](https://www.bloomberg.com/asia), 也就是大名鼎鼎的彭博社, 因為我早期對於判斷假新聞、假消息, 有濃厚的興趣, 故針對這類型文章做了些閱讀, 至於在這方面的應用有什麼優缺點, 則容我以下敘述。

## Introduction

Parody是一個歷史悠久的藝術活動, 人們透過裝扮、模仿知名人物的語氣、外表, 說一些不是知名人物會講的話, 表現在電視節目、卡通、漫畫等, 來嘲諷或是幽默一下, 現今, 很多電視節目上都還有類似的表演, 譬如台灣的全民大悶鍋; 相同的, 在Twitter也有類似的活動, 人們會創一些帳號, 並假裝是知名人物譬如川普、歐巴馬等, 時不時依照新聞或是現況發表些搏君一笑的twit, 本意就是為了幽默而已, 或許很難想像, 基本上這些發文都會屬於非正式、天馬行空的言論。

相較於知名人物如川普、歐巴馬等人的發文, 若是正式帳號, 往往會採用比較正式的敘述和文筆, 來陳述理念或是對於現實的想法。

當然在Twitter上, Parody帳號和正式帳號是會做一些區隔的, 譬如Parody帳號通常會用Parody結尾, 如[ObamaParody\_](https://twitter.com/ObamaParody\_), 或是像[nicedonaidtrump](https://twitter.com/nicedonaidtrump), 會在帳號介紹時註明自己是Parody帳號, 否則就會被Twitter刪除。

而本篇論文主要是做些牛刀小試, 希望能透過資料訓練, 用來判斷Parody帳號和正式帳號的發言, 如同前面提到, 兩者帳號發文文體通常有一定差距, 故特徵比較明顯, 是有機會分辨的。

當然, Parody的本意只是為了搏君一笑, 希望不會有人出來賞巴掌。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-8-1024x360.png?resize=525%2C185&ssl=1)Parody和Real帳號的發言差別## Task & Data

在本文, 作者也是用了相當簡單明瞭的做法, 應用Twitter提供的資料集, 直接了當地坐了二分類, Parody和Real, 接著進行訓練、驗證和訓練, 基本上都是以80/10/10的方式來切。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-9-1024x452.png?resize=525%2C232&ssl=1)此外也做了幾種差異比較, 算是承上, 再往下延伸, 分別依照帳號性別、所在地區來做資料切分, 之所以會這樣做是因為Parody這事情是很多人都會想做的, 譬如俄國人會想坐美國總統的嘲諷, 美國人想做俄國人的。

這裡的地區切分, 是以帳號主人的所在地做切分, 假定你今天申請的是美國總統的Parody帳號, 但你是台灣人, 則你的文章是歸類在台灣(RoW, 英美以外國家), 之所以會這樣切分, 是考量不同地區的人, 文法不盡相同。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-11.png?resize=525%2C457&ssl=1)另外有趣的是, 也依照性別切分了資料集。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-10.png?resize=525%2C342&ssl=1)## Predictive Models

以下用了幾個模型, 來做訓練和驗證。

* Linear Baselines
	+ LR-BOW
	+ LR-BOW+POS
* BiLSTM-Att
* ULMFit
* BERT
* RoBERTa
* XLNet

Results

以下是訓練和測試結果, 可以發現RoBERTa 分數最高, 這是可以預期的, RoBERTa是BERT的改進版，通過改進訓練任務和資料生成方式、訓練更久、使用更大批次、使用更多資料等獲得了State of The Art的效果, 換句話說, 就是用更大量、更久的訓練, 讓模型學了更多詞彙。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-12-1024x413.png?resize=525%2C212&ssl=1)另外作者嘗試了用男性文章的資料集, 去驗證女性文章的資料集, 除了分數依然是RoBERTa最高外, 可以發現如此切換, 並沒有發現太多的差異, F->M, 指的是用女性文章訓練, 驗證男性文章, M->F則相反, 這裡也可以得到一個結論, 在網路上的文字表述, 男女其實沒有太大的差異, 故也反映在分數上。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-13.png?resize=525%2C435&ssl=1)倒是當用地區別來訓練Model, 然後再進行驗證時, 明顯可以看出一些差異, 且確實表現在BERT、RoBERTa和 XLNet之上, 這也說明了英美地區、和其他地區發文的用字遣詞是有差異的。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-14.png?resize=525%2C362&ssl=1)## Error Analysis

本文提到的方法, 簡單粗暴, 並且稍微比較了一下幾個差異, 但也點出了單純用Model來區分Parody的缺陷, 以下為例, 歐巴馬的官方發文通常不甚正式, 而Model也因此沒法分辨出其文章和Parody文章的差別。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-15-1024x197.png?resize=525%2C101&ssl=1)另一個例子, 當Parody的發文文體比較正式, Model也無法分辨出和正式文章的差異。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-16-1024x307.png?resize=525%2C157&ssl=1)簡單來說, 想要透過單純Model來分別文體, 通常需要建立在兩分類具備一定特徵, 而且需要有一定差異, 才能獲得不錯的效果, 另一方面, 如果把Model加大、加強或許是可行的, 但卻是一條無止盡的路, 因為人會演化, 廢文也會越來越不容易察覺。

