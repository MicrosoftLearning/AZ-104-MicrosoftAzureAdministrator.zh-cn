---
lab:
  title: 实验室 09b：实现 Azure 容器实例
  module: Administer PaaS Compute Options
---

# 实验室 09b - 实现 Azure 容器实例

## 实验室简介

在本实验室中，了解如何实现和部署 Azure 容器实例。

本实验室需要 Azure 订阅。 订阅类型可能会影响此实验室中功能的可用性。 可更改区域，但这些步骤是使用“美国东部”编写的****。

## 预计用时：15 分钟

## 实验室方案

你的组织有一个在本地数据中心的虚拟机上运行的 Web 应用程序。 组织希望将所有应用程序移动到云，但不希望拥有大量要管理的服务器。 你决定评估 Azure 容器实例和 Docker。 
## 交互式实验室模拟

你可能会发现一些交互式实验室模拟对本主题很有用。 通过模拟，可按照自己的节奏点击浏览类似的场景。 交互式模拟与本实验室之间存在差异，但许多核心概念是相同的。 不需要 Azure 订阅。

+ [部署 Azure 容器实例](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%203)。 使用 Azure 容器实例创建、配置和部署 Docker 容器。
  
+ [实现 Azure 容器实例](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014)。  使用 Azure 容器实例部署 Docker 映像。 

## 体系结构关系图

![任务关系图。](../media/az104-lab09b-aci-architecture.png)

## 工作技能

- 任务 1：使用 Docker 映像部署 Azure 容器实例。
- 任务 2：测试并验证 Azure 容器实例的部署。

## 任务 1：使用 Docker 映像部署 Azure 容器实例

在此任务中，你将使用 Docker 映像创建一个简单的 Web 应用程序。 Docker 是一个平台，可用于在称为容器的独立环境中打包和运行应用程序。 Azure 容器实例为容器映像提供计算环境。

1. 登录 **Azure 门户** - `https://portal.azure.com`。

1. 在 Azure 门户中，搜索并选择 `Container instances`，然后在“容器实例”边栏选项卡上，单击“+ 创建”********。

1. 在“创建容器实例”边栏选项卡的“基本设置”选项卡中，指定以下设置（其他设置保留默认值） ：

    | 设置 | 值 |
    | ---- | ---- |
    | 订阅 | 选择 Azure 订阅 |
    | 资源组 | `az104-rg9`（如有必要，请选择“新建”****） |
    | 容器名称 | `az104-c1` |
    | 区域 | 美国东部****（或你附近可用的区域）|
    | 映像源 | **快速启动映像** |
    | 映像 | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. 单击“下一步:**网络”>** 指定以下设置（其他设置保留默认值）：

    | 设置 | 值 |
    | --- | --- |
    | DNS 名称标签 | 任何有效且全局唯一的 DNS 主机名 |

    >**注意**：可以在 dns-name-label.region.azurecontainer.io 公开访问你的容器。 如果你收到了“DNS 名称标签不可用”的错误消息，请指定其他值。

1. 单击“**下一步: 监视 >**”并取消选中“**启用容器实例日志**”。 

1. 单击“下一步:**高级”>**，查看设置而不进行任何更改。

1. 单击“查看 + 创建”，确保通过验证，然后选择“创建”********。

    >备注：请等待部署完成。 这应该需要 2-3 分钟。

    >**注意**：在等待期间，你可能有兴趣查看[示例应用程序背后的代码](https://github.com/Azure-Samples/aci-helloworld)。 若要查看代码，请浏览 \\app 文件夹。

## 任务 2：测试并验证 Azure 容器实例的部署 

在此任务中，你将查看容器实例的部署。 默认情况下，可以通过端口 80 访问 Azure 容器实例。 部署实例后，可以使用上一任务中提供的 DNS 名称导航到容器。

1. 部署完成后，选择“**转到资源**”链接。

1. 在容器实例“概述”边栏选项卡中，验证“状态”是否报告为“正在运行”。

1. 复制容器实例的值“FQDN”，打开一个新的浏览器选项卡，然后导航到相应的 URL。

     ![门户中 ACI 概述页的屏幕截图。](../media/az104-lab09b-aci-overview.png)

1. 验证“欢迎使用 Azure 容器实例”的页面是否显示。 多次刷新页面以创建一些日志条目，然后关闭浏览器选项卡。  

1. 在容器实例边栏选项卡的“设置”部分中，单击“容器”，然后单击“日志”************。

1. 验证你是否看到代表通过在浏览器中显示应用程序生成的 HTTP GET 请求的日志条目。
   
## 清理资源

如果使用自己的订阅，需要一点时间删除实验室资源****。 这将确保资源得到释放，并将成本降至最低。 删除实验室资源的最简单方法是删除实验室资源组。 

+ 在 Azure 门户中，选择资源组，选择“删除资源组”，输入资源组名称，然后单击“删除”************。
+ `Remove-AzResourceGroup -Name resourceGroupName`（使用 Azure PowerShell）。
+ `az group delete --name resourceGroupName`（使用 CLI）。

## 使用 Copilot 扩展学习
Copilot 可帮助你了解如何使用 Azure 脚本工具。 Copilot 还可以帮助了解实验室中未涵盖的领域或需要更多信息的领域。 打开 Edge 浏览器并选择“Copilot”（右上角）或导航到*copilot.microsoft.com*。 花几分钟时间尝试这些提示。

+ 总结创建和配置 Azure 容器实例的步骤。
+ 在 Azure 上运行无服务器容器的方法有哪些？

## 通过自定进度的培训了解详细信息

+ [在 Azure 容器实例中运行容器映像](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/)。 了解 Azure 容器实例如何帮你快速部署容器，如何设置环境变量，以及指定容器重启策略。

## 关键结论

恭喜你完成本实验室的内容。 下面是本实验室的主要内容。 

+ Azure 容器实例 (ACI) 是一项服务，可用于在 Microsoft Azure 公有云上部署容器。
+ ACI 不需要你预配或管理任何基础结构。
+ ACI 支持 Linux 容器和 Windows 容器。
+ ACI 上的工作负荷通常由某种进程或触发器启动和停止，通常生存期较短。 

    
