---
lab:
  title: 15 - 配置 Azure AD 多重身份验证注册策略
  learning path: "02"
  module: Module 02 - Implement an Authentication and Access Management Solution
ms.openlocfilehash: 36e8f1cde0636008adc3dd390d90ea320af53f03
ms.sourcegitcommit: 80c5c0ef60c1d74fcc58c034fe6be67623013cc0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2022
ms.locfileid: "146823211"
---
# <a name="lab-15---configure-an-azure-ad-multi-factor-authentication-registration-policy"></a>实验室 15 - 配置 Azure AD 多重身份验证注册策略

## <a name="lab-scenario"></a>实验室方案

Azure AD 多重身份验证提供了一种除使用用户名和密码以外的方式来验证你的身份。 它为用户登录提供了附加的安全层。为了使用户能够响应 MFA 提示，他们必须首先注册 Azure AD 多重身份验证。 你需要将 Azure AD 组织的 MFA 注册策略配置为分配给所有用户。

#### <a name="estimated-time-10-minutes"></a>预计用时：10 分钟

### <a name="exercise-1---set-up-mfa-registration-policy"></a>练习 1 - 设置 MFA 注册策略

#### <a name="task-1---policy-configuration"></a>任务 1 - 策略配置

1. 使用全局管理员帐户登录到 [https://portal.azure.com]( https://portal.azure.com)。

2. 打开门户菜单，然后选择“Azure Active Directory”。

3. 在“Azure Active Directory”页的“管理”下，选择“安全性” 。

4. 在“安全性”页的左侧导航栏中，选择“标识保护”。

5. 在“标识保护”页的左侧导航栏中，选择“MFA 注册策略”。

    ![显示“MFA 注册策略”页的屏幕图像，其中突出显示了浏览路径](./media/lp2-mod4-browse-to-mfa-registration-policy.png)

6. 在“分配”下

7. 在“分配”下，选择“所有用户”，查看可用选项。

8. 你可以从“所有用户”中进行选择，如果部署受到限制，则可以“选择个人和组”。

9. 此外，还可以选择从策略中排除用户。

10. 在“控制”下，注意“需要 Azure AD MFA 注册”处于选中状态且无法更改。

11. 在“强制执行策略”下，选择“启用”，然后选择“保存”。

#### <a name="task-2---configure-azure-ad-identity-protection-policy-for-mfa-registration"></a>任务 2 - 为 MFA 注册配置 Azure AD 标识保护策略

**注意**：Azure AD 标识保护需要激活 Azure AD Premium P2。 

1. 在 Azure 门户中，导航到搜索栏中的“Azure AD 标识保护”。

1. 在菜单中的“保护”下，选择“MFA 注册策略” 。

1. 在“分配”下，选择“用户”下的“所有用户”，然后选择一个用户来强制实施 MFA 。

1. 将“强制实施策略”策略从“关闭”更改为“打开”  。

1. 选择“保存”。 

这将要求用户在下次尝试登录时完成 MFA 注册。

1. 从专用浏览器导航到 <login.microsoftonline.com>。 输入租户的用户名和密码。  请注意要求用户输入的附加安全信息要求。