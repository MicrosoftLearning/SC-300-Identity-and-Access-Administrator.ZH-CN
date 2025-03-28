---
lab:
  title: 20 - 实现应用的访问管理
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# 实验室 20 - 实现应用的访问管理

### 登录类型 = Microsoft 365 管理

## 实验室方案

你的组织要求仅特定用户或组有权访问企业应用程序。 你需要将用户分配到特定应用程序。

#### 预计用时：5 分钟

### 练习 1 - 配置企业应用

#### 任务 1 - 将应用添加到 Microsoft Entra 租户

1. 使用提供的管理员帐户登录到  [https://entra.microsoft.com](https://entra.microsoft.com) 。

2. 查看屏幕左侧的菜单。

3. 在“标识”菜单上的“应用程序”**** 下，选择“企业应用程序”****。

4. 在“企业应用程序”窗格中，选择“+ 新建应用程序”。

    ![企业应用程序页的屏幕图像，其中突出显示了“新建应用程序”](./media/lp3-mod1-new-enterprise-application.png)

5. 在“浏览 Microsoft Entra 库”页的“搜索应用程序”**** 框中，输入“GitHub”****。

    ![显示“浏览 Microsoft Entra 库”页的屏幕图像，其中突出显示了搜索框](./media/lp3-mod1-azure-ad-gallery-search.png)

6. 在结果中，选择“GitHub Enterprise Cloud – 企业帐户”。

7. 在“GitHub Enterprise Cloud – 企业帐户”中，检查设置，然后选择“创建”。

8. 创建帐户后，你将被重定向到“GitHub Enterprise Cloud – 企业帐户”页。

#### 任务 2 - 向应用分配用户

1. 在“GitHub Enterprise Cloud - 企业帐户”页的“概述”页面的“开始”下，选择“1. 分配用户和组”。

2. 或者，可以在左侧导航的“管理”下，选择“用户和组”。

3. 在“用户和组”页面的菜单上，选择“+ 添加用户/组”****。

4. 在“添加分配”页面上的“用户和组”**** 部分中，选择“未选择任何内容”****。

5. 在“用户和组”窗格中，选择 Adele Vance 和你的 MOD 管理员帐户。

6. 选择“选择”  。

    ![显示如何将用户帐户分配给应用的屏幕图像，其中突出显示了“选择”按钮 ](./media/lp3-mod1-add-app-assignment.png)

7. 选择“分配”。

