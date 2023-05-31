---
lab:
  title: 27 - 适用于 Azure AD 数据源的 Microsoft Sentinel Kusto 查询
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# 实验室 27 - 适用于 Azure AD 数据源的 Microsoft Sentinel Kusto 查询

注意 - 此实验室需要 Azure Pass。 有关说明，请参阅实验室 00。

## 实验室方案

Microsoft Sentinel 是 Microsoft 的云原生 SIEM 和 SOAR 解决方案。  通过连接来自 Microsoft 和第三方安全解决方案的数据源，可以执行安全操作任务。  在本实验室练习中，你将创建一个 Microsoft Sentinel 工作区，其中包含 Azure AD 的数据连接器，用于使用 Kusto 查询语言 (KQL) 执行搜寻查询。 

#### 预计用时：30 分钟

### 练习 1 - 为 Kusto 查询配置 Microsoft Sentinel

#### 任务 1 - 创建 Microsoft Sentinel 工作区

1. 以全局管理员身份登录  [https://portal.azure.com](https://portal.azure.com)  。

1. 搜索“Microsoft Sentinel”并将其选中。 

1. 选择“创建 Microsoft Sentinel”。

1. 在“将 Microsoft Sentinel 添加到工作区”磁贴中，选择“创建新工作区” 。

1. 在“资源组”中，选择“新建”并输入“Sentinel-RG”  。

1. 命名工作区。  示例 - SentinelLogAnalytics。

1. 选择附近的区域。

1. 选择“审阅 + 创建”，然后选择“创建” 。

1. Log Analytics 工作区部署完成后，选择“刷新”按钮。 然后选择工作区，选择“添加”。  这会将工作区添加到 Microsoft Sentinel 并打开 Microsoft Sentinel。

1. 如果出现提示，请选择“确定”以激活 Microsoft Sentinel 免费试用版。

#### 任务 2 - 将 Azure AD 添加为数据源

1. 在“Microsoft Sentinel”中，导航到“配置”菜单，然后选择“数据连接器”  。

1. 在数据连接器列表中，找到“Azure Active Directory”并选中它。

1. 在右侧，将打开一个预览磁贴。  选择“打开连接器页”。

1. 在连接器页中，将为数据连接器提供说明和后续步骤。 确认每个先决条件旁都有复选标记，以继续进行配置 。

1. 在“配置”下，选中“登录日志”和“审核日志”的框  。 其他日志源可用，但目前处于预览阶段，超出了本课程的范围。

1. 选择“应用更改”。 

1. 系统会提供已成功应用更改的通知。 通过选择连接器页右上角的 X 导航到 Microsoft Sentinel 工作区 。

1. 在“Microsoft Sentinel | 数据连接器”磁贴上选择“刷新”，数字 1 随即显示在“已连接”计数中  。

   注意 - Azure AD 数据连接器可能需要几分钟时间才能显示在活动计数中。 

#### 任务 3 - 对用户活动运行 Kusto 查询

1. 在“Microsoft Sentinel”中，导航到“常规”菜单标题下的“日志”  。

1. 关闭“欢迎使用 Log Analytics”窗口。

1. 随即会打开一个窗口，其中包含示例查询，选择“审核”，然后滚动以查找“用户 ID” 。

1. 选择“运行”。 

1. 这将提供 Azure AD 上的用户 ID 列表。  由于我们刚刚才创建工作区，因此可能不会看到结果。  请注意查询的格式。

1. 在菜单中的“威胁管理”下，选择“搜寻” 。 

1. 向下滚动以查找查询“用户帐户和身份验证应用程序的异常登录位置”。  这个关于 Azure Active Directory 登录的查询会考虑每个 Azure Active Directory 应用程序的所有用户登录，并为单个应用程序中的用户选出位置配置文件中最异常的更改。 目的是搜寻用户帐户入侵行为（可能通过特定的应用程序向量）。 

1. 选择“查看查询结果”以运行查询。

1. 这可能不会为新工作区提供结果，但你现在已经了解了如何运行查询来收集信息或搜寻潜在威胁。
