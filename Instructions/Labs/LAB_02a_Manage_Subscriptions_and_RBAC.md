---
lab:
    title: '02a - 管理订阅和 RBAC'
    module: '模块 02 - 治理与合规性'
---

# 实验室 02a - 管理订阅和 RBAC
# 学生实验室手册

## 实验室要求：

本实验室需要具有创建 Azure Active Directory (Azure AD) 用户、创建自定义 Azure 基于角色的访问控制 (RBAC) 角色，以及将这些角色分配给 Azure AD 用户的权限。并非所有实验室主机托管服务提供商都提供此功能。请询问讲师是否可以使用本实验室。

## 实验室场景

为了改善 Contoso 中 Azure 资源的管理，你的任务是实现以下功能：

- 创建一个将包括所有 Contoso Azure 订阅的管理组

- 向指定 Azure Active Directory 用户授予为管理组中所有订阅提交支持请求的权限。该用户的权限应仅限于： 

    - 创建支持请求票证
    - 查看资源组 

## 目标

在本实验室中，你将完成以下任务：

+ 任务 1：实现管理组
+ 任务 2：创建自定义 RBAC 角色 
+ 任务 3：分配 RBAC 角色


## 预计用时：30 分钟

## 体系结构图

![图像](../media/lab02a.png)


## 说明

### 练习 1

#### 任务 1：实现管理组

在此任务中，你将需要创建和配置管理组。 

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 搜索并选择 **“管理组”**，导航到 **“管理组”** 边栏选项卡。

1. 查看 **“管理组”** 边栏选项卡顶部的信息。如果看到的信息为 **“你已注册为目录管理员，但没有访问根管理组的必要权限”**，请执行下面的一系列步骤：

    1. 在 Azure 门户中，搜索并选择 **“Azure Active Directory”**。
    
    1.  在显示 Azure Active Directory 租户属性的边栏选项卡上，左侧垂直菜单的 **“管理”** 部分中，选择 **“属性”**。
    
    1.  在 Azure Active Directory 租户的 **“属性”** 边栏选项卡上， **“Azure 资源的访问管理”** 部分中，选择 **“是”**，然后选择 **“保存”**。
    
    1.  导航回 **“管理组”** 边栏选项卡，并选择 **“刷新”**。

1. 在“**管理组**”边栏选项卡上，单击“**+ 创建**”。

    >**备注**： 如果此前尚未创建管理组，请选择 **“开始使用管理组”**

1. 使用以下设置创建一个新的管理组：

    | 设置 | 值 |
    | --- | --- |
    | 管理组 ID | **az104-02-mg1** |
    | 管理组显示名称 | **az104-02-mg1** |

1. 在管理组列表中，单击表示新建管理组的条目。

1. 在 **“az104-02-mg1”** 边栏选项卡上，单击 **“订阅”**。 

1. 在“**az104-02-mg1 \| 订阅**”边栏选项卡上，单击“**+ 添加**”，在“**添加订阅**”边栏选项卡上的“**订阅**”下拉列表中，选择你在此实验室中使用的订阅，然后单击“**保存**”。

    >**备注**： 在 **“az104-02-mg1 \| 订阅”** 上边栏选项卡，将 Azure 订阅的 ID 复制到剪贴板。在下一个任务中需要使用它。

#### 任务 2：创建自定义 RBAC 角色

在此任务中，你创建自定义 RBAC 角色的定义。

1. 在实验室计算机上，在笔记本中打开名为 **“\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json”** 的文件并查看其内容：

   ```json
   {
      "Name": "Support Request Contributor (Custom)",
      "IsCustom": true,
      "Description": "Allows to create support requests",
      "Actions": [
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
          "/providers/Microsoft.Management/managementGroups/az104-02-mg1",
          "/subscriptions/SUBSCRIPTION_ID"
      ]
   }
   ```
    > **备注**：如果不确定文件在实验室环境中的本地存储位置，请询问讲师。

1. 将 JSON 文件中的 `SUBSCRIPTION_ID` 占位符替换为复制到剪贴板中的订阅 ID，然后保存更改。

1. 在 Azure 门户中，通过单击搜索文本框右侧的“**工具栏**”图标，打开 Cloud Shell 窗格。

1. 提示选择 **“Bash”** 或 **“PowerShell”** 时，选择 **“PowerShell”**。 

    >**备注**：如果这是你第一次启动 **Cloud Shell**，并看到“**未装载任何存储**”消息，请选择在本实验室中使用的订阅，然后单击“**创建存储**”。 

1. 在Cloud Shell 窗格的工具栏中，单击 **“上传/下载文件”** 图标，在下拉菜单中单击 **“上传”**，然后将文件 **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** 上传到 Cloud Shell 主目录中。

1. 在 Cloud Shell 窗格中，运行下列命令以创建自定义角色定义：

   ```powershell
   New-AzRoleDefinition -InputFile $HOME/az104-02a-customRoleDefinition.json
   ```

1. 关闭 Cloud Shell 窗格。

#### 任务 3：分配 RBAC 角色

在此任务中，你将创建一个 Azure Active Dicrectory 用户，将在上一个任务中创建的 RBAC 角色分配给该用户，并验证该用户可以执行 RBAC 角色定义中指定的任务。

1. 在 Azure 门户中，搜索并选择 **“Azure Active Directory”**，在 **“Azure Active Directory”** 边栏选项卡上，单击 **“用户”**，然后单击 **“+ 新建用户”**。

1. 使用以下设置创建新用户（将其他设置保留为默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 用户名 | **az104-02-aaduser1**|
    | 名称 | **az104-02-aaduser1**|
    | 让我创建密码 | 已启用 |
    | 初始密码 | **Pa55w.rd1234** |

    >**备注**：将完整**用户名**称**复制到剪贴板**。稍后将在本实验室用到它。

1. 在 Azure 门户中，浏览到 **“az104-02-mg1”** 管理组并显示其 **“详细信息”**。

1. 单击 **“访问控制(IAM)”**，再单击 **“+ 添加”** 和 **“角色分配”**，然后将 **“支持请求参与者(自定义)”** 角色分配给新创建的用户帐户。

1. 打开一个 **InPrivate** 浏览器窗口，并使用新创建的用户帐户登录到 [Azure 门户](https://portal.azure.com)。当提示更新密码时，更改该用户的密码。

    >**备注**： 可以粘贴剪贴板的内容，而不必键入用户名。

1. 在 **InPrivate** 浏览器窗口中，在 Azure 门户中搜索并选择 **“资源组”**，验证 az104-02-aaduser1 用户可以看到所有资源组。

1. 在 **InPrivate** 浏览器窗口中，在 Azure 门户中搜索并选择 **“全部资源”**，验证 az104-02-aaduser1 用户看不到任何资源。

1. 在 **InPrivate** 浏览器窗口中，在 Azure 门户中搜索并选择“**帮助 + 支持**”，然后单击“**+ 创建支持请求**”。 

1. 在 **InPrivate** 浏览器窗口的“**帮助 + 支持 - 新建支持请求**”边栏选项卡的“**问题描述/摘要**”选项卡上，在“摘要”字段键入“**服务和订阅限制**”，然后选择“**服务与订阅限制(配额)**”问题类型。请注意，“**订阅**”下拉列表中列出了你在本实验室中使用的订阅。

    >**备注**： 你在本实验室中使用的订阅显示在 **“订阅”** 下拉列表中，表示你使用的帐户具有适当权限，可以创建特定于订阅的支持请求。

    >**备注**： 如果没有看到 **“服务与订阅限制(配额)”** 选项，请从 Azure 门户注销并重新登录。

1. 不要继续创建支持请求。相反，请以“az104-02-aaduser1”用户身份从 Azure 门户注销，然后关闭 InPrivate 浏览器窗口。

#### 清理资源

   >**备注**：请记得删除任何新创建且不会再使用的 Azure 资源。 

   >**备注**：虽然在此实验室中创建的资源不会产生额外的费用，但请删除未使用的资源，以确保不会有意外费用产生。

1. 在 Azure 门户中，搜索并选择“**Azure Active Directory**”，在“Azure Active Directory”边栏选项卡上，单击“**用户**”。

1. 在 **“用户-所有用户”** 边栏选项卡中，单击 **“az104-02-aaduser1”**。

1. 在 **“az104-02-aaduser1 - Profile”** 边栏选项卡中，复制 **“对象 ID”** 属性值。

1. 在 Azure 门户中，启动 **Cloud Shell** 中的 **“PowerShell”** 会话。

1. 在 Cloud Shell 窗格中，运行以下命令以删除自定义角色定义的分配（将 `[object_ID]` 占位符替换为你在此任务的前面部分复制的 **az104-02-aaduser1** Azure Active Directory 用户帐户的 **“对象 ID”** 属性的值）：

   ```powershell
   $scope = (Get-AzRoleAssignment -RoleDefinitionName 'Support Request Contributor (Custom)').Scope

   Remove-AzRoleAssignment -ObjectId '[object_ID]' -RoleDefinitionName 'Support Request Contributor (Custom)' -Scope $scope
   ```

1. 在 Cloud Shell 窗格中，运行下列命令以删除自定义角色定义：

   ```powershell
   Remove-AzRoleDefinition -Name 'Support Request Contributor (Custom)' -Force
   ```

1. 在Azure 门户中，返回 **“Azure Active Directory”** 的 **“用户 - 所有用户”** 边栏选项卡，然后删除 **“az104-02-aaduser1”** 用户帐户。

1. 在 Azure 门户中，导航回 **“管理组”** 边栏选项卡。 

1. 在 **“管理组”** 边栏选项卡上，选择 **az104-02-mg1** 管理组下订阅旁边的 **“省略号”** 图标，然后选择 **“移动”** 以将订阅移动到 **“租户根管理组”**。

   >**备注**： 目标管理组很有可能就是 **“租户根管理组”**，除非你在运行此实验室之前创建了自定义管理组层次结构。
   
1. 选择 **“刷新”**，验证订阅已成功移至 **“租户根管理组”**。

1. 导航回 **“管理组”** 边栏选项卡，右键单击 **“az104-02-mg1”** 管理组右侧的省略号图标，然后单击 **“删除”**。

#### 回顾

在本实验室中，你已：

- 实现管理组
- 创建自定义 RBAC 角色 
- 分配 Azure RBAC 角色
