---
lab:
  title: 14 - 启用 Azure AD 多重身份验证
  learning path: "02"
  module: Module 02 - Implement an Authentication and Access Management Solution
ms.openlocfilehash: 1561aad5cf589b73ffbb3592fb20e2910ddff9c6
ms.sourcegitcommit: 448f935ad266989a6f0086019e0c0e0785ad162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2022
ms.locfileid: "138421363"
---
# <a name="lab-14---working-with-security-defaults"></a>实验室 14 - 使用安全默认值

## <a name="lab-scenario"></a>实验室方案

你需要在组织中配置 Azure Active Directory 安全默认值设置。
    这是一个完全可选的实验室！  若只是为了查找菜单选项的位置，可以启用/禁用安全默认值。  但要记住的要点来自培训。  请注意，如果启用了安全默认值，但不禁用它，则涉及条件访问的后续实验室将无法使用。

#### <a name="estimated-time-7-minutes"></a>预计用时：7 分钟

### <a name="exercise---prework"></a>练习 - 准备

为了能够启用安全默认值，必须删除或禁用任何现有的条件访问策略。  提醒一下，在上一实验室中，我们构建了一个策略以对 Delia 强制实施 MFA。  需要先禁用该策略，然后才能执行以下步骤。

1. 登录到 Azure 门户。
2. 打开 Azure Active Directory。
3. 从菜单的“安全性”部分选择“安全性”，然后选择“条件访问” 。
4. 单击设置为“打开”或“仅限报告”的任何条件访问策略，并将其更改为“关闭”。

### <a name="exercise-1---enabling-security-defaults"></a>练习 1 - 启用安全默认值

#### <a name="task-1---turn-on-security-defaults"></a>任务 1 - 启用安全默认值

若要在目录中启用安全默认值：

1. 浏览到 [https://portal.azure.com](https://portal.azure.com)，使用目录的全局管理员帐户登录。

2. 选择“显示门户菜单”汉堡图标，然后选择“Azure Active Directory”。

    ![Azure 门户菜单，其中选择了“Azure Active Directory”](./media/azure-portal-menu-aad.png)

3. 在左侧导航栏的“管理”部分中，选择“属性”。

4. 在“属性”边栏选项卡底部，选择“管理安全默认值”。

5. 将“启用安全默认值”切换键设置为“是”。

6. 可能已启用此选项。

7. 选择“保存”。

#### <a name="task-2---disabling-security-defaults"></a>任务 2 - 禁用安全默认值

选择实施条件访问策略来取代安全默认值的组织必须禁用安全默认值。

若要在目录中禁用安全默认值：

1. 浏览到 [https://portal.azure.com](https://portal.azure.com/)，使用目录的全局管理员帐户登录。

2. 选择“显示门户菜单”汉堡图标，然后选择“Azure Active Directory”。

3. 在“属性”边栏选项卡底部，选择“管理安全默认值”。

4. 将“启用安全默认值”切换键设置为“否”。

    ![“安全默认值”处于禁用状态的屏幕图像，并且已选择所需的禁用原因。 在本例中，组织将使用条件访问。](./media/security-defaults-disable-before-conditional-access.png)

5. 选择“保存”。
