---
lab:
  title: 16 - 配置身份验证会话控件
  learning path: "02"
  module: Module 02 - Implement an Authentication and Access Management Solution
ms.openlocfilehash: 8fadbe8fc6c26f79f2f75efae07e90cae714088b
ms.sourcegitcommit: 448f935ad266989a6f0086019e0c0e0785ad162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2022
ms.locfileid: "138421360"
---
# <a name="lab-16---configure-authentication-session-controls"></a>“实验室 16 - 配置身份验证会话控件”

## <a name="lab-scenario"></a>实验室方案

作为你所在公司的大型安全配置的一部分，你需要测试可用于控制登录频率的条件访问策略。

#### <a name="estimated-time-10-minutes"></a>预计用时：10 分钟

### <a name="exercise-1---configure-sign-in-frequency-controls-using-a-conditional-access-policy"></a>练习 1 - 使用条件访问策略配置登录频率控制

#### <a name="task---user-the-azure-portal-to-configure-conditional-access"></a>任务 - 使用 Azure 门户配置条件访问

1. 浏览到 [https://portal.azure.com](https://portal.azure.com)，使用目录的全局管理员帐户登录。

2. 打开门户菜单，然后选择“Azure Active Directory”。

3. 在“Azure Active Directory”边栏选项卡的“管理”下，选择“安全性”。

4. 在“安全性”边栏选项卡的左侧导航栏中，选择“条件访问”。

5. 在顶部菜单中选择“+ 新建策略”，然后从下拉列表中选择“创建新策略” 。

    ![显示“条件访问”边栏选项卡的屏幕图像，其中突出显示了“新建策略”](./media/lp2-mod1-conditional-access-new-policy.png)

6. 在“名称”框中，输入“登录频率”。

7. 在“分配”下，选择“用户或工作负载标识” 。

8. 在“包括”选项卡上，选中“用户和组”复选框。

9. 在“选择”窗格中，选择“Grady Archie”帐户，然后选择“选择” 。

10. 选择“云应用或操作”。

11. 确认已选择“云应用”，然后选择“选择应用”。

12. 在“选择”窗格中，选择“Office 365”，然后选择“选择”。

13. 在“访问控制”下，选择“会话”。

14. 在“会话”窗格中，选择“登录频率”。

15. 在值框中，输入“30”。

16. 依次选择单位菜单、“天数”，然后选择“选择”。

17. 在“启用策略”下，选择“仅限报告”，然后选择“创建”。

    ![显示新的条件访问策略的屏幕图像，其中突出显示了策略设置](./media/lp2-mod3-create-session-conditional-access-policy.png)

   备注 - 仅限报告模式是一种新的条件访问策略状态，允许管理员在其环境中启用条件访问策略之前评估该策略的影响。 随着仅限报告模式的发布：
    
    - 可以在仅限报告模式下启用条件访问策略。
    - 在登录过程中，将评估仅限报告模式下的策略，但不强制执行这些策略。
    - 结果记录在登录日志详细信息的“条件访问”和“仅限报告”选项卡中。
    - 具有 Azure Monitor 订阅的客户可以使用条件访问见解工作簿来监视其条件访问策略的影响。
