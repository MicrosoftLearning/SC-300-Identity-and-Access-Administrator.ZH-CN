---
lab:
  title: 04 - 还原已删除的用户
  learning path: "01"
  module: Module 01 - Implement an identity management solution
ms.openlocfilehash: 9ec368f11cce9ad63fbc297149a68582e1773358
ms.sourcegitcommit: 448f935ad266989a6f0086019e0c0e0785ad162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2022
ms.locfileid: "138421309"
---
# <a name="lab-04-restore-a-deleted-user"></a>实验室 04：还原已删除的用户

## <a name="lab-scenario"></a>实验室方案

可能出现已删除帐户，然后需要恢复此帐户的情况。 你需要验证是否可以恢复最近已删除的帐户。

#### <a name="estimated-time-5-minutes"></a>预计用时：5 分钟

### <a name="exercise-1---remove-a-user-from-azure-active-directory"></a>练习 1 - 从 Azure Active Directory 删除用户

#### <a name="task-1---remove-a-user"></a>任务 1 - 删除用户

1. 浏览到 [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview)。

2. 在左侧导航栏的“管理”下，选择“用户”。

3. 在“用户”列表中，选中要删除的用户对应的复选框。 例如，选择“Chris Green”。

    提示 - 通过从列表中选择用户可以同时管理多个用户。 如果选择某个用户，打开该用户的边栏选项卡后，你将只能管理这一个用户。

    ![屏幕图像显示“所有用户”用户列表，其中有一个用户复选框处于选中状态，另一个复选框突出显示，指示能够从列表中选择多个用户。](./media/lp1-mod2-remove-user.png)

4. 选中用户帐户后，在菜单上选择“删除用户”。

5. 查看对话框，然后选择“确定”。

#### <a name="task-2---restore-a-deleted-user"></a>任务 2 - 还原已删除的用户

1. 在“用户”边栏选项卡的左侧导航栏中，选择“已删除的用户”。

2. 查看已删除用户的列表，并选择刚删除的用户。

    重要说明 - 默认情况下，已删除的用户帐户会在 30 天后自动从 Azure Active Directory 中永久删除。

3. 在菜单上，选择“还原用户”。

4. 查看对话框，然后选择“确定”。

5. 在左侧导航栏中，选择“所有用户”。

6. 验证用户是否已还原。
