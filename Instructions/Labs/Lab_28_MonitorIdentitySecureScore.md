---
lab:
  title: 28 - 使用标识安全分数监视和管理安全状况
  learning path: "04"
  module: Module 04 - Plan and Implement and Identity Governance Strategy
ms.openlocfilehash: 14a8a2a123429f44b84ea45284f34a04bac6aa81
ms.sourcegitcommit: b5fc07c53b5663eaa1883cf38b70c57cd88470ca
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2022
ms.locfileid: "146741627"
---
# <a name="lab-28---monitor-and-managed-security-posture-with-identity-secure-score"></a>实验室 28 - 使用标识安全分数监视和管理安全状况

## <a name="lab-scenario"></a>实验室方案

Azure AD 标识保护为基于标识的风险提供自动检测和修正，并在门户中提供数据以调查潜在风险。 Azure AD 标识保护还提供标识安全分数来监视和改进标识安全状况。  与 Microsoft 365 Defender 和 Microsoft Defender for Cloud 相同，标识安全分数提供了改进操作和建议，可改进 Azure Active Directory 中标识的总体安全状况。  本实验室将探讨此功能。 

#### <a name="estimated-time-15-minutes"></a>预计用时：15 分钟

### <a name="exercise-1---using-identity-secure-score-to-monitor-and-manage-identity-security-posture"></a>练习 1 - 使用标识安全分数监视和管理标识安全状况

#### <a name="task-1---review-identity-secure-score-and-improvement-actions"></a>任务 1 - 查看标识安全功能分数和改进操作

1. 以全局管理员身份登录 [https://portal.azure.com](https://portal.azure.com)。

1. 搜索并选择“Azure AD 标识保护”。

1. 在“概述”磁贴中，你将找到“标识安全分数” 。

1. 选择“标识安全分数”。  此时会转到“标识安全分数”仪表板。

1. 向下滚动以“改进操作”。

1. 与 Microsoft Defender for Cloud 和 Microsoft 365 Defender 中的改进操作相比，这些改进操作特定于标识。  这为安全状况管理提供了更集中的潜在操作列表。  由此列表发起的任何改进操作也将对整体租户安全状况产生影响。 

#### <a name="task-2---execute-an-improvement-action"></a>任务 2 - 执行改进操作

1. 若要改进标识安全状况的一个区域，请选择“启用登录风险策略”。

1. 在打开的磁贴中，向下滚动并选择“开始使用”。

1. 将为“标识保护 | 登录风险策略”打开一个新选项卡。

1. 选择“分配”下的“所有用户” 。

1. 在“登录风险”下，选择“中等及以上” 。

1. 在“控件”下选择“允许” - “需要多重身份验证”  。

1. 将“强制执行策略”设为“开启”，然后选择“保存”  。

1. 你已经创建了一个登录风险策略，你的标识安全分数应会相应提高。  最多需要 24 小时才能反馈在标识安全分数中。

1. 查看其他改进操作以及创建和启用这些操作的步骤。


