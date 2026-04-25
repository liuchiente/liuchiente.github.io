---
title: "(閱讀心得)MSˆ2: A Dataset for Multi-Document Summarization of Medical
Studies￼"
date: 2022-03-24T15:11:00Z
lastmod: 2024-11-22T09:22:49.319Z
tags: ['自然語言研究趨勢和論文', '中興大學資工所']
aliases:
  - /2022/03/ms2-dataset-for-multi-document.html
draft: false
---

這篇文章收錄於[EMNLP 2021](https://2021.emnlp.org), 作者是Jay DeYoung,  Iz Beltagy,  Madeleine van Zuylen,  Bailey Kuehl,  Lucy Lu Wang , 第一Jay DeYoung於[Northeastern University](https://www.northeastern.edu)服務, 其餘都是第二作者, 服務於[Allen Institute for AI](https://allenai.org), 本文僅為個人閱讀後分享, 並加上個人看法, 若有侵權請告知。

原論文詳見 [https://arxiv.org/abs/2104.06486](https://arxiv.org/abs/2104.06486)。

## Introduction

本文屬自然語言處理(NLP)類應用, 基於醫學領域撰寫, 並以文獻探討中的Systematic review發想, 嘗試將[多文本摘要(Multi-Documents Summarization)](https://liuchien.ink.tw/%e4%b8%ad%e8%88%88%e5%a4%a7%e5%ad%b8%e8%b3%87%e5%b7%a5%e6%89%80/%e8%87%aa%e7%84%b6%e8%aa%9e%e8%a8%80%e8%99%95%e7%90%86-nlp-2-%e6%96%87%e6%9c%ac%e6%91%98%e8%a6%81text-summarization/)應用到Systematic review之上, 透過訓練, 讓模型可以做多文本摘要, 而後用模型(Model)來取代原本的Systematic review工作。

何謂Systematic review？在醫學領域, 系統綜述(Systematic review)是文獻探討的一種，針對特定研究主題的所有報告、文件蒐集整理起來，並將之識別、評論, 主要是了解和主題相關的概念、理論、研究方法、實證資料，讓研究人員可以進行引用、思考、批判和評估; 然而, 做系統綜述需要花費大量時間研讀,每篇review約莫需花費1-2年時間, 甚至最長到8年; 也就所以, 作者嘗試減少在這類工作上所需花費的時間成本。

簡單說, 系統綜述(Systematic review)本質就是把相同研究主題的文章, 透過人工去整理, 歸納後, 產出一篇文章, 內容記錄和此主題有關的回顧和探討, 也就所以文章上會引用、引用多篇不同來源文章。

## Dataset

在本文中, 首先需整理資料集, 而作者使用的手法是採用前人已經做好的Systematic review, 來反推出每篇綜述引用、摘要的文章, 每篇Systematic review都引用一篇到多篇文章(Documents), 透過整理, 完成了擁有來源文章(Documents)、摘要結果(Systematic review)的資料集, 用以訓練、驗證、測試模型的訓練狀況。

在本文中, 用REVIEW表示既有的系統綜述(Systematic review), 用STUDY表示引用、依據的研究文章,是意圖如下, 可能有點醜。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image.png?resize=418%2C348&ssl=1)使用幾個步驟來找出合適的REVIEW作為資料集

1. 在Semantic Scholar上找出title或是abstract含有” systematic review”關鍵字的文章, [Semantic Scholar](https://www.semanticscholar.org/)是 [Allen Institute for AI](https://allenai.org)開發並於2015年11月公開發布的人工智慧支持的學術出版物搜索引擎, 它使用自然語言處理方面的先進技術為學術論文提供摘要。
2. 只保留有在PubMed 收錄, 且是生物醫學領域的文章 ，PubMed是主要用於檢索MEDLINE資料庫中生命科學和生物醫學參照文獻及索引的免費搜尋引擎。本系統屬於美國國立衛生研究院下屬的國家醫學圖書館維護的Entrez資訊檢索系統的一部分。 自1971年至1997年，MEDLINE資料庫主要通過大學圖書館等學術機構存取。
3. 找出這些文章引用的研究文章, 但只保留研究類型為臨床實驗之類的文章(MeSH), MeSH的介紹可以[參考此連結,](http://tul.blog.ntu.edu.tw/archives/7180) 主要是PubMed提供的查詢規則。。
4. 使用分類模型, 再做一次過濾。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-1-1024x152.png?resize=525%2C78&ssl=1)基本上可以看到前4個步驟, 除了交代資料來源外, 再來就是盡量篩選資料, 清洗資料後, 並透過控制研究主題、類型, 讓文章維持一定的品質, 再來MeSH則是要反查出引用的文章, 在做完上述動作後, 再用第5步驟的分類模型篩選, 會剩下20K 的Reviews, 也就是20K左右的摘要文章。

往下講到第5步驟, 透過分類模型篩選文章, 再次過濾, 篩選合適的Review

1. 5個相關背景的學生從220 review abstract中標記3000個句子,標記成9個類別。
2. 2個相關背景的學生針對上述標記再做一次校正。
3. 類別標記中, 研究問題,是為BACKGROUND; 研究結果是TARGET, 不相干的或是太細節資訊的則為OTHER, 是將文章內的句子大略分為9類, 但在訓練時只留下3類, 可以減少訓練時的複雜度, 和花費的時間。
4. 使用SciBert進行Fine-tune後, 得到一個分類用的Model,並用Model來標記剩下的資料集(20K), 也就是說接下來把每篇文章內的句子丟進Model分類, 可以得到每篇文章中, 每段句子屬於哪一類, 當然文章會先被移除掉贅字, 連接詞等, 然後分句進行預測。

承上, 作者發現, 模型在分辨BACKGROUND時的F1則是94.1, 效果很好, 可以直接使用; 分辨TARGET的F1則是77.4, 效果較差, 會影響資料集的可靠性, 所以另外以人工對剩下資料集(2K)進行校正。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-2.png?resize=525%2C249&ssl=1)BACKGROUND、OTHER、TARGET的範例而作者如何進行篩選呢? 作者在170K個REVIEW中挑出220 篇來做為訓練樣本, 但為了避免資料量過大, 作者選擇只用了文章的abstract來標出訓練資料, 接著用訓練完的Model去篩選剩下的每篇REVIEW, 若是文章都沒找出分類為TARGET的句子, 則直接捨棄, 主要是考量若文章內沒有分類為TARGET類型的句子, 則代表該篇文章可能沒有結論、或是結論和原本的訓練文章相差過遠。

以此脈絡來看, 一開始用來標記句子類型的REVIEW, 必須具備一定的代表性, 且最好有不同的敘事句型, 這樣才能確保Model預測可以更精準更多樣化。

## Experiment

接著開始訓練摘要模型, 承上, 我們已經用分類模型找出文章裡面屬於BACKGROUND、TARGET的句子, 故在此會用分類模型來把剩下文章內的BACKGROUND、TARGET句子都找出來, 在訓練摘要模型時, TARGET是主摘要目標, BACKGROUND則是輸入的多文本, 而為了讓輸入更複雜化點, 每個BACKGROUND會串接上該句子所在的文章Abstract, 這樣的好處是可以讓模型學到更多變化形態的句子, 而這些句子都會指向同一個摘要TARGET。

但是問題來了, 不同REVIEW是可能對應到多篇文章的, 以上述規則來做, 也就所以, 不同的TARGET, 有機會對應到同一篇BACKGROUND串接上該句子所在的文章Abstract, 這意味著一篇文章可能對應到兩種REVIEW上, 這對現有模型來說, 是會影響最終判別結果的。

在本文則用了兩種Model來訓練分類模型, 以前面論點來說, 一般BART上, 訓練資料互相影響的問題會比較嚴重, 而用LonformerEncoderDecoder(LED), 透過比較大的輸入維度, 則把一個BACKGROUND串接多篇有關的文章, 加入訓練, 各文章間相互影響會比較小, 相反的, 各文章間反而可以相互認識, 效果會更好, 多樣性更好。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-3.png?resize=525%2C198&ssl=1)訓練摘要模型的兩種方法然而實際訓練出來的效果卻沒相差太多, 推測, 也許是因為各文章Abstract的文字數量不一致, 反而重點更混淆了? 詳細可能性需要再想想。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-4.png?resize=525%2C187&ssl=1)在本文原本提出的方法, 是想希望可以訓練一個模型, 並產生一篇摘要, 且可以正確產生多樣論點, 這是個很有趣的議題, 因為現行摘要的手法多會產生同一種論點的議題, 這是基於詞彙、詞頻計算得到的結果, 以現有框架來說還有一段路要走, 故本文最後放棄多樣相異論點這件事情, 選擇釋出資料集, 在後面, 多樣相異論點這議題, 仍會有一段路要走。

最後附上資料集的截圖, 如有需要引用資料集, 請到原文詢問作者。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-5-1024x373.png?resize=525%2C191&ssl=1)綜合以上, 本文最後的摘要分數其實不差, 關鍵在於資料清洗階段, 作者就用分類模型把容易造成混淆的REVIEW清洗掉了, 而標出了BACKGROUND, 建立起兩端關聯, 也提升了摘要的精準度。

## Structured form

本文還提到另一種思路, 我個人覺得很有趣, 以傳統的文本摘要來說, 我們評估的手法是用Maximum Likelihood Estimation(MLE), 計算摘要在原文摘要中出現的概率, 藉以得到一個值, 也是目前最常用的計算方法, 乍看下合理, 但實際上, 摘要在原文摘要中出現次數多, 並不代表他能精確抓到原文摘要的內容, 充其量只能說Model做摘要時有摸到這個點, 但未必能精準表達出原文精隨(聽起來真玄)。

於是本文提到一個論點, 參考[Nutribullets Hybrid: Multi-document Health Summarization](https://arxiv.org/abs/2104.03465)想法, 一篇文章是否可以轉為由幾個關鍵字組成的結構格式呢? 換句話說, 就是把一篇文章, 轉換成像表格一樣, 在此概念下, 文章內的句子可以轉為切成各個顆粒, 而每個顆粒可以分為P、I、C、O, 這樣的手法, 若應用在固定敘事格式的文章上, 或許是個不錯的方法。

* P, Population: *who is studied?*
* I, Intervention: *what intervention was studied?*
* C, Comparator: *what was the intervention compared against?*
* O, Outcome: *what was measured?*

## Evidence Inference, 摘要衡量指標

經過這轉換, 每篇文章、摘要, 都可以被轉換成許多組的P、I、C、O, 而這個轉換的過程, 本文是借重[Evidence Inference 2.0: More Data, Better Models](https://arxiv.org/abs/2005.04177)的Model和Dataset, Dataset是找出可以被歸類為P、I、C、O的關鍵字, Model的部分, 則是一個預訓練模型, 主要是評估P、I、C、O的關鍵字, 是屬於哪個分類, 分類共有*increases*, *no\_change*, *decreases* }等3類。

值得注意的是, 在文章內實際會用到的關鍵字組合, 只有I、O, 主要是P, *who is studied*對於摘要結果影響不大, 意思是說美國學者、台灣學者研究出來的結果一樣重要; 而C, *what was the intervention compared against?* 加入後, 基本上就不容易切詞、分辨, 故作者捨棄了這兩分類。

故作者用PICO把文章結構化之後, 基本概念如下圖, 從輸入、輸出到實際摘要, 都可以轉為PICO格式。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-6-1024x279.png?resize=525%2C143&ssl=1)PICO在文章轉換示意作者提出一個評估指標, 以摘要模型產生出來的REVIEW, 和實際預期REVIEW, 都能萃取出符合PICO格式的集合, 接著用PICO分類Model, 去計算這些集合會落在哪個分類中, 譬如*increases*, *no\_change*, *decreases* }等3類, 並表示為(1,0,0), (0,1,0),(0,0,1); 生成摘要的I/O Pairs(Q), 和真實摘要的I/O Pairs(P), 計算Jensen-Shannon Distance (JSD). 越小越好, 代表PQ距離越相近。

值得思考的是, 如果依賴PICO的規則, 則產出的摘要, 將會很大程度依賴其準確度, 在實際應用上, 是否能切確表達出真實意涵, 依舊無法說得準, 且PICO的分類模型將會決定摘要品質, 但可以確定是摘要結果將會更往關鍵字推進, 而非仰賴抽象化的摘要文字。

## Table-to-table task

以前述為基礎, 作者延伸思考, 若是PICO可以衡量摘要和真實摘要(Ground Truth)的之間的距離, 那是不是也能衡量STUDY和REVIEW的距離？也就所以往下提出了分別對摘要前文章、摘要結果個別做PICO後, 再去計算兩者距離, 並產出了以下結果, 但是和我前述想法一樣, 產出結果受到PICO規則影響, 但作為一個度量標準, 是有可能實現的, 也能某個程度體現STUDY和REVIEW間的關係, 這樣的做法某程度上也是MLE, 只是轉換成用關鍵字製成的表, 再用表去計算兩者相進度而已。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2022/03/image-7.png?resize=525%2C178&ssl=1)最後總結作者提出的幾個要點如下：

* 雖然生成摘要很通順,也符合主題,但和正確摘要的方向並不理想。
* 指代問題和PICO的萃取情形,限制了Model在文章上標記的正確性。
* 摘要的評估指標是個不容易解決的問題, 即便把摘要結果先做分類後再計算, 仍顯不夠真實。

而PICO的標記法, 仍受到指代問題影響, 此外本文提出的手法, 可以有效應用在固定格式和陳述手法的文章, 例如學術研究文章, 但若是要做到更廣幅度的辨認, 則需要應用Retrieval的方法, 才有機會做到 Open Domain文章的處理；此外相異論點的語句仍是一個挑戰, 且面對相異論點, 如何做到兼容, 並產出同時闡述兩論點的結果？

