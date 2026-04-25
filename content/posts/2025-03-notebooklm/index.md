---
title: "擁抱NotebookLM？聊聊用Gemini建立更可靠的知識庫"
date: 2025-03-15T14:37:00Z
lastmod: 2025-03-17T06:48:11.189Z
tags: ['大語言模型', '機器學習與自然語言處理', 'Large Language Model', 'ChatGPT', '自然語言處理', '深度學習', 'Multi-Documents Summarization', '機器學習', 'NotebookLM', '人工智慧', 'Summarization', 'Gemini']
aliases:
  - /2025/03/notebooklm.html
draft: false
---

## 前言

開始之前，我們不妨先了解一下**知識庫**是什麼？ **知識庫**（Knowledge Base）是一個集中管理和儲存各種資訊、知識、解決方案或專業知識的系統或資料庫，最終目的是為了幫助人們或是快速找到所需的資訊，解決問題，甚至獲得相關的專業知識。

[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-2517061773757238611.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEioMMzLzF-BDbo6eZfBioaOK9\_FWBmEaWwpGrNLza\_z8RgXFpxkh0aPVJvMTfXF5Ype6b-l5DNdWBn-PZKLeZ0haZi5lzoQLHsXJUJdINy9q1fE\_CIwtJxs-Bi2JY2VA60SY-bxTwOeI6bT10duMZwvDD2IINmYjwGLmKD9S6p4LmiSS6QvpgpaOCf1kgk/s640/pexels-rdne-8369827.jpg)

在日常應用中，知識庫可以應用在很多領域裡：

1. **技術支援**，像是使用指南、常見問題或解決方案等等，這樣可以讓客戶或使用者自行得到答案，減少聯繫支援人員的書信往返。
2. **知識管理**，在企業或組織內部，知識庫可以儲存公司政策、技術文檔等，目的是讓員工可以更快速找到需要的資料，做到知識傳承，進而提高工作效率和生產力。
3. **學術研究**，學術上的突破，通常要仰賴很多大大小小不同的研究資料、學術文章和期刊等等，才能得出一個具有公信力的結論，透過知識庫整合，可以有效提升找出結論的效率。

而作為一個知識庫，需要具備幾項特徵，才能滿足讓使用者快速找到需要的知識和解決方案：

1. **結構化內容**，資料需要有條理的、可分類的，容易被檢索。
2. **可搜索性**，使用者的問題可能很零散，所以需用片段文字或關鍵字能快速找到資訊。
3. **持續更新**：在知識爆炸的年代，怎麼跟上現況，更新資訊或解決方案就顯得特別重要。
4. **自助服務**：用戶可以不需要依賴支援，就能獲得所需的資訊。

綜合以上，要完成一個真正強而有力的知識庫，著實不是一件容易的事情，不僅僅是一個聊天機器人、搜尋引擎或是計程車司機、軍訓課教官、補習班老師就能滿足所有人的需求．再加上封閉領域（Closed-Domain）和開放領域（Open-Domain）的知識裡亦有許多歧義問題要面對，因此在深度學習和巨量資料研究蓬勃發展的今日，這仍是一個迷人且具備高挑戰性的課題。

在ChatGPT橫空出世後，大語言模型**（**Larger Language Model**）** 捲起了一波新浪潮，面對人類的提問，ChatGPT幾乎有問必答，我們甚至會覺得他在開放知識領域的問答(Question-Answering)任務表現上幾乎無敵了，但是Chat畢竟是Chat，基於[Autoregressive](https://zh.wikipedia.org/zh-tw/%E8%87%AA%E6%88%91%E8%BF%B4%E6%AD%B8%E6%A8%A1%E5%9E%8B)的GPT Model在[NLG（Natural Language Generation](https://zh.wikipedia.org/zh-tw/%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E7%94%9F%E6%88%90)）表現非常好，但LLM還是存在幾個與生俱來的缺陷如：

1. **知識更新延遲**：LLM的資料來源常基於訓練時的資料，可能無法反映最新的資訊或事件。
2. **偏見與不當內容生成**：除了幻覺（hallucinations）外，LLM可能從訓練資料中學習到偏見，導致生成帶有性別、種族或其他偏見的內容。

也因為模型是基於已有參數產生結果，所以這些答案是有限的，，但也是無限的，你無法知道得到的回結果是否有符合某些規範或是領域，即便[先前有論文](基於 Fusion-in-Decoder 之中文開放領域問答研究)提到，只要搭配良好的Prompt和參考資料，模型也能回答出指定的問題，但我們卻也發現，模型無法很肯定地給出使用者認知範圍內的答案，



| ![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-2247613630945600360.jpg) |
| --- |


  
 [![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-8100046968017851099.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEijqx18q8QumUxdy\_2j8g1WgXtSTRUVgJMWNyt0E\_qH6f0xOIVOdyUJ9DRlc2J0XxiEXDT1pVSlpMTfc8hhGpVUIW1S6xkwkg1R\_WEoIBRmlQxIzEFhZPXcmglzidXv1tWWjl41yePiLG2FzHHK42OsNe4ZNsIqH2rDYeg-ZLGmhrYw8YWquNSvBvG6Ha4/s770/1741319444469.jpg)  
[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-5844670462887590994.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgdjrVaWYd9rEzCnrhQNTEczpxHhs0O7meFJLhqV2C-PPKI4iejJEeDgdN0V1iRETIU-Jiq6pXrDUusLIE4FvCdyK0R1YaQRB5\_BrG89dtKDMvPI2IuMbX1XnXcOHip7QB7sgh3msAHZO9vwnv4PDs96D6375FX3d\_1RqWzsCVo-c1ZSERr8GaQp28SL24/s769/1741319490613.jpg) 從上面的範例可以看到，即便答案不存在參考清單中，模型仍會試圖依照自己已有的資訊嘗試回答，如果以建立知識庫來說，這就會成為一個謬誤，但也有解決方法，只是開放領域知識的相互影響就是如此，如果要應用在金融保險、醫學藥理上，可能就會造成誤解。 

我們希望AI可以全能，有趣的是，有些時候看來我們必須做些取捨。

## 不一樣的大腦

接著來說[NotebookLM](https://notebooklm.google)，它是在2023年由[Google實驗室](https://zh.wikipedia.org/wiki/Google實驗室)推出的一款筆記服務，雖然市面上的筆記服務相當多如[Evernote](https://evernote.com)、[Notion](https://www.notion.com/zh-tw)等，且各自在協同工作和知識庫管理都有所長，但就在Google為這服務裝上一顆[Gemini](https://gemini.google.com/?hl=zh-TW)的大腦後，帶給使用者很棒的全新體驗。   


在開始之前，不妨先簡單了解一下Gemini。Gemini是由Google DeepMind開發的一個人工智慧系統，旨在進一步提升機器學習和自然語言處理的能力。雖然它在簡介上看起來與其他AI系統相似，但Gemini與ChatGPT有著顯著的不同之處。ChatGPT採用了基於GPT（Generative Pretrained Transformer）架構的語言模型，主要專注於文本生成和自然語言理解。而Gemini則採用了Pathways架構中的PaLM 2（Pathways Language Model），這是一種更為先進的模型，能夠在多模態學習中展現出卓越的表現。多模態學習指的是處理來自不同來源的資料，如影像、聲音和影片等，Gemini在這些方面表現得更加出色，能夠有效融合和分析來自不同媒介的信息。


此外，Gemini的推理能力也比ChatGPT更為強大。雖然ChatGPT在文字生成方面表現優異，但Gemini能夠從圖像或聲音中推理出文字，這使得它在許多場景中更加靈活。例如，當需要從圖像中提取文本或理解圖像內容時，Gemini的推理能力可以提供更準確的解釋和答案。總體來說，Gemini在處理更複雜的多模態任務方面展現了顯著優勢，這也使得它在某些應用場景中比ChatGPT更為強大。

## 支援廣泛的知識來源

 支援多模態輸入（Multimodal Input）的關係，模型不需要額外使用檔案轉文字的上游任務，另一方面，同時處理來自不同來源或形式的資料，如文字、圖片、語音、影片等，並將這些不同形式的資料結合起來進行分析和理解。對於AI模型來說，這樣的輸入能帶來很多優勢，像是
1. **更豐富的資料來源，**模型能夠從更多樣化的資料來源中學習和推斷，避免忽略或是無法充分利用來自其他模態的線索，例如，結合圖像和文字，模型能夠更準確地解釋圖片內容，或從圖像中提取更多隱藏的訊息。
2. **提升理解能力，**多模態學習能夠增強模型的推理能力。舉例來說，圖像中的物體可以用文字來描述，而文字中的描述也能幫助模型更好地理解圖像內容，這樣，模型能夠進行更深層的推理，比如從語音推理出文字，或者從圖像中提取出有意義的資訊。
3. **更強的語意感知，**結合不同模態的資料能讓模型更加靈活地處理複雜的語境時。例如，在語音識別中，模型可以結合語音和影片來理解說話者的情感或語氣，或者在圖片標註中，模型可以結合文字和圖片來提供更精確的標註。


| ![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-2176842787806633624.jpg) |
| --- |
| 當然不要忘記的是，完成這些複雜工作的前提是：請給我黃金。 |


  
## 更有想像空間的應用

所幸的是，Google不僅幫助我們省去了Gemini模型預訓練的巨大花費，還嘗試提供一些服務來展示它的實力。接下來，我們來看看NotebookLM能做些什麼。在進入頁面之前，系統會引導你加入一些參考資料。

#### 上傳參考資料

[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-4689606028803765773.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgHGeQ\_cqTuhC7837UxJrdploJrFy6KwcdXcSeGEpO9Pnkf3EO41-ymPqFF\_60u1V5tcoFkW5FqHiDnF2orkG\_wYYGDAuVlArqmcUB4fJSbBP14OXKwI9v1qH4Vwp\_Y0pUEc34sKbD7Tjp3lEwLZwjL84ZTDss-V-FGHg4sYvHPRT600BhCM1WbwBhjtkY/s2024/1741596332609.jpg) 我們可以選擇我們需要的知識來源，包括文字檔案、PDF、Markdown和MP3。這些資料有一個關鍵優勢，那就是它們都是開放格式，並且與模型最初的訓練來源密切相關。此外，我們還可以指定來自Google雲端硬碟上的Google文件，甚至是易讀的網頁，如Wikipedia，當然也少不了Google自家的YouTube。

#### 產生初步結論

在我嘗試上傳以下幾個連結，作為我的知識庫來源，會發生什麼事情呢？ 

1. [Everything You Need to Know About Your Passport](https://www.youtube.com/watch?v=aTT3\_C\_X4gE)
2. # [Vietnamese identity card - Wikipedia](https://www.google.com/url?sa=i&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FVietnamese\_identity\_card&psig=AOvVaw1KQryuYtUpBMvuX2lhDSck&ust=1741771775417000&source=images&cd=vfe&opi=89978449&ved=0CAYQrpoMahcKEwj4tbbR24GMAxUAAAAAHQAAAAAQBA)
3. # [中華民國國民身分證- 維基百科](https://www.google.com/url?sa=i&url=https%3A%2F%2Fzh.wikipedia.org%2Fzh-tw%2F%25E4%25B8%25AD%25E8%258F%25AF%25E6%25B0%2591%25E5%259C%258B%25E5%259C%258B%25E6%25B0%2591%25E8%25BA%25AB%25E5%2588%2586%25E8%25AD%2589&psig=AOvVaw3QaF-pDc5dfewLZndGRvX9&ust=1741771662249000&source=images&cd=vfe&opi=89978449&ved=0CAYQrpoMahcKEwiglZKj24GMAxUAAAAAHQAAAAAQBA)
4. [中華民國護照- 維基百科，自由的百科全書](https://www.google.com/url?sa=i&url=https%3A%2F%2Fzh.wikipedia.org%2Fzh-tw%2F%25E4%25B8%25AD%25E8%258F%25AF%25E6%25B0%2591%25E5%259C%258B%25E8%25AD%25B7%25E7%2585%25A7&psig=AOvVaw1K2xGanMrlYi3ol4JCnG\_y&ust=1741771738598000&source=images&cd=vfe&opi=89978449&ved=0CAYQrpoMahcKEwj43bW\_24GMAxUAAAAAHQAAAAAQBA)

可以看到，除了最中間會先產生一個簡單的文字摘要外，還有幾個可用選項。

在頁面右上角，您可以選擇生成一段語音摘要，這段語音是由兩個主持人進行討論，並對這些資料進行分析。可以將這個功能視為自動生成一個類似Podcast的語音內容。這項技術的實現非常複雜，背後涉及到AI的多項高級技術：首先，AI需要理解並解析您的資料（自然語言理解，NLU），接著根據這些資料生成對話（自然語言生成，NLG），並且可以進行連貫的對話交流。

[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-1112112205590731343.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-aoDXATuyDWStYEP1URKoNDNBuV5jiPg6b3k5sbtwoTD3mLsojIbrHLgcd3qqRVMHDJokuIJFcC7i6WH7-3X3Ty8XP1X1\_HgYBVM-q3hDH9lLDGlwegM4rGbnW4l4kkL54LX4hhps1l8D7V-wP59ORMdDpPz6Lb0MCCKJiwJHz8rJzsvDs4jjLIyD3pY/s1446/1741684607377.jpg)另外你可以在記事頁面，選擇針對這些資料產生研讀指南、產生簡報。

[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-9157073990683776497.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjdAcpNClvjUQVdE6T8XWMHAu88WWlRmoGuHMbRP7i0ir2Y4MczJF\_uarrHmB\_aHATaZzj8dYLiZWUhb6SXEO713vfu7qKhW60RrN3RTeTfe\_olNsGlureURjn1TXdVBa3tKhYHQMLxRR2Z7vh2ziBIKvXpiQcbUxSORKb9RGQGkDU00wb30eCFXLgv9EY/s1436/1741684575307.jpg) 除了研讀指南、產生簡報，更有趣的是它可以針對資料內容去做年份排序，比如說你有三份資料，各自記載不同年份的相關內容，而模型可以把這些文字讀取進來後，產生一份像是大事紀的內容。

[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-3202503244242954931.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgOjQCV\_-pW3cOn3n47yzew7d0OcF2pAOUD5oTO9wplqpmtM1Fhyphenhyphenj\_Hk9uWNIUXOVhSGdJg6Q\_vDnrOEWdzEEjs4gFOqmezHZkQEadcmIGSIzRYj8Z1PvFhX7G5X1MNOhEyV8i6n6dXaws4y43dlBdIBqFM-DnfcashUxVDjJVejpl8iLfa0zq0nOF6HR4/s1440/1741684638518.jpg)## 再多玩點什麼

#### 知識彙整和提問

我嘗試把PMP（Project Management Professional）課程的相關資料上傳到NotebookLM中，可以看到他先是簡單地幫我完成了一個總結。[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-1676382985522338524.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgYYpmtNWDd1Z7kJ95WXCNg8EIBBa80GTHlc0PiAESUqm4NMSLq3HkybDyvmfVWmpeOfP9ZA5j0-6nWwvj5Mbynhp-UZlTDsVWWVuohoiT6aX6Z4MayTRukVsBey98QpyRd73MIPGTNSZ59bocNfSf6OB0gR0Evtc0C0n\_CZ3VPLOLq8HSQQgOxuUq-3xQ/s1436/1741941953303.jpg)接著我們可以像是使用ChatBOT一樣，不停地不停地對他發問，而他會很有耐心地回答每個問題，在他的回應中，你會發現系統刻意標記了數字出來，代表這個答案是基於哪份參考資料，表示這些內容都是有所依據，可以得到驗證。## [![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-7225623878720524477.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjla7RckvEutLwtw\_wIZU7Z7tYO1i8Cxzea6rDq4CM4x\_\_6dnWBN1ynfa\_gi7aDw0EILqpI\_DHMqrjlilDpc6RqtVI\_CGmV-0qDI1PqmRN9MxqccNJwPFSA7N9LsrAD45WBRGO1pyTx-wG65NecqNP8eOC7hBooy59Ob37o2k567dMlj7T-TZtiQXLA0Wc/s1439/1741942196097.jpg)

當然你也可以使用不一樣的Prompt來得到想要的結果，這部份就像一般的LLM一樣，像是我下的指令，請他依照我的資料產生5個選擇題，而且還標記了正確答案出來，效果非常好，也許以後老師們不用再絞盡腦汁出考題了？## [![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-545211114091046778.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj3Bxn2UXXWg7pp8Q9FSPHb9N2o1KeWRggnLd0pgqHR6YulMPG6gKvog-RZi\_OC-19NJIdqu-tNucRmAbbh6MrXTR-kvznGelB5feNeh0eXhODPeLuttWKY5mU\_gjsZliVlVtYqBF7hlitKcAgEt-5eDybinDB9DmZ-OtzNZWoJn2QDQQI94J2JngufP9A/s1441/1741942123905.jpg)

我們可以使用比較迂迴的問法，進一步了解模型對資料的理解程度。例如，透過設計假想情境來測試模型的應對能力，要求它在特定範圍內提供解決方案。 這樣的提問不僅能幫助我們測試模型的準確性，也能了解它如何解釋和應用資料中的知識。特別有趣的是，模型在處理這類情境題時，能夠提供清晰的解決方案，並且給出相關的說明，進一步加強了它的分析能力。更重要的是，這些補充說明往往不會偏離主題，保持高一致性，這對於需要精確解答的情境來說非常關鍵。 透過這樣的方式，我們不僅能夠驗證模型對資料的理解程度，還能深入探討它如何在具體情境中提供實用的解決策略。這種靈活運用模型的能力，使得我們能夠更全面地分析和利用資料，並獲得具體且有價值的答案。## [![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-1361449116535859033.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjC5FwKLDF3YuARUg71812SINpFTWnxdaPFexKINKMR-SQgs1nUu25K6GGf2mk8T4uigKUcXcTBH7ChO3SeeBdhIjtiK\_IyvqwFqn6SCYZOEhqKYOIyBBN0NFUbWU5AuJ0WEXilcfM-CSxReJmUgZA8DkPOyvXMO\_JZiSWelHHSccLRuLwza6zZagOp9mk/s1442/1741942068748.jpg)[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-8150793124311863853.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEidUgT7uh4S\_kzXtqQ5TGueLU4bG9kxc15f2XPTrPPlKL\_IdlcFZPgCmsiZD5u9shUk0897cIGknNkqsKuxnfFOchYpDZkZSxTgi-4l4AXqZZNsphEe43qf8Cw\_VWyQErE4cf3o\_w-yNod77F0ji-Eol7tu\_D1C3OezooogkRlwgt2XGIyXUHvhZG7fIcM/s1444/1741942913779.jpg)

當然，如果你亂提問，或是問的問題已經超出範圍，模型還是可以辨別的。[![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-160603344941981579.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh7MdkIO3QkVw-OYSIuxxYhE8hCxwz7bpUd5Ssgp1YNjulM04d-yx4QcMrJV3UIUe8wwhj\_FlelUCnzL9hmByYDF-lTtrzgmA\_x70p9NaGoWKqHBGA6kaKwtDF8lw3pGcTrAvvr75UbUJNWdAozhBUNSPeYxgu3YKNW9-yTaCiDLybhcCluUH6ePkk18sY/s1435/1741942949419.jpg)#### 統計和分析

接著，我上傳了一份許多保險從業人員都很熟悉的資料：商業道德和金融市場常識題庫。這些題庫匯集了歷年來有關這兩個測驗的考題。我想了解這些題庫是否有什麼特徵，或者需要特別掌握的重點。過去，我們的做法是將每個考題讀取進來，然後進行統計和分析，以便獲取一些有用的訊息。但在使用NotebookLM之後，我可以選擇透過Prompt來完成這些工作。於是，我向它提問：「請幫我分析這些考試題庫的出題趨勢是什麼？」#### [![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-874824762238742925.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi1t4S\_1ZQvcAg2MhPe4ltnGZCfSFrABDnUHwn4dAtmgZ9Lmh0MtOKkiRH-Mwf5v-ysg6VUi5W4-z2\_PMNeOQS130dx6ovQW73TG0txPThewAw5YtrasR-r1JXYTKTsg0hPeKd\_oUw5SHIq4rWleCSNsaOp-SHzjG\_u-ByoOHZPQjUhyphenhyphenQ-MS9Z23pNFeTM/s1446/1741944914773.jpg)

我們可以看到模型為我們提供了清晰的分析，列出了主要要點，並做了相關的說明。當然，我們也可以換個方式提問，例如問「哪些類型的題目最常出現？」。這樣一方面是透過不同表達方式，但隱含相似需求的Prompt，來進一步對比模型的回答，從而了解其回答的可靠性。這也正是將知識庫鎖定於相同來源的好處，因為這樣我們可以更容易驗證模型的答案是否正確。#### [![](/posts/2025-03-notebooklm/images/2025-03-notebooklm-1297266397180569623.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgxeepG-zM\_BeMXTjppviI2ZN5ssgIOm8EzL2jbhaNGf-653F9plbca3VbsOaGulTL5xTCthgMAGsjJsgikXCBtcncMjukx\_Psp7-cSEcQCA4aFV086sRYNfvbM\_jsm4bQGckBEyq\_5yIZJjjnKf-\_rkpz5nP8vHHbEuosrgwGNiJoeaSzvbDJAolFfEOM/s1440/1741944836494.jpg)

## 誰比較好？

在開始討論誰比較好之前，首先，我們必須畫靶再射箭，這次的主軸是使用LLM建立知識庫，在整體系統來說，NotebookLM的體驗帶來比較多樣，雖然他在整個操作行為上仍有需多工程專業的影子，但絲毫沒有影響到Gemini大秀肌肉，再回到透過模型提供知識的初衷，我們來簡單為ChatGPT和NotebookLM做點比較。#### ChatGPT

採用[GPT-4](https://zh.wikipedia.org/zh-tw/GPT-4)或GPT-3.5系列模型為主，這些模型以[Autoregressive Model](https://en.wikipedia.org/wiki/Autoregressive\_model)為核心，通過巨量資料的預訓練來支持，在處理文字生成的任務方面表現相當強大。因此，它們在對話生成、問題解答、寫作，甚至程式碼生成等領域都有優異的表現。畢竟，這些模型背後的巨大參數，代表著先前的智慧和研究成果。在開放知識領域中，它們的確少有對手。#### NotebookLM

在模型設計的初期，我們就將理解不同檔案內容作為核心任務之一，些檔案中不僅僅包含文字，還可能包含表格、圖片等其他形式的資料；當這些不同形式的資訊結合後，所能傳遞的內容變得更加豐富。隨著模型對這些檔案的理解能力不斷提高，其分析結果將會更加全面。


此外，從某種程度上來看，這樣的模型還能實現“Retrieval-Reader”的概念：具體來說，就是當輸入問題後，模型可以進行搜索，找到對應的知識庫資料，並透過模型的閱讀能力產生相應的結果，相比GPT系列用[無監督學習](https://zh.wikipedia.org/zh-tw/%E7%84%A1%E7%9B%A3%E7%9D%A3%E5%AD%B8%E7%BF%92)來擴充參數量，這個方法的時間差的影響更小，模型依然能有效地處理這些複雜的任務。

#### 結論

有段時間，許多人開始在生活中使用ChatGPT來實現各種系統，解決各種不同的問題。這些系統涵蓋了從簡單的問題解答到複雜的文書創作、程式碼生成等範疇。儘管回過頭來反思，會發現像這樣的NLG（自然語言生成）方法確實表現不錯，能夠完成許多任務，但我們也常會發現，這些結果並不總是完全符合我們的預期。這其中的關鍵在於，模型生成的內容雖然在語言流暢度上優秀，但當涉及到特定領域的精確性和深度時，往往難以保證完全符合我們的需求。


舉個例子，像總結（Summarization）這類任務，不論是Extraction還是Abstraction，在模型訓練過程中實際上缺乏有效的驗證方法。當模型生成總結時，我們無法簡單地依賴一個固定的指標來判斷結果的準確性，這就是[ROUGE](https://en.wikipedia.org/wiki/ROUGE\_(metric))或[BLEU](https://en.wikipedia.org/wiki/BLEU)等常用指標無法解決的問題。這些指標通常側重於匹配度，但真正的挑戰在於，如何界定所謂的「Gold Answer」——即理想的參考答案。每個任務的理想答案其實有很大的變數，這使得對模型結果的驗證變得非常困難。

總的來說，我仍然傾向於使用ChatGPT來讓我的語言更加流暢和優美，這對於日常交流或簡單的創作任務來說非常合適。ChatGPT能夠將我們的想法轉化為簡潔、優雅的表達，無論是在寫作、對話還是其他語言生成的任務中，都能提供很好的支持。然而，如果我需要更詳細、且我認為更準確的答案時，尤其是涉及到需要深度理解和具體專業知識的問題，我會選擇NotebookLM。NotebookLM在處理專業知識和技術性問題上有更強的優勢，它能夠提供更精確的資訊，並且能夠根據資料庫中的具體內容來生成答案。


總結來說，不同的模型在不同的場景下有不同的優勢，理解這些模型的設計初衷和應用場景，能夠幫助我們更有效地選擇工具，從而達到最佳的工作效果。

