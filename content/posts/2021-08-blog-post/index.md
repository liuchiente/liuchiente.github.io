---
title: "淺談人工智慧和學習"
date: 2021-08-12T14:42:00Z
lastmod: 2025-01-14T06:30:55.026Z
tags: ['機器學習與自然語言處理', '自然語言處理', '深度學習', '機器學習', '人工智慧']
aliases:
  - /2021/08/blog-post.html
draft: false
---

休息了一小段時間, 總是該開始寫些我真正想要的東西, 當時選擇到研究所進修, 主要就是對深度學習這個領域相當感興趣, 也嘗試過自行摸索, 但總感覺就是少了些什麼, 後來我覺得, 也許就是少了一個有脈絡的學習吧？！接下來我會開始從比較細節的知識開始整理起, 畢竟這才是我真正想要從研究所帶走的東西。

先岔個題, 聊聊我曾嘗試做過的搞怪玩意兒, 我在前公司主要是負責產品研發, 那時我深深為人工智慧著迷, 所以我很想看看能不能在既有產品上加入點「智能」成分, 當時我負責研發的系統是BPMN流程服務, 那是一個基於工作流程而生的引擎服務, 你可以利用這工具來設計任何符合你工作環境需求的工作流程, 在您身處的環境中, 也許就是公文簽呈、製造流程等, 搭配ERP你可以做到出庫、投料甚至接單生產等自動化, 可以說, 流程引擎將串起你工作環境中的每個節點。

而我希望可以**讓流程自動轉送到更正確的地方, 甚至於, 加速每個節點的處理速度**, 於是乎, 我會開始記錄每個節點處理不同工作的效率和時間, 接著我會把這些紀錄做萃取和計算, 以此了解每個節點對這工作的重視度, **預測負責人的心理, 得出一套行為模式**, 在引擎接到工作後, 以此行為模式來判斷該如何加快節點的處理速度, 避免無意義的閒置。

[![](/posts/2021-08-blog-post/images/2021-08-blog-post-1460040633518792401.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEja8yXi1wCNklkxmyiBmt2HM8QgirrrWTOY7Nd\_pi9nOfF27G9m7xvpQpqks6NKrjEyo8DENrcUWcvl8dHAbBexuJKL9U6k9JN7VPIHem-kA4FGr4kpBfK3nkCiSwmOF2T9u7lgjLdyuTdxjczu2DKe\_Tpu3gUatUB-BhVrTJ3xyPrs9bI8poF0SFaGDl8/s640/pexels-startup-stock-photos-7367.jpg) 綜合上述行為, 基礎的人工智慧, 不外乎就是以下這樣：

1. 收集資料（Gathering data ）
2. 準備數據（Preparing that data）
3. 選擇模型（Choosing a model）
4. 訓練機器（Training）
5. 評估分析（Evaluation）
6. 調整參數（Hyperparameter tuning）
7. 預測推論（Prediction）

當然人工智慧的領域相當廣泛, 從最廣最廣的層面來看, 就包含了「時間序列與預測」、「圖像辨識」、「音訊處理」、「自然語言處理」、「動態影像處理」等 。

**時間序列與預測**, 就是我最早最早做出來的土炮預測, 我們針對歷史數據作趨勢分析, 接著做預測分析、風險分析, 甚至推薦引擎等應用。

**圖像處理**, 則是專做靜態影像的處理, 最常見就是人臉辨識、圖像辨識和機器視覺等, 我也曾嘗試用CNN做了一個模型, 也曾達到不錯的效果。

**音訊處理**, 則是專做聲音資料處理, 每段聲音其實都有自己的波形, 而這波型就是一個特徵, 最常看到的應用就是語音識別、情感分析和語音搜尋等, 而我們常看到的Siri, 或是先前轟轟烈烈的「[甘安捏](https://www.youtube.com/watch?v=Nw7jmoH-Yt8)」,也都有音訊處理的影子存在, 可以特別注意, 影片剪輯的都是同一人講的「甘安捏」。

**自然語言處理**則是專門處理字詞的領域, 甚至可以往下分為自然語言理解 (NLU), 自然語言生成 (NLG), 而NLU可以再往下分到情感分析、文章摘要, NLG則專注在文本生成。

**動態影像處理**, 想像一段動態影像, 他其實是由一堆的靜態影像處理, 但你得記住上一個動作和下一個動作的關係, 可以做到動態預測, 近期也有人開始做Deep Fake影像識別, 當然Deep Fake也是動態影像的一塊。

而人工智慧, 其實又不完全是真正擬人智慧, 往下還可分為機器學習和深度學習, 以下是我從網路上找來的介紹圖, 清晰且彼此的關係更能一目瞭然。

![](https://i0.wp.com/liuchien.ink.tw/wp-content/uploads/2021/08/1I-rTLtMaWeRG1dpaiUczNg.png?w=525&ssl=1)從上可以看到AI是個很廣很大的領域, 而機器學習只是其中一個分支, 深度學習則又是機器學習底下的一個分支, 再補充網路上找來的概要。


> 以下節錄自Wikipedia
> 
> ****Artificial intelligence** (**AI**) is [intelligence](https://en.wikipedia.org/wiki/Intelligence) demonstrated by [machines](https://en.wikipedia.org/wiki/Machine), as opposed to the **natural intelligence** [displayed by humans](https://en.wikipedia.org/wiki/Human\_intelligence) or [animals](https://en.wikipedia.org/wiki/Animal\_cognition). Leading AI textbooks define the field as the study of “[intelligent agents](https://en.wikipedia.org/wiki/Intelligent\_agent)“: any system that perceives its environment and takes actions that maximize its chance of achieving its goals.**  
>   
> **Machine learning** (**ML**) is the study of computer [algorithms](https://en.wikipedia.org/wiki/Algorithm) that improve automatically through experience and by the use of data.  
>   
> **Deep learning** (also known as **deep structured learning**) is part of a broader family of [machine learning](https://en.wikipedia.org/wiki/Machine\_learning) methods based on [artificial neural networks](https://en.wikipedia.org/wiki/Artificial\_neural\_networks) with [representation learning](https://en.wikipedia.org/wiki/Representation\_learning). Learning can be [supervised](https://en.wikipedia.org/wiki/Supervised\_learning), [semi-supervised](https://en.wikipedia.org/wiki/Semi-supervised\_learning) or [unsupervised](https://en.wikipedia.org/wiki/Unsupervised\_learning).

簡單來說, AI就是透過機器去達到任何智慧化的目標, 這也是最廣泛的定義, 也是市面上最常見到的詞彙, 傳統上我們用Rule based來實現這個願景, 當然大多智慧化的工具也以此為基礎去發展,而將來我們力求越少Rule越好, 畢竟Rule是永遠寫不完的, 又或者說, Rule是很主觀的, 我們要怎麼讓Rule更客觀？

也就所以**ML(Machine Learning**)開始嶄露頭角, 就以上對機器學習(ML)的定義, 我們開始透過資料收集, 特徵分析, 來得到更好的Rule, 再用這些Rule來改善我們的工作, 在這裡最重要的特點, 就是我們用資料來驅動我們的方案。

那又為什麼有**DL(Deep Learning)**深度學習？因為我們希望可以減少特徵分析這個行為, 因為特徵分析是費時又費工的, 且也帶有一定的主觀成分, 故我們希望讓模型(Model), 基本上是多層的類神經網路, 來自動得到特徵, 並將特徵學習起來, 好作為預測依據, 當然深度學習又可以分為非監督式、監督式學習, 差別就在我們怎麼去提示模型取得特徵。

[![](/posts/2021-08-blog-post/images/2021-08-blog-post-7714748304111882030.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgPkFHmH0aCx5824M3P80p9szTpa9k5A7nB-WDvJVvh-fTHGXTD1KhlgfB7HXNadIqKbRYDxXd2UcN0K9960QeLigs1r14QrGgM9Rt6AkcVaXCRi5iRsQuZ84auZJkEkdwLagTOCp2IvwZbNsSKOUG\_CJq9x2aKzqS4hJTCPal-MEXeVcg\_yoxXzRbR6QA/s640/pexels-kureng-workx-2546437-4314674.jpg)綜合以上, 我的土炮嘗試是屬於機器學習的一環, 我利用傳統的方法把每個節點的使用歷程記錄下來, 然後設定一定的Rule來分析和萃取歷程, 在用分析萃取完的結果來預測這個節點將來要怎麼面對剩下的工作; 在進入研究所後, 我開始接觸深度學習, 並以自然語言處理作為研究主題, 往後則將開始記錄在這門研究上我需要用到的知識和技術。

我們要過的精彩, 肯定得充滿期待。

