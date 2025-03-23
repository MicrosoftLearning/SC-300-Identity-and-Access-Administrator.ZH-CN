---
lab:
  title: 10 - 适用于 Windows 和 Linux 虚拟机的 Microsoft Entra ID 身份验证
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# 实验室 10 - 适用于 Windows 和 Linux 虚拟机的 Microsoft Entra 身份验证

### 登录类型 = Azure 资源登录

## 实验室方案

该公司决定使用 Microsoft Entra ID 登录虚拟机进行远程访问。  本实验室将演示如何为 Windows 和 Linux 虚拟机设置此方案。

#### 预计用时：30 分钟

### 练习 1 - 使用 Microsoft Entra ID 登录到 Azure 中的 Windows 虚拟机

#### 任务 1 - 创建启用了 Microsoft Entra ID 登录的 Windows 虚拟机

1. 浏览到 [https://portal.azure.com](https://portal.azure.com)

1. 选择“+ 创建资源”。

1. 在“搜索市场”搜索栏中键入“**Windows 11**”，然后键入 **Enter**。

1. 在 **Windows 11** 框中，选择“**创建 v**”，然后从打开的菜单中选择“**Windows 11 企业版，版本 22H2**”。

1. 在“**基本信息**”选项卡上使用以下值创建 VM：

  | 字段 | 要使用的值 |
  | :-- | :-- |
  | 订阅 | 接受默认值 |
  | 资源组 | 新建 - rgEntraLogin |
  | 虚拟机名称 | vmEntraLogin |
  | 区域 | *default* |
  | 可用性选项 | 没有所需的基础结构冗余 |
  | 安全类型 | 标准 |
  | 大小 | Standard DC1s_v3 - 1 vCPU, 8 GiB 内存 |
  | 管理员用户名 | vmEntraAdmin |
  | 管理员密码 | 使用实验室环境提供的密码，或者让我们成为可以记住的安全密码 |
  | 许可 | 确认你拥有许可证 |

1. 无需更改“**磁盘**”或“**网络**”选项卡上的任何内容，但可以查看这些值。

1. 在“管理”选项卡上，选中“Microsoft Entra ID”部分下的“使用 Microsoft Entra ID 登录”框。********

        NOTE: You will notice that the **System assigned managed identity** under the Identity section is automatically checked and turned grey. This action should happen automatically once you enable Login with Microsoft Entra ID.

1. 完成创建虚拟机的其余体验。 

1. 依次选择“**查看 + 创建**”、“**创建**”。

#### 任务 2 - 现有 Azure 虚拟机的 Microsoft Entra ID 登录

1. 浏览到 [https://portal.azure.com](https://portal.azure.com) 中的**虚拟机**。

1. 选择“任务 1”中新创建的虚拟机。

1. 选择“访问控制 (IAM)”。

1. 依次选择“+ 添加”和“添加角色分配”，以打开“添加角色分配”页面 。

1. 分配以下设置：
  - **工作职能角色**
  - **角色**：虚拟机管理员登录
  - **成员**：选择“用户、组或服务主体”。  然后使用“**+ 选择成员**”将 **User2** 添加为 VM 的特定用户。

1. 选择“**查看+分配**”以完成该过程。

#### 任务 3 - 更新虚拟机以允许 Microsoft Entra ID 登录。

1. 选择“连接”菜单项。

1. 在“RDP”选项卡上，选择“下载 RDP 文件” 。  如果出现提示，请为文件选择“保留”选项。  它将保存到“下载”文件夹中。

1. 在文件管理器中打开“下载”文件夹。

1. 打开 RDP。

1. 选择以备用用户身份登录。

1. 使用设置虚拟机时创建的管理员 (vmEntraAdmin) 用户名和密码。
   - 如果出现提示，请选择“是”以允许访问虚拟机或 RDP 会话。

1. 等待虚拟机打开并加载所有软件。

1. 选择虚拟机中的“启动”按钮。

1. 键入“控制面板”并启动控制面板应用。

1. 从设置列表中选择“系统和安全”。

1. 从“系统”设置中，选择“允许远程访问”选项 。

1. 在打开的对话框底部，你将看到“远程桌面”部分。

1. 取消选中标记为“只允许运行带网络级身份验证的远程桌面的计算机连接”的框 。

1. 依次选择“应用”、“确定” 。

1. 退出虚拟机 RDP 会话。

#### 任务 4 - 修改 RDP 文件以支持 Microsoft Entra ID 登录

1. 在文件管理器中打开“下载”文件夹。

1. **创建 RDP 文件的副本**，并在文件名末尾添加 **-EntraID**。

1. 编辑刚刚使用**记事本**复制的 RDP 文件的新版本。 将这两行文本添加到文件底部：
     ```
        enablecredsspsupport:i:0
        authentication level:i:2
     ```
 
 1. 保存 RDP 文件。  现在应有该文件的两个版本：
      - <<virtual machine name>>.RDP
      - <<virtual machine name>>-EntraID.RDP

#### 任务 5 - 使用 Microsoft Entra ID 登录名连接到 Windows 虚拟机

1. 打开 **<<virtual machine name>>-EntraID.RDP

1. 当对话框打开时，选择“连接”。

1. 系统不会提示你使用哪个用户帐户登录，而是应收到一条消息，提示你是否要连接到远程计算机。

1. 选择屏幕底部的“是”。

1. 远程桌面会话随即打开；并显示 Windows Server 登录屏幕。  应显示带有“确定”按钮的其他用户。

1. 选择“确定”。

1. 在登录对话框中输入以下信息：
   - 用户名 = **AzureAD\User2@ 你的域名**
   - 密码 = 输入实验室提供商提供的密码

   备注：User2 是我们在任务 1 期间授权其以管理员身份登录的用户。

1. Windows 应确认登录并打开正常桌面。

#### 任务 6 - 进行测试（可选）以探索 Microsoft Entra ID 登录

1. 检查 User2 是否是添加到管理员组的唯一用户。

1. 使用辅助鼠标单击“开始”按钮，然后在弹出菜单中选择“计算机管理”****。

1. 打开“本地用户和组”，然后导航到“组、管理员” 。

1. 应在列表中看到 **AzureUser2....**。

1. 检查其他 Microsoft Entra ID 成员是否可以登录。

1. 退出远程桌面会话。

1. 再次启动 **<<server name>>-EntraID.RDP** 文件。

1. 尝试以其他 Microsoft Entra ID 成员身份登录。

1. 你应该注意到，这些用户中的每一个都被拒绝访问。

### 可选练习 2 - 使用 Microsoft Entra ID 登录到 Azure 中的 Linux 虚拟机

#### 任务 1 - 创建具有系统分配的托管标识的 Linux VM

1. 浏览到 [https://portal.azure.com](https://portal.azure.com)

1. 选择“+ 创建资源”。

1. 搜索“Ubuntu”****。

1. 在“Ubuntu Server 22.04 LTS”下选择“创建”********。 可将其他 Linux 服务器用于此测试实验室。

1. 在“管理”选项卡上，选中用于启用“使用 Microsoft Entra ID 登录”的框。********

1. 确保选中**系统分配的托管标识**。

1. 完成创建虚拟机的其余体验。 在此预览版中，必须使用用户名和密码或 SSH 公钥创建管理员帐户。

#### 任务 2 - 现有 Azure 虚拟机的 Microsoft Entra ID 登录

1. 浏览到 [https://portal.azure.com](https://portal.azure.com) 中的**虚拟机**。

1. 选择“访问控制 (IAM)”。

1. 选择“添加”>“添加角色分配”，打开“添加角色分配”页面。

1. 分配以下角色。 
    - **角色**：虚拟机管理员登录或虚拟机用户登录
    - **分配访问权限对象**：用户、组、服务主体或托管标识

1. 有关详细步骤，请参阅使用 Azure 门户分配 Azure 角色。
