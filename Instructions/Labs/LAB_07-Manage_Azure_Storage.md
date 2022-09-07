---
lab:
  title: 07 - 管理 Azure 存储
  module: Administer Azure Storage
---

# <a name="lab-07---manage-azure-storage"></a>实验室 07 - 管理 Azure 存储
# <a name="student-lab-manual"></a>学生实验室手册

## <a name="lab-scenario"></a>实验室方案

You need to evaluate the use of Azure storage for storing files residing currently in on-premises data stores. While majority of these files are not accessed frequently, there are some exceptions. You would like to minimize cost of storage by placing less frequently accessed files in lower-priced storage tiers. You also plan to explore different protection mechanisms that Azure Storage offers, including network access, authentication, authorization, and replication. Finally, you want to determine to what extent Azure Files service might be suitable for hosting your on-premises file shares.

若要以交互式指南格式预览此实验室，请[单击此处](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011)。

## <a name="objectives"></a>目标

在此实验中，将执行以下操作：

+ 任务 1：预配实验室环境
+ 任务 2：创建和配置 Azure 存储帐户
+ 任务 3：管理 Blob 存储
+ 任务 4：管理 Azure 存储的身份验证和授权
+ 任务 5：创建并配置 Azure 文件存储共享
+ 任务 6：管理 Azure 存储的网络访问

## <a name="estimated-timing-40-minutes"></a>预计用时：40 分钟

## <a name="architecture-diagram"></a>体系结构关系图

![image](../media/lab07.png)


## <a name="instructions"></a>说明

### <a name="exercise-1"></a>练习 1

#### <a name="task-1-provision-the-lab-environment"></a>任务 1：预配实验室环境

在此任务中，你将部署一个 Azure 虚拟机，稍后将在本实验室中用到。

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 在 Azure 门户中，单击 Azure 门户右上方的图标，打开 Azure Cloud Shell。

1. 如果系统提示选择“Bash”或“PowerShell”，请选择“PowerShell”  。

    >**注意**：如果这是你第一次启动 Cloud Shell，并看到消息“未装载任何存储”，请选择你将在本实验室中使用的订阅，然后选择“创建存储”  。

1. 在 Cloud Shell 窗格的工具栏中，单击“上传/下载文件”图标，在下拉菜单中，单击“上传”，然后将文件 \\Allfiles\\Labs\\07\\az104-07-vm-template.json 和 \\Allfiles\\Labs\\07\\az104-07-vm-parameters.json 上传到 Cloud Shell 主目录中   。

1. Edit the <bpt id="p1">**</bpt>Parameters<ept id="p1">**</ept> file you just uploaded and change the password. If you need help editing the file in the Shell please ask your instructor for assistance. As a best practice, secrets, like passwords, should be more securely stored in the Key Vault. 

1. 在 Cloud Shell 窗格中运行以下命令，以创建将托管虚拟机的资源组（将 '[Azure_region]' 占位符替换为你打算在其中部署 Azure 虚拟机的 Azure 区域的名称）

    >**注意**：若要列出 Azure 区域的名称，请运行 `(Get-AzLocation).Location`。注意：应单独键入每个命令

    ```powershell
    $location = '[Azure_region]'
    ```
  
    ```powershell
     $rgName = 'az104-07-rg0'
    ```

    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```
    
1. 在 Cloud Shell 窗格中运行以下命令，通过使用上传的模板和参数文件来部署虚拟机：

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-07-vm-template.json `
      -TemplateParameterFile $HOME/az104-07-vm-parameters.json `
      -AsJob
   ```

    >**注意**：请不要等待部署完成，而是继续执行下一个任务。

    >**注意**：如果遇到提示 VM 大小不可用的错误，请向讲师寻求帮助并尝试以下步骤。
    > 1. 单击 CloudShell 中的 `{}` 按钮，从左侧栏中选择“az104-07-vm-parameters.json”，并记下 `vmSize` 参数值。
    > 1. Check the location in which the 'az104-04-rg1' resource group is deployed. You can run <ph id="ph1">`az group show -n az104-04-rg1 --query location`</ph> in your CloudShell to get it.
    > 1. 在 CloudShell 中运行 `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"`。
    > 1. 将 `vmSize` 参数的值替换为刚运行的命令返回的一个值。
    > 1. Now redeploy your templates by running the <ph id="ph1">`New-AzResourceGroupDeployment`</ph> command again. You can press the up button a few times which would bring the last executed command.

1. 关闭 Cloud Shell 窗格。

#### <a name="task-2-create-and-configure-azure-storage-accounts"></a>任务 2：创建和配置 Azure 存储帐户

在此任务中，你将创建和配置 Azure 存储帐户。

1. 在 Azure 门户中，搜索并选择“存储帐户”，然后单击“+ 创建” 。

1. 在“创建存储帐户”边栏选项卡的“基本信息”选项卡上，指定以下设置（其他设置保留默认值） ：

    | 设置 | “值” |
    | --- | --- |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | 新资源组名称 az104-07-rg1  |
    | 存储帐户名称 | 由字母和数字组成、长度介于 3 到 24 个字符之间的任意全局唯一名称 |
    | 区域 | 可以在其中创建 Azure 存储帐户的 Azure 区域的名称  |
    | 性能 | **标准** |
    | 冗余 | **异地冗余存储 (GRS)** |

1. 单击“下一步:高级 >”，在“创建存储帐户”边栏选项卡的“高级”选项卡上，查看可用选项，接受默认设置，然后单击“下一步:   网络 >”。

1. 在“创建存储帐户”边栏选项卡的“网络”选项卡上，查看可用选项，接受默认选项“启用来自所有网络的公共访问”，然后单击“下一步:    数据保护 >”。

1. 在“创建存储帐户”边栏选项卡的“数据保护”选项卡中，查看可用选项，接受默认设置，然后单击“查看 + 创建”，等待验证过程完成，然后单击“创建”   。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the Storage account to be created. This should take about 2 minutes.

1. 在“部署”边栏选项卡上，单击“前往资源”，以显示“Azure 存储帐户”边栏选项卡。

1. 在“存储帐户”边栏选项卡的“数据管理”部分，单击“异地复制”并记下辅助位置 。 

1. 在“存储帐户”边栏选项卡的“设置”部分，选择“配置”，在“复制”下拉列表中选择“本地冗余存储(LRS)”并保存更改   。

1. 切换回“异地复制”边栏选项卡，请注意，此时存储帐户只有主位置。

1. 再次显示存储帐户的“配置”边栏选项卡，将“访问层(默认)”设置为“冷”并保存更改  。

    > **注意**：对于不经常访问的数据，冷访问层是最佳选择。

#### <a name="task-3-manage-blob-storage"></a>任务 3：管理 Blob 存储

在此任务中，你将创建 blob 容器并在其中上传 blob。

1. 在“存储帐户”边栏选项卡上的“数据存储”部分，单击“容器” 。

1. 单击“+ 容器”并使用以下设置创建容器：

    | 设置 | 值 |
    | --- | --- |
    | 名称 | az104-07-container  |
    | 公共访问级别 | **专用（不允许匿名访问）** |

1. 在容器列表中，单击 az104-07-container，然后单击“上传”。

1. 浏览到实验室计算机上的“\\Allfiles\\Labs\\07\\LICENSE”，然后单击“打开” 。

1. 在“上传 blob”边栏选项卡上，展开“高级”部分，并指定以下设置（其他设置则保留为默认值） ：

    | 设置 | Value |
    | --- | --- |
    | 身份验证类型 | **帐户密钥**  |
    | Blob 类型 | **块 blob** |
    | 块大小 | **4 MB** |
    | 访问层 | **热访问层** |
    | 上传到文件夹 | **许可证** |

    > **注意**：可为每个 blob 设置访问层。

1. 单击“上载” 。

    > **注意**：请注意，上传时会自动创建一个名为 licenses 的子文件夹。

1. 返回到 az104-07-container 边栏选项卡，单击 licenses，然后单击 LICENSE。

1. 在 licenses/LICENSE 边栏选项卡上，查看可用选项。

    > 你需要评估可否使用 Azure 存储来存储当前位于本地数据存储中的文件。

#### <a name="task-4-manage-authentication-and-authorization-for-azure-storage"></a>任务 4：管理 Azure 存储的身份验证和授权

在此任务中，你将配置 Azure 存储的身份验证和授权。

1. 在“licenses/LICENSE”边栏选项卡的“概述”选项卡上，单击 URL 条目旁边的“复制到剪贴板”按钮   。

1. 使用 InPrivate 模式打开另一个浏览器窗口，并导航到上一步中复制的 URL。

1. 你应该可以看到一个 XML 格式的消息，显示“ResourceNotFound”或“PublicAccessNotPermitted”。

    > **注意**：这很正常，因为你创建的容器将公共访问级别设置成了“专用(禁止匿名访问)”。

1. 关闭 InPrivate 模式浏览器窗口，返回到显示 Azure 存储容器 licenses/LICENSE 边栏选项卡的浏览器窗口，然后切换到“生成 SAS”选项卡。

1. 在 licenses/LICENSE 边栏选项卡的“生成 SAS”选项卡上，指定以下设置（其他设置则保留为默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 签名密钥 | 密钥 1 |
    | 权限 | **读取** |
    | 开始日期 | 昨天的日期 |
    | 开始时间 | 当前时间 |
    | 到期日期 | 明天的日期 |
    | 到期时间 | 当前时间 |
    | 允许的 IP 地址 | 留空 |
    

1. 单击“生成 SAS 令牌和 URL”。

1. 单击“Blob SAS URL”条目旁边的“复制到剪贴板”按钮。

1. 使用 InPrivate 模式打开另一个浏览器窗口，并导航到上一步中复制的 URL。

    > 虽然不经常访问其中大部分文件，但也有一些例外。

    > **注意**：这很正常，因为你现在的访问基于新生成的 SAS 令牌进行授权。

    > 你希望将访问频率较低的文件放在价格较低的存储层中，以最大程度地降低存储成本。

1. 关闭 InPrivate 模式浏览器窗口，返回到显示 Azure 存储容器 licenses/LICENSE 边栏选项卡的浏览器窗口，然后从中导航回 az104-07-container 边栏选项卡。

1. 单击“身份验证方法”标签旁边的“切换到 Azure AD 用户帐户”链接。

    > 你还计划探索 Azure 存储提供的不同保护机制，包括网络访问、身份验证、授权和复制。  

    > **注意**：此时，你没有更改身份验证方法的权限。

1. 在 az104-07-container 边栏选项卡上，单击“访问控制(IAM)” 。

1. 在“检查访问权限”选项卡上，单击“添加角色分配” 。

1. 在“添加角色分配”选项卡上，指定以下设置：

    | 设置 | 值 |
    | --- | --- |
    | 角色 | **存储 Blob 数据所有者** |
    | 将访问权限分配到 | 用户、组或服务主体 |
    | 成员 | 你的用户帐户名称 |

1. 单击“查看 + 分配”，再单击“查看 + 分配”，并返回到 az104-07-container 容器的“概述”边栏选项卡，然后验证是否可将身份验证方法更改为（切换到 Azure AD 用户帐户）   。

    > **注意**：更改可能需要大约 5 分钟才生效。

#### <a name="task-5-create-and-configure-an-azure-files-shares"></a>任务 5：创建并配置 Azure 文件存储共享

在此任务中，你将创建和配置 Azure 文件存储共享。

> **注意**：开始本任务前，请确保你在本实验室第一个任务中预配的虚拟机正在运行。

1. 在 Azure 门户中，导航回到你在本实验室第一个任务中创建的存储帐户的边栏选项卡，然后在“数据存储”部分，单击“文件共享” 。

1. 单击“+ 文件共享”并使用以下设置创建文件共享：

    | 设置 | 值 |
    | --- | --- |
    | 名称 | az104-07-share |

1. 单击新创建的文件共享，然后单击“连接”。

1. 最后，你需要确定 Azure 文件存储服务有多适合用于托管本地文件共享。

1. 在 Azure 门户中，搜索并选择“虚拟机”，然后在虚拟机列表中，单击 az104-07-vm0。

1. 在 az104-07-vm0 边栏选项卡上的“操作”部分，单击“运行命令”。

1. 在“az104-07-vm0 - 运行命令”边栏选项卡上，单击 RunPowerShellScript。

1. 在“运行命令脚本”边栏选项卡上，将先前在此任务中复制的脚本粘贴到“PowerShell 脚本”窗格并单击“运行”。

1. 验证脚本是否成功完成。

1. 将“PowerShell 脚本”窗格中的内容替换为以下脚本，然后单击“运行”：

   ```powershell
   New-Item -Type Directory -Path 'Z:\az104-07-folder'

   New-Item -Type File -Path 'Z:\az104-07-folder\az-104-07-file.txt'
   ```

1. 验证脚本是否成功完成。

1. 导航回到 az104-07-share 文件共享边栏选项卡，单击“刷新”，并验证 az104-07-folder 出现在文件夹列表中。

1. 单击 az104-07-folder，并验证 az104-07-file.txt 出现在文件列表中。

#### <a name="task-6-manage-network-access-for-azure-storage"></a>任务 6：管理 Azure 存储的网络访问

在此任务中，你将配置 Azure 存储的网络访问。

1. 在 Azure 门户中，导航回到你在本实验室第一个任务中创建的存储帐户的边栏选项卡，然后在“安全 + 网络”部分，单击“网络”，然后单击“防火墙和虚拟网络”  。

1. 单击“已从所选虚拟网络和 IP 地址启用”选项，并查看启用此选项后可用的配置设置。

    > **注意**：你可以使用这些设置和服务终结点来配置虚拟网络指定子网上的 Azure 虚拟机与存储帐户之间的直接连接。

1. 单击复选框“添加客户端 IP 地址”，并保存更改。

1. 使用 InPrivate 模式打开另一个浏览器窗口，然后导航到你在上一个任务中生成的 blob SAS URL。

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: If you did not record the SAS URL from task 4, you should generate a new one with the same configuration. Use Task 4 steps 4-6 as a guide for generating a new blob SAS URL. 

1. 系统随即显示“MIT 许可证(MIT)”页面的内容。

    > **注意**：这很正常，因为你从客户端 IP 地址进行连接。

1. 关闭 InPrivate 模式浏览器窗口，返回到显示 Azure 存储帐户的“网络”边栏选项卡的浏览器窗口。

1. 在 Azure 门户中，单击 Azure 门户右上方的图标，打开 Azure Cloud Shell。

1. 如果系统提示选择“Bash”或“PowerShell”，请选择“PowerShell”  。

1. 在 Cloud Shell 窗格中，运行以下命令，尝试从存储帐户的 az104-07-container 容器下载 LICENSE blob（将 `[blob SAS URL]` 占位符替换为你在上一个任务中生成的 blob SAS URL）：

   ```powershell
   Invoke-WebRequest -URI '[blob SAS URL]'
   ```
1. 验证下载尝试是否失败。

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: You should receive the message stating <bpt id="p2">**</bpt>AuthorizationFailure: This request is not authorized to perform this operation<ept id="p2">**</ept>. This is expected, since you are connecting from the IP address assigned to an Azure VM hosting the Cloud Shell instance.

1. 关闭 Cloud Shell 窗格。

#### <a name="clean-up-resources"></a>清理资源

><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a long time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. You might also try to delete the Resource Group where the resources reside. That is a quick Administrator shortcut. If you have concerns speak to your instructor.

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

1. 运行以下命令，列出在本模块各实验室中创建的所有资源组：

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*'
   ```

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。

#### <a name="review"></a>审阅

在此实验室中，你执行了以下操作：

- 预配实验室环境
- 创建和配置 Azure 存储帐户
- 管理 Blob 存储
- 管理 Azure 存储的身份验证和授权
- 创建和配置 Azure 文件存储共享
- 管理 Azure 存储的网络访问
