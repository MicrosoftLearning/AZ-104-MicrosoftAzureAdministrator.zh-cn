---
lab:
  title: 实验室 09c：实现 Azure 容器应用
  module: Administer PaaS Compute Options
---

# 实验室 09c - 实现 Azure 容器应用

## 实验室简介

在本实验室中，了解如何实现和部署 Azure 容器应用。

本实验室需要 Azure 订阅。 订阅类型可能会影响此实验室中功能的可用性。 可更改区域，但这些步骤是使用“美国东部”编写的****。

## 预计用时：15 分钟

## 实验室方案

你的组织有一个在本地数据中心的虚拟机上运行的 Web 应用程序。 组织希望将所有应用程序移动到云，但不希望拥有大量要管理的服务器。 你决定评估 Azure 容器应用。

## 交互式实验室模拟

本主题没有交互式实验室模拟。 

## 体系结构关系图

![任务关系图。](../media/az104-lab09b-aca-architecture.png)

## 工作技能

- 任务 1：创建并配置 Azure 容器应用和环境。
- 任务 2：测试并验证 Azure 容器应用的部署。

## 任务 1：创建并配置 Azure 容器应用和环境

Azure 容器应用将托管 Kubernetes 群集的概念向前推进了一步，管理群集环境，并在群集之上提供其他托管服务。 与 Azure Kubernetes 群集（在该群集中，仍必须管理群集）不同，Azure 容器应用实例减少了一些设置 Kubernetes 群集的复杂性。

1. 在 Azure 门户中，搜索并选择 `Container Apps`。

1. 选择“**+ 创建**”，从下拉菜单中选择“**容器应用**”。 注意其他选择。 

1. 使用以下信息在“基本信息”选项卡上填写详细信息。*****

    | 设置 | 操作 |
    |---|---|
    | 订阅 | 选择 Azure 订阅 |
    | 资源组 | `az104-rg9` |
    | 容器应用名称 |  `my-app` |
    | 区域    | **美国东部**（|
    | 容器应用环境 | 选择“**新建**”> 将“环境名称”设置为`my-environment` > “**创建**” |

1. 单击“**下一步: 容器**”选项卡，确保选中“**使用快速入门映像**”。 可能需要向上滚动才能查看此设置。 

1. 确保将“**快速入门映像**”设置为“**简单的 Hello World 容器**”。 注意其他选择。 

1. 选择“查看并创建”，然后选择“创建”********。

    >**注意：** 等待容器应用进行部署。 几分钟后即可创建完毕。 
 
## 任务 2：测试并验证 Azure 容器应用的部署

默认情况下，创建的 Azure 容器应用将使用示例 Hello World 应用程序接受端口 80 上的流量。 Azure 容器应用将为应用程序提供 DNS 名称。 复制并导航到此 URL，以确保应用程序已启动并运行。

1. 选择“转到资源”，查看新的容器应用。

1. 选择“应用程序 URL”旁边的链接以查看应用程序。

    ![门户中 ACA 概述页的屏幕截图。](../media/az104-lab09b-aca-overview.png)

1. 确认你收到了“你的 Azure 容器应用已上线”消息。
   
## 清理资源

如果使用自己的订阅，需要一点时间删除实验室资源****。 这将确保资源得到释放，并将成本降至最低。 删除实验室资源的最简单方法是删除实验室资源组。 

+ 在 Azure 门户中，选择资源组，选择“删除资源组”，输入资源组名称，然后单击“删除”************。
+ `Remove-AzResourceGroup -Name resourceGroupName`（使用 Azure PowerShell）。
+ `az group delete --name resourceGroupName`（使用 CLI）。

## 使用 Copilot 扩展学习
Copilot 可帮助你了解如何使用 Azure 脚本工具。 Copilot 还可以帮助了解实验室中未涵盖的领域或需要更多信息的领域。 打开 Edge 浏览器并选择“Copilot”（右上角）或导航到*copilot.microsoft.com*。 花几分钟时间尝试这些提示。

+ 总结创建和配置 Azure 容器应用的步骤。
+ 将 Azure 容器应用与 Azure Kubernetes 服务进行比较和对比。

## 通过自定进度的培训了解详细信息

+ [在 Azure 容器应用中配置容器应用](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/)。 探讨 Azure 容器应用的特性和功能，然后重点介绍如何使用 Azure 容器应用创建、配置、缩放和管理容器应用。


## 关键结论

恭喜你完成本实验室的内容。 下面是本实验室的主要内容。 

+ Azure 容器应用 (ACA) 是一个无服务器平台，可让你在运行容器化应用程序时保持更少的基础结构并节省成本。
+ 容器应用提供服务器配置、容器业务流程和部署详细信息。 
+ ACA 上的工作负载通常是长时间运行的进程，例如 Web 应用。

     
