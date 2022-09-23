---
title: 联机托管说明
permalink: index.html
layout: home
ms.openlocfilehash: 3f1d9eb9d23b41f212641797985d518792859159
ms.sourcegitcommit: 317faec6fdf204c92dfe1461d4a30f5b8d8ea950
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2022
ms.locfileid: "138357269"
---
# <a name="content-directory"></a>内容目录

可在[此处下载](https://github.com/MicrosoftLearning/SC-300-Identity-and-Access-Administrator/archive/master.zip)所需的实验室文件

下面列出了每个实验室练习和演示的超链接。

## <a name="labs"></a>实验室

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| 模块 | 实验室 |
| --- | --- | 
{% 表示实验室 % 中的活动}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
