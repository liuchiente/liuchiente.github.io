---
title: "後AI時代的多代理 AI Agent 實作：使用 CrewAI 與 LiteLLM 打造自動化專案程式架構分析"
date: 2025-11-01T13:40:00Z
lastmod: 2025-11-01T13:40:32.193Z
tags: ['AI Agent', '後AI時代', 'Generative AI', 'MCP', 'CrewAI', 'LiteLLM', 'Multi-Agent', 'LLM', 'AI Framework', 'AI Architecture', 'Python AI', 'AI 自動化', 'AI 協作', 'AI']
aliases:
  - /2025/11/ai-ai-agent-crewai-litellm.html
draft: false
---

## 在「後AI時代」，AI Agent 正逐步改變我們對人工智慧的想像。本文從 Generative AI 年會談起，介紹 MCP（Model Context Protocol）、CrewAI 與 LiteLLM 的整合實作，說明如何打造能協作、自主決策的多代理（Multi-Agent）AI 系統。從單一 LLM 到智能協作架構，這正是 AI 演化的新篇章。

## 前言

前陣子在觀賞[2025 Generative AI 年會](https://gaiconf.com)，聽到有位分享者提到「後AI時代」，為什麼說後AI時代呢，因為AI Agent的概念出現，加上MCP逐漸成熟，前兩年的超大LLM霸主時期不免出現一些小變化，在過去，我們人類先是提出了Auto Regressive，接著整理出大量的訓練語料，接著我們用Text-Generation解決了很多問答、總結的NLP難題，下一步則開始考慮多模態問題，接著需要更多更好的機制來解決Token轉換問題，還要有更多機器來支援算力消耗，屬實不容易，大部分LLM走在一條用單一模型解決所有問題的道路上，但這實在不容易，畢竟很多任務是這樣的，你讓他執行單一任務，他可以做到很精確，像是圖像辨識，CNN幾乎就有了碾壓性的效果，但是你要讓他兼顧幾個不同任務，這是個大課題。  
於是就有人想了，那如果我把幾個Model結合起來，會是什麼感覺呢？我先破個梗，如果處理得不好，大概就是以下這種狀況。  
[![](/posts/2025-11-ai-ai-agent-crewai-litellm/images/2025-11-ai-ai-agent-crewai-litellm-708145892580018748.jpg)](https://truth.bahamut.com.tw/s01/202308/909b0d9a99f76e2a3a25e61ad71672f9.JPG)  
這沒亂蓋吧？這張圖要多傳神就有多傳神，更何況他還說出了LLM的特色。  
[![](/posts/2025-11-ai-ai-agent-crewai-litellm/images/2025-11-ai-ai-agent-crewai-litellm-8586702796028722730.jpg)](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ff16ce6bb-a28e-47f7-ad1b-8e6fa9c03a68.png&width=740&sign=yfYTu5Fs623l-Gw2PUiUovOXdSol7cjg23Kks13eGV8)  
好啦，回歸主題，接著介紹說幾個在本次實作會應用到概念。  
**[AI Agent](https://zh.wikipedia.org/zh-tw/智能代理)**是能自主理解任務、規劃步驟、執行行動並根據結果調整策略的概念，結合了大語言模型（LLM）、記憶、工具與回饋機制，能完成多步驟任務。例如自動撰寫報告、分析資料或審查程式碼，不再只是被動回答，而能主動行動。  
**[MCP](https://zh.wikipedia.org/zh-tw/模型上下文协议)**是一種標準化協定，用來讓 AI Agent 與工具、資料、記憶庫之間交換上下文與任務資訊。它幫助 Agent 保留任務狀態、共享工具清單、追蹤進度，實現多代理協作，MCP 讓 AI 系統有「記憶」與「任務持續性」。  
**[CrewAI](https://www.crewai.com)** 是一個多代理（Multi-Agent）開發框架，讓開發者能定義多個 AI Agent 角色、任務與互動流程。它支援任務拆解、工具整合與自動協作，可讓不同專長的 Agent 協同完成目標。CrewAI 常用於自動化工作流與智能應用開發。  
**[LiteLLM](https://www.litellm.ai)** 是一個開源介面，讓開發者能以統一方式呼叫多個語言模型（如 OpenAI、Google等）。它提供 OpenAI 風格 API、成本管理與模型切換功能，使系統能輕鬆整合不同 LLM，並與框架如 CrewAI 搭配使用。  
綜合以上，簡單解釋我們怎麼實現AI Agent：其實MCP本質上很像[RPC](https://zh.wikipedia.org/zh-tw/遠程過程調用)，透過通訊協議我們可以在程式中任意呼叫不同的LLM，再搭配Crew AI，幫我們實現了簡單定義Agent，Agent應負責的任務，提供流程上的支援，最後完成一套AI Agen的工作流程。## 

## 程式內容

### Crew AI with AI Angent By LiuChien (Gemini)

Crew AI是一套Python函式庫，讓你可以輕鬆實現AI Agent，如果想搭配更多樣化的LLM，也可以搭配LiteLM。

這個筆記本的程式雖然不多，但卻簡單實現了將一個 Ionic Project從Git下載之後進行解析，並且依照專案架構和相依套件進行解析，判斷目前專案出有什麼重大問題。



---

接下來我們採用了幾個步驟，為一個專案做健康檢查：

下載專案 分析專案架構 設定Crew AI、LiteLM (這裡設定用Gemini模型) 設定Agent (包含結構專家、依賴判斷專家、重構建議) 設定任務 (分別進行結構、依賴判斷，並給出建議) 開始解析並產出結果



---

參考資料

* [https://www.crewai.com](https://www.google.com/url?q=https%3A%2F%2Fwww.crewai.com)
* [https://docs.litellm.ai](https://www.google.com/url?q=https%3A%2F%2Fdocs.litellm.ai)
* [https://docs.litellm.ai/docs/providers](https://www.google.com/url?q=https%3A%2F%2Fdocs.litellm.ai%2Fdocs%2Fproviders)
* [https://aistudio.google.com/u/4/api-keys?hl=zh-tw](https://www.google.com/url?q=https%3A%2F%2Faistudio.google.com%2Fu%2F4%2Fapi-keys%3Fhl%3Dzh-tw)
  
### ㄧ、安裝Crew AI與其他工具

1. Langchain
2. CrewAI
3. OpenAI
4. LiteLM

### 二、設定金鑰

1. HuggingFace金鑰 (給LiteLM使用)
2. DeepSeek金鑰

因為我們會用到LiteLM串接HuggingFace上的LLM模型，如果使用的不是HuggingFace上的模型就不需要設定。

### 三、示範用的Github專案

1. 一個年久失修，Ionic V1版本為基礎的Hybrid APP專案

### 四、掃描專案架構

1. 專案架構和目錄組成
2. 相依套件組成

### 五、定義Agent並指定LLM模型

依照以下目標，我們對每個Agent設定了角色、工作目標、背景知識，必要的話也可以切換不同LLM模型一起協作。

1. 結構分析
2. 依賴分析
3. 重構建議

特別注意我們這裡用的是LiteLM，串接HuggingFace上的Text-Generation模型，所以這裡傳入的是指定型位置的字串。 想知道有哪些LLM可以用，可以參閱[https://docs.litellm.ai/docs/providers。](https://www.google.com/url?q=https%3A%2F%2Fdocs.litellm.ai%2Fdocs%2Fproviders%25E3%2580%2582)

### 六、定義各Agent需要執行的任務

前兩個任務分別是分析專案結構、套件相依問題，並且可以指定任務，第三個任務是重構建議，則可以直接請Agent用前兩個任務總結下去做建議，Crew AI會自動幫你處理上下文，想像一下你就是在一個聊天視窗，分別作以下三個任務的行為。

### 七、列印出分析結果和建議內容

以下是分析結果和建議內容。

  


根據您提供的依賴分析結果，專案目前處於一個極度不穩定、過時且充滿潛在風險的狀態。由於缺少實際的程式碼目錄結構文字輸出，我將專注於從**技術棧層面**提出重構建議與**框架層次**的模組優化方向。一旦底層技術棧穩定並現代化，上層的業務邏輯模組拆分與優化將能更順利地進行。

以下是具體的重構建議與模組優化方向：

### 前言：當前專案核心問題概括

目前專案最嚴重的問題是多重、衝突且嚴重過時的框架及套件依賴。這不僅導致專案無法正常運作，也帶來了巨大的安全漏洞、兼容性問題和維護負擔。任何上層的業務模組優化，都必須先建立在一個穩固且現代化的技術基礎之上。

### 一、核心技術棧重構與現代化 (基石重建)

這是專案最優先且必須執行的重構任務。目標是將整個應用程式升級到一個統一、最新且受到官方支援的技術棧。

1. **Ionic Framework 統一與升級：**


	* **問題根源：** 同時存在 Ionic 2 (`ionic-native`), Ionic 4 (`@ionic/angular`), Ionic 7 (`@ionic/core`) 的衝突依賴，導致無法正常編譯與運行。
	* **重構建議：**
		+ **策略：** 進行一次徹底的 Ionic Framework 大版本升級。強烈建議直接升級到**最新穩定版 Ionic (目前為 Ionic 7 或 Ionic 8 beta)**。這將涉及重大變更，但長遠來看是避免未來技術債務的最佳途徑。
		+ **具體步驟：**
			- 完全移除所有舊的 Ionic 相關套件 (`@ionic/angular: ^4.11.13`, `@ionic/core: ^7.8.6`, `ionic-native: ^2.9.0`)。
			- 安裝最新的 `@ionic/angular` 和 `@ionic/cli` 版本。
			- 根據 Ionic 官方升級指南 (例如從 v4 升級到 v5, v5 到 v6, v6 到 v7) 逐步調整程式碼。這可能涉及 Angular 版本、Router 配置、生命週期鉤子、元件用法等變更。
			- **模組優化方向：** 升級至現代 Ionic/Angular 版本後，將能充分利用 Angular 的模組化能力（如 `NgModule`、Standalone Components/APIs），更好地組織應用程式。
2. **建置系統現代化 (Gulp 移除或升級)：**


	* **問題根源：** Gulp 3 及其配套插件嚴重過時，可能與新版 Node.js 不兼容，且缺乏維護，存在安全風險。
	* **重構建議：**
		+ **策略一 (推薦)：** 徹底移除 Gulp，轉而使用 **Angular CLI 內置的建置系統**。對於 Ionic/Angular 專案，Angular CLI 提供了完整的開發、建置、測試工具鏈，性能更優且更易於維護。
		+ **策略二 (次選)：** 如果專案有極其複雜且難以遷移的 Gulp 任務，則將 Gulp 升級到 **Gulp 4**，並將所有相關 Gulp 插件 (`gulp-concat`, `gulp-minify-css`, `gulp-rename` 等) 升級到最新穩定版，或替換為現代替代方案 (例如 `gulp-clean-css` 替代 `gulp-minify-css`)。
		+ **模組優化方向：** 現代化的建置系統能更好地支持 Tree Shaking、Lazy Loading 等優化技術，減少打包大小，提高應用程式性能。
3. **Cordova 插件與 `ionic-native` 統一升級：**


	* **問題根源：** 大量 Cordova 插件版本過時，且 `ionic-native: ^2.9.0` 已廢棄，與現有或未來的 Ionic 版本不兼容，帶來安全隱患和運行時不穩定。
	* **重構建議：**
		+ **策略：** 將所有 Cordova 插件和 `ionic-native` 依賴升級到最新且兼容 AndroidX 的版本。
		+ **具體步驟：**
			- 移除 `ionic-native: ^2.9.0`。
			- 對於每個舊的 Cordova 插件 (`cordova-plugin-device: ^1.1.7`, `cordova-plugin-splashscreen: ^4.1.0`, `cordova-plugin-statusbar: 2.4.2` 等)，查找其最新穩定版，並替換為官方維護的 `@ionic-native/*` 範疇套件 (例如 `@ionic-native/device`) 和相應的 Cordova 插件版本。
			- 仔細檢查每個插件的 Breaking Changes，並調整程式碼以適應新版本 API。
			- **模組優化方向：** 統一使用 `@ionic-native/*` 套件，可以將裝置相關功能封裝在獨立的服務模組中，例如 `DeviceService`，提高可重用性和測試性。
4. **AndroidX 兼容性解決方案：**


	* **問題根源：** 雖然有 `cordova-plugin-androidx` 和 `cordova-plugin-androidx-adapter`，但由於其他插件過於老舊，可能仍無法完全兼容。
	* **重構建議：** 在完成 Ionic Framework 和所有 Cordova 插件升級後，重新評估並調整 AndroidX 的兼容性。最新的 Cordova 和插件通常會內置 AndroidX 支援，使得這些輔助插件不再必要或需要更新到最新版本。

### 二、模組結構優化與最佳實踐 (基於 Angular/Ionic 現代化架構)

在完成技術棧的底層重構後，可以開始實施現代化 Angular/Ionic 專案的模組化策略。

1. **清晰的職責分離 (Separation of Concerns)：**


	* **優化方向：** 將應用程式的各個層次明確劃分職責，例如：
		+ **`core` 模組：** 放置應用程式級別的單例服務 (如認證、日誌、核心配置)、Guard、Interceptor 等。這些服務通常在應用程式啟動時載入一次。
		+ **`shared` 模組：** 放置在多個功能模組中被重用的元件 (Components)、指令 (Directives)、管道 (Pipes) 和輔助服務 (Utility Services)。
		+ **`features` 模組：** 根據業務功能劃分，每個功能模組包含其特有的頁面、元件、服務和路由。
2. **Angular 特性模組化 (Feature Modules)：**


	* **優化方向：** 根據主要業務功能，將應用程式拆分為多個獨立的 Angular Feature Modules。
	* **實踐：**
		+ 例如：`AuthModule` (處理登入、註冊、忘記密碼), `DashboardModule`, `UserProfileModule`, `SettingsModule` 等。
		+ 每個 Feature Module 應具備自己的路由配置，並可實現延遲載入 (Lazy Loading)，以優化應用程式的初始載入時間。
		+ 此策略極大地降低了模組間的耦合度，提升了開發效率和可維護性。
3. **共用元件與服務模組 (`SharedModule`)：**


	* **優化方向：** 建立一個 `SharedModule` 來匯出 (export) 那些在整個應用程式中都會被使用到的元件、指令、管道和常用 Angular Modules (如 `CommonModule`, `FormsModule`, `IonicModule`)。
	* **實踐：** 避免在 `SharedModule` 中提供 (provide) 服務，以防止單例服務被多個延遲載入模組重複實例化。服務應在 `CoreModule` 或特定的 Feature Module 中提供。
4. **資料存取抽象模組 (`DataModule` 或 `APIModule`)：**


	* **優化方向：** 將所有與後端 API 互動、資料處理、資料庫存取 (如果應用程式有離線資料庫) 的邏輯集中在一個或幾個特定的模組中。
	* **實踐：**
		+ 定義統一的 API 服務，例如 `UserService`, `ProductService`，這些服務只負責與後端通信和資料轉換，而不包含業務邏輯。
		+ 可以使用 `HttpClientModule`、`ngrx/store` (如果需要狀態管理) 或其他資料管理庫。
		+ 這有助於將後端細節與前端業務邏輯解耦，提高資料層的可測試性和可替換性。
5. **設定檔模組化 (`ConfigModule`)：**


	* **優化方向：** 集中管理應用程式的所有配置設定 (如 API 端點、環境變數、常數)。
	* **實踐：** 建立一個 `ConfigService` 或使用 Angular 的 `environment.ts` 檔案，確保配置參數在不同環境 (開發、測試、生產) 下能被正確管理和載入。

### 三、技術債務清理與風險管理

1. **廢棄套件移除：** 清理 `package.json`，移除所有已明確廢棄或不再使用的套件依賴，減輕專案負擔。
2. **安全性漏洞修復：** 透過升級所有套件到最新版本，修復已知的老舊套件帶來的安全性漏洞。定期使用 `npm audit` 或類似工具檢查依賴安全問題。
3. **性能與維護性提升：**
	* 採用最新的 Angular/Ionic 功能 (如 Standalone Components, Signals 等)，簡化程式碼，提高執行效能。
	* 精簡打包體積，利用 Tree Shaking 和 Lazy Loading 優化應用程式啟動速度。
	* 確保所有程式碼都符合 Linting 規範 (如 TSLint 或 ESLint)，保持程式碼風格一致性。

### 四、實施策略建議

由於涉及大規模的技術棧升級，建議採用以下策略：

1. **階段性重構：** 不要試圖一次性升級所有東西。可以先專注於 Ionic Framework 和 Angular 的核心升級，確保應用程式能重新運行，然後再逐步處理 Cordova 插件和 Gulp 移除。
2. **建立全面的測試：** 在開始大規模重構之前，盡可能多的建立現有功能的自動化測試（單元測試、整合測試），以確保在重構過程中不會引入新的錯誤，並能驗證升級後的應用程式行為符合預期。
3. **版本控制與分支策略：** 使用強大的版本控制系統 (如 Git)，並為重構工作建立一個專門的長期分支。頻繁提交並在小階段完成後合併回主分支。
4. **團隊培訓：** 如果團隊成員不熟悉新的 Ionic/Angular 版本和最佳實踐，需要提供相應的培訓。

這個專案的重構是一個巨大的挑戰，但也是一次徹底清理技術債務、提升專案品質和未來可維護性的絕佳機會。

## 結論

我這次一共實作了 **Gemini** 和 **DeepSeek** 兩個版本。整體來說，實作成本並不高，只需要準備好呼叫模型所需的 **API 金鑰**。

使用不同的 LLM 作為 Agent 是一件很有趣的事。雖然這次的範例中我都採用了相同的 LLM，但實際應用時，**依照不同 LLM 的特性進行分工與配置**，往往能獲得更佳的效益。文末我也附上了原始碼，大家可以自己試著玩玩看 —— 不過記得要先申請金鑰喔。

以 **MCP 的規劃** 來看，它的目標並不只是讓 LLM 扮演 RPC 的角色而已。對 AI Agent 而言，最具突破性的能力是「**互為 Agent、甚至能相互分享**」。這樣的設計讓 AI Agent 的概念再次向前推進一大步。

在不久的將來，AI 的成長速度可能會比我們想像中更快、更強，而我們正親眼見證這段歷史的展開。

### 原始碼

[Crew\_AI\_with\_AI\_Angent\_By\_LiuChien\_(Gemini)\_Public.ipynb (Github.com)](https://github.com/liuchiente/code\_from\_colab\_and\_kaggle/blob/8a43714aeb4d9eb9eabdf5a7f951810e868e2e8f/Crew\_AI\_with\_AI\_Angent\_By\_LiuChien\_(Gemini)\_Public.ipynb)[Crew\_AI\_with\_AI\_Agent\_By\_LiuChien\_(DeepSeek)\_Public.ipynb (Github.com)](https://github.com/liuchiente/code\_from\_colab\_and\_kaggle/blob/1e65d4b85b2663769d573fd293a10440b0038876/Crew\_AI\_with\_AI\_Agent\_By\_LiuChien\_(DeepSeek)\_Public.ipynb)