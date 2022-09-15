---
lab:
  title: 03c - 使用 Azure PowerShell 管理 Azure 资源
  module: Administer Azure Resources
---

# <a name="lab-03c---manage-azure-resources-by-using-azure-powershell"></a>实验室 03c - 使用 Azure PowerShell 管理 Azure 资源
# <a name="student-lab-manual"></a>学生实验室手册

## <a name="lab-scenario"></a>实验室方案

Now that you explored the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups by using the Azure portal and Azure Resource Manager templates, you need to carry out the equivalent task by using Azure PowerShell. To avoid installing Azure PowerShell modules, you will leverage PowerShell environment available in Azure Cloud Shell.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%206)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>目标

在此实验中，将执行以下操作：

+ 任务 1：在 Azure Cloud Shell 中启动 PowerShell 会话
+ 任务 2：使用 Azure PowerShell 创建资源组和 Azure 托管磁盘
+ 任务 3：使用 Azure PowerShell 配置托管磁盘

## <a name="estimated-timing-20-minutes"></a>预计用时：20 分钟

## <a name="architecture-diagram"></a>体系结构关系图

![image](../media/lab03c.png)

## <a name="instructions"></a>说明

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Always create your own secure password for any virtual machine or user account you create. If the virtual machine is created for you, use <bpt id="p1">**</bpt>Reset password<ept id="p1">**</ept> in the Portal to update the password. 

### <a name="exercise-1"></a>练习 1

#### <a name="task-1-start-a-powershell-session-in-azure-cloud-shell"></a>任务 1：在 Azure Cloud Shell 中启动 PowerShell 会话

在此任务中，你将在 Cloud Shell 中打开一个 PowerShell 会话。 

1. 单击 Azure 门户右上方的图标，在门户中打开 Azure Cloud Shell。

1. 如果系统提示选择“Bash”或“PowerShell”，请选择“PowerShell”  。 

    >**注意**：如果这是你第一次启动 Cloud Shell，并看到消息“未装载任何存储”，请选择你将在本实验室中使用的订阅，然后选择“创建存储”  。 

1. 如果出现提示，请单击“创建存储”，然后等到出现“Azure Cloud Shell”窗格。 

1. 确保“PowerShell”出现在 Cloud Shell 窗格左上角的下拉菜单中。

#### <a name="task-2-create-a-resource-group-and-an-azure-managed-disk-by-using-azure-powershell"></a>任务 2：使用 Azure PowerShell 创建资源组和 Azure 托管磁盘

在此任务中，你将在 Cloud Shell 中使用 Azure PowerShell 会话来创建资源组和 Azure 托管磁盘

1. 要通过 Cloud Shell 中的 PowerShell 会话创建一个资源组，并且使该资源组与上一个实验室中创建的资源组 **az104-03b-rg1** 位于同一 Azure 区域，请运行以下命令：

   ```powershell
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. 要检索新创建资源组的属性，请运行以下命令：

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```
1. 要创建一个新的托管磁盘，并使该托管磁盘与你在本模块先前的实验室中创建的托管磁盘具有相同特征，请运行以下命令：

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -Sku Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. 要检索新建磁盘的属性，请运行以下命令：

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

#### <a name="task-3-configure-the-managed-disk-by-using-azure-powershell"></a>任务 3：使用 Azure PowerShell 配置托管磁盘

在此任务中，你将使用 Cloud Shell 中的 Azure PowerShell 会话来管理 Azure 托管磁盘的配置。 

1. 要通过 Cloud Shell 中的 PowerShell 会话将 Azure 托管磁盘的大小增加到 64 GB，请运行以下命令：

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. 要验证更改是否生效，请运行以下命令：

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. 要验证当前 SKU 是否为 Standard_LRS，请运行以下命令：

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. 要通过 Cloud Shell 中的 PowerShell 会话将磁盘性能 SKU 更改为“Premium_LRS”，请运行以下命令：

   ```powershell
   New-AzDiskUpdateConfig -Sku Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. 要验证更改是否生效，请运行以下命令：

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

#### <a name="clean-up-resources"></a>清理资源

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not delete resources you deployed in this lab. You will reference them in the next lab of this module.

#### <a name="review"></a>审阅

在此实验室中，你执行了以下操作：

- 在 Azure Cloud Shell 中开启 PowerShell 会话
- 使用 Azure PowerShell 创建一个资源组和一个 Azure 托管磁盘
- 使用 Azure PowerShell 配置托管磁盘
