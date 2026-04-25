---
title: "我怎麼說你就怎麼做！聊聊LLM的Prompt"
date: 2025-01-10T07:09:00Z
lastmod: 2025-01-14T06:15:57.630Z
tags: ['大語言模型', '機器學習與自然語言處理', 'ChatGPT', 'ChatBot', 'Prompt', '學習紀錄', '人工智慧']
aliases:
  - /2025/01/llmprompt.html
draft: false
---

延續上一次的文章[Prompt, Fine-tune和
Training，誰才是大工程？](https://www.blogger.com/u/1/)，受惠於大語言模型(Large Language Model，*LLM*)的發展，以往我們必須對模型去做Fine-tuning甚至Training的工作，只要基於LLM，再加上一些適當的Prompt，就可以達到不錯的效果，在筆者的論文[基於 Fusion-in-Decoder之中文開放領域問答研究](https://www.blogger.com/u/1/) 中，就曾用LLM加上Prompt，對比自己訓練出來的問答模型，發現效果不分軒輊。



| ![](/posts/2025-01-llmprompt/images/2025-01-llmprompt-145442599787638341.jpg) |
| --- |
| 也許我們可以做更多？ |


  

那麼，如果我們想讓LLM達到更好的效果，有哪幾種手法呢？
根據使用目的和方式的不同，Prompt可以區分為幾種類型：


**1.** **問題導向的Prompt (Question-Based Prompts)**


* **用途**：提出具體問題，讓模型進行回答，也是最典型的QA(Quesiton-Answering)
* **例子**：
+ "台灣的首都是哪裡？"
+ "量子物理是什麼？"


這類Prompt通常是直接的問題，模型會根據已有的知識提供簡潔且已知的答案。


**2.** **指令式Prompt (Instruction-Based Prompts)**


* **用途**：用來給模型提供具體的指令，要求模型進行某種形式的處理。
* **例子**：
+ "寫一篇關於人工智慧的短文。"
+ "將這句話翻譯成英文：'我今天感覺很開心。'"


這類Prompt常用於生成具體內容，例如創作、翻譯等。

 


**3.** **填空式Prompt (Fill-in-the-Blank Prompts)**


* **用途**：要求模型在指定的句子中填入缺失的部分。
* **例子**：
+ "台灣的首都是\_\_\_。"
+ "愛因斯坦的相對論公式是 E = mc²，這裡的 c 代表\_\_\_。"


這類Prompt通常用於測試模型對語言結構的理解，或者要求模型補充缺失的訊息，他也是一個典型的克漏字(Cloze)任務。


**4.** **對話式Prompt (Conversational Prompts)**


* **用途**：進行多輪對話的Prompt，通常是一段上下文的對話，模型需要根據之前的對話生成回應，這是Chat Bot 的重點任務。
* **例子**：
+ 人類："月亮為什麼會有陰晴圓缺？"
+ 模型："月亮的陰晴圓缺是由於它繞地球運行時與太陽、地球的相對位置改變所致..."
+ 人類："那為什麼每個月都會有這樣的現象呢？


這類Prompt要求模型在一個對話框架中生成回應訊息，並可能會根據前後文改變其答案。


**5.** **範例導向的Prompt (Few-Shot or Zero-Shot Prompts)**


* **用途**：提供一些範例給模型，讓它從中學習如何完成任務，或在完全沒有範例的情況下進行任務。
* **例子**：
+ **Few-Shot**：提供一些簡單例子，讓模型依照你的例子去做接下來的事情。
- '你好，今天過得怎麼樣？'  翻譯成 
 'Bonjour, comment ça va ?'
- '我很高興見到你' 代表  'Je suis
 content de vous rencontrer'，現在請翻譯 '我想學習中文。'

+ **Zero-Shot**： 不提供範例，也是一般的提示指令。
- "將下面的句子翻譯成法語：'我想學習中文。'"


"Few-Shot"指的是在Prompt中提供幾個範例，"Zero-Shot"則是直接要求模型完成某個任務，沒有範例提供。


**6.** **反向提示式Prompt (Inverse or Negative Prompts)**


* **用途**：引導模型生成與某種預期回答相反或不符合常規的回應。
* **例子**：
+ "列出三件與巴黎無關的事物。"
+ "寫一個故事，描述英雄最終未能成功。"


這類Prompt要求模型跳出常規的思維方式，生成具有創意或者反向的答案，在模型發展初期，也有人透過這個手法，來略過模型對於正向問題的限制。


**7.** **角色扮演式Prompt (Role-Playing Prompts)**


* **用途**：讓模型扮演某個角色，根據特定的身份或情境來生成回答。
* **例子**：
+ "你是一隻智慧的老貓，會給我什麼建議？"
+ "假設你是醫生，該如何治療感冒？"


這類Prompt讓模型根據設定的角色背景來調整其回答方式，使對話更有趣或更具啟發性。


**8.** **長文本生成Prompt (Long-Form Generation Prompts)**


* **用途**：要求模型生成較長的內容，如文章、故事、報告等。
* **例子**：
+ "寫一篇有關氣候變遷如何影響農業的文章。"
+ "寫一篇故事，描述一個人如何在無人島上求生。"


這類Prompt通常包含更具挑戰性的要求，要求模型建立結構清晰且詳盡的內容。


**9.** **分析與推理式Prompt (Analysis and Reasoning Prompts)**


* **用途**：要求模型進行分析、推理或解釋。
* **例子**：
+ "為什麼大多數科技公司選擇在矽谷設立總部？"
+ "為什麼莎士比亞的作品至今仍然影響世界文學？"


這類Prompt要求模型運用邏輯思維和推理能力來解釋問題或做出評價。


**10.** **多選式Prompt (Multiple-Choice Prompts)**


* **用途**：要求模型從若干選項中選擇一個最合適的答案。
* **例子**：
+ "以下哪一項是地球的最大海洋？  

 a) 太平洋  

 b) 大西洋  

 c) 印度洋  

 d) 南冰洋"


這類Prompt要求模型從選項中選擇正確答案。




| ![](/posts/2025-01-llmprompt/images/2025-01-llmprompt-3038848826968072183.jpg) |
| --- |
| 一切都需要從輕楚、良好的引導開始。 |


  
看完上述的Prompt外，相信已經夠讓人眼花撩亂了，但除此以外，還有一些其他高級的 **Prompt
設計技巧**，這些方法可以幫助你更有效地引導LLM生成更好的結果，這些高級技巧大多數都能幫助提升生成的精確度、創意、以及模型在處理複雜任務時的表現，以下是幾個高級
**Prompt** 的設計方法：


**1. Chain-of-Thought Prompting (CoT)**


* **用途**：這種方法引導模型在解決問題時進行邏輯推理，特別適用於數學問題、推理題目等需要步驟過程的情境。
* **例子**：
+ 問題：「如果你有 5 顆蘋果，給了朋友 2 顆，還剩下幾顆？」
+ **Prompt**：  

 "Let's think step by step. You start with 5 apples. Then, you give 2
 apples to your friend. How many apples do you have left?"
+ **結果**：模型會先解釋「5
 - 2 = 3」，然後得出正確答案 "3"。


**Chain-of-Thought** 可以讓模型在解答過程中進行步驟推理，避免直接給出答案，提升答案的清晰度和邏輯性。


**2. Self-Consistency Prompting**


* **用途**：多次請求模型對同一問題給出多個不同的答案，然後選擇最一致的答案，這樣有助於提高最終答案的正確性。
* **例子**：
+ 問題：「為什麼天空是藍色的？」
+ **Prompt**："Please
 answer the following question three times: Why is the sky blue? Then, choose
 the answer that is the most consistent across all three."
+ **結果**：模型會根據不同的答案進行多次生成，並最終選擇一個最一致的解釋。


**Self-Consistency** 主要用於處理模型可能會有些不穩定或多樣化的情況，幫助提高答案的可信度。


**3. Prompt Engineering with External Knowledge (****外部知識整合)**


* **用途**：結合外部知識庫或API的資料，來增強模型的輸出。這對於處理模型內部知識庫之外的領域特定問題尤為重要。
* **例子**：
+ 問題：「什麼是最新的人工智慧發展趨勢？」
+ **Prompt**：  

 "Using the latest data from the internet, summarize the top three AI
 trends in 2025. Incorporate relevant facts from trusted online sources
 like research papers and tech blogs."


這種方法要求模型結合外部資料，能有效解決單一訓練資料庫中缺乏最新信息的問題。


**4. InstructGPT/Instruction Tuning**


* **用途**：這是一種讓模型學習如何根據指令來執行任務的方法。通過強化學習，模型能夠根據給定的指令進行生成，並且按預期進行高效的任務完成。
* **例子**：
+ 問題：「編寫一篇關於太空探索的文章，強調其對人類未來的意義。」
+ **Prompt**：
 "Write an article on space exploration, emphasizing its importance
 for humanity’s future. Focus on the technological advancements, economic
 potential, and global cooperation in space missions."


這種方法鼓勵模型根據具體指令生成符合期望格式或內容的回應，並且能夠有效提升模型對複雜指令的執行力。


**5. Interactive Prompting (****交互式提示)**


* **用途**：在一個持續的交互式環境中與模型進行對話，以便逐步獲得更符合需求的答案。這可以適用於長時間的多輪交互，適合解決較為複雜的問題。
* **例子**：
+ 人類： 「我想了解如何編寫高效的Python代碼。」
+ 模型： 「請告訴我你目前的程式設計經驗和具體需求。」
+ 人類： 「我是初學者，想從基本語法開始學。」
+ 模型： 「好的，我會從變數、數據結構開始介紹。首先，Python中如何創建變數呢？」


交互式提示可以在長時間的對話中根據用戶的反饋調整回答，逐步提供更加精確的內容。


**6. Adversarial Prompting (****對抗性提示)**


* **用途**：創建挑戰性的提示，要求模型生成能夠處理模糊、不確定或具挑戰性問題的回答。這樣的Prompt可以幫助測試模型的穩定性和對意外情況的應對能力。
* **例子**：
+ 問題：「如果所有的魚都不再存在，會對人類和生態系統造成什麼影響？」
+ **Prompt**：
 "Imagine a world where all fish species have gone extinct. Discuss
 how this would impact both human society and the ecosystem, focusing on
 food security, biodiversity, and economic activities."


**Adversarial Prompting** 主要用於測試模型的邏輯推理能力以及它對極端假設情境的應對能力。


**7. Contextual Prompting (****上下文提示)**


* **用途**：在生成回應時提供豐富的上下文信息，讓模型更好地理解問題背景並生成更相關的答案。
* **例子**：
+ 問題：「請幫我寫一封辭職信。」
+ **Prompt**：  

 "Based on the following context: I have been working at XYZ Corp for
 5 years, but I have decided to leave due to personal reasons. Write a
 professional resignation letter."


**Contextual Prompting** 是通過提供詳盡的背景信息，幫助模型理解任務的具體情境，生成更精確的結果。


**8. Role Prompting (****角色提示)**


* **用途**：讓模型扮演特定角色來處理任務，這樣可以根據角色的視角或專業領域生成專業的回答。
* **例子**：
+ 問題：「你是一位科學家，如何解釋量子力學的基本原理？」
+ **Prompt**： "Imagine
 you are a physicist. Explain the fundamental principles of quantum
 mechanics in simple terms, focusing on concepts like superposition and
 entanglement."


**Role Prompting** 讓模型假設具體角色的專業知識，來處理複雜領域的問題，並根據角色的背景提供不同的視角和解釋。


那除了這些以外，[先前我們用來寫程式的Prompt又算什麼？](https://www.blogger.com/u/1/)因為它牽涉廣泛的思考和結構，並且要有完整的起承轉合關係，包含情境、步驟、對應措施等，雖然複雜，但可以讓LLM承擔一些更有趣的任務，他就是**Multi-Chain Prompt**。


**9. Multi-Chain Prompt****（多鏈接提示）**


**Multi-Chain Prompt**是一種高級的 **Prompt
設計技巧**，旨在通過將多個子任務或多輪推理結合起來，讓語言模型（LLM）逐步生成結果。這種方法特別適用於處理複雜的問題、需要多步推理的情境，或者當問題涉及多個方面且需要進行多輪推理時。


在 **Multi-Chain Prompt** 中，模型的每個步驟（或 "鏈"）都與前一個步驟緊密相連，形成一個連貫的邏輯推理鏈條。每一個 "鏈" 都會逐步構建問題的解決方案，並為下一步提供必要的上下文或推理結果。這樣的設計能夠幫助模型進行深度推理，尤其適用於涉及多個變量或需要多步計算的問題。


* **用途**：是讓模型在一個連貫的過程中進行推理，逐步解決問題，而不是一次性給出最終答案
* **例子**：
+ 假設我們要解決一個有多方面因素的問題，比如「如何減少城市中的空氣污染？」
+ Prompt
- 讓我們分步來思考如何減少城市中的空氣污染：
- 1. 首先，列出城市空氣污染的主要原因，並簡要說明每個原因。
- 2. 接著，探討一些有效的技術或政策措施，能夠減少這些污染源的排放。
- 3. 最後，討論這些措施的實施挑戰，並提出如何克服這些挑戰的建議。

因為這個Prompt 比較複雜一點，所以我們往下解釋一下它的工作原理。  

1. **拆解問題**：首先將問題拆分成多個子問題或子任務，每一個子任務代表問題的一部分。
2. **逐步推理**：每個子問題（鏈）會基於前一個鏈的結果來推進，確保邏輯是一貫的。
3. **合併結果**：最後，所有的推理結果會組合成最終答案。


這種方式不僅讓模型能夠進行逐步推理，還能在每一步中根據前一個步驟的結果調整方向，這樣可以避免模型在回答過程中出現邏輯錯誤或偏差，而且他具備以下優勢：


1. **提高解答的準確性**：通過分步驟推理，模型能夠逐步構建答案，避免一次性生成錯誤的結論。
2. **加強推理能力**：模型在每個鏈步中都能進行反思和推理，這有助於解決複雜問題。
3. **適應性強**：適用於各種需要多輪推理的情境，無論是數學計算、複雜概念的解釋，還是多方面的問題分析。
4. **提升結果的可解釋性**：逐步推理過程使得結果更容易理解和解釋，特別是對於複雜問題。


故事回到當初我們使用大型語言模型（LLM）[來撰寫程式碼的經歷](https://liuchien.ink.tw/2024/12/chatgpt.html)。透過精確的系統分析與設計，我們可以將需求的每個細節清楚地分解出來，就像玩樂高積木一樣，每一塊積木代表一個功能或元素，將它們有效地組合在一起，最終就能構建出完整的系統。在這個過程中，結合 **Multi-Chain Prompt** 技術，我們可以將程式設計的各個步驟分成不同的鏈條來處理，這不僅能幫助我們更加精細地規劃每個開發階段，還能大幅度節省開發時間和精力。每一個鏈條都像是一步步的指引，幫助我們逐步完成任務，而不會感到過於混亂或失控。

這種方法的應用並不限於程式開發，實際上，它在許多其他領域也能大派用場。例如，測試階段也能採用相同的方式，只要我們將測試的需求明確分解，並且善用 **Prompt** 技術，就能讓測試過程更加高效。透過將複雜的任務拆解成小塊，並利用適當的提示和引導，我們能夠以事半功倍的效果完成很多工作。只要問題不過於複雜，這種方法在各種領域都能大大提升生產力，讓我們能夠更輕鬆、更高效地達成目標。

 



| ![](/posts/2025-01-llmprompt/images/2025-01-llmprompt-8511168528863539374.jpg) |
| --- |
| 善用指令，你也是會個好騎師。 |


  

<span style="font-family: arial;">@font-face
 {font-family:Wingdings;
 panose-1:5 0 0 0 0 0 0 0 0 0;
 mso-font-charset:0;
 mso-generic-font-family:decorative;
 mso-font-pitch:variable;
 mso-font-signature:3 0 0 0 -2147483647 0;}@font-face
 {font-family:新細明體;
 panose-1:2 2 5 0 0 0 0 0 0 0;
 mso-font-alt:PMingLiU;
 mso-font-charset:136;
 mso-generic-font-family:roman;
 mso-font-pitch:variable;
 mso-font-signature:-1610611969 684719354 22 0 1048577 0;}@font-face
 {font-family:"Cambria Math";
 panose-1:2 4 5 3 5 4 6 3 2 4;
 mso-font-charset:0;
 mso-generic-font-family:roman;
 mso-font-pitch:variable;
 mso-font-signature:-536870145 1107305727 0 0 415 0;}@font-face
 {font-family:Rockwell;
 panose-1:2 6 6 3 2 2 5 2 4 3;
 mso-font-charset:0;
 mso-generic-font-family:roman;
 mso-font-pitch:variable;
 mso-font-signature:3 0 0 0 1 0;}@font-face
 {font-family:標楷體;
 panose-1:2 1 6 1 0 1 1 1 1 1;
 mso-font-charset:136;
 mso-generic-font-family:script;
 mso-font-pitch:fixed;
 mso-font-signature:-251646977 702545919 55 0 1048831 0;}@font-face
 {font-family:"\@新細明體";
 panose-1:2 1 6 1 0 1 1 1 1 1;
 mso-font-charset:136;
 mso-generic-font-family:roman;
 mso-font-pitch:variable;
 mso-font-signature:-1610611969 684719354 22 0 1048577 0;}@font-face
 {font-family:"\@標楷體";
 mso-font-charset:136;
 mso-generic-font-family:script;
 mso-font-pitch:fixed;
 mso-font-signature:-251646977 702545919 55 0 1048831 0;}p.MsoNormal, li.MsoNormal, div.MsoNormal
 {mso-style-unhide:no;
 mso-style-qformat:yes;
 mso-style-parent:"";
 margin:0cm;
 mso-pagination:none;
 font-size:12.0pt;
 font-family:"Rockwell",serif;
 mso-ascii-font-family:Rockwell;
 mso-ascii-theme-font:minor-latin;
 mso-fareast-font-family:標楷體;
 mso-fareast-theme-font:minor-fareast;
 mso-hansi-font-family:Rockwell;
 mso-hansi-theme-font:minor-latin;
 mso-bidi-font-family:"Times New Roman";
 mso-bidi-theme-font:minor-bidi;
 mso-font-kerning:1.0pt;}.MsoChpDefault
 {mso-style-type:export-only;
 mso-default-props:yes;
 font-family:"Rockwell",serif;
 mso-bidi-font-family:"Times New Roman";
 mso-bidi-theme-font:minor-bidi;
 mso-ligatures:none;}div.WordSection1
 {page:WordSection1;}ol
 {margin-bottom:0cm;}ul
 {margin-bottom:0cm;}</span>