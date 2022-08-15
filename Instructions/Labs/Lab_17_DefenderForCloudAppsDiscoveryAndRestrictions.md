---
lab:
  title: 17 - Defender for Cloud Apps 应用程序发现和强制实施限制
  learning path: "03"
  module: Module 03 - Implement Access Management for Apps
ms.openlocfilehash: 2810d72197e31241d7a3c30d85b40cc5682be0bf
ms.sourcegitcommit: fa8ae52be1e809ca81d07036fefeebffb54c1861
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2022
ms.locfileid: "147521460"
---
# <a name="lab-17---defender-for-cloud-apps-application-discovery-and-enforcing-restrictions"></a>实验室 17 - Defender for Cloud Apps 应用程序发现和强制实施限制

## <a name="lab-scenario"></a>实验室方案

Microsoft Defender for Cloud Apps 利用来自网络流量的日志来识别用户正在访问的应用程序。  来自本地防火墙的流量日志将提供有关最常见应用程序和访问这些应用的用户的快照报表。  来自托管设备的流量将馈送到 Microsoft Defender for Cloud Apps 发现概述仪表板

#### <a name="estimated-time-10-minutes"></a>预计用时：10 分钟

### <a name="exercise-1---defender-for-cloud-apps-discovery"></a>练习 1 - Defender for Cloud Apps 发现

#### <a name="task-1---discovery-apps-in-defender-for-cloud-apps"></a>任务 1 - Defender for Cloud Apps 中的发现应用

1. 使用全局管理员帐户登录 [https://security.microsoft.com](https://security.microsoft.com)。

1. 在左侧菜单中，滚动到底部，然后选择“更多资源”。

1. 在“更多资源”窗口中，找到并选择“Microsoft Defender for Cloud Apps”下的“打开”  。  此时将转到 Microsoft 365 帐户中的“Microsoft Defender for Cloud Apps”门户。

1. 在左侧菜单中选择“发现”下拉列表，然后选择“云应用目录” 。

1. 在“按类别浏览”中，选择“云存储空间” 。

1. 在应用列表中，注意应用名称旁边的风险评分。  

1. 打开另一个浏览器选项卡并导航到 Dropbox.com。

1. 你将能够访问此网站。


#### <a name="task-2---restrict-apps-in-defender-for-cloud-apps"></a>任务 2 - 限制 Defender for Cloud Apps 中的应用

1. 返回到“已发现的应用”磁贴并为 Dropbox 选择“标记为未经批准” 。  **注意**：它位于带圆圈的复选标记旁。

1. 此过程允许你阻止公司策略中未批准的应用程序，从而限制组织内的影子 IT。

**注意**：在取消批准应用程序和阻止该应用程序之间存在延迟。

1. 一旦应用程序因未经批准而被阻止，将无法通过浏览器、专用浏览器或商店下载访问该应用程序。



