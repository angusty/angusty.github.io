---
layout: post
title: 关于mysql的group by
category: 技术
tags: [PHP]
keywords: mysql,group by
description: mysql的group by统计数据条数
---

### 关于统计分组总条数

> mysql经常会使用group by用于数据分组,分组很爽，但是当分组完毕，你发现要统计数据总条数是一个棘手的事情。如果你头脑里一直想着在分组里面统计总条数，那你就会像进了一个坑。怎么办呢，尽快从坑里跳出来吧。恩，是的，先把 group by 去掉吧。

#### 解决方案

> 分组的目的一般是为了去除重复，所以为什么要用group by呢，所以把 group by 去掉，然后用 COUNT(DISTINCT id)来统计

```
SELECT
    COUNT(DISTINCT article.id)
FROM
  article
  LEFT JOIN article_tag  ON article.id = article_tag.article_id
  LEFT JOIN tag  ON tag.id = article_tag.tag_id

-- GROUP BY article.id   # 删除

```
