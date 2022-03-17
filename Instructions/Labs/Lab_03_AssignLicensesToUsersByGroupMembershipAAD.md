---
lab:
  title: 03 - 使用组成员身份分配许可证
  learning path: "01"
  module: Module 01 - Implement an identity management solution
ms.openlocfilehash: 9ecd9c244867c5c995a5e6dc39bbdc47f48ad5c7
ms.sourcegitcommit: fbe0a16b7c6a9a2ac2c2ea3b3f707faa0afe23a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2022
ms.locfileid: "139258490"
---
# <a name="lab-03-assigning-licenses-using-group-membership"></a>实验室 03：使用组成员身份分配许可证

## <a name="lab-scenario"></a>实验室方案

你的公司决定使用 Azure AD 中的安全组来管理许可证。 你需要配置新的安全组，向该组分配许可证，以及验证组成员许可证是否已更新。

#### <a name="estimated-time-10-minutes"></a>预计用时：10 分钟

### <a name="exercise-1---create-a-security-group-and-add-a-user"></a>练习 1 - 创建安全组并添加用户

#### <a name="task-1---check-to-see-if-delia-dennis-has-access-to-office-365"></a>任务 1 - 检查 Delia Dennis 是否可以访问 Office 365

1. 启动一个新的 InPrivate 浏览器窗口。
2. 连接到 [https://www.office.com](https://www.office.com)。
3. 单击“登录”并以 Delia Dennis 的身份进行连接。

    | 设置| **值**|
    | :--- | :--- |
    | 用户名 | DeliaD@`your domain name.com`|
    | 密码| 在“资源”中输入全局管理员的密码|

4. 你应会连接到 Office.com 网站，但会看到一条消息，该消息表示你没有许可证。

    ![Office.com 网站的屏幕图像，其中 Delia Dennis 已登录，但办公应用程序不可用，因为没有分配许可证。](./media/delia-no-office-license.png)
    
5. 关闭浏览器窗口。

#### <a name="task-2----create-a-security-group-in-azure-active-directory"></a>任务 2 - 在 Azure Active Directory 中创建安全组

1. 浏览到 [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview)。

2. 在左侧导航栏的“管理”下，选择“组”。
3. 在“组”边栏选项卡的菜单中，选择“新建组”。
4. 使用以下信息创建组：

    | **设置**| **值**|
    | :--- | :--- |
    | 组类型| 安全|
    | 组名称| sg-SC300-O365|
    | 成员身份类型| 已分配|
    | 所有者| *分配自己的管理员帐户作为组所有者*|

5. 单击“成员”下的“未选定成员”文本。
6. 从用户列表中选择“Delia Dennis”。
7. 单击“选择”按钮。

    ![显示“新建组”边栏选项卡的屏幕图像，其中突出显示了“组类型”、“组名称”、“所有者”和“成员”](./media/lp1-mod2-create-group.png)

8. 单击“创建”按钮。
9. 完成后，验证名为“sg-SC300-O365”的组是否显示在“所有组”列表中 。

#### <a name="task-3---assign-a-license-to-a-group"></a>任务 3 - 向组分配许可证

1. 在“所有组”列表中，选择“sg-SC300-O365” 。
2. 在“市场营销”边栏选项卡的“管理”下，选择“许可证”。
3. 在菜单中选择“+ 分配”。
4. 在“更新许可证分配”边栏选项卡的“选择许可证”下，查看可用许可证列表，然后选中“Office 365 E3”复选框 。

    提示 - 选择多个许可证后，可使用“查看许可证选项”菜单来选择特定许可证，并查看该许可证的许可证选项。

    ![屏幕图像显示已选定并已分配给组的许可证。 “查看许可证”菜单也已选中，其中显示多个选择选项。](./media/lp1-mod2-assign-license-group.png)

6. 选择“保存”  。

#### <a name="taks-4---confirm-the-office-365-license"></a>任务 4 - 确认 Office 365 许可证

1. 启动一个新的 InPrivate 浏览器窗口。
2. 连接到 [https://www.office.com](https://www.office.com)。
3. 单击“登录”并以 Delia Dennis 的身份进行连接。

    | 设置| **值**|
    | :--- | :--- |
    | 用户名 | DeliaD@`your domain name.com`|
    | 密码| 在“资源”中输入全局管理员的密码|

4. 你应连接到 Office.com 网站，然后未看见关于许可证的消息。 所有的 Office 应用程序都可在左侧获取。

    ![Office.com 网站的屏幕图像，其中 Delia Dennis 已登录，并且办公应用程序可用，因为分配了许可证。](./media/delia-office-license.png)
    
5. 关闭浏览器窗口。
    
