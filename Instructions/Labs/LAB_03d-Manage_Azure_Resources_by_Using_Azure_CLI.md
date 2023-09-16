---
lab:
  title: 实验 03d：使用 Azure CLI 管理 Azure 资源（可选）
  module: Administer Azure Resources
---

# 实验室 03d - 使用 Azure CLI 管理 Azure 资源
# 学生实验室手册

## 实验室方案

你已经探索了与使用 Azure 门户、Azure 资源管理器模板和 Azure PowerShell 预配资源并根据资源组整理资源相关的基本 Azure 管理功能，现在需要使用 Azure CLI 来执行等效的任务。 为了避免安装 Azure CLI，你将利用 Azure Cloud Shell 中提供的 Bash 环境。

                **注意：** 我们提供 **[交互式实验室模拟](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%207)** ，让你能以自己的节奏点击浏览实验室。 你可能会发现交互式模拟与托管实验室之间存在细微差异，但演示的核心概念和思想是相同的。 

## 目标

在此实验中，将执行以下操作：

+ 任务 1：在 Azure Cloud Shell 中启动 Bash 会话
+ 任务 2：使用 Azure CLI 创建资源组和 Azure 托管磁盘
+ 任务 3：使用 Azure CLI 配置托管磁盘

## 预计用时：20 分钟

## 体系结构关系图

![image](../media/lab03d.png)

### Instructions

## 练习 1

## 任务 1：在 Azure Cloud Shell 中启动 Bash 会话

在此任务中，你将在 Cloud Shell 中打开 Bash 会话。 

1. 单击 Azure 门户右上方的图标，在门户中打开 Azure Cloud Shell。

1. 如果系统提示选择“Bash”或“PowerShell”，请选择“Bash”。 

    >**注意**：如果这是你第一次启动 Cloud Shell，并看到消息“未装载任何存储”，请选择你将在本实验室中使用的订阅，然后选择“创建存储”  。 

1. 如果出现提示，请单击“创建存储”，然后等到出现“Azure Cloud Shell”窗格。 

1. 确保“Bash”出现在“Cloud Shell”窗格左上角的下拉菜单中。

## 任务 2：使用 Azure CLI 创建资源组和 Azure 托管磁盘

在此任务中，你将使用 Cloud Shell 中的 Azure CLI 会话来创建资源组和 Azure 托管磁盘。

1. 要通过 Cloud Shell 中的 Bash 会话创建一个资源组，并且使该资源组与上一个实验室中创建的资源组 az104-03c-rg1 位于同一 Azure 区域，请运行以下命令：

   ```sh
   LOCATION=$(az group show --name 'az104-03c-rg1' --query location --out tsv)

   RGNAME='az104-03d-rg1'

   az group create --name $RGNAME --location $LOCATION
   ```
1. 要检索新创建资源组的属性，请运行以下命令：

   ```sh
   az group show --name $RGNAME
   ```
1. 要通过 Cloud Shell 中的 Bash 会话创建一个新的托管磁盘，并使该托管磁盘与你在本模块先前的实验室中创建的托管磁盘具有相同特征，请运行以下命令：

   ```sh
   DISKNAME='az104-03d-disk1'

   az disk create \
   --resource-group $RGNAME \
   --name $DISKNAME \
   --sku 'Standard_LRS' \
   --size-gb 32
   ```
    >**注意**：使用多行语法时，请确保每行都以反斜杠 (`\`) 结尾且没有尾随空格，并确保每行开头都没有前导空格。

1. 要检索新建磁盘的属性，请运行以下命令：

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME
   ```

## 任务 3：使用 Azure CLI 配置托管磁盘

在此任务中，你将使用 Cloud Shell 中的 Azure CLI 会话来管理 Azure 托管磁盘的配置。 

1. 要通过 Cloud Shell 中的 Bash 会话将 Azure 托管磁盘的大小增加到 64 GB，请运行以下命令：

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --size-gb 64
   ```

1. 要验证更改是否生效，请运行以下命令：

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query diskSizeGB
   ```

1. 要通过 Cloud Shell 中的 Bash 会话将磁盘性能 SKU 更改为“Premium_LRS”，请运行以下命令：

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --sku 'Premium_LRS'
   ```

1. 要验证更改是否生效，请运行以下命令：

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query sku
   ```

## 清理资源

 > **注意**：记得删除所有不再使用的新建 Azure 资源。 删除未使用的资源可确保不会出现意外费用。

 > **注意**：如果不能立即删除实验室资源，也不要担心。 有时资源具有依赖项，需要较长的时间才能删除。 这是监视资源使用情况的常见管理员任务，因此，只需定期查看门户中的资源即可查看清理方式。 

1. 在 Azure 门户中，在 Cloud Shell 窗格中打开 Bash Shell 会话 。

1. 运行以下命令，列出在本模块各实验室中创建的所有资源组：

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].name" --output tsv
   ```

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >**注意**：该命令以异步方式执行（由 --nowait 参数确定），因此，尽管可立即在同一 Bash 会话中运行另一个 Azure CLI 命令，但实际上要花几分钟才能删除资源组。

## 审阅

在此实验室中，你执行了以下操作：

- 在 Azure Cloud Shell 中启动 Bash 会话
- 使用 Azure CLI 创建资源组和 Azure 托管磁盘
- 使用 Azure CLI 配置托管磁盘
