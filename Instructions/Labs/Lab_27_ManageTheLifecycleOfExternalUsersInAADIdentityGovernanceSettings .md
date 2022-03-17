---
lab:
  title: 27 - 使用 Azure AD Identity Governance 设置来管理外部用户的生命周期
  learning path: "04"
  module: Module 04 - Plan and Implement and Identity Governance Strategy
ms.openlocfilehash: de71fb2cd16bc2dbaf0d28ff9325dc5d342597ef
ms.sourcegitcommit: 448f935ad266989a6f0086019e0c0e0785ad162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2022
ms.locfileid: "138421435"
---
# <a name="lab-27-manage-the-lifecycle-of-external-users-in-azure-ad-identity-governance-settings"></a>实验室 27：使用 Azure AD Identity Governance 设置来管理外部用户的生命周期  

## <a name="lab-scenario"></a>实验室方案

你可以选择当外部用户（已通过正在审批的访问包请求邀请到你的目录的用户）不再有任何访问包分配时将发生什么情况。 如果用户放弃其所有访问包分配，或者其最后一个访问包分配过期，则可能会发生这种情况。 默认情况下，当外部用户不再有任何访问包分配时，系统会阻止其登录到你的目录。 30 天后，系统会从你的目录中删除外部用户的来宾用户帐户。

#### <a name="estimated-time-5-minutes"></a>预计用时：5 分钟

### <a name="exercise-1---azure-ad-identity-governance-settings"></a>练习 1 - Azure AD Identity Governance 设置

#### <a name="task-1---manage-the-lifecycle-of-external-users-in-azure-ad-identity-governance-settings"></a>任务 1 - 使用 Azure AD Identity Governance 设置来管理外部用户的生命周期

1. 以全局管理员身份登录 [https://portal.azure.com](https://portal.azure.com)。

2. 具有全局管理员或用户管理员角色的帐户才能完成这些任务。

3. 打开 Azure Active Directory，然后选择“Identity Governance”。

4. 在左侧导航菜单的“权利管理”下，选择“设置”。

5. 在顶部菜单中，选择“编辑”。

    ![屏幕图像显示“Identity Governance 设置”页，其中突出显示“管理外部用户的生命周期”。](./media/lp4-mod1-manage-lifcycle-of-ext-users.png)

6. 在“管理外部用户的生命周期”部分中，查看外部用户的其他设置。

7. 当外部用户失去其对任何访问包的最后一个分配时，如果想要阻止其登录到此目录，请将“阻止外部用户登录到此目录”设置为“是”。

8. 如果阻止用户登录到目录，则用户将无法在此目录中重新请求访问包或请求其他访问权限。 如果用户以后需要请求访问其他访问包，请不要通过配置阻止其登录。

9. 当外部用户失去其对任何访问包的最后一个分配时，如果想要删除其在此目录中的来宾用户帐户，请将“删除外部用户”设置为“是”。

    备注 - 使用“权利管理”，仅可删除通过“权利管理”邀请的帐户。 另请注意，系统会阻止用户登录并从此目录中删除用户，即使已将该用户添加到此目录中不是访问包分配的资源。 如果来宾在接收访问包分配之前已存在于此目录中，则会将其保留。 但是，如果是通过访问包分配邀请的来宾，并且在邀请之后还将其分配给了 OneDrive for Business 或 SharePoint Online 站点，则仍会将其删除。

10. 如果要删除此目录中的来宾用户帐户，可以设置一个天数，该天数过后即可将其删除。 如果要在来宾用户帐户丢失其对任何访问包的最后一个分配时立即将其删除，请将“从此目录中删除外部用户之前需等待的天数”设置为“0”。 

11. 如果进行了任何更改，请选择“保存”。
