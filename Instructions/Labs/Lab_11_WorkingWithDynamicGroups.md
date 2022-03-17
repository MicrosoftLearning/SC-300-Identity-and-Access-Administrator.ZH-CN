---
lab:
  title: 11 - 使用动态组
  learning path: "02"
  module: Module 02 - Implement an Authentication and Access Management Solution
ms.openlocfilehash: 51da3b93097f3e97d8b8936ee6ba2601834a5eec
ms.sourcegitcommit: 448f935ad266989a6f0086019e0c0e0785ad162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2022
ms.locfileid: "138421420"
---
# <a name="lab-11-working--with-dynamic-groups"></a>实验室 11：使用动态组

## <a name="lab-scenario"></a>实验室方案

随着公司发展，手动组管理正在变得过于耗时。 由于对目录进行了标准化，现在可以利用动态组。 必须创建新的动态组以确保准备好在生产环境中创建动态组。

#### <a name="estimated-time-10-minutes"></a>预计用时：10 分钟

### <a name="exercise-1---creating-a-dynamic-group-with-all-users-as-members"></a>练习 1 - 创建将所有用户添加为成员的动态组

#### <a name="task-1---create-the-dynamic-group"></a>任务 1 - 创建动态组

1. 使用在租户中分配了全局管理员或用户管理员角色的帐户登录到 [https://portal.azure.com](https://portal.azure.com)。

2. 选择“Azure Active Directory”  。

3. 在“管理”下选择“组”，然后选择“新建组”    。

4. 在“新建组”  页的“组类型”  下选择“安全性”  。

5. 在“组名”框中，输入“SC300-myDynamicGroup” 。

6. 选择“成员身份类型”菜单，然后选择“动态用户”。

7. 在“动态用户成员”下，选择“添加动态查询”。

8. 在“规则语法”框上方的右侧，选择“编辑”。

9. 在“编辑规则语法”窗格中的“规则语法”框中输入以下表达式：

    ```powershell
    user.objectid -ne null
    ```

    警告 - `user.objectid` 区分大小写。

10. 选择“确定”。 规则会出现在“规则语法”框中。

    ![显示“动态组成员资格规则”边栏选项卡的屏幕图像，其中突出显示了规则语法](./media/lp1-mod3-dynamic-group-membership-rule.png)

11. 选择“保存”  。 新的动态组现在将包含 B2B 来宾用户和成员用户。

12. 在“新建组”页上，选择“创建”以创建组。

#### <a name="task-2---verify-the-members-have-been-added"></a>任务 2 - 验证是否已添加成员

1. 单击“主页”`Azure Active Directory`.
2. 启动 Azure Active Directory。
3. 在“管理”菜单中单击“组” 。
4. 在筛选框中键入“SC300”，此时将列出新创建的组。
5. 单击“SC300-myDynamicGroup”以打开组。
6. 请注意，该组显示包含 30 多个直接成员。
7. 在“管理”菜单中单击“成员” 。
8. 查看成员。

#### <a name="task-3---experiment-with-alternate-rules"></a>任务 3 - 试验替代规则

1. 尝试创建仅包含来宾用户的组：
   - (user.objectid -ne null) 和 (user.userType -eq "Guest")

2. 尝试创建仅包含 Azure AD 用户成员的组。
   - (user.objectid -ne null) 和 (user.userType -eq "Member")
