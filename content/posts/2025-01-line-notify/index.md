---
title: "Line Notify的替代方案？給你一個完整的分析。"
date: 2025-01-21T09:29:00Z
lastmod: 2025-02-08T02:07:24.260Z
tags: ['Zalo', 'Line', 'Telegram', 'Line Notify', 'Wechat', 'Google Chat', 'Line Message', 'Discord']
aliases:
  - /2025/01/line-notify.html
draft: false
---

[Line Notify](https://notify-bot.line.me/zh\_TW/)自推出以來，雖然其存在一些侷限，譬如無法自定義訊息格式，也無法對單一用戶傳遞訊息，但仍依靠著簡單易用和高穩定性，搭配著IFTTT服務，成為許多系統通知的愛用方案之一，走過幾年來的風風雨雨，正當大家都習慣Line Notify帶來的便利之際，Line官方突然宣布Line Notify即將停止服務，詳見[LINE Notify結束服務公告](https://notify-bot.line.me/closing-announce)。  
  
那當LINE Notify結束服務後，原本這些用戶該何去何從呢？官方也推薦了使用Line Message API作為替代方案，詳見從 LINE Notify 轉移到 LINE Messaging API 發送更多樣訊息與輕鬆查找用戶 ID、群組 ID，文中也提到了一些基本串接；的確，LINE Messaging API依靠著每月有200則免費訊息的優惠，可以滿足許多極輕量用戶的需求，但這個方案，卻還是存在很多先天限制的，因為各個國家也有不同的市場使用狀況，即便LINE Notify用戶遍及東亞，卻還是需依不同狀況做出不同方案選擇。



![image](images/2025-01-line-notify-7451626228677959209.jpg)

  
  
以下我們針對在東亞普及的訊息服務，挑選了幾個常見的產品做介紹；  
  
[Telegram](https://web.telegram.org/)是一款免費、快速、安全的即時通訊應用程式，支持文字、語音和視訊通話，並提供多種豐富的功能。它的特點包括端對端加密，確保用戶的隱私和資料安全；群組和頻道功能，讓用戶可以輕鬆與朋友、同事或追隨者分享資訊。Telegram也支持大型群組（最多可容納20萬人），還能進行文件傳輸、雲端儲存及自訂機器人等。無論是日常聊天、工作協作還是公共資訊分享，Telegram都是一個強大且靈活的工具。無論在手機、平板或桌面端，都可以輕鬆同步，提供跨平台使用的無縫體驗。由於其簡潔的介面和穩定的運行，Telegram迅速成為全球用戶喜愛的通訊工具之一。  
  
[WeChat](https://www.wechat.com/zh\_HK/)（微信）是一款由中國科技公司騰訊推出的綜合性即時通訊應用程式，擁有超過十億用戶。它不僅支援基本的文字、語音和視頻通話功能，還集成了豐富的社交、支付和生活服務。用戶可以透過“朋友圈”分享照片和動態，與朋友保持聯繫；同時，微信支付功能讓用戶可輕鬆進行線上支付、轉帳及購物。微信的“公眾號”讓企業和媒體能夠直接與用戶互動，提供資訊和服務。此外，微信還支持小程序，讓用戶在不離開應用的情況下使用各種工具和服務，無需下載額外的應用。微信的跨平台使用方式（支持iOS、Android和桌面端）也使得它成為日常生活中不可或缺的數字工具。  
  
[LINE](https://www.line.me/tw/)是一款由日本公司NHN Japan（現為LINE Corporation）開發的即時通訊應用程式，深受亞洲各地用戶的喜愛。它不僅提供基本的文字、語音和視頻通話功能，還具有許多有趣的社交元素。用戶可以透過「貼圖」和「表情符號」來表達情感，增加交流的趣味性。LINE的「時間軸」功能讓朋友們可以分享照片、文章或日常狀況，並相互互動。除了即時通訊，LINE還提供LINE Pay支付服務，讓用戶進行轉帳、購物和線下付款。LINE的「官方帳號」讓企業能夠直接與消費者溝通，提供服務和推廣活動。LINE還有多樣的小遊戲和內容，為用戶提供豐富的娛樂選項。支持跨平台使用，使其成為現代生活中的一站式數位平台。  
  
[Zalo](https://zalo.me/pc)是一款越南本地開發的即時通訊應用程式，廣受越南用戶的喜愛。它提供文字、語音和視頻通話功能，並且強調簡便易用。用戶可以利用Zalo與朋友、家人保持聯繫，或在群組中進行協作交流。除了基本的訊息傳遞，Zalo還支持豐富的多媒體內容分享，包括圖片、影片和語音訊息，並具有貼圖和表情符號功能，增添溝通趣味。Zalo的「ZaloPay」支付功能使得用戶能夠輕鬆進行在線支付和轉帳。此外，Zalo還提供公共帳號功能，幫助商家和品牌與顧客進行互動、推廣商品或服務。這款應用的簡單界面和高效能使其成為越南國內流行的社交平台，並逐漸拓展至國際市場。  
  
[Discord](https://discord.com/)是一款專為社群、遊戲玩家和創作者設計的即時通訊應用程式，提供語音、視頻和文字聊天功能。最初以遊戲玩家為主要用戶群體，Discord迅速擴展到各種興趣小組和專業社群。用戶可以創建或加入不同的伺服器，進行專題討論、協作或娛樂互動。每個伺服器內有多個頻道，支持文字、語音及視頻交流，並且具有靈活的權限設定，讓管理者能夠有效組織社群。Discord還支援各種機器人、遊戲串流和多媒體內容分享，讓用戶體驗更加多元。此外，它與其他平台的整合性也很好，方便用戶跨平台使用。無論是娛樂、學習還是工作，Discord都能提供一個豐富且高效的社交與協作空間。  
  
[Google Chat](https://chat.google.com)是Google推出的即時通訊和協作工具，主要面向企業和團隊使用。它整合了Google Workspace（前身為G Suite）的各種服務，讓用戶可以輕鬆協作和共享文件。Google Chat提供單對單的即時訊息傳遞，以及支援多達數百人的群組聊天，並支持直接在聊天中共享Google文件、Google日曆事件等。用戶還可以利用其強大的搜尋功能快速找到歷史訊息，並且支援任務分配和集成第三方應用。Google Chat還與Google Meet無縫整合，方便用戶一鍵發起視頻會議。其簡潔的界面和高效的協作工具，讓團隊能夠更輕鬆地進行項目管理和工作協作，是一款適合企業日常溝通的強大工具。  
  
再來我們設定幾個情境，因為LINE Notify主要用在群組推送通知，所以這次受到影響的客群，我會鎖定在群組用戶，再來考量到我們這個群組會用到LINE Notify，大多是用來協作，或是需要告知一個大家都需要知道的訊息，因此我們假定對象是一個成員數1人以上的群組，故設定以下情境為：  
  
如果一個成員數50人以上的群組，我們要用哪個方案好？

  


再來我們依照前述條件，來試算一下如果繼續使用Lne Message這個方案，會需要多少成本：






| Line Message 統計 | | | | | | |
| --- | --- | --- | --- | --- | --- | --- |
| 群組 | GROUP 1 | GROUP 2 | 各群組總計 | | | |
| 統計 | 總計 | 月平均 | 總計 | 月平均 | 總計 | 月平均總計 |
| 訊息數 | 106 | 36 | 124 | 42 | 308 | 103 |
| 成員數 | 42 | 32 | 38 | 38 | 150 | 258 |
| Line Message
 訊息量(預估) | 4346 | 1116 | 4588 | 1554 | 8934 | 2670 |



table
 {mso-displayed-decimal-separator:"\.";
 mso-displayed-thousand-separator:"\,";}.font5
 {color:windowtext;
 font-size:9.0pt;
 font-weight:400;
 font-style:normal;
 text-decoration:none;
 font-family:新細明體;
 mso-generic-font-family:auto;
 mso-font-charset:136;}tr
 {mso-height-source:auto;
 mso-ruby-visibility:none;}col
 {mso-width-source:auto;
 mso-ruby-visibility:none;}br
 {mso-data-placement:same-cell;}td
 {padding-top:1px;
 padding-right:1px;
 padding-left:1px;
 mso-ignore:padding;
 color:black;
 font-size:12.0pt;
 font-weight:400;
 font-style:normal;
 text-decoration:none;
 font-family:新細明體;
 mso-generic-font-family:auto;
 mso-font-charset:136;
 mso-number-format:General;
 text-align:general;
 vertical-align:middle;
 border:none;
 mso-background-source:auto;
 mso-pattern:auto;
 mso-protection:locked visible;
 white-space:nowrap;
 mso-rotate:0;}.xl65
 {text-align:center;
 border:.5pt solid windowtext;}.xl66
 {border:.5pt solid windowtext;}.xl67
 {text-align:center;
 border-top:.5pt solid windowtext;
 border-right:none;
 border-bottom:.5pt solid windowtext;
 border-left:.5pt solid windowtext;}.xl68
 {text-align:center;
 border-top:.5pt solid windowtext;
 border-right:none;
 border-bottom:.5pt solid windowtext;
 border-left:none;}.xl69
 {text-align:center;
 border-top:.5pt solid windowtext;
 border-right:.5pt solid windowtext;
 border-bottom:.5pt solid windowtext;
 border-left:none;}ruby
 {ruby-align:left;}rt
 {color:windowtext;
 font-size:9.0pt;
 font-weight:400;
 font-style:normal;
 text-decoration:none;
 font-family:新細明體;
 mso-generic-font-family:auto;
 mso-font-charset:136;
 mso-char-type:none;
 display:none;

 

參考原先50人的群組，我們把前述方案做了一些整理，考量[Google Chat](https://chat.google.com)和[WeChat](https://www.wechat.com/zh\_HK/)的方案需要比較全面的訂閱，像是[企業微信號](https://work.weixin.qq.com/)，所以我們只列出收費機制比較簡單的方案：






| 方案 | Telegram | Zalo | Discord | Line Message API |
| --- | --- | --- | --- | --- |
| 預估方案 | 無 | Plan Premium GMF-50 | 無 | 高用量方案 (6000 則 ) 額外加購訊息 |
| 預計費用 | 無 | $399,000 VND/month ($519 NTD) $55,000 VND/month ($71 NTD) $330 VND/msg ($0.43 NTD) | 無 | $1200 NTD/month $0.2 NTD/month |
| 介面和裝置 | 英文、俄文 | 英文、越文 | 中文、英文、越文 | 中文、英文 |
| 備註 | 現有移轉案例：慈濟 收費方案：個人加值服務 | 支援 3 個群組 每群組最多 50 個成員 需訂閱 Official Accouint ( 免費 ) 只接受越南 API 呼叫服務 | 收費方案：個人加值服務 | 需訂閱 Official Accouint ( 免費 ) 可用訊息封包不足時須加購 訊息計算量須乘上成員數 |


Zalo比較側重在社群經營和用戶互動，這點和[Line Official Account](https://tw.linebiz.com/login/)很像，詳情可見[Zalo OA](https://zalo.cloud/oa/pricing)，因此他們的定價策略也是極為相似的，首先你需要一個Officoal Account，即便他可能是免費的，但是因為在社群經營互動方面，經營者面向的是海量用戶，所以身為一個服務提供者，費用的收入就會側重在訊息傳播渠道（群組）、訊息傳播數量（訊息數），但是對商家來說，在無論是Line Official 或是Zalo OA，可以拿到比較多的用戶交互資訊，以及相關統計，這樣的工具用在通路行銷是非常強大的。

相對的，[Telegram](https://web.telegram.org/)則是維持在做訊息交換和傳達，即便群組號稱可以容納25,0000，但實際在用戶分析上，倒沒有太多著墨，而[Telegram](https://web.telegram.org/)用在訊息傳遞上，是相當可靠且穩定的服務，另外[Telegram](https://web.telegram.org/)短期內推出的加值服務，也是著重在個人加值，例如訊息貼圖等，可見短期內應沒有改變策略的跡象，唯一讓人比較難推動的，則是官方還不支援中文。

 [Discord](https://discord.com/)的角色和[Telegram](https://web.telegram.org/)其實頗為相似，只是除了訊息傳遞外，[Discord](https://discord.com/)更在意群組溝通，很棒的一點是[Discord](https://discord.com/)也支援機器人，也支援多國語系如中文，收費方案也是著重在個人加值，例如訊息貼圖等，短期內也沒有改變策略的跡象。

Zalo、Telegram 和 Discord 是三個不同的社交平台，且在定價策略和功能上有各自的特色。Zalo 比較注重社群經營和用戶互動，這與 Line Official Account 類似。Zalo 的定價策略強調訊息傳播的渠道和數量，並提供商家豐富的用戶交互資訊與統計數據，適用於通路行銷。而 Telegram 則以訊息傳遞為核心，雖然支持大量用戶的群組，但對用戶分析的功能並不強調，且近期的加值服務主要針對個人使用者，並未顯示有太大策略變動。Discord 與 Telegram 類似，但在群組溝通方面更加重視，並支持機器人和多國語言（如中文），收費方案也偏向個人加值，像是訊息貼圖等，策略也短期內不會改變。總體而言，這三個平台在功能和定價策略上各有側重，適合不同的用戶需求。 

@font-face
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
 {margin-bottom:0cm;}

