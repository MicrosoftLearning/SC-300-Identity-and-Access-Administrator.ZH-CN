---
lab:
  title: 16 - 将 Azure Key Vault 用于托管标识
  learning path: "02"
  module: Module 02 - Implement an Authentication and Access Management Solution
ms.openlocfilehash: 8be1dddef68268c7da2dccc5e6e8e6830b311fb4
ms.sourcegitcommit: 80c5c0ef60c1d74fcc58c034fe6be67623013cc0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2022
ms.locfileid: "146823202"
---
# <a name="lab-16---using-azure-key-vault-for-managed-identities"></a>实验室 16 - 将 Azure Key Vault 用于托管标识

注意 - 此实验室需要 Azure Pass。 有关说明，请参阅实验室 00。

## <a name="lab-scenario"></a>实验室方案

使用 Azure 资源托管标识时，代码可以获取访问令牌，以针对支持 Azure AD 身份验证的资源进行身份验证。但是，并非所有 Azure 服务都支持 Azure AD 身份验证。 若要将 Azure 资源的托管标识用于这些服务，请将服务凭据存储在 Azure Key Vault 中，然后使用托管标识访问 Key Vault 以检索凭据。

#### <a name="estimated-time-20-minutes"></a>预计用时：20 分钟

### <a name="exercise-1---use-azure-key-vault-to-manage-virtual-machine-identities"></a>练习 1 - 使用 Azure Key Vault 管理虚拟机标识

#### <a name="task-1---create-a-windows-virtual-machine"></a>任务 1 - 创建 Windows 虚拟机

1. 浏览到 [https://portal.azure.com](https://portal.azure.com)

1. 选择“+ 创建资源”。 

1. 在“搜索市场”搜索栏中键入“Windows Server”。

1. 选择“Windows Server”，然后从“选择软件计划”下拉列表中选择“Windows Server 2019 Datacenter”。

1. 必须在“基本信息”选项卡上为 VM 创建管理员用户名和密码。

1. 在“管理”选项卡上，选中“Azure AD”部分下的“使用 Azure AD 登录”框。

1. 完成创建虚拟机的其余体验。 

1. 选择“创建”。

#### <a name="task-2---create-a-key-vault"></a>任务 2 - 创建密钥保管库

1. 使用全局管理员帐户登录到 [https://portal.azure.com]( https://portal.azure.com)。

1. 在左侧导航栏的顶部，选择“创建资源”

1. 在“搜索市场”框中，键入“Key Vault”。  

1. 从结果中选择“Key Vault”。

1. 选择“创建”。

1. 填写所有必需的信息，如下所示。 请确保选择用于本实验室的订阅。

 - **资源组** - sc300KeyVaultrg
 - **密钥保管库名称** - sc300keyvault

1. 选择“查看 + 创建”。

1. 选择“创建”。


#### <a name="task-3---create-a-secret"></a>任务 3 - 创建机密

1. 导航到新创建的密钥保管库。

1. 选择“机密”。

1. 选择“+ 生成/导入”。

1. 在“创建机密”屏幕的“上传选项”中，将“手动”保留为选中状态。

1. 输入密钥的名称和值。  该值可以是任何需要的内容。 

1. 明确指定激活日期和到期日期，并将“已启用”设置为“是”。 

1. 选择“创建”以创建机密。

#### <a name="task-4---grant-access-to-key-vault"></a>任务 4 - 授予对 Key Vault 的访问权限

1. 导航到新创建的密钥保管库

1. 在左侧菜单中选择“访问策略”。

1. 选择“添加访问策略”。

1. 在“从模板配置(可选)”下的“添加访问策略”部分的下拉菜单中选择“机密管理”。

1. 对于“选择主体”，选择“未选择任何内容”打开要选择的主体列表 。 在搜索字段中输入任务 2 中创建的 VM 的名称。  选择结果列表中的 VM，然后选择“选择”。

1. 选择“添加”  。

1. 选择“保存” 。

#### <a name="task-5---access-data-with-key-vault-secret-with-powershell"></a>任务 5 - 通过 PowerShell 使用 Key Vault 机密访问数据

1. 在实验室虚拟机中，打开 PowerShell。  

1. 在 PowerShell 中，调用租户上的 Web 请求，为 VM 特定端口中的本地主机获取令牌。  

    ```
    $Response = Invoke-RestMethod -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"}
    ```

1. 接下来，从响应中提取访问令牌。  

    ```
    $KeyVaultToken = $Response.access_token
    ```

1. 使用 PowerShell 的 Invoke-WebRequest 命令检索之前在 Key Vault 中创建的密钥，在授权标头中传递访问令牌。  将需要 Key Vault 的 URL，该 URL 位于 Key Vault 的“概述”页的“基本要素”部分。  

    ```
    Invoke-RestMethod -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}
    ```
1. 应收到类似于下面的响应： 
    ```
    'My Secret' https://mi-lab-vault.vault.azure.net/secrets/mi-test/50644e90b13249b584c44b9f712f2e51 @{enabled=True; created=16…
    ```
1. 此机密可用于对需要名称和密码的服务进行身份验证。
