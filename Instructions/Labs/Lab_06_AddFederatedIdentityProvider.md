---
lab:
  title: 06 - 添加联合标识提供者
  learning path: "01"
  module: Module 01 - Implement an identity management solution
ms.openlocfilehash: da25fe1a291e37a641448b8e4587352989993c96
ms.sourcegitcommit: 80c5c0ef60c1d74fcc58c034fe6be67623013cc0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2022
ms.locfileid: "146823196"
---
# <a name="lab-06-add-a-federated-identity-provider"></a>实验室 6：添加联合标识提供者

## <a name="lab-scenario"></a>实验室方案

你的公司与许多供应商合作，有时你需要将一些供应商帐户作为来宾添加到你的目录并允许他们使用其 Google 帐户登录。

#### <a name="estimated-time-25-minutes"></a>预计用时：25 分钟

### <a name="exercise-1---configure-identity-providers"></a>练习 1 - 配置标识提供者

#### <a name="task-1---configure-google-to-be-used-as-an-identity-provider"></a>任务 1 - 将 Google 配置为标识提供者

注意 - 本练习需要 Google 上的 Gmail 帐户。 创建新的 Google 帐户，然后按照练习的步骤操作。

1. 转到 Google API (https://console.developers.google.com )，并使用 Google 帐户登录。 我们建议使用共享团队 Google 帐户。

1. 如果系统提示接受服务条款，请接受它。

1. 创建新项目：在页面顶部，选择项目菜单以打开“选择项目”页。 选择“新建项目”。  将其余字段保留为默认设置。

1. 在“新建项目”页上，为项目指定名称（例如 MyB2BApp），然后选择“创建” 。

1. 通过选择“通知”消息框中的链接，或通过使用页面顶部的项目菜单，打开该新项目。

1. 在左侧菜单中，选择“API 和服务”，然后选择“OAuth 同意屏幕” 。

1. 在“用户类型”下，选择“外部”，然后选择“创建” 。

1. 在“OAuth 同意屏幕”上的“应用信息”下，输入应用名称，例如 Azure AD 。

1. 在“用户支持电子邮件”下，选择电子邮件地址。 这应包括用于登录 Google 的电子邮件地址。

1. 在“已授权域”下，选择“添加域”，然后添加 microsoftonline.com 域。

   ```
   microsoftonline.com
   ```

1. 在“开发人员联系信息”下，输入用于登录门户的实验室帐户的电子邮件地址。

1. 选择“保存并继续”。 

1. 在左侧菜单中，选择“凭据”。

1. 选择“+ 创建凭据”，然后选择“OAuth 客户端 ID” 。

1. 在“应用程序类型”菜单中，选择“Web 应用程序”。 为应用程序指定合适的名称，例如 Azure AD B2B。 在“已授权的重定向 URI”下，添加以下 URI：

   ```
       - https://login.microsoftonline.com
       - https://login.microsoftonline.com/te/**tenant ID**/oauth2/authresp
       (where <tenant ID> is your tenant ID)
       - https://login.microsoftonline.com/te/**tenant name**.onmicrosoft.com/oauth2/authresp
       (where <tenant name> is your tenant name)
   ```

1. 选择“创建”。 复制你的客户端 ID 和客户端密码 。 你将在 Azure 门户中添加标识提供者时用到它们。

1. 可以将项目保留为“测试”的发布状态，并将测试用户添加到 OAuth 同意屏幕。 也可以选择 OAuth 同意屏幕上的“发布应用”按钮，以便让任何拥有 Google 帐户的用户都可以使用该应用。

#### <a name="task-2---configure-azure-ad-for-google-federation"></a>任务 2 - 为 Google 联合配置 Azure AD

1. 以拥有受限的管理员目录角色或来宾邀请者角色的用户身份登录 [https://portal.azure.com](https://portal.azure.com)。

1. 选择“Azure Active Directory” 。

1. 在“管理”下，选择“外部标识” 。

1. Microsoft 为 Google 标识提供者提供直接联合身份验证。  可通过从“外部标识”|“所有标识提供者”页选择 + Google 来启动此操作 
 
1. 选择 + Google 后，将打开另一个页面，其中包含将 Google 配置为标识提供者所需的其他信息。  

1. 输入之前获得的客户端 ID 和客户端密码。 选择“保存”。

1. 这将完成将 Google 配置为标识提供者的过程。

注意 - 你可能会收到无法将 Google 添加为社交标识提供者的消息。  导航出该屏幕并返回到“外部标识”，Google 应会在列表中列出。

1. 如果使用了现有的 Gmail 帐户，请记得通过“外部标识”|“所有标识提供者”删除该帐户。 还可返回到 Google 开发人员控制台并删除所创建的项目。

   注意 - 可通过此链接完成查找 Google 客户端 ID 和客户端密码的步骤。
   https://docs.microsoft.com/azure/active-directory/external-identities/google-federation

   注意 - Facebook 也可轻松配置为标识提供者。 可在以下位置访问这些步骤： https://docs.microsoft.com/azure/active-directory/external-identities/facebook-federation 。 如果你更喜欢使用 Facebook 帐户而不是 Google 进行本练习，则可以完成此选项。

1. 设置完成后，可打开专用浏览器并输入以下 Web 地址：

   ```
   login.microsoftonline.com
   ```

1. 输入创建的 Google 电子邮件地址和密码。  此时应会进入 Microsoft 联机门户以访问应用。
