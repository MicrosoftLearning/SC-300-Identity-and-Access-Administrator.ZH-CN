---
title: 联机托管说明
permalink: index.html
layout: home
---

# 内容目录

可在[此处下载](https://github.com/MicrosoftLearning/SC-300-Identity-and-Access-Administrator/archive/master.zip)所需的实验室文件

下面列出了每个实验室练习和演示的超链接。

## 实验室

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| 模块 | 实验室 |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
