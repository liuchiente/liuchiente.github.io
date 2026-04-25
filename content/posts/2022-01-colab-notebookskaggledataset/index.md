---
title: "如何在Colab Notebooks上引用Kaggle資料集(Dataset)"
date: 2022-01-11T14:19:00Z
lastmod: 2025-01-22T07:33:11.041Z
tags: ['Colab Notebooks', '影像處理與深度學習', 'Kaggle Datasets', '學習紀錄', 'Kaggle']
aliases:
  - /2022/01/colab-notebookskaggledataset.html
draft: false
---

Kaggle可以說是前幾名的數據建模和數據分析競賽平台，不管是企業或是個人都可以在上面發布競賽，也就所以在各競賽中往往可以找到相當易懂的程式或是其他競賽者苦心整理好的資料集。 而我[個人在Colab上開發程式時](https://liuchien.ink.tw/學習紀錄/深度學習與自然語言處理/淺談人工智慧和學習/)，也很常參考Kaggle上的程式，或是引用Kaggle的資料集(Dataset)，如果資料集不大的話，我通常會先下載到我的電腦，然後再上傳到Colab上，這樣一來一往就耗費掉很多時間，所以更多時候我會選擇從Colab直接載入Kaggle的資料集，除了省掉時間外，也簡約封包，就是一個減少暖化救救北極熊的概念~。在開始之前，你應該先準備好一個Kaggle的帳號，並且通過認證，並且有加入這個資料集/競賽，如此一來你才能正確下載檔案。

![image](images/2022-01-colab-notebookskaggledataset-4249705221762178399.png)

  
接著你可以在Account畫面中，找到API的欄位，點選Create New API Token，下載一個API Token，檔名是kaggle.json，裡面會包含認證資訊。

![image](images/2022-01-colab-notebookskaggledataset-2148576573590671357.png)

接著你需要在Colab上安裝Kaggle套件，以目前狀況，直接下pip安裝即可，不需要另外下載再安裝，著實很方便。

![image](images/2022-01-colab-notebookskaggledataset-4935545011604292047.png)

再來在Colab引入檔案函式庫，並呼叫files.upload()，在畫面上會出現一個上傳檔案的畫面，你可以上傳我們剛剛抓下來的Kaggle.json認證檔案，請注意，你上傳的認證檔案，會放在你當前的工作目錄。

![image](images/2022-01-colab-notebookskaggledataset-5370737561314215504.png)

接著我們在**根目錄**建立一個Kaggle資料夾，為什麼要強調是根目錄呢?因為Kaggle套件只抓此目錄底下的json檔來做認證，以Linux主機概念來看，這個目錄應該會在root目錄底下，也就是/.kaggle。

![image](images/2022-01-colab-notebookskaggledataset-1626164389903094688.png)

為了讓Kaggle套件可以正確抓到json認證檔案，我們需要把剛剛上傳到工作目錄的json檔複製到/.kaggle底下，別忘了調整一下檔案權限，讓檔案擁有者享有讀寫權限(600)。![](/posts/2022-01-colab-notebookskaggledataset/images/2022-01-colab-notebookskaggledataset-2090808322088109409.jpg)

完成上述動作，若是你的認證檔案放置正確，權限也正確，就可以正確呼叫Kaggle套件，下載你需要的資料集了，譬如像我一樣，下載水果資料集，詳情可以參考此網址: [https://www.kaggle.com/sriramr/fruits-fresh-and-rotten-for-classification](https://www.kaggle.com/sriramr/fruits-fresh-and-rotten-for-classification)。

![image](images/2022-01-colab-notebookskaggledataset-4677374582661081784.png)

下載回來之後直接解壓縮就可以開始用了，當然別忘了要先解壓縮。

![image](images/2022-01-colab-notebookskaggledataset-4948659170262884417.png)

另外還有些不同的指令可以用，譬如你可以條列一下你可以看到多少資料集，或是找找看特定資料集。

![image](images/2022-01-colab-notebookskaggledataset-8270713860871782777.png)

當然你也可以抓特定競賽的資料集，一切就看你的需求了。

![image](images/2022-01-colab-notebookskaggledataset-7996790754547006542.png)

以上整理，希望對你有幫助，我記得2020年時，我還需要把認證json先貼到Colab，然後存入./kaggle底下才能完成認證，辦法果然是人想出來的，哈。January 11, 2022 at 10:14PM

  
