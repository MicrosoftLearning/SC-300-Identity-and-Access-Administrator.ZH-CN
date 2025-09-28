---
lab:
  title: 11 - 在 Privileged Identity Management 中分配 Azure 资源角色
  learning path: '02'
  module: Module 02 - Implement an authentication and access management solution
---

# 实验室 11 - 在 Privileged Identity Management 中分配 Azure 资源角色

### 登录类型 = Azure 资源登录

## 实验室方案

Microsoft Entra Privileged Identity Management (PIM) 可以管理内置的 Azure 资源角色以及自定义角色，包括（但不限于）：

- “所有者”
- 用户访问管理员
- 参与者
- 安全管理员
- 安全管理器

你需要使用户符合 Azure 资源角色的条件。

#### 预计用时：10 分钟

### 练习 1 - 针对 Azure 资源的 PIM

#### 任务 1 - 分配 Azure 资源角色

1. 使用提供的管理员帐户登录到 [https://entra.microsoft.com](https://entra.microsoft.com)。

2. 搜索并选择“Microsoft Entra Privileged Identity Management”****。

3. 在“Privileged Identity Management”页的左侧导航窗格中选择“Azure 资源”。

**实验室提示** - 为旧版 Azure 资源体验编写后续几个步骤。  你可以切换到屏幕顶部的旧版体验。 或者，可以在新体验中完成练习，而无需分步完成。

4. 在“订阅”下拉列表中，选择 MOC 订阅 ##### 项。 然后在屏幕底部，选择“**管理资源**”。

5. 在“Azure 资源 - 发现”页中，选择订阅。

6. 在“概述”**** 页中，查看信息。

   ![显示最近添加的 Azure 资源的屏幕图像](./media/lp4-mod3-pim-az-resource-overview.png)

   **实验室提示** - 由于实验室环境的性质，你看不到任何资源。 有关示例，请参阅图片。

7. 在左侧导航菜单中，在“管理”下选择“角色”以查看适用于 Azure 资源的角色列表。

8. 在顶部菜单中，选择“+ 添加分配”。

9. 在“添加分配”页中，选择“选择角色”菜单，然后选择“API 管理服务参与者” 。

10. 在“选择成员”下，选择“未选择任何成员”。

11. 在“选择成员或组”中，搜索组织中将被分配角色的管理员角色 **User1-######@LODSPRODMCA.onmicrosoft.com**。  然后选择“选择”。

12. 选择“**下一页**”。

13. 在“设置”选项卡的“分配类型”下，选择“符合条件”。

   - “合格”  分配要求该角色的成员执行某个操作才能使用该角色。 操作可能包括执行多重身份验证 (MFA) 检查、提供业务理由或请求获得指定审批者的批准。

   - “活动”分配不要求成员执行任何操作便可使用该角色。 分配为“活动”的成员拥有始终分配给该角色的特权。

14. 通过更改开始和结束日期和时间来指定分配持续时间。

15. 完成后，选择“分配”。

16. 创建新的角色分配后，会显示状态通知。

#### 任务 2 - 更新或删除现有资源角色分配

**备注** - 由于在此实验室环境中强制实施安全性，因此无法完成这些步骤。  请查看用户界面中的步骤，但无法应用更改。  我们正在积极研究解决这一问题的办法。

按照以下步骤更新或删除现有的角色分配。

1. 打开 Microsoft Entra Privileged Identity Management。

2. 选择“Azure 资源”****。

3. 选择要管理的订阅，打开其概述页。

4. 在“管理”下选择“分配” 。

5. 在“符合条件的分配”**** 选项卡的“操作”列中，查看可用选项。

6. 选择“删除” 。

7. 在“删除”对话框中，查看信息，然后选择“是”。
