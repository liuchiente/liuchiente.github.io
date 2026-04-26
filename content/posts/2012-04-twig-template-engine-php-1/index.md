---
title: "Twig template engine 新一代的PHP樣板引擎之安裝及使用"
date: 2012-04-01T13:57:00Z
lastmod: 2020-03-13T15:10:12.603Z
tags: ['Smarty', 'Language', 'Template Engine', 'Twig', 'PHP']
aliases:
  - /2012/04/twig-template-engine-php_1.html
draft: true
---

綜觀PHP的樣板引擎概念推出以來，也可以說是百家齊放，一時之間，網路上也充斥著一堆樣板引擎，只是時間一久，終歸還是回到PHP官方推出的Smarty身上，幾乎可以說，許多人入門時使用的，不外乎PHP+MySQL+Apache+Smarty，當然PHP是否應該使用樣板引擎，這也是一個頗受爭議的問題，畢竟總是會有人說，PHP本身就是一個樣板引擎，又何必疊床架屋？  
  
但是不能否認是，當我們要控制PHP在頁面上的大量呈現時，往往會因為標籤的關係，導致頁面一片混亂，因此是否採用樣板引擎，個人覺得是自身斟酌吧，畢竟也是會有不必用到樣板引擎的PHP程式，樣板引擎的使用是為了方便程式設計人員與美工人員的溝通與後續維護。  
  
Twig是一個來自於Symfony　Framework的樣板引擎，依據官方表示，Twig擁有以下的特色。  
  
* 快速：在編譯完頁面的呈現，由於針對頁面做了最佳化處理，因此顯示速度快。
  
* 安全：擁有Sandbox機制，會評估未知的template code，以避免不安全的結果產生。
  
* 彈性：方便使用者自行定義tag和filter，並建立自己特有的DSL。

  
當然以上的翻譯可能差勁了點，建議想要看原文的人可以來[官方網站](http://twig.sensiolabs.org/)看看。  
  
所以我們就直接跳往主題吧，首先先來實現一個簡單的樣板引擎使用，假定你是個Smarty用戶，那對於Twig的轉移，你一定會相當得心應手。  
  
Twig 可以採用相當多種安裝法，基於在Windows上使用，我們可以採用最簡單的，就是下載並解壓縮，在安裝之前，有些問題可能要注意一下，Twig要求PHP版本至少為5.2.4才可使用，假定閣下是Appserv之類的愛用者，不妨先注意一下版本，依照Appserv官方發布，現在可使用的版本應該為[Appserv 2.5.10](http://www.appservnetwork.com/modules.php?name=News&file=article&sid=46)。  
  
2. 將你想要的[版本](https://github.com/fabpot/Twig/tags)下載下來。
  
4. 解壓縮，放在你想放的地方，當然你要記得下載你能解開的格式啦，譬如Zip或是tar，一般在Windows我們會下載zip比較方便一點。
  
6. 解開之後，資料夾名可能會一長串，請把資料夾命名為你喜歡的名字，譬如為**twig**。
  
8. 接著如同Smarty的用法，請給他兩個資料夾，一是置放樣板的資料夾，譬如為**template**。
  
10. 另一是供樣板引擎編譯的資料夾，譬如為**templateComile**，別忘了為資料夾開放寫入權限，譬如Linux底下為777。

  
用於MC端開發，也就是你的PHP檔案上，首先需引入twig所在資料夾，並且進行註冊。  
  
`Require(‘/path/path/Autoloader.php’);  
  
Twig_Autoloader()::register();`  
  
接著新建物件，並用這物件傳入後，新建樣板物件。  
  
`$loader=new Twig_Loader_String();  
  
$twig=new Twig_Environment($loader);`  
  
假定我們要更彈性的指定個別樣板的路徑，在新建參數時，指定所要對應的樣板目錄。  
  
`$loader=new Twig_Loader_Filesystem(‘/path/template’);  
  
$twig=new Twig_Environment($loader,array(‘cache’=>’/path/templateComile’));`  
  
綜合以上作法，我們可以靈活的切換多個樣板目錄或是多個快取，然後用多個不同的樣板來源，實做我們想要的呈現。  
  
接著進行變數的給定，使用我們所建出來的物件，呼叫loadTemplate方法來指定我們要的樣板目錄，然後新建出另一個物件，接著使用render方法給定變數，  
  
`$template=$twig->loadTemplate(‘yuanli.html’);  
  
$template->render(array(‘temp1’=>$var1,’temp2’=>$var2));`  
  
如此一來資料會傳遞上樣板上，然後接下來就可以在樣板使用到它了  
  
`#yuanlu.html  
  
<div>{{ temp1 }}</div>  
  
<span>{{ temp2 }}</span>`  
  
當然也可以偷懶一點，直接用$twig物件，他也有render方法，這樣就可以省去loadTemplate的步驟，第一個參數是樣板名稱，第二個則是變數集合。  
  
`$twig->render(‘yuanli.html’,array(‘temp1’=>$var1,’temp2’=>$var2));`  
  
然後接下來就可以在樣板使用到它了。  
  
`#yuanlu.html  
  
<div>{{ temp1 }}</div>  
  
<span>{% temp2 %}</span>`  
  
以上兩種在使用上的差異有哪些呢？基於我使用的觀點，假定你load幾個不同的樣版進來成為物件，在操控上可以更為自由，而且後續維護更容易變識，比較不容易混淆，譬如以下。  
  
`$template988=$twig->loadTemplate(‘yuanli.html’);  
  
$template766=$twig->loadTemplate(‘miaoli'.html’);  
  
$template988->render(array(‘temp1’=>$var1,’temp2’=>$var2));  
  
$template766->render(array('temp9'=>$var5,'temp8'=>$var7));`  
  
當針對多個樣板進行操控，你就會體會到那種感覺，而不再是追著display或是fetch跑，我可以更快速的掌握哪個物件是針對哪個樣板，使用上也會更加靈活。  
  
值得注意的是，當我們呼叫render，實際上因為render會是回傳一個html字串，因此我們使用echo來協助呈現。  
  
`echo $template->render(array(‘temp1’=>’var1’,’temp2’=>’var2’));  
  
echo $twig->render(‘yuanli.html’,array(‘temp1’=>$var1,’temp2’=>$var2));`  
  
換句話說，透過render的回傳，我們可以得到版面輸出後的結果，這也意味著，我們可以因此來取代Smarty裡面常用的fetch。  
  
`$content=$template1->render(array(‘temp1’=>$var1,’temp2’=>$var2));  
  
$template2->render(array(‘maincontent’=>$content’));`  
  
當然我們今天只是要作頁面的呈現，就可以用display來達到我們的目的。  
  
`$template2->display(array(‘maincontent’=>$content’));`  
  
用於樣板排版，Twig使用以下標籤來定義  
  
{{  }}：用於單一資料呈現，譬如{{ variable }}。  
  
{% %}：用於流程控制跟資料操作，譬如{% if %}跟{% set variable %}。  
  
{# #}：用於註解，譬如{# 這是註解 #}。  
  
相較於Smarty則是使用{}，為了避免跟Javascript混淆，則會有人改成<{}>，或自定義其他標籤。  
  
下一步來聊聊實際套版上的樣板語言吧。