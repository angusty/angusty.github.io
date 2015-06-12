---
layout: post
title: 关于mysql的GROUP BY
category: 技术
tags: [PHP]
keywords: mysql,GROUP BY
description: mysql的GROUP BY统计数据条数
---

> 这里是关于mysql的GROUP BY的一些总结，会写一些查询的demo，下面是demo所用到的表结构

### 表结构

    article表
    | id | title | content |
    |----|-------|---------|

    tag表
    | id | tag_name |
    |----|----------|

    article_tag表
    | article_id | tag_id |
    |------------|--------|


### 统计分组总条数

> mysql经常会使用GROUP BY用于数据分组,分组很爽，但是当分组完毕，你发现要统计数据总条数是一个棘手的事情。如果你头脑里一直想着在分组里面统计总条数，那你就会像进了一个坑。怎么办呢，尽快从坑里跳出来吧。恩，是的，那就把GROUP BY去掉吧。一旦你这么做了，恭喜你，你出坑了。

#### 解决方案

> 分组的目的一般是为了去除重复，所以为什么要用GROUP BY呢，所以把GROUP BY去掉，然后用 COUNT(DISTINCT id)来统计，DISTINCT是mysql的关键字，用于去除重复的。

```
SELECT
    COUNT(DISTINCT article.id)
FROM
    article
    LEFT JOIN article_tag  ON article.id = article_tag.article_id
    LEFT JOIN tag  ON tag.id = article_tag.tag_id

--    GROUP BY article.id   # 删除

```

### 列合并

> 在多对多的关系的时候，一般会有一张中间表记录多对多的关系，比如这三张表： article - article_tag - tag 。每条文章(article)记录里面会有一个标记(tag)。这时候使用联合查询，会出现许多重复的文章(article)记录，所以我们要用GROUP BY来分组，顺便达到去除重复文章的目的。



#### 一般是这样查询的

```

SELECT
    article.*,tag.*
FROM
    article
    LEFT JOIN article_tag ON article.id = article_tag.article_id
    LEFT JOIN tag ON tag.id = article_tag.tag_id
GROUP BY
    article.id

```

> 但是如果这样的话，问题随之就来了，有些tag被省略了。所以我们这样来查询。

#### 轮到 GROUP_CONCAT() 上场

```

SELECT
    article.*, GROUP_CONCAT(DISTINCT tag_name SEPARATOR ',') as tags
FROM
    article
    LEFT JOIN article_tag ON article.id = article_tag.article_id
    LEFT JOIN tag ON tag.id = article_tag.tag_id
GROUP BY
    article.id

```

> 这里主要用到了一个 GROUP_CONCAT 函数用于合并列，SEPARATOR ',' 设置连接符，DISTINCT 去重复。
