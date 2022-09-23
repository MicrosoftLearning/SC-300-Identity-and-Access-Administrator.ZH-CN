---
lab:
  title: 10 - 适用于 Windows 和 Linux 虚拟机的 Azure AD 身份验证
  learning path: "02"
  module: Module 02 - Implement an Authentication and Access Management Solution
ms.openlocfilehash: 82dcb43bdae3df54a80625dc29134d4497207d8c
ms.sourcegitcommit: 80c5c0ef60c1d74fcc58c034fe6be67623013cc0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2022
ms.locfileid: "146823187"
---
# <a name="lab-10---azure-ad-authentication-for-windows-and-linux-virtual-machines"></a>实验室 10 - 适用于 Windows 和 Linux 虚拟机的 Azure AD 身份验证

注意 - 此实验室需要 Azure Pass。 有关说明，请参阅实验室 00。

## <a name="lab-scenario"></a>实验室方案

该公司决定使用 Azure Active Directory 登录虚拟机进行远程访问。  本实验室将演示如何为 Windows 和 Linux 虚拟机设置此方案。

#### <a name="estimated-time-30-minutes"></a>预计用时：30 分钟

### <a name="exercise-1---login-to-windows-virtual-machines-in-azure-with-azure-ad"></a>练习 1 - 使用 Azure AD 登录到 Azure 中的 Windows 虚拟机

#### <a name="task-1---create-a-windows-virtual-machine-with-azure-ad-login-enabled"></a>任务 1 - 创建启用了 Azure AD 登录的 Windows 虚拟机

1. 浏览到 [https://portal.azure.com](https://portal.azure.com)

1. 选择“+ 创建资源”。 

1. 在“搜索市场”搜索栏中键入“Windows Server”。

1. 选择“Windows Server”，然后从“选择软件计划”下拉列表中选择“Windows Server 2019 Datacenter”。

1. 必须在“基本信息”选项卡上为 VM 创建管理员用户名和密码。

1. 在“管理”选项卡上，选中“Azure AD”部分下的“使用 Azure AD 登录”框。

1. 确保选中“标识”部分下的“系统分配的托管标识”。 使用 Azure AD 启用登录后，此操作将自动执行。

1. 完成创建虚拟机的其余体验。 

1. 选择“创建”。

#### <a name="task-2---azure-ad-login-for-existing-azure-virtual-machines"></a>任务 2 - 现有 Azure 虚拟机的 Azure AD 登录

1. 浏览到 [https://portal.azure.com](https://portal.azure.com) 中的 **虚拟机**。

1. 选择“访问控制 (IAM)”。

1. 依次选择“添加”和“添加角色分配”，以打开“添加角色分配”页面 。

1. 分配以下角色。 
    - **角色**：虚拟机管理员登录或虚拟机用户登录
    - **分配访问权限对象**：用户、组、服务主体或托管标识

1. 有关详细步骤，请参阅使用 Azure 门户分配 Azure 角色。

### <a name="exercise-2---login-to-linux-virtual-machines-in-azure-with-azure-ad"></a>练习 2 - 使用 Azure AD 登录到 Azure 中的 Linux 虚拟机

#### <a name="task-1---create-a-linux-vm-with-system-assigned-managed-identity"></a>任务 1 - 创建具有系统分配的托管标识的 Linux VM

1. 浏览到 [https://portal.azure.com](https://portal.azure.com)

1. 选择“+ 创建资源”。 

1. 在“热门”视图中的“Ubuntu Server 18.04 LTS”下，选择“创建” 。

1. 在“管理”选项卡上，选中框以启用“使用 Azure Active Directory 登录(预览)” 。

1. 确保选中“系统分配的托管标识”。

1. 完成创建虚拟机的其余体验。 在此预览版中，必须使用用户名和密码或 SSH 公钥创建管理员帐户。

#### <a name="task-2---azure-ad-login-for-existing-azure-virtual-machines"></a>任务 2 - 现有 Azure 虚拟机的 Azure AD 登录

1. 浏览到 [https://portal.azure.com](https://portal.azure.com) 中的 **虚拟机**。

1. 选择“访问控制 (IAM)”。

1. 选择“添加”>“添加角色分配”，打开“添加角色分配”页面。

1. 分配以下角色。 
    - **角色**：虚拟机管理员登录或虚拟机用户登录
    - **分配访问权限对象**：用户、组、服务主体或托管标识

1. 有关详细步骤，请参阅使用 Azure 门户分配 Azure 角色。
