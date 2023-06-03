---
lab:
  title: 05 - 将来宾用户添加到目录
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# 实验室 05：将来宾用户添加到目录

## 实验室方案

你的公司与许多供应商合作，有时你需要将一些供应商帐户作为来宾添加到你的目录。

#### 预计用时：20 分钟

### 练习 1 - 将来宾用户添加到目录

#### 任务 - 添加来宾用户

1. 以拥有受限的管理员目录角色或来宾邀请者角色的用户身份登录  [https://portal.azure.com](https://portal.azure.com)  。

2. 选择“Azure Active Directory” **** 。

3. 在“管理”下，选择“用户” ****  **** 。

4. 选择“+ 新建用户” **** 。

5. 在“新建用户”菜单上，选择“邀请外部用户”，然后将信息添加为来宾用户。

    备注 - 不支持组电子邮件地址；输入个人的电子邮件地址。 另外，某些电子邮件提供程序允许用户向其电子邮件地址中添加加号 (+) 和附加文本来帮助执行收件箱筛选之类的操作。 但是，Azure AD 当前不支持在电子邮件地址中使用加号。 为避免在传送时出现问题，请省略加号及其之后的任何字符，直至 @ 符号。

6. 输入电子邮件地址，例如 **sc300externaluser1@sc300email.com** 。

7. 完成后选择“邀请”。

8. 在“用户”页上，验证是否列出了帐户，并在“用户类型”列中，验证是否显示“来宾” 。

发送邀请后，该用户帐户将以来宾的形式自动添加到目录。


### 练习 2 - 批量邀请来宾用户

#### 任务 1 - 批量邀请用户

最近与另一家公司建立了合作伙伴关系。 目前，合作伙伴公司的员工将作为来宾添加。 你需要确保可一次性导入多个来宾用户。

1. 以全局管理员身份登录 [https://portal.azure.com](https://portal.azure.com)。

2. 在导航窗格中选择“Azure Active Directory”。

3. 在“管理”下，选择“用户” 。

4. 在“用户”页的菜单上，选择“批量操作”>“批量邀请”。

     ![显示“所有用户”页面的屏幕图像，其中突出显示了“批量操作”和“批量邀请”菜单选项](./media/lp1-mod3-bulk-invite-option.png)

5. 在“批量邀请用户”窗格中，选择“下载”到具有邀请属性的示例 CSV 模板。

6. 使用编辑器查看 CSV 文件，查看模板。

7. 打开 .csv 模板，为每个来宾用户添加一行。 必需的值为：

    - **要邀请的电子邮件地址** - 会收到邀请的用户
    - **重定向 URL** - 一个 URL，受邀请的用户在接受邀请后会被系统定向到该 URL。

    ![显示示例批量邀请来宾模板 CSV 的屏幕图像](./media/lp1-mod3-template-csv.png)

8. 保存文件。

9. 在“批量邀请用户”页中，在“上传 csv 文件”下，浏览找到该文件。

     备注 - 选择 .csv 文件后立即开始对它的验证。

10. 验证文件内容后，你将看到“文件成功上传”。 如果有错误，必须修正错误，然后才能提交作业。

    ![显示“批量邀请用户”的屏幕图像，其中突出显示了“文件成功上传”消息](./media/lp1-mod3-bulk-invite-users-upload-csv.png)

11. 文件通过验证以后，请选择“提交”，开始用于添加邀请的 Azure 批量操作。

12. 若要查看作业状态，请选择“选择此处查看每项操作的状态”。 也可在“活动”部分中选择“批量操作结果”。 要详细了解批量操作中每个行项，请选择“成功数”、“失败数”或“请求总数”列下的值  。 如果失败，则会列出失败原因。

    ![显示批量操作结果的屏幕图像](./media/lp1-mod3-bulk-operations-results.png)

13. 作业完成后，会显示一条通知，指出批量操作成功。

#### 任务 2 - 使用 PowerShell 邀请来宾用户

1. 以管理员身份打开 PowerShell。  这可以通过在 Windows 中搜索 PowerShell 并选择“以管理员身份运行”来完成。  

1. 如果以前未使用过 Azure AD PowerShell 模块，则需要添加它。  运行以下命令：Install-Module AzureAD。  出现提示时，选择“Y”继续。

    ``` 
    Install-Module AzureAD
    ```

1. 通过运行以下命令确认模块正确安装：  

    ```
    Get-Module AzureAD 
    ```

1. 接下来，需要通过运行以下命令登录到 Azure：  

    ```
    Connect-AzureAD
    ```
    
1. 将显示 Microsoft 登录窗口，供你登录 Azure AD。  

1. 若要验证是否已连接并查看现有用户，请运行：  

    ```
    Get-AzureADUser 
    ```

1. 你已准备好邀请来宾用户。  以下命令将填充用户信息并运行。  如果要添加多个用户，可以使用记事本 txt 文件添加用户信息并将其复制/粘贴到 PowerShell 中。 

    ```
    New-AzureADMSInvitation -InvitedUserDisplayName "Display" -InvitedUserEmailAddress name@emaildomain.com -InviteRedirectURL https://myapps.microsoft.com -SendInvitationMessage $true 
    ```

现在，你已了解如何在 Azure AD 门户、Microsoft 365 管理中心中邀请用户，使用 csv 文件批量邀请以及使用 PowerShell 命令邀请用户。
