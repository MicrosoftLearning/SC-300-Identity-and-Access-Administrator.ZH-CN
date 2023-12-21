---
lab:
  title: 18 - Microsoft Defender for Cloud Apps 访问策略
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# 18 - Microsoft Defender for Cloud Apps 访问和会话策略

## 实验室方案

Microsoft Defender for Cloud Apps 可用于创建特定于要监视的云应用的其他条件访问策略。  可从 Microsoft Defender for Cloud Apps 门户中的“控制”菜单中创建这些策略。

#### 预计用时：20 分钟

### 练习 1 - 创建并测试条件访问应用控制策略

#### 任务 1 - 确认 PradeepG 可无条件访问 FORMS

1. 启动一个新的 InPrivate 浏览窗口。
2. 连接到 [https://forms.microsoft.com](https://forms.microsoft.com)。
3. 选择页面右上角的登录。
4. 以 Pradeep Gupta 身份登录。
   - Username = PradeepG@<<<your lab hoster provided domain>>>
   - 密码 = 资源选项卡中的密码
5. 确认 Microsoft Forms 打开，并且未收到任何警告消息。
6. 关闭 InPrivate 浏览窗口。

#### 任务 2 - 将 Microsoft Entra ID 配置为使用 Defender for Cloud 应用

1. 导航到 https://entra.microsoft.com[](https://entra.microsoft.com)，然后转到“Microsoft Entra ID”。

2. 在“标识”**** 下，选择“保护”****。

3. 然后选择“条件访问”****。

4. 选择“+ 创建新策略”。

5. 输入策略名称，例如“使用 Forms 监视 Pradeep”。

6. 在“分配”**** 下，依次选择“已选择 0 个用户和组”****、“包括特定用户”****、“选择用户和组”****，然后标记“用户和组”****。

7. 选择实验室租户的 Pradeep Gupta 帐户，然后选择“选择” 。

8. 在“目标资源”下，选择“未选择目标资源”********。

9. 选择“选择应用”，然后选择“Microsoft Forms”，然后选择“选择”  。 

10. 在“访问控制”下****，选择“会话”**** 和“已选择 0 个控件”****。

11. 选择“使用条件访问应用控制”框，保留默认值“仅监视”，然后选择“选择”  。

12. 在“启用策略”下，选择“启用”，然后选择“创建”  。

#### 任务 3 - 登录到 Forms 并验证条件访问是否在监视

1. 启动一个新的 InPrivate 浏览窗口。
2. 连接到 [https://forms.microsoft.com](https://forms.microsoft.com)。
3. 选择页面右上角的登录。
4. 以 Pradeep Gupta 身份登录。
   - Username = PradeepG@<<<your lab hoster provided domain>>>
   - 密码 = 资源选项卡中的密码
5. 确认 Pradeep 具有访问权限，并且你收到一条新消息：
   - 贵公司正在监视此应用程序的使用情况。
6. 关闭 InPrivate 浏览窗口。

### 练习 2 - 在 Microsoft Defender for Cloud Apps 中设置警报

#### 任务 1 - 访问 Microsoft Defender for Cloud Apps 并创建条件访问应用控制

注册应用程序会在应用与 Microsoft 标识平台之间建立信任关系。 信任是单向的：应用信任 Microsoft 标识平台，但标识平台并不信任应用。

1. 使用全局管理员帐户登录 [https://security.microsoft.com](https://security.microsoft.com)。

1. 在左侧菜单中，滚动到底部，然后选择“更多资源”。

1. 在“更多资源”窗口中，找到并选择“Microsoft Defender for Cloud Apps”下的“打开”  。  此时将转到 Microsoft 365 帐户中的“Microsoft Defender for Cloud Apps”门户。

1. 在 Microsoft Defender for Cloud Apps 门户菜单中，选择“控制”的下拉箭头，然后选择“策略”  。

1. 选择“+ 创建策略”。 选择“访问策略”。

1. 输入策略的名称，如“监视 Microsoft Forms 访问”。

1. 将“类别”保留为“访问控制” 。

1. 在“符合以下所有条件的活动”**** 下，选择“符合 Intune 且已加入混合 Microsoft Entra”**** 的下拉菜单，然后取消选择“已加入混合 Microsoft Entra”****。

1. 选择“选择应用”下拉列表。  选择“Microsoft Forms”。

1. 将“操作”保留为“测试” 。

1. 在“警报”下，选中“创建警报...”并选择“以电子邮件形式发送警报”  。

1. 输入实验室管理员电子邮件地址，然后按 Enter。

1. 选择“创建”以创建访问策略。

#### 任务 2 - 以 Pradeep 身份登录到 Forms 以触发活动

1. 启动一个新的 InPrivate 浏览窗口。
2. 连接到 [https://forms.microsoft.com](https://forms.microsoft.com)。
3. 选择页面右上角的登录。
4. 以 Pradeep Gupta 身份登录。
   - Username = PradeepG@<<<your lab hoster provided domain>>>
   - 密码 = 资源选项卡中的密码
5. 确认 Pradeep 具有访问权限，并且你收到一条新消息：
   - 贵公司正在监视此应用程序的使用情况。
6. 关闭 InPrivate 浏览窗口。

#### 任务 3 - 查看 Defender for Cloud Apps 中的活动

1. 返回到运行 Defender for Cloud Apps 的浏览器。
2. 刷新浏览器以确保下载最新数据。
3. 从****“调查”菜单中选择“活动日志”****。
4. 使用“应用: 筛选”从列表中选择“Microsoft Forms” 。
5. 请注意 Pradeep 的登录记录。
