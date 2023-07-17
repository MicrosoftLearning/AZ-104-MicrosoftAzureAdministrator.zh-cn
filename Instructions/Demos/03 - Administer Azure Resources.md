---
demo:
  title: 演示 03：管理 Azure 资源
  module: Administer Administer Azure Resources
---
# 03 - 管理 Azure 资源

## 演示 - Azure 门户

在此演示中，我们将探讨 Azure 门户。

参考：[管理 Azure 门户设置和首选项](https://docs.microsoft.com/azure/azure-portal/set-preferences)

[参考：在 Azure 门户中创建仪表板](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)

[参考：如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

1. 访问 Azure 门户。

1. 选择顶部横幅上的“支持和故障排除”图标 **** 。 查看“支持资源”链接。 

1. 选择顶部横幅上的“设置”图标。 查看“外观 + 启动视图”设置。 

1. 使用左侧菜单并选择“仪表板”。 使用“磁贴库”编辑仪表板 。 讨论自定义选项。 

1. 询问学生是否对如何使用 Azure 门户有任何疑问。 

## 演示 - Cloud Shell

在本演示中，我们将尝试使用 Cloud Shell。

参考：[Azure Cloud Shell 快速入门](https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?tabs=azurecli)

**配置 Cloud Shell**

1.  访问 Azure 门户 **** 。

1.  单击顶部横幅上的 Cloud Shell 图标 ****  。

1.  在“欢迎使用 Shell”页面上，注意 Bash 或 PowerShell 等选项。 选择“PowerShell” **** 。

1.  讨论 Azure Cloud Shell 如何需要 Azure 文件共享来保留文件。 如有必要，请配置存储共享。 

**尝试使用 Azure PowerShell 和 Bash**

1. 确保已选择 PowerShell shell，并尝试使用一些命令。 例如 Get-AzSubscription 和 Get-AzResourceGroup 。

1. 显示自动完成的工作原理。 显示如何清除屏幕 cls。 

1. 确保已选择 Bash shell，并尝试使用一些命令。 例如，az account list 和 az resource list 。

1. 询问学生是否对使用 PowerShell 或 Bash 命令有任何疑问。 

**使用 Cloud shell 编辑器进行试验（可选）**

1. 若要使用 Cloud Editor，请选择大括号图标。

1. 从左侧导航窗格中选择一个文件。 例如，.profile **** 。

1. 请注意编辑器顶部横幅，设置（文本大小和字体）和上传/下载文件等选项。

1. 请注意最右侧省略号 (\...)，用于保存、关闭编辑器和打开文件   。

1. 如果有时间，请尝试进行操作，然后关闭 Cloud Editor ****  。

1. 关闭 Cloud Shell。

## 演示 - 快速启动模板

在本演示中，我们将探索快速入门模版。

[参考：教程 - 创建和部署模板 - Azure 资源管理器](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

1. 首先浏览到  [Azure 快速入门模板库](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager)。 请注意，有 JSON 和 Bicep 示例。 

1. 询问学生是否有任何感兴趣的特定模板。 如果没有，请选择模板。 例如，[使用标记部署简单的 Windows VM ](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/vm-tags/) 模板。

1. 讨论“部署到 Azure”按钮如何支持直接通过 Azure 门户部署模板 ****  。

1. 部署 JSON 模板并讨论如何编辑模板和参数文件。 查看文件的用途。 如果有时间，可查看语法。 

1. 返回到代码示例库，并找到 Bicep 模板。 例如，[创建标准存储帐户](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/storage-account-create/)。 

1. 部署 Bicep 模板并讨论如何编辑模板和参数文件。 如果有时间，可查看语法。 
