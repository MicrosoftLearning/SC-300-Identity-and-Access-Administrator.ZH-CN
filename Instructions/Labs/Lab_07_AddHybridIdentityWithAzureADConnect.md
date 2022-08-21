---
lab:
  title: 07 - 使用 Azure AD Connect 添加混合标识
  learning path: "01"
  module: Module 01 - Implement an identity management solution
ms.openlocfilehash: dfb0bbb4416be0984784f159833f1873dfa5ff0d
ms.sourcegitcommit: 3125be5da88aa4b5fee51cf653c13bc9ee0bb799
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/15/2022
ms.locfileid: "147573426"
---
# <a name="lab-07-add-hybrid-identity-with-azure-ad-connect"></a>实验室 07：使用 Azure AD Connect 添加混合标识

注意 - 此实验室需要 Azure Pass。 有关说明，请参阅实验室 00。

注意 2 - 如果以前使用过 Azure AD Connect，此实验室应被视为可选。  此实验室至少需要 30 分钟的设置时间才能使用脚本部署和配置域控制程序。  如果要执行实验室，建议启动脚本，并在它在后台运行时处理其他活动。



## <a name="lab-scenario"></a>实验室方案

你公司在本地拥有 Active Directory 域服务。  他们希望继续使用本地 Active Directory 作为其标识和访问管理解决方案，但还需要用户能够使用相同的用户名和密码访问云应用程序。

#### <a name="estimated-time-60-minutes"></a>估计时间：60 分钟

### <a name="exercise-1---setup-on-premises-infrastructure"></a>练习 1 - 设置本地基础结构

#### <a name="task-1---create-the-on-premises-active-directory-infrastructure"></a>任务 1 - 创建本地 Active Directory基础结构

1. 可通过以下链接访问部署模板：[本地测试实验室指南](https://github.com/maxskunkworks/TLG/tree/master/tlg-base-config_3-vm)。

    **学员和 MCT 须知** - 部署此模板可能需要 30-60 分钟，因此可以在此步骤稍作休息，或者在讲课之前运行部署。

    **实验室提供者须知** - 如果可能，学生在实验室环境设置过程中完成和部署会很有帮助。

2. 在“TLG (测试实验室指南) - 3 VM 基础配置(v1.0)”页上，选择“部署到 Azure” 。

   注意 - 3 VM 基础配置使用指定的域名预配名为 DC1 的 Windows Server 2016 Active Directory 域控制器，以及运行 Windows Server 2016 的名为 APP1 的域成员服务器。 它还提供了一个选项，可用于预配运行 Windows 10 的客户端 VM，但我们不会在实验室中使用它（主要是源于在 Azure 中运行 Windows 10 VM 时适用的许可要求）。 域成员服务器 (APP1) 已自动安装 .NET 4.5 和 IIS。  
   
   注意 - 此实验室所需的 VM 为 DC1 。  如果使用 Azure Pass，则限制为 2 个 VM，因此客户端 VM 可能会失败。  本实验室不需要这样做。

3. 在“自定义部署”页上，指定以下设置，然后选择“查看 + 创建”，再选择“创建”  。

   -   订阅：要在其中预配实验室环境 Azure VM 的目标 Azure 订阅的名称。
   -   资源组：（新建）hybrididentity-RG
   -   位置：将托管实验室环境 Azure VM 的 Azure 区域的名称。
   -   配置名称：TlgBaseConfig-01
   -   域名：corp.contoso.com
   -   服务器 OS：2016-Datacenter
   -   管理员用户名：demouser
   -   管理员密码：demo@pass123
      - 强烈建议输入一个你知道且可以记住的安全密码。
   -   部署客户端 VM：**否**
   -   客户端 VHD URI：留空
   -   VM 大小：Standard_D2s_v3
   
   注意 - 如果订阅不支持列出的大小，请使用类似的 VM 大小。 此处为文档链接：<https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes>。

   -   DNS 标签前缀：任何有效且全局唯一的 DNS 名称（由字母、数字和连字符组成的唯一字符串，以字母开头，长度不超过 47 个字符）。

   -   _artifacts 位置：接受默认值
   -   _artifacts 位置 Sas 令牌：留空

4. 选择“查看 + 创建”  。

5. 通过验证后，选择“创建”。
    
6. 等待部署完成。 此过程大约需要 60 分钟。


### <a name="task-2---configure-the-lab-environment-azure-vms"></a>任务 2 - 配置实验室环境 Azure VM

1. 在显示 Azure 门户的浏览器窗口中，导航到 DC1 Azure VM 并通过远程桌面连接到它。 出现提示时，请使用以下凭据登录：

   -   用户名：demouser
   -   密码：demo\@pass123
      - 强烈建议输入一个易记的安全密码。

2.  在与 DC1 的远程桌面会话中，启动 Windows PowerShell ISE，将以下脚本添加到脚本窗格中，并运行该脚本以禁用 DC1 和 APP1 Azure VM 上的 Internet Explorer 增强的安全配置和用户访问控制   ：

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 00000000}
    ```

    **注意：** 要在同一文件中运行多个 PowerShell 脚本，可以突出显示特定脚本，然后选择绿色播放按钮旁边的“运行选定内容”。 

3.  在 Windows PowerShell ISE 窗口中，将以下脚本添加到脚本窗格中，并运行该脚本以同时在 *DC1 和 APP1 Azure VM 上安装远程服务器管理工具：

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Install-WindowsFeature RSAT -IncludeAllSubFeature} 
    ```

4.  在 Windows PowerShell ISE 窗口中，将以下脚本添加到脚本窗格中，并运行该脚本以同时在 *DC1 和 APP1 Azure VM 上启用 TLS 1.2：

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force}
    Invoke-Command -ComputerName $vmNames {New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value 1 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 0 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value 1 –PropertyType DWORD}
    ```

5.  在 Windows PowerShell ISE 窗口中，将以下脚本添加到脚本窗格中，并运行该脚本以在 APP1 Azure VM 上托管的默认网站上配置 Windows 集成身份验证 ：

    ```pwsh

    $vmNames = @('app1')
    Invoke-Command -ComputerName $vmNames {Enable-WindowsOptionalFeature -Online -FeatureName IIS-WindowsAuthentication}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/anonymousAuthentication" -Name Enabled -Value False -PSPath IIS:\ -Location "Default Web Site"}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/windowsAuthentication" -Name Enabled -Value True -PSPath IIS:\ -Location "Default Web Site"}
    ```

### <a name="task-3---restart-the-azure-vms"></a>任务 3 - 重启 Azure VM

1. 在 Windows PowerShell ISE 窗口中，从控制台窗格运行以下命令以重启 APP1 ：

    ```pwsh

    Restart-Computer -ComputerName 'APP1'
    ```

2. 在 Windows PowerShell ISE 窗口中，从控制台窗格运行以下命令以重启 DC1 ：

    ```pwsh
    Restart-Computer -ComputerName 'DC1'
    ```

### <a name="task-5---configure-contosolocal-active-directory"></a>任务 5 - 配置 contoso.local Active Directory

1. 通过远程桌面再次连接到 DC1 Azure VM。 出现提示时，请使用以下凭据登录：

    -   用户名：demouser

    -   密码：demo\@pass123
       - 强烈建议输入一个易记的安全密码。

2.  在与 DC1 的远程桌面会话中，启动 Internet Explorer，导航到以下链接。

    ```
    https://github.com/microsoft/MCW-Hybrid-identity/tree/main/Hands-on%20lab/studentfiles
    ```

3. 在“为 Active Directory 演示/测试环境创建用户/组”页上，选择 CreateDemoUsers.ps1 链接，接受许可条款，并将相应的脚本保存到本地文件系统 。

4. 在“为 Active Directory 演示/测试环境创建用户/组”页上，选择 CreateDemoUsers.csv 链接（位于 PowerShell 代码部分正上方），并将相应的 csv 文件保存到与CreateDemoUsers.ps1 文件相同的位置  。

5. 在与 DC1 的远程桌面会话中，启动文件资源管理器，导航到下载这两个文件的文件夹，右键选择文件 CreateDemoUsers.ps1，选择“属性”，在“CreateDemoUsers.ps1 属性”对话框中，选中“取消阻止”复选框，然后选择“确定”     。

6. 在文件资源管理器窗口中，再次在 CreateDemoUsers.ps1 文件上右键单击，然后选择“编辑” 。 

7. 在“管理员:**Windows PowerShell ISE”窗口中，对行 148 进行以下更改** ：

    ```pwsh
    $UserCount = 1000 #Up to 2500 can be created
    ```

   to
    ```pwsh
    $UserCount = 2500 #Up to 2500 can be created
    ```

8. 在 Windows PowerShell ISE 窗口中，保存更改并运行 CreateDemoUsers.ps1 脚本以创建实验室环境组织单位层次结构，并使用测试用户帐户填充该脚本 。 

9.  在 Windows PowerShell ISE 窗口中，将以下脚本添加到脚本窗格中，并运行该脚本以修改将在本实验室中使用的 AD 用户帐户的设置：

    ```pwsh

    $adUser1 = Get-ADUser -Filter {samAccountName -eq "AGAyers"}
    $adUser1groups = $adUser1 | Get-ADPrincipalGroupMembership 
    $adUser1groups | foreach { if ($_.name -ne 'Domain Users') {Remove-ADPrincipalGroupMembership -MemberOf $_.name -Identity $adUser1.DistinguishedName} }
    Add-ADPrincipalGroupMembership -MemberOf 'Engineering' -Identity $adUser1.DistinguishedName
    Move-ADObject -Identity $adUser1.DistinguishedName -TargetPath 'OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Set-ADAccountPassword -Identity 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "demo@pass123" -Force)

    $adUser2 = Get-ADUser -Filter {samAccountName -eq "TFBell"}
    $adUser2groups = $adUser2 | Get-ADPrincipalGroupMembership 
    $adUser2groups | foreach { if ($_.name -ne 'Domain Users') {Remove-ADPrincipalGroupMembership -MemberOf $_.name -Identity $adUser2.DistinguishedName} }
    Add-ADPrincipalGroupMembership -MemberOf 'Engineering' -Identity $adUser2.DistinguishedName
    Move-ADObject -Identity $adUser2.DistinguishedName -TargetPath 'OU=VT,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Set-ADAccountPassword -Identity 'CN=Bell\, Teresa,OU=VT,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "demo@pass123" -Force)
    Get-ADGroup -Identity 'Domain Admins' | Add-ADGroupMember -Members 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    Get-ADGroup -Identity 'Enterprise Admins' | Add-ADGroupMember -Members 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    ```

10. 在 Windows PowerShell ISE 窗口中，将以下脚本添加到脚本窗格中，并运行该脚本以创建名为“服务器”和“客户端”的其他组织单位，并将 APP1 计算机帐户移到第一个组织单位   ：

    ```pwsh

    New-ADOrganizationalUnit -Name 'Servers' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    New-ADOrganizationalUnit -Name 'Clients' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Move-ADObject -Identity 'CN=APP1,CN=Computers,DC=corp,DC=contoso,DC=com' -TargetPath 'OU=Servers,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    ```

11. 从 DC1 退出登录。

## <a name="exercise-2-integrate-an-active-directory-forest-with-an-azure-active-directory-tenant"></a>练习 2：将 Active Directory 林集成到 Azure Active Directory 租户中

### <a name="task-1-create-an-azure-active-directory-tenant-and-activate-an-ems-e5-trial"></a>任务 1：创建 Azure Active Directory 租户并激活 EMS E5 试用版

在此任务中，你需要使用以下设置创建 Azure Active Directory 租户： 

-   组织名称：Contoso

-   初始域名：任何有效的唯一域名。

-   国家或地区：美国

1. 从实验室计算机中启动新的 Web 浏览器窗口，并导航到 Azure 门户 (<https://portal.azure.com>)（如果尚未这样做）。

2. 出现提示时，登录到在“动手实验室准备工作”练习中部署资源的 Azure 订阅。

3. 在实验室计算机的 Azure 门户上选择“+ 创建资源”。

4. 在“新建”页的“搜索市场”文本框中，键入 Azure Active Directory，并在结果列表中选择“Azure Active Directory”   。

5. 在“Azure Active Directory”页上，选择“创建” 。

6. 在“创建目录”页上，指定以下设置并选择“创建” ：

    -   组织名称：Contoso

    -   初始域名：任何有效的唯一域名。

    -   国家或地区：美国

7. 创建后，导航到订阅页。 选择“更改目录”。

8. 在“更改目录”页的右侧，从下拉列表中选择 Contoso，然后选择“更改”  。 

9. 在门户的左侧导航栏中，选择“Azure Active Directory”。 

10. 在 Azure Active Directory 页中，选择“切换租户”，并选择“Contoso”框，然后选择“切换”   。 

    >**注意**：可能需要几分钟时间才能正确显示所有内容。

11. 在“Contoso”-“概述”页上，选择左侧导航上的“管理”下的“许可证”  。

12. 在“Contoso”-“许可证”页上，选择 “所有产品”，然后选择“+ 试用/购买”  。

13. 在“激活”页的“企业移动性 + 安全性 E5”部分，选择“免费试用版”，然后选择“激活”   。

   > **注意**：激活通常需要大约 5 分钟。

### <a name="task-2-create-and-configure-azure-ad-users"></a>任务 2：创建和配置 Azure AD 用户

在此任务中，你需要使用以下设置在新建的 Azure AD 租户中配置 Azure AD 用户帐户。 这包括将 EM+S E5 许可证分配给用于此实验室的用户帐户，以及使用以下设置创建新的 Azure AD 用户帐户，并为其分配全局管理员角色和 EM+S E5 许可证。

- 名称：john.doe

- 名字：John

- 姓氏：Doe
    
- 密码：自动生成密码
    
- 显示密码：**已启用**

- 组：未选择任何组
    
- 角色：**全局管理员**
    
- 阻止登录：否
    
- 使用位置：美国
    
- 职务：留空
    
- 部门：留空

1. 在实验室计算机上，在 Azure 门户中导航回“Contoso”-“概述”页。

2. 在“Contoso”-“概述”页上，选择左侧导航上的“管理”下的“用户”  。

3. 在“用户”-“所有用户”页上，单击代表你的用户帐户的条目。

4. 在用户帐户的“配置文件”页上，选择“编辑” 。

5. 在“设置”部分的“使用位置”下拉列表中，选择“美国”条目，再选择“保存”   。

6. 在用户帐户的“配置文件”页上，选择左侧“管理”下的“许可证”  。 

7. 在“许可证”页上，选择“+ 分配” 。

8. 在“更新许可证分配”页上，启用“企业移动性 + 安全性 E5”复选框，确保启用所有相应的许可证选项，然后选择“保存”  。

9. 在“用户”-“所有用户”页上，选择“+ 新建用户” 。

10. 在“新建用户”页上，确保选中“创建用户”选项，指定以下设置，然后选择“创建”  ：

    - 用户名：john.doe\@*Azure AD 租户域名*，其中 Azure AD 租户域名是创建 Contoso Azure AD 租户时指定的域名 。

    - 名称：john.doe

    - 名字：John

    - 姓氏：Doe
    
    - 密码：自动生成密码
    
    - 显示密码：**已启用**

    - 组：未选择任何组
    
    - 角色：**全局管理员**
    
    - 阻止登录：否
    
    - 使用位置：美国
    
    - 职务：留空
    
    - 部门：留空


    > **注意**：将用户名和密码值复制到记事本 。 稍后将在本实验室用到它们。

11. 在“用户”-“所有用户”页上，单击代表新建的用户帐户的条目。

12. 在“john.doe”-“配置文件”页上，选择左侧“管理”下的“许可证”  。

13. 在“john.doe”-“许可证”页上，选择“+ 分配” 。

14. 在“更新许可证分配”页上，启用“企业移动性 + 安全性 E5”复选框，确保启用所有相应的许可证选项，然后选择“保存”  。

### <a name="task-3-purchase-a-custom-domain-name"></a>任务 3：购买自定义域名

在此任务中，你需要使用 <https://docs.microsoft.com/en-us/azure/app-service/manage-custom-dns-buy-domain> 所述的功能购买自定义 DNS 域名。

1. 在实验室计算机上，在显示 Azure 门户的浏览器中，导航到订阅页，然后选择“更改目录”。 在“更改目录”页的右侧，从下拉列表中选择“默认目录”，然后选择“更改”  。 
   
2. 返回到 Azure Active Directory 概述页面。 选择“切换租户”，然后选择与在“动手实验室准备工作”练习中部署部署的 Azure 订阅关联的默认目录，再选择“切换”  。 

3. 在 Azure 门户的左导航中，选择“创建资源”。

4. 在“新建”页上，选择“Web 应用”中的“创建”  。

5. 在“Web 应用”页的“基本信息”选项卡中，指定以下设置并选择“下一页:  部署”，然后选择“下一页:监视”：

    - 订阅：“动手实验室准备工作”练习中你在其中部署资源的 Azure 订阅的名称。

    - 资源组：（新建）contosohilab-RG。

    - 名称：任何有效的全局唯一名称。
    
    - 发布：代码

    - 运行时堆栈：.NET Core 3.1 (LTS)

    - 操作系统：**Windows**

    - 区域：可在目标订阅中创建 Azure Web 应用的任何 Azure 区域。

    - 应用服务计划：接受默认设置。

    - SKU 和大小：共享 D1（如果需要，请选择“更改大小”，再选择“开发/测试”，选择 D1，然后选择“应用”）   
  
6. 在“Web 应用”页的“监视”选项卡上，指定以下设置，然后选择“查看 + 创建”，再选择“创建”   ：

    - 启用 Application Insights：**否**

7. 在 Azure 门户中，在顶部搜索栏上搜索并选择“应用服务域”。

8. 在“应用服务域”页上，选择“创建应用服务域” 。

9. 在“创建应用服务域”页上，选择 contosohilab-RG 资源组 。 然后在“搜索域...”文本框中，键入要购买的域名，再选择文本框下方列出的可用域名之一旁边的框。 请确保记下所选域。 

10. 选择“下一页:联系人信息”，键入必需的信息。

11. 选择“下一页:高级”，确保将“启用隐私保护”设置为“禁用” 。

12. 单击“查看 + 创建”，然后单击“创建” 。 

### <a name="task-4-assign-a-custom-domain-name-to-the-contoso-azure-ad-tenant"></a>任务 4：为 Contoso Azure AD 租户分配自定义域名

在此任务中，你需要为 Contoso Azure AD 租户分配新购买的自定义 DNS 域名。 

1. 在实验室计算机上的 Azure 门户中，选择 Azure 门户工具栏中的“目录 + 订阅”图标（位于 Cloud Shell 图标的右侧）并切换到 Contoso Azure AD 租户 。 

2. 在 Azure 门户的左侧导航中，选择 Azure Active Directory 以导航到“Contoso”-“概览”页面 。

3. 在“Contoso”-“概览”页上，选择左侧“管理”下的“自定义域名”  。

4. 在“Contoso”-“自定义域名”页上，选择“+ 添加自定义域” 。

5. 在右侧显示的“添加自定义域”页面的“自定义域名”文本框中，输入你在上一个任务中购买的域名，然后选择“添加域”  。 此操作将重定向到显示自定义域名设置的新页面。

6. 标识自定义域名页上 TXT 记录的值。 

7. 从实验室计算机中启动另一个浏览器选项卡，并导航到 Azure 门户。

8. 在 Azure 门户中，选择 Azure 门户工具栏中的“目录 + 订阅”图标（位于 Cloud Shell 图标的右侧），切换到与在“动手实验前准备工作”练习中你在其中部署了资源（默认目录）的 Azure 订阅关联的 Azure AD 租户  。 

9.  在 Azure 门户中的左侧导航窗格中选择“所有服务”。 在“搜索所有内容”文本框中，键入 DNS 区域，然后在搜索结果列表中选择“DNS 区域”条目  。

10. 在“DNS 区域”页上，选择与在上一任务中购买的自定义域名匹配的名称的条目。

11. 在“DNS 区域”页上，选择“+ 记录集”。 

12. 在“添加记录集”窗格中，指定以下设置，然后选择“确定” ：

    - 名称：\@

    - 类型：TXT

    - TTL：**1**

    - TTL 单位：**小时数**

    - 值：在“自定义域名”页上标识的“目标或指向地址”条目的值 。

13. 切换回显示自定义域名页面的浏览器窗口，然后选择“验证”。 确保验证成功。 

14. 选择“设为主域名”，并在出现提示时确认更改。 

### <a name="task-5-configure-dns-suffix-in-the-contoso-active-directory-forest"></a>任务 5：在 Contoso Active Directory 林中配置 DNS 后缀

在此任务中，你需要配置 Contoso Active Directory 林的 DNS 后缀，以匹配新验证的 Azure AD 自定义域名。

1. 在实验室计算机上的 Azure 门户中，确认你已登录到与在“动手实验前准备工作”练习中你在其中部署了资源（默认目录）的 Azure 订阅关联的 Azure AD 租户。 如果没有，请选择 Azure 门户工具栏中的“目录 + 订阅”图标（位于 Cloud Shell 图标的右侧）以切换到该 Azure AD 租户 。 

2. 在 Azure 门户中，导航到 DC1 虚拟机页面。

3. 在 DC1 虚拟机页上，通过远程桌面连接到 DC1 。 当系统提示登录时，请输入名称“demouser”和密码“demo\@pass123” 。 

4. 在与 DC1 的远程桌面会话中，在“服务器管理器”窗口中，启动“工具”下的“Active Directory 域和信任关系”控制台   。 

5. 在“Active Directory 域和信任关系”控制台中，右键单击左侧的“Active Directory 域和信任关系 [DC1.corp.contoso.com]”，然后选择“属性”  。

6. 在“Active Directory 域和信任关系 [DC1.corp.contoso.com]”窗口的“UPN 后缀”选项卡上的“其他 UPN 后缀”文本框中，键入你在上一个任务中验证的自定义域的名称，选择“添加”，然后选择“确定”    。

7. 在与 DC1 的远程桌面会话中，在“服务器管理器”窗口中，启动“工具”下的“Active Directory 用户和计算机”控制台   。 

8. 在“Active Directory 用户和计算机”控制台中，展开左侧的 corp.contoso.com，并检查域的组织单位层次结构以及域组的组成员身份 。 

9.  在与 DC1 的远程桌面会话中，启动 Windows PowerShell ISE，并在脚本窗格中运行以下命令，将工程组所有用户的 UPN 后缀替换为与 Contoso Azure AD 租户的自定义验证域名匹配的域名（将占位符 `<custom_domain_name>` 替换为分配给 Contoso Azure AD 租户的自定义验证域名的实际名称） 。 

    ```pwsh
    $domainName = '<custom_domain_name>'
    $users = Get-ADGroupMember -Identity 'Engineering' -Recursive | Where-Object {$_.objectClass -eq 'user'}

    foreach ($user in $users) {
        $user = Get-ADUser -Identity $User.SamAccountName
        $userName = $user.UserPrincipalName.Split('@')[0] 
        $upn = $userName + "@" + $domainName 
        $user | Set-ADUser -UserPrincipalName $upn
    }
    ```

### <a name="task-6-install-azure-ad-connect"></a>任务 6：安装 Azure AD Connect

在此任务中，你需要安装 Azure AD Connect。

1. 在与 DC1 的远程桌面会话的“服务器管理器”中，单击“本地服务器”，确保已禁用“IE 增强的安全配置”  。 如果未禁用，请选择“IE 增强的安全配置”旁边的“开启”链接，将管理员设置设置为“关闭”，然后选择“确定”    。

2. 在与 DC1 的远程桌面会话中，打开 Windows PowerShell ISE 窗口，并运行以下命令以安装 Chrome 浏览器 。

    ```pwsh
    $LocalTempDir = $env:TEMP; $ChromeInstaller = "ChromeInstaller.exe"; (new-object System.Net.WebClient).DownloadFile('http://dl.google.com/chrome/install/375.126/chrome_installer.exe', "$LocalTempDir\$ChromeInstaller"); & "$LocalTempDir\$ChromeInstaller" /silent /install; $Process2Monitor = "ChromeInstaller"; Do { $ProcessesFound = Get-Process | ?{$Process2Monitor -contains $_.Name} | Select-Object -ExpandProperty Name; If ($ProcessesFound) { "Still running: $($ProcessesFound -join ', ')" | Write-Host; Start-Sleep -Seconds 2 } else { rm "$LocalTempDir\$ChromeInstaller" -ErrorAction SilentlyContinue -Verbose } } Until (!$ProcessesFound)
    ```

2. 在与 DC1 的远程桌面会话中，启动 Chrome 浏览器并导航到 Azure 门户 (<https://portal.azure.com>)。

3. 当系统提示登录时，请输入 john.doe Azure AD 用户帐户的凭据（你在本练习前面已复制到记事本中）。

4. 出现提示时，请更改 john.doe 用户帐户的密码。 
  
    > **注意**：如果收到消息“我们曾多次看到过该密码，请选择更难猜测的密码”，则需要修改密码，确保密码唯一。

5. 如果系统提示是否“保持登录状态？”， 请选择“否”。 此时，将重定向到 Azure 门户界面。 

6. 如果显示“欢迎使用 Microsoft Azure”对话框，请选择“以后再说” 。 

7. 在 Azure 门户的左侧导航中，选择 Azure Active Directory 以导航到“Contoso”-“概览”页面 。

8. 在“Contoso”-“概览”页上，选择左侧“管理”下的 Azure AD Connect  。

9.  在“Azure AD Connect”页上，选择“下载 Azure AD Connect”链接 。

10. 在 Microsoft Azure Active Directory Connect”网页的“Microsoft 下载”站点上，选择“下载” 。

11. 当提示是运行还是保存 AzureADConnect.msi 时，请选择“运行” 。 这将下载该文件，并自动启动 Microsoft Azure Active Directory Connect 向导。 

12. 在“欢迎使用 Azure AD Connect”页上，选中“我同意许可条款和隐私声明”框，然后选择“继续”  。

13. 在“快速设置”页上，选择“自定义”按钮 。

14. 在“安装所需组件”页面中，取消选择所有可选配置选项并选择“安装”。

15. 在“用户登录”页上，选择“直通身份验证”选项和“启用单一登录”复选框，然后选择“下一步”   。

16. 在“连接到 Azure AD”页上，使用 john.doe 帐户的凭据登录，然后选择“下一步”  。

17. 在“连接目录”页上，确保“林”下拉列表中显示 corp.contoso.com 条目，然后选择“添加目录”   。 在“AD 林帐户”中，确保选择了“创建新的 AD 帐户”选项，在“企业管理员用户名”文本框中，输入 CORP.CONTOSO.COM\\demouser，在“密码”文本框中，输入 demo\@pass123，然后选择“确定”      。


18. 回到“连接目录”页面，选择“下一步” 。

19. 在“Azure AD 登录配置”页面上，确保自定义域名列为经过验证的 Active Directory UPN 后缀，并且 userPrincipalName 条目显示在“用户主体名称”下拉列表中   。 请注意警告“如果 UPN 后缀与已验证的域名不一致，用户将无法使用本地凭据登录 Azure AD”。 选中“继续操作且不将所有 UPN 后缀与已验证的域进行匹配”复选框，然后选择“下一步”。  

    >**注意**：这是预期行为，因为某些用户仍使用 contoso.local UPN 后缀进行配置，该后缀不可路由，无法配置为 Azure AD 租户的已验证自定义域名。

20. 在“域和 OU 筛选”页面上，确保仅选择 DemoAccounts OU 及其所有子 OU，然后选择“下一步”  。 


21. 在“唯一标识你的用户”页面中，接受默认设置，然后选择“下一步” 。 


22. 在“筛选用户和设备”页面中接受默认设置，然后选择“下一步” 。 

23. 在“可选功能”页面中接受默认设置，然后选择“下一步” 。

24. 在“启用单一登录”页面上，选择“输入凭据”，在“林凭据”对话框中，使用 CORP\\demouser 用户名和 demo\@pass123 密码登录，然后选择“下一步”     。


25. 在“准备配置”页面上，确保未选中“配置完成后启动同步过程”复选框，然后选择“安装”   。


   > **注意**：在启用同步过程之前，需要配置属性级筛选。

   > **注意**：安装需要约 2 分钟。

26. 在“配置完成”页面上，选择“退出” 。


### <a name="task-7-enable-active-directory-recycle-bin"></a>任务 7：启用 Active Directory 回收站

在此任务中，你需要在 Contoso Active Director 域中启用回收站。 

1. 在与 DC1 的远程桌面会话中，从服务器管理器的“工具”菜单，启动“Active Directory 管理中心” 。


2. 在“Active Directory 管理中心”控制台中，右键选择“corp (local)”，然后选择“启用回收站”  。 出现确认提示时，请选择“确定”。


3. 当系统提示刷新 AD 管理中心时，请选择“确定”。

   > **注意**：有关混合方案中回收站权益的信息，请参阅 <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-recycle-bin>

### <a name="task-8-configure-azure-ad-connect-attribute-level-filtering"></a>任务 8：配置 Azure AD Connect 属性级筛选条件

在此任务中，你需要配置 Azure AD Connect 属性级别筛选条件，使其限制将用户帐户同步到其 UPN 后缀与目标 Azure AD 租户的自定义域名匹配的帐户。

   > **注意**：正筛选选项至少需要两个同步规则。 其中一个确定要同步的对象的正确范围。 另一个全方位同步规则用于筛选出尚未标识为属于应同步对象的所有对象。

1. 在与 DC1 的远程桌面会话中，在“开始”菜单中的“Azure AD Connect”下启动“同步规则编辑器”  。


2. 在“同步规则编辑器”窗口的“查看和管理同步规则”页面上，确保“入站”出现在“方向”下拉列表中，然后选择“添加新规则”   。 这将启动“创建入站同步规则”向导。


3. 在“创建入站同步规则”-“说明”页上，指定以下设置，然后选择“下一步” ：

    - 名称：**Custom In from AD - UPN Filter**

    - 说明：**自定义入站规则 - 包括其 UPN 设置为匹配 Azure AD 自定义域的用户**

    - 连接的系统：corp.contoso.com

    - 连接的系统对象类型：user

    - Metaverse 对象类型：person

    - 链接类型：Join

    - 优先级：50

    - 标记：留空

    - 启用密码同步：留空

    - 禁用：留空


4. 在“创建入站范围筛选器”页上，选择“添加组”，再选择“添加子句”并指定以下内容，然后选择“下一步”   ：

    - 属性：userPrincipalName

    - 运算符：**ENDSWITH**

    - 值：\@\<your custom domain name>

5. 在“联接规则”页上，选择“下一步” 。

6. 在“转换”页上，选择“添加转换”并指定以下内容，然后选择“添加”  ：

    - 流类型：**Constant**

    - 目标属性：cloudFiltered

    - 源：**False**

7. 出现“警告”对话框并显示消息“在下一个同步周期中，将在 corp.contoso.com 上运行完全导入和完全同步”时，请选择“确定”  。

   > **注意**：这应使你回到“查看和管理同步规则”界面，其中在规则列表顶部列出了新规则。 

8. 回到“同步规则编辑器”窗口的“查看和管理同步规则”页面，确保“入站”出现在“方向”下拉列表中，然后选择“添加新规则”    。 这将启动“创建入站同步规则”向导。

9. 在“说明”页面上，指定以下设置，然后选择“下一步” ：

    - 名称：**Custom In from AD - Catch-all Filter**

    - 说明：**自定义入站规则 - 排除其 UPN 未设置为匹配 Azure AD 自定义域的所有用户**

    - 连接的系统：corp.contoso.com

    - 连接的系统对象类型：user

    - Metaverse 对象类型：person

    - 链接类型：Join

    - 优先级：**51**

    - 标记：留空

    - 启用密码同步：留空

    - 禁用：留空


10. 在“范围筛选器”页上，选择“下一步” 。

11. 在“联接规则”页上，选择“下一步” 。

12. 在“转换”页上，选择“添加转换”并指定以下内容，然后选择“添加”  ：

    - 流类型：**Constant**

    - 目标属性：cloudFiltered

    - 源：**True**

13. 出现“警告”对话框并显示消息“在下一个同步周期中，将在 corp.contoso.com 上运行完全导入和完全同步”时，请选择“确定”  。

    >**注意**：这应使你回到“查看和管理同步规则”界面，其中在规则列表顶部列出了新规则。 

### <a name="task-9-initiate-and-verify-directory-synchronization"></a>任务 9：启动并验证目录同步

1. 在与 DC1 的远程桌面会话中，双击并选择 Azure AD Connect 桌面快捷方式 。

2. 在“欢迎使用 Azure AD Connect”页上，选择“配置” 。 

3. 在“其他任务”页面上，选择“自定义同步选项”，然后选择“下一步”  。

4. 在“连接到 Azure AD”页上，使用 john.doe 帐户的凭据登录，然后选择“下一步”  。

5. 在“连接目录”页上选择“下一步”。

6. 在“域和 OU 筛选”页上选择“下一步”。 

7. 在“可选功能”页面中接受默认设置，然后选择“下一步” 。

8. 在“启用单一登录”页面上，选择“下一步” 。

9. 在“准备配置”页面，选中“配置完成后启动同步过程”复选框，然后选择“配置”  。

10. 在“配置完成”页面上，选择“退出” 。

11. 在与 DC1 的远程桌面会话中，在显示 Azure 门户的 Microsoft Edge 浏览器窗口中，导航到 Azure AD 租户的“用户”-“所有用户”页面 。

12. 在“用户”-“所有用户”页上，请注意，用户对象列表包括其 UPN 后缀与 Azure AD 租户的自定义域名匹配的所有用户帐户。 可能需要刷新页面或等待几分钟才能查看更改。

13. 在 Azure 门户中，导航到 Contoso Azure AD 租户的“组”-“所有组”页，并请注意，所有 corp.contoso.com 域组也已同步。 

14. 在 Azure 门户中，导航到“Contoso”-“Azure AD Connect”页，然后选择左侧的 Azure AD Connect 。 验证是否已设置以下设置： 

    - Azure AD Connect 同步状态：**已启用** 
  
    - 上次同步：此内容应为某种时间戳。 
  
    - 密码哈希同步：**已禁用** 
  
    - 联合：**已禁用**
   
    - 无缝单一登录：针对 1 个域启用 
  
    - 直通身份验证：通过 1 个代理启用

   > **注意**：在生产环境中，需要安装其他代理来实现冗余。 有关详细信息，请参阅<https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-quick-start>。

### <a name="task-10-configure-hybrid-azure-ad-join"></a>任务 10：配置混合 Azure AD 加入

在此任务中，你需要配置 Azure AD Connect 设备同步选项。

1. 在与 DC1 的远程桌面会话中，双击并选择 Azure AD Connect 桌面快捷方式 。

2. 在“欢迎使用 Azure AD Connect”页上，选择“配置” 。 

3. 在“其他任务”页面上，选择“配置设备选项”，然后单击“下一步”  。

4. 在“概述”页上，查看有关混合 Azure AD 加入和设备写回的信息，然后选择“下一步”   。

5. 在“连接到 Azure AD”页上，使用 john.doe 帐户的凭据登录，然后选择“下一步”  。

6. 在“设备选项”页上，确保已选择“配置混合 Azure AD 加入”选项，然后选择“下一步”  。 

7. 在“设备操作系统”页上，选中“Windows 10 或更高版本加入域的设备”和“支持的 Windows 下层加入域的设备”复选框，然后选择“下一步”   。 

   > **注意**：仅当对托管域使用无缝 SSO 或对联盟域使用联合身份验证服务（如 AD FS）时，才支持 Windows 下层设备。

8. 在“SCP 配置”页面上，选中 corp.contoso.com Active Directory 林框，选择“身份验证服务”下拉列表中的 Azure Active Directory 条目，然后选择“添加”    。

9. 当系统提示你输入 corp.contoso.com 的企业管理员凭据时，请在“Windows 安全”对话框中，使用 CORP\\demouser 用户名和 demo\@pass123 密码登录  。

10. 返回到“SCP 配置”页，选择“下一步” 。

11. 在“已准备好进行配置”页上，选择“配置”。

12. 在“配置完成”页上，验证任务是否已成功完成，然后选择“退出” 。

   > **注意**：有关为托管域配置混合 Azure Active Directory 加入的详细信息，请参阅 <https://docs.microsoft.com/en-us/azure/active-directory/devices/hybrid-azuread-join-managed-domains#configure-hybrid-azure-ad-join>。


### <a name="task-11-perform-hybrid-azure-ad-join"></a>任务 11：执行混合 Azure AD 加入

1. 在实验室计算机上的 Azure 门户中，确认你已登录到与在“动手实验前准备工作”练习中你在其中部署了资源（默认目录）的 Azure 订阅关联的 Azure AD 租户。 如果没有，请选择 Azure 门户工具栏中的“目录 + 订阅”图标（位于 Cloud Shell 图标的右侧）以切换到该 Azure AD 租户 。 

2. 在 Azure 门户中，导航到 APP1 虚拟机页面。

3. 在 APP1 虚拟机页上，通过远程桌面连接到 APP1 。 当提示登录时，请使用AGAyers\@<custom_domain_name> 用户名和密码 demo@pass123（其中 <custom_domain_name> 占位符表示你在本练习前面分配给 Contoso Azure AD 租户的自定义 DNS 域名  。

4. 在与 APP1 的远程桌面会话的“服务器管理器”窗口中，启动“工具”下的“任务计划程序”   。 


5. 在“任务计划程序”控制台中，导航到“任务计划程序库” > “Microsoft” > “Windows” > “工作区加入”    。 从这里启用并运行 Automatic-Device-Join 任务。 


6. 将远程桌面会话切换到 DC1，然后从 Windows PowerShell ISE 窗口的控制台窗格中，通过运行以下命令启动 Azure AD Connect 增量同步：

   ```pwsh
   Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'
   
   Start-ADSyncSyncCycle -PolicyType Delta
   ```

7. 切换回与 APP1 的远程桌面会话，然后启动命令提示符 。

8. 在命令提示符窗口中，运行以下命令，检查 APP1 的 Azure AD 注册状态： 

   ```
   dsregcmd /status
   ```

9. 验证命令的输出是否如下所示：

   ```
   +----------------------------------------------------------------------+
   | Device State                                                         |
   +----------------------------------------------------------------------+

        AzureAdJoined : YES
     EnterpriseJoined : NO
             DeviceId : 61eea2b8-efbe-43d9-b267-126433c8ee34
           Thumbprint : BBAAA0FB4A55E880388851BED955A2669A961A96
       KeyContainerId : 2eb75eb8-0a1d-437b-99d9-9dd161ca0d90
          KeyProvider : Microsoft Software Key Storage Provider
         TpmProtected : NO
         KeySignTest: : PASSED
                  Idp : login.windows.net
             TenantId : xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx
           TenantName : xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx
          AuthCodeUrl : https://login.microsoftonline.com/xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx/oauth2/authorize
       AccessTokenUrl : https://login.microsoftonline.com/xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx/oauth2/token
               MdmUrl :
            MdmTouUrl :
     MdmComplianceUrl :
          SettingsUrl :
       JoinSrvVersion : 1.0
           JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/
            JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net
        KeySrvVersion : 1.0
            KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/
             KeySrvId : urn:ms-drs:enterpriseregistration.windows.net
         DomainJoined : YES
           DomainName : CORP

   +----------------------------------------------------------------------+
   | User State                                                           |
   +----------------------------------------------------------------------+

               NgcSet : NO
      WorkplaceJoined : NO
        WamDefaultSet : NO
           AzureAdPrt : NO

   +----------------------------------------------------------------------+
   | Ngc Prerequisite Check                                               |
   +----------------------------------------------------------------------+

        IsUserAzureAD : NO
        PolicyEnabled : NO
       DeviceEligible : YES
   SessionIsNotRemote : NO
     X509CertRequired : NO
         PreReqResult : WillNotProvision

   ```
11. 切换回与 DC1 的远程桌面会话，在显示 Azure 门户的 Microsoft Edge 浏览器窗口中，导航到 Contoso Azure AD 租户的”设备”-”所有设备”页面并验证是否存在表示 APP1 服务器并且其加入类型设置为已加入混合 Azure AD 的条目   。

   > **注意**：可能需要等待直到正确报告 Azure AD 注册状态并且其 Azure AD 对象显示在 Azure 门户中。


