---
lab:
  title: 27 - 适用于 Microsoft Entra 数据源的 Microsoft Sentinel Kusto 查询
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# 实验室 27 可选 - 适用于 Microsoft Entra 数据源的 Microsoft Sentinel Kusto 查询

**注意** - 此实验室目前无法在提供的训练实验室环境中完成。  我们将在此处退出实验室步骤，因此，你可以选择在“自带订阅” (BYOS) 环境中试用。  请阅读这些步骤，了解可能的情况。  我们积极努力更新此实验室，以在实验室环境中查找解决方法，并很快对其进行更新。

### 登录类型 = Azure 资源登录

## 实验室方案

Microsoft Sentinel 是 Microsoft 的云原生 SIEM 和 SOAR 解决方案。  通过连接来自 Microsoft 和第三方安全解决方案的数据源，可以执行安全操作任务。  在本实验室练习中，你将创建一个 Microsoft Sentinel 工作区，其中包含 Microsoft Entra ID 的数据连接器，用于使用 Kusto 查询语言 (KQL) 执行搜寻查询。 

#### 预计用时：30 分钟

### 练习 1 - 为 Kusto 查询配置 Microsoft Sentinel

#### 任务 1 - 创建 Microsoft Sentinel 工作区

1. 以全局管理员身份登录  [https://portal.azure.com](https://portal.azure.com)  。

1. 搜索“Microsoft Sentinel”并将其选中。 

1. 在左上角选择“+ 创建”****。

1. 在“将 Microsoft Sentinel 添加到工作区”磁贴中，选择“+ 新建工作区”********。

1. 在“资源组”中，选择“新建”并输入“Sentinel-RG”  。

1. 命名工作区。  示例 - SentinelLogAnalytics。

1. 选择附近的区域。

1. 选择“审阅 + 创建”，然后选择“创建” 。

1. Log Analytics 工作区部署完成后，选择“刷新”按钮。 然后选择工作区，选择“添加”。  这会将工作区添加到 Microsoft Sentinel 并打开 Microsoft Sentinel。

1. 如果出现提示，请选择“确定”以激活 Microsoft Sentinel 免费试用版。

#### 任务 2 - 将 Microsoft Entra ID 添加为数据源

1. 在“Microsoft Sentinel”**** 中，在菜单上导航到“内容管理”****，然后选择“内容中心”****。

1. 使用搜索框在连接器列表中查找“Entra”，找到“Microsoft Entra ID”并标记复选框。********

1. 在右侧，将打开一个预览磁贴。  选择“安装”  。

1. 安装完成后，在“配置”菜单中选择“数据连接器”**** 菜单项。

    **备注** - 应显示“已安装 1 个连接器”并列出 Microsoft Entra ID****。

1. 选择“Microsoft Entra ID”****，然后选择“打开连接器页”****。

1. 在连接器页中，将为数据连接器提供说明和后续步骤。 确认每个先决条件旁都有复选标记，以继续进行配置 。

1. 在“配置”下，选中“登录日志”和“审核日志”的框  。 其他日志源可用，但目前处于预览阶段，超出了本课程的范围。

1. 选择“应用更改”。 

1. 系统会提供已成功应用更改的通知。 通过选择连接器页右上角的 X 导航到 Microsoft Sentinel 工作区 。

1. 在“Microsoft Sentinel | 数据连接器”磁贴上选择“刷新”，数字 1 随即显示在“已连接”计数中  。

   注意 - Microsoft Entra ID 数据连接器可能需要几分钟时间来显示在活动计数中****。 

#### 任务 3 - 对用户活动运行 Kusto 查询

1. 在“Microsoft Sentinel”中，导航到“常规”菜单标题下的“日志”  。

1. 关闭“欢迎使用 Log Analytics”窗口。

1. 将打开包含示例查询的窗口，选择“审核”****，然后搜索以查找“用户 ID”****。

1. 选择**运行**。 

1. 这将提供 Microsoft Entra ID 上的用户 ID 列表。  由于我们刚刚才创建工作区，因此可能不会看到结果。  请注意查询的格式。
