<%*
const title = await tp.system.prompt("文章標題");
const slug = title
  .toLowerCase()
  .replace(/[^\w\s-]/g, "")
  .replace(/\s+/g, "-");

const tags = await tp.system.prompt("標籤（用逗號分隔）", "筆記");
const category = await tp.system.prompt("分類", "技術");
const desc = await tp.system.prompt("SEO 描述");

const now = tp.date.now("YYYY-MM-DDTHH:mm:ssZ");
%>
---
title: "<%= title %>"
date: <%= now %>
lastmod: <%= now %>
draft: false
slug: "<%= slug %>"
description: "<%= desc %>"
tags: [<%= tags.split(",").map(t => `"${t.trim()}"`).join(", ") %>]
categories: ["<%= category %>"]
keywords: [<%= tags.split(",").map(t => `"${t.trim()}"`).join(", ") %>]
author: "你的名字"
---

# <%= title %>

寫你的內容...