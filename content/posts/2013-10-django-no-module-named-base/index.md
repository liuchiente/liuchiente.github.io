---
title: "Django啟動時顯示錯誤 No module named base"
date: 2013-10-21T15:10:00Z
lastmod: 2020-03-13T15:10:10.686Z
tags: ['Pyhon', 'Django', 'SQLite']
aliases:
  - /2013/10/django-no-module-named-base.html
draft: true
---

當使用Django設定好setting.py時，執行 python manage.py syncdb 或是 python manage.py runserver 可能會跳出錯誤 No module named base 但是setting已經設定好DATABASES的ENGINE為sqlite3了，卻依舊出錯， 這其中的原因可能是因為python找不到完整的sqlite3　engine路徑， 因此把database enging設定為　django.db.backends.sqlite3，可能可 以解決這個問題。