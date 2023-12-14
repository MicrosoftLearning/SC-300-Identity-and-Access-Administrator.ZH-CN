---
lab:
  title: 17 - Defender for Cloud Apps 应用程序发现和强制实施限制
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# 实验室 17 - Defender for Cloud Apps 应用程序发现和强制实施限制

## 实验室方案

Microsoft Defender for Cloud Apps 利用来自网络流量的日志来识别用户正在访问的应用程序。来自本地防火墙的流量日志将提供有关最常见应用程序和访问这些应用的用户的快照报表。来自托管设备的流量将馈送到 Microsoft Defender for Cloud Apps 发现概述仪表板

#### 预计用时：10 分钟

### 练习 1 - Defender for Cloud Apps 发现

#### 任务 1 - Defender for Cloud Apps 中的发现应用

1. 使用全局管理员帐户登录 [https://security.microsoft.com](https://security.microsoft.com)  。

1. 在左侧菜单中，滚动到名为“云应用”的标题，然后单击“云应用目录” 。

1. 在“按类别浏览”窗格中，选择“云存储空间” 。

1. 在应用列表中，注意应用名称旁边的风险评分。  

1. 打开另一个浏览器选项卡并导航到 Dropbox.com。

1. 你将能够访问此网站。


#### 任务 2 - 限制 Defender for Cloud Apps 中的应用

1. 返回到“已发现的应用”磁贴并为 Dropbox 选择“标记为未经批准” 。  **注意**：它位于带圆圈的复选标记旁。

1. 单击“保存”

1. 此过程允许你阻止公司策略中未批准的应用程序，从而限制组织内的影子 IT。

**注意**：在取消批准应用程序和阻止该应用程序之间存在延迟。

一旦应用程序因未经批准而被阻止，将无法通过浏览器、专用浏览器或客户端上的商店下载访问该应用程序，该客户端已载入与 Microsoft Defender for Cloud Apps 集成的 MDE (Microsoft Defender for Endpoint)。



