---
lab:
  title: 06 - 添加联合标识提供者
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# 实验室 6：添加联合标识提供者

### 登录类型 = Microsoft 365 管理

## 实验室方案

你的公司与许多供应商合作，有时你需要将一些供应商帐户作为来宾添加到你的目录并允许他们使用其 Google 帐户登录。

#### 预计用时：25 分钟

### 练习 1 - 配置标识提供者

#### 任务 1 - 将 Google 配置为标识提供者

重要说明 - 本练习需要 Google 上的 Gmail 帐户。 创建新的 Google 帐户，然后按照练习的步骤操作。  请务必记下电子邮件地址和密码，这是完成实验室所必需的。

1. 转到 Google API (https://console.developers.google.com )，并使用 Google 帐户登录。 我们建议使用共享团队 Google 帐户。

2. 如果系统提示接受服务条款，请接受它。

创建新项目：
3. 在页面顶部，选择项目菜单以打开“选择项目”页。 选择“新建项目”。  将其余字段保留为默认设置。

4. 在“新建项目”页上，为项目指定名称（例如 MyB2BApp），然后选择“创建” 。

5. 通过选择“通知”消息框中的链接，或通过使用页面顶部的项目菜单，打开该新项目。

6. 在左侧菜单中，选择“API 和服务”，然后选择“OAuth 同意屏幕” 。

7. 在“用户类型”下，选择“外部”，然后选择“创建” 。

8. 在“OAuth 同意屏幕”**** 的“应用信息”下，输入应用名称，例如 Microsoft Entra ID****。

9. 在“用户支持电子邮件”下，选择电子邮件地址。 这应包括用于登录 Google 的电子邮件地址。

10. 在“已授权域”下，选择“+ 添加域”，然后添加 microsoftonline.com 域。

   ```
   microsoftonline.com
   ```

11. 在“开发人员联系信息”下，输入用于登录门户的实验室帐户的电子邮件地址。

12. 选择“保存并继续”。

13. 在左侧菜单中，选择“凭据”。

14. 选择“+ 创建凭据”，然后选择“OAuth 客户端 ID” 。

15. 在“应用程序类型”菜单中，选择“Web 应用程序”。 为应用程序提供合适的名称，例如 Microsoft Entra B2B。 在“已授权的重定向 URI”下，添加以下 URI：

   ```
      https://login.microsoftonline.com
   ```
      https://login.microsoftonline.com/te/**tenant ID**/oauth2/authresp    （其中 <tenant ID> 是你的租户 ID）
   ```
      https://login.microsoftonline.com/te/**tenant name**.onmicrosoft.com/oauth2/authresp
       (where <tenant name> is your tenant name)
   ```

**实验室提示** - 结果应该与此类似，包括租户 ID 和租户名称。
| URI # | 链接。 |
| :--- | :--- |
| URI 1 | https://login.microsoftonline.com |
| URI 2 | https://login.microsoftonline.com/te/aaaa1111bbbb2222cccc |
| URI 3 | https://login.microsoftonline.com/te/MyTenantName.onmicrosoft.com/oauth |

16. 选择**创建**。 复制你的客户端 ID 和客户端密码 。 你将在 Azure 门户中添加标识提供者时用到它们。

17. 可以将项目保留为“测试”的发布状态。

#### 任务 2 - 添加测试用户
18. 在“API 和服务”菜单下选择“OAuth 同意屏幕”。

19. 在页面的“测试用户”部分中，选择“+ 添加用户”。

20. 输入你为此实验室创建（或正在使用）的 Gmail 帐户。

21. 选择“保存”


### 练习 2 - 配置 Azure 以使用外部标识提供者

#### 任务 1 - 为 Google 联合身份验证配置 Microsoft Entra ID
1. 以管理员身份登录到 [https://entra.microsoft.com](https://entra.microsoft.com)  。

2. 选择“Microsoft Entra ID” ****。

3. 在“标识” **** 下，选择“外部标识” ****。

4. 从左侧菜单中选择“所有标识提供者”。

5. Microsoft 为 Google 标识提供者提供直接联合身份验证。可通过从“外部标识”|“所有标识提供者”页选择 + Google 来启动此操作
 
6. 选择 + Google 后，将打开另一个页面，其中包含将 Google 配置为标识提供者所需的其他信息。  

7. 输入之前获得的客户端 ID 和客户端密码 。

8. 选择“保存”。

这将完成将 Google 配置为标识提供者的过程。

#### 任务 2 - 邀请你测试用户帐户
9. 如果使用了现有的 Gmail 帐户，请记得通过“外部标识”|“所有标识提供者”删除该帐户。 还可返回到 Google 开发人员控制台并删除所创建的项目。

10. 打开 Microsoft Entra ID。

11. 转到“用户”，然后选择“所有用户”****。

12. 选择“+ 新建用户”。

13. 从下拉菜单中选择“邀请外部用户”。

14. 输入你在练习 1 任务 2 中设置为 Google 应用的测试用户的 Gmail 帐户的信息。

15. 根据需要输入个人信息。

16. 选择“邀请”。

#### 任务 3 - 接受邀请并登录
17. 使用 InPrivate 浏览器登录到 Gmail 帐户。

18. 在收件箱中打开“Microsoft 邀请代表方”。

19. 选择邮件中的“接受邀请”链接。

20. 按照登录对话框中的要求输入你的用户名和密码（如果需要）。
   注意：如果联合身份验证正常运行，你将在此处看到新 Google 外部标识提供者的第一个结果。  你将转到登录屏幕，并能够使用 Gmail 凭据登录。  如果联合身份验证不起作用或尚未设置，则会在登录后向用户发送帐户验证电子邮件以确认帐户。  使用联合身份验证时，不需要额外的验证。

   注意：如果收到访问错误 500，请等待大约 30 秒并刷新页面。  选择“重新提交”。  此错误仅在实验室环境中是时间问题。

21. 阅读你收到的新的“权限请求者:”消息。  此消息来自 Azure 实验室域。

22. 选择“接受”。

23. 登录完成后，系统会向你发送 MyApplications。

#### 任务 4 - 使用 Google 帐户登录到 Microsoft 365
24. 完成任务 3 的外部用户邀请过程后，可以直接登录到 Microsoft Online。

25. 在已打开的浏览器中打开一个新选项卡。
   请注意，如果未在任务 3 中打开新的 InPrivate 浏览器，则应在此步骤中执行此操作。

26. 输入以下 Web 地址：

   ```
   login.microsoftonline.com
   ```

27. 在对话框中选择“登录选项”。
 
28. 选择“登录到组织”。

29. 在框中输入“实验室租户域名”，然后选择“下一步” 。

30. 输入创建的 Google 电子邮件地址和密码。
此时，你应会看到帐户已传递给 Google 进行确认；随后请输入 Microsoft Office 门户。
