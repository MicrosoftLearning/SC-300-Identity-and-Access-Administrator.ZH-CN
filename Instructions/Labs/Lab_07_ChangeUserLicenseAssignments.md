---
lab:
  title: 07 - 更改用户帐户许可证分配
  learning path: "01"
  module: Module 01 - Implement an identity management solution
ms.openlocfilehash: 5926db4909933fe31574632ff625a45717bfa9ee
ms.sourcegitcommit: 448f935ad266989a6f0086019e0c0e0785ad162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2022
ms.locfileid: "138421372"
---
# <a name="lab-07-change-user-account-license-assignments"></a>实验室 07：更改用户帐户许可证分配

## <a name="lab-scenario"></a>实验室方案

组织中的一些用户帐户将不会获得其分配的许可证中的所有可用产品，或者将需要更新或补充其许可证分配。 你需要确保能够在 Azure AD 中更新用户帐户的许可证分配。

#### <a name="estimated-time-5-minutes"></a>预计用时：5 分钟

### <a name="exercise-1---add-a-windows-10-license-to-a-user-account"></a>练习 1 - 向用户帐户添加 Windows 10 许可证

#### <a name="task-1---find-your-unlicensed-user-in-azure-active-directory"></a>任务 1 - 在 Azure Active Directory 中查找未获许可的用户

1. 浏览到 [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview)。

2. 在左侧导航栏的“管理”下，选择“用户”。 

3. 在“用户”边栏选项卡的搜索框中输入“Raul”。

4. 单击“Raul Razo”
5. 查看 Raul 的个人资料并确保他设置了“使用位置”。

    警告 - 若要将许可证分配给某个用户，该用户必须分配有使用位置。

6. 单击左侧菜单中的“许可证”菜单项。
7. 确保 Rual 的状态为“未找到许可证分配”。

#### <a name="task-2---update-user-license-assignments"></a>任务 2 - 更新用户许可证分配

1. 浏览到 [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview)。

2. 在左侧导航栏的“管理”下，选择“用户” 

3. 在“用户”边栏选项卡上，选择“Raul Razo”。

4. 在左侧导航栏中，选择“许可证”。

5. 选择“+ 分配”按钮。 

6. 在“更新许可证分配”边栏选项卡上，选中“Windows 10 企业版 E3”许可证复选框。

    ![显示“更新许可证分配”页的屏幕图像，其中突出显示许可证选项](./media/lp1-mod2-assign-user-license-options.png)

7. 完成后选择“保存”。
8. 在屏幕顶部，单击 `Home > Contoso Marketing > User >` Raul Razo
9. 请注意，许可证已分配。
