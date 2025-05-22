---
lab:
  title: 实验室 03：使用 Azure 资源管理器模板管理 Azure 资源
  module: Administer Azure Resources
---

# 实验室 03 - 使用 Azure 资源管理器模板管理 Azure 资源

## 实验室简介

在本实验室中，你将了解如何自动执行资源部署。 你将了解 Azure 资源管理器模板和 Bicep 模板。 你将了解用来部署模板的各种方式。 

本实验室需要 Azure 订阅。 订阅类型可能会影响此实验室中功能的可用性。 可更改区域，但这些步骤是使用“美国东部”编写的****。 

## 预计用时：50 分钟

## 交互式实验室模拟

你可能会发现一些交互式实验室模拟对本主题很有用。 通过模拟，可按照自己的节奏点击浏览类似的场景。 交互式模拟与本实验室之间存在差异，但许多核心概念是相同的。 不需要 Azure 订阅。 

+ [](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205)使用 Azure 资源管理器模板管理 Azure 资源。 使用模板查看、创建和部署托管磁盘。
  
+ [](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209)使用模板创建虚拟机。 使用快速启动模板部署一个虚拟机。
  
## 实验室方案

你的团队希望找到自动化和简化资源部署的方法。 你的组织正在寻找减少管理开销、减少人为错误和提高一致性的方法。  

## 体系结构关系图

![任务关系图。](../media/az104-lab03-architecture.png)

## 工作技能

+ 任务 1： 创建 Azure 资源管理器模板。
+ 任务 2：编辑 Azure 资源管理器模板并重新部署模板。
+ 任务 3：使用 Azure PowerShell 配置 Cloud Shell 并部署模板。
+ 任务 4：使用 CLI 部署模板。 
+ 任务 5：使用 Azure Bicep 部署资源。

## 任务 1：创建 Azure 资源管理器模板

在此任务中，我们将在 Azure 门户中创建托管磁盘。 托管磁盘是设计用于虚拟机的存储。 部署该磁盘后，你将导出一个模板，可在其他部署中使用该模板。

1. 登录 **Azure 门户** - `https://portal.azure.com`。

1. 搜索并选择 `Disks`。 
   
1. 在“磁盘”页上，选择“创建”。****

1. 在“创建托管磁盘”页上，配置该磁盘，然后选择“确定”。******** 
    
    | 设置 | 值 |
    | --- | --- |
    | 订阅 | *用户的订阅* | 
    | 资源组 | `az104-rg3`（如有必要，请选择“新建”。****）
    | 磁盘名称 | `az104-disk1` | 
    | 区域 | **美国东部** |
    | 可用性区域 | 不需要基础结构冗余 | 
    | 源类型 | **无** |
    | 性能 | **** 标准 HDD（更改大小） |
    | 大小 | **** 32 Gib | 

    >**注意：** 我们将创建一个简单的托管磁盘，以便你可以使用模板进行练习。 Azure 托管磁盘是由 Azure 管理的块级存储卷。

1. 单击“查看 + 创建”，然后选择“创建”。********

1. 监视通知（右上角），在部署后选择“转到资源”。**** 

1. 在“自动化”边栏选项卡中，选择“导出模板”。******** 

1. 花点时间查看 **Template** 和 **Parameters** 文件。

1. 单击“下载”并将模板保存到本地驱动器。**** 这会创建压缩的 zip 文件。 

1. 使用文件资源管理器将已下载文件的内容提取到你的计算机上的“下载”文件夹。**** 请注意，有两个 JSON 文件（template 和 parameters）。 

   >你知道吗？****  你可以导出整个资源组，也可以仅导出该资源组中的特定资源。

## 任务 2：编辑一个 Azure 资源管理器模板，然后重新部署该模板

在此任务中，你将使用下载的模板来部署新的托管磁盘。 此任务概述了如何快速轻松地重复进行部署。 

1. 在 Azure 门户中，搜索并选择`Deploy a custom template`。

1. 在“自定义部署”边栏选项卡上，请注意可以使用“快速启动模板”。******** 如下拉菜单中所示，有许多内置模板。 

1. 不使用快速启动，请选择“在编辑器中构建自己的模板”。****

1. 在“编辑模版”边栏选项卡上，单击“加载文件”并上传你下载到本地磁盘的 template.json 文件。************

1. 在编辑器窗格中，进行这些更改。

    + 将 **disks_az104_disk1_name** 更改为 `disk_name`（需要更改两处）
    + 将 az104-disk1 更改为 `az104-disk2`（要更改一处）****

1. 请注意，这是一个标准磁盘。**** 位置是 **eastus**。 磁盘大小是 **32GB**。

1. **保存**所做更改。

1. 不要忘记 parameters 文件。 选择“编辑参数”，单击“加载文件”并上传 **parameters.json**。******** 

1. 进行此更改，使其与模板文件匹配。

    将 **disks_az104_disk1_name** 更改为 **disk_name**（需要更改一处）

1. **保存**所做更改。 

1. 完成自定义部署设置：

    | 设置 | 值 |
    | --- |--- |
    | 订阅 | *用户的订阅* |
    | 资源组 | `az104-rg3` |
    | 区域 | **(美国)美国东部** |
    | Disk_name | `az104-disk2` |

1. 选择“查看 + 创建”，然后选择“创建”。

1. 选择“转到资源”。 验证 **az104-disk2** 已创建。

1. 在“概述”边栏选项卡上，选择资源组 **az104-rg3**。**** 你现在应该有两个磁盘。
   
1. 在“设置”部分中，单击“部署”。********

    >**注意：** 所有部署详细信息都记录在资源组中。 在使用模板进行大规模操作之前，最好先查看前几次基于模板的部署以确保成功。

1. 选择一个部署并查看“输入”和“模板”边栏选项卡的内容。********

## 任务 3：配置 Cloud Shell 并使用 PowerShell 部署模板 

在此任务中，你将使用 Azure Cloud Shell 和 Azure PowerShell。 Azure Cloud Shell 是一个交互式、经过身份验证且可通过浏览器访问的终端，用于管理 Azure 资源。 它使用户能够灵活选择最适合自己工作方式的 shell 体验，无论是 Bash 还是 PowerShell。 在此任务中，你将使用 PowerShell 来部署模板。 

1. 选择 Azure 门户右上角的 Cloud Shell 图标。**** 或者，可以直接导航到 `https://shell.azure.com`。

   ![Cloud Shell 图标的屏幕截图。](../media/az104-lab03-cloudshell-icon.png)

1. 如果系统提示选择“Bash”**** 或“PowerShell”****，请选择“PowerShell”****。 

    >你知道吗？****  如果你主要使用 Linux 系统，Bash (CLI) 会让你感觉更熟悉。 如果你主要使用 Windows 系统，Azure PowerShell 会让你感觉更熟悉。 

1. 在“入门”屏幕上，选择“装载存储帐户”，选择你的存储帐户订阅，然后选择“应用”****************。

1. 选择“我想创建存储帐户”，然后选择“下一步”********。 完成“创建存储帐户”信息****。 
    
    | 设置 | 值 |
    |  -- | -- |
    | 资源组 | **az104-rg3** |
    | 区域 | ** 选择你的区域 | 
    | 存储帐户（新建） | *必须全局唯一，长度为 3 到 24 个字符，并且只能使用数字和小写字母* |
    | 文件共享（新建） | `fs-cloudshell` |

1. 完成后，选择“创建”****。

    >预配存储将需要几分钟时间。

1. 选择“设置”（顶部栏），然后选择“转到经典版本”********。

1. 选择“上传/下载文件”图标（顶部栏），然后选择“上传”********。

1. 从“下载”目录上传模板和参数文件****。 

1. 选择“编辑器”（大括号）图标，然后在导航窗格中导航到左侧的模板 JSON 文件****。

1. 进行更改。 例如，将磁盘名称更改为 **az104-disk3**。 按 **Ctrl +S** 保存所做的更改。 

    >**注意**：你可以将模板部署的目标定为资源组、订阅、管理组或租户。 你将根据部署范围使用不同的命令。

1. 若要部署到资源组，请使用 **New-AzResourceGroupDeployment**。

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. 确保命令完成并且 ProvisioningState 为“已成功”。****

1. 确认该磁盘已创建。

   ```powershell
   Get-AzDisk
   ```
   
## 任务 4：使用 CLI 部署模板 

1. 还是在 Cloud Shell 中，选择“Bash”。******** 确认你的选择。****

1. 验证你的文件是否在 Cloud Shell 存储中可用。 如果已完成前面的任务，则模板文件应当可用。 

    ```sh
    ls
    ```

1. 选择“编辑器”（大括号）图标并导航到模板 JSON 文件****。

1. 进行更改。 例如，将磁盘名称更改为 **az104-disk4**。 按 **Ctrl +S** 保存所做的更改。 

    >**注意**：你可以将模板部署的目标定为资源组、订阅、管理组或租户。 你将根据部署范围使用不同的命令。

1. 若要部署到资源组，请使用 **az deployment group create**。

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
    
1. 确保命令完成并且 ProvisioningState 为“已成功”。****

1. 确认该磁盘已创建。

     ```sh
     az disk list --output table
     ```
   
## 任务 5：使用 Azure Bicep 部署资源

在此任务中，你将使用 Bicep 文件部署托管磁盘。 Bicep 是基于 ARM 模板构建的声明性自动化工具。

1. 找到 **\Allfiles\Lab03\azuredeploydisk.bicep** 文件。

1. 继续在 Cloud Shell 中的 Bash 会话中工作。********

1. 选择“**管理文件**”，然后将 Bicep 文件“**上传**”到 Cloud Shell。 

1. 单击“**编辑器**”，出现提示时，请**确认**切换到经典 Cloud Shell。

1. 选择 **azuredeploydisk.bicep** 文件 

1. 花一分钟时间从头到尾阅读 Bicep 模板文件。 请注意磁盘资源的定义方式。 
   
1. 做出以下更改：

    + 将 **managedDiskName** 值（第 2 行）更改为 **az104-disk5**。
    + 将 **sku 名称**值（第 26 行）更改为 **StandardSSD_LRS**。
    + 将 **diskSizeinGiB** 值（第 7 行）更改为 **32**。
    
1. 使用 **Ctrl + S** 保存所做的更改。

1. 现在，部署该模板。

    ```
    az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
    ```

1. 确认该磁盘已创建。

    ```sh
    az disk list --output table
    ```

    >**注意：** 你已成功部署了五个托管磁盘，每个磁盘采用不同的方式。 做得不错！

## 清理资源

如果使用自己的订阅，需要一点时间删除实验室资源****。 这将确保资源得到释放，并将成本降至最低。 删除实验室资源的最简单方法是删除实验室资源组。 

+ 在 Azure 门户中，选择资源组，选择“删除资源组”，输入资源组名称，然后单击“删除”************。
+ `Remove-AzResourceGroup -Name resourceGroupName`（使用 Azure PowerShell）。
+ `az group delete --name resourceGroupName`（使用 CLI）。

## 使用 Copilot 扩展学习

Copilot 可帮助你了解如何使用 Azure 脚本工具。 Copilot 还可以帮助了解实验室中未涵盖的领域或需要更多信息的领域。 打开 Edge 浏览器并选择“Copilot”（右上角）或导航到*copilot.microsoft.com*。 花几分钟时间尝试这些提示。

+ Azure 资源管理器模板文件的格式是什么？ 使用示例解释每个组件。 
+ 如何使用现有的 Azure 资源管理器模板？
+ 比较和对比 Azure 资源管理器模板和 Azure Bicep 模板。 


## 通过自定进度的培训了解详细信息

+ [](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/)使用 JSON ARM 模板部署 Azure 基础结构。 使用 Visual Studio Code 编写 JSON Azure 资源管理器模板（ARM 模板），以一致且可靠的方式将基础结构部署到 Azure。
+ [](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/)回顾 Azure Cloud Shell 的功能和工具。 Cloud Shell 功能和工具。 
+ [](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/)使用 Windows PowerShell 管理 Azure 资源。 本模块介绍如何安装云服务管理所需的模块，以及如何使用 PowerShell 命令对云资源（例如 Azure 虚拟机、Azure 订阅和 Azure 存储帐户）执行简单的管理任务。
+ [](https://learn.microsoft.com/training/modules/bash-introduction/)Bash 简介。 使用 Bash 管理 IT 基础结构。
+ [](https://learn.microsoft.com/training/modules/build-first-bicep-template/)构建你的第一个 Bicep 模板。 在 Bicep 模板中定义 Azure 资源。 提高部署的一致性和可靠性，减少所需的手动工作量，并跨环境缩放部署。 通过使用参数、变量、表达式和模块，你的模板将非常灵活，并且可以重复使用。

## 关键结论

恭喜你完成本实验室的内容。 下面是本实验室的主要内容。 

+ Azure 资源管理器模板可以将解决方案中的所有资源作为一个组进行部署、管理和监视，而无需分别处理这些资源。
+ Azure 资源管理器模板是一个 JavaScript 对象表示法 (JSON) 文件，可用于以声明方式而不是使用脚本来管理基础结构。
+ 你可以使用包含参数值的一个单独 JSON 文件，而不是在模板中以内联值的形式传递参数。
+ Azure 资源管理器模板可以采用各种方式进行部署，包括 Azure 门户、Azure PowerShell 和 CLI。
+ Bicep 是 Azure 资源管理器模板的一种替代方法。 Bicep 使用声明性语法来部署 Azure 资源。
+ Bicep 提供简洁的语法、可靠的类型安全，并支持代码重用。 Bicep 会针对你的 Azure 基础结构即代码解决方案提供一流创作体验。


