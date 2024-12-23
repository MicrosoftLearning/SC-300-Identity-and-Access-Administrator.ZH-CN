---
lab:
  title: 09 - 启用 Microsoft Entra 自助式密码重置
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# 实验室 09 - 启用 Azure AD 自助式密码重置

### 登录类型 = Microsoft 365 管理

## 实验室方案

公司已决定为员工提供支持，并启用自助式密码重置。 你需要在组织中配置此设置。

#### 预计用时：15 分钟

### 练习 1 - 创建启用 SSPR 的组并添加用户

#### 任务 1 - 创建要将 SSPR 分配到其中的组

需要先向人数有限的一组用户推送 SSPR，以确保 SSPR 配置按预期工作。 让我们创建用于受限推出的安全组，并将用户添加到该组。

1. 在 Microsoft Entra 管理中心左侧，打开“标识”**** 导航菜单。
1. 在“组”**** 下，选择“所有组”****，然后在右侧窗口上选择“新建组”****。

2. 使用以下信息创建新组：

    | **设置**| **值**|
    | :--- | :--- |
    | 组类型| 安全|
    | 组名称| SSPRTesters|
    | 组说明| SSPR 推送活动的测试人员|
    | 成员身份类型| 已分配|
    | 成员| Alex Wilber |
    | |  Allan Deyoung |
    | | Bianca Pisani |
  
    
3. 选择“创建” **** 。

    ![显示“新建组”页的屏幕图像，其中突出显示了“组类型”、“组名称”和“创建”](./media/lp2-mod2-create-sspr-security-group.png)

#### 任务 2 - 为测试组启用 SSPR

为组启用 SSPR。

1. 浏览回“标识”**** 导航菜单。

2. 在“保护” **** 下，选择“密码重置” ****。

3. 在“密码重置”页的“属性”页上，在“已启用自助式密码重置”下，选择“选定” **** 。

4. 在“**选择组**”下，将现有的 SSPRSecurityGroupUsers 替换为刚刚创建的 **SSPRTesters**。

5. 在“密码重置”页的“属性”页上，选择“保存” **** 。

    ![显示密码重置属性页的屏幕图像，其中突出显示了“选定”、“选择组”和“保存”](./media/lp2-mod2-enable-password-reset-for-selected-group.png)

6. 在“密码重置”**** 屏幕上，查看 **管理*，选择并查看每个 **身份验证方法 **、 **注册**、通知**** 和自定义**** 设置的默认值。

    备注：虽然选择“电话”作为本实验室其余部分的身份验证方法很重要，但也可使用其他选项 。

#### 任务 3 - 使用 Allan 注册 SSPR

SSPR 配置完成后，可以为创建的用户注册移动电话号码。

1. 打开其他浏览器或打开 InPrivate 或 Incognito 浏览器会话，然后浏览到 [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)。

    这是为了确保系统能提示你进行用户身份验证。

2. 使用提供的密码以 **AllanD@**`<<organization-domain-name>>.onmicrosoft.com` 身份登录。

    备注 - 将 organization-domain-name 替换为自己的域名。

3. 如果系统提示更新密码，请输入所选的新密码。 请确保记录下新密码。

4. 如果系统提示保持登录状态，请选择“是”。

5. 在“需要更多信息”对话框中，选择“下一步”。

6. 在“保持帐户安全”页上，选择“**下一步**”以使用 Authenticator 应用。

7. 按照屏幕上的说明扫描 QR 码，在 Authenticator 中设置账户。

8. 注册成功后，选择“**完成**”，完成注册过程。

  - **备注** - 此时，已通过单个步骤注册 SSPR 和 MFA。

11. 关闭浏览器。 无需完成登录过程。

#### 任务 4 - 测试 SSPR

现在来测试用户是否可以重置其密码。

1. 打开其他浏览器或打开 InPrivate 或 Incognito 浏览器会话，然后浏览到  [https://portal.azure.com](https://portal.azure.com)。

    这是为了确保系统能很好地提示你进行用户身份验证。

2. 输入 AlexW@ `<<organization-domain-name>>.onmicrosoft.com`，然后选择“下一步” 。

    备注 - 将 organization-domain-name 替换为自己的域名。

3. 在“输入密码”页上，选择“忘记密码”。

4. 在“重新登录帐户”页上，完成要求填写的信息，然后选择“下一步”。

5. 按照屏幕上的说明从 Microsoft Authenticator 应用获取验证码。

6. 输入验证码，然后选择“下一步”。

7. 在“选择新密码”步骤中，输入并确认新密码。

8. 完成后，选择“完成”****。

9. 使用创建的新密码以 **AllanD** 身份登录。

10. 输入验证码，然后验证是否可以完成登录过程。

11. 完成后，关闭浏览器。

#### 任务 5 - 如果尝试使用 SSPRTesters 组之外的用户会怎样？

1. 在测试中，打开一个新的 InPrivate 浏览器窗口并尝试以 GradyA 身份登录到 Azure 门户，然后选择“忘记密码”选项。
