---
lab:
  title: 25 - 为内部和外部用户创建访问评审
  learning path: "04"
  module: Module 04 - Plan and Implement and Identity Governance Strategy
ms.openlocfilehash: c5207f3a8886c28390b38d3e1387168ca1d65021
ms.sourcegitcommit: b5fc07c53b5663eaa1883cf38b70c57cd88470ca
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2022
ms.locfileid: "146741629"
---
# <a name="lab-25---creating-access-reviews-for-internal-and-external-users"></a>实验室 25 - 为内部和外部用户创建访问评审  

## <a name="lab-scenario"></a>实验室方案

应以类似的方式定期查看特权用户访问。  由于这些是提升的访问权限分配，因此应根据公司确定的一致基础对这些内容进行评审。  应删除未使用和不必要的特权分配。  还应为已离职或已在公司内更换部门的用户配置自动删除。

#### <a name="estimated-time-5-minutes"></a>预计用时：5 分钟

### <a name="exercise-1---create-an-internal-access-review"></a>练习 1 - 创建内部访问评审

#### <a name="task---create-a-new-access-review"></a>任务 - 创建新的访问评审

1. 以全局管理员身份登录 [https://portal.azure.com](https://portal.azure.com)。

2. 访问评审可以管理访问生命周期。  在“Azure Active Directory”中，在“管理”菜单下查找“Identity Governance”  。  在“标识治理”中，选择“访问评审” 。

3. 命名访问评审并设置开始日期和频率。 持续时间是访问评审在过期之前运行的时间。  可以设置为在特定日期或执行次数后结束。  然后选择访问评审的用户范围。

4. 选择将属于此作用域的角色的范围。  可以添加所有角色，也可以仅选择特定角色进行某些评审。 

5. 下一步是确定审阅者。  这些审阅者可以是进行自我评审的成员自身，如果评审整个部门的访问权限，也可以分配给主管。 还可以设置在审阅者未响应时自动从成员中删除该特权访问的操作。

6. 高级设置允许将消息作为评审的一部分。  选择“开始”以启动访问评审。

7. 创建访问评审后，访问评审列表将填充评审的角色和所有者。

8. 接受评审的成员将在评审开始时收到一封电子邮件。

9. 选择其中一个角色的访问评审将提供这些访问评审的状态。

