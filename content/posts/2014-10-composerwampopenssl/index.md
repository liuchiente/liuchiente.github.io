---
title: "Composer安裝WAMP的OpenSSL問題"
date: 2014-10-04T06:44:00Z
lastmod: 2020-03-13T15:10:10.034Z
tags: ['Composer', 'PEAR', 'Symfony', 'PHP', 'Pecl', 'Larvel']
aliases:
  - /2014/10/composerwampopenssl.html
draft: true
---

composer是一套用來下載管理PHP套件或函式庫的工具，其實就像PEAR和Pecl一樣，但是前述這兩個工具 算在近期的動態是少了點，而這幾年一些如Symfony或是Larvel的Framework也都開始支援用composer來做 下載。 嚴格來講，算是單純化了吧？以下是Composer的官方網站。 [Composer - Dependency Manager for PHP](http://getcomposer.org/) 不過今天嚴格來講，我只是想紀錄一下在安裝Composer會遇到的小問題，Composer有提供了Windows和Linux 系列的安裝支援，而在安裝時會檢查Open SSL的支援與否，在前幾版都還算會警告而已，但是呢...近期的版 本，在安裝時如果檢查不到Open SSL就會中斷無法繼續了，想當然，其實只要去php把openssl的extension打 開就好。 extension=php\_openssl.dll 就是把上述這行的分號拿掉，不過有一點值得注意的是，因為WAMP裡面，如果你去改apache底下的php.ini， 那是不夠的，主要是因為composer他檢查的是php cli的設置環境，所以如果你使用了WAMP，改了apache底下 的php.ini，那也只是改變了Web端的支援，因此你需要做的是，去把bin\php底下的php.ini做修改，打開openssl 支援，才能真正通過composer的檢查。 大概是這樣。