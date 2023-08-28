---
demo:
  title: 演示 09：管理 PaaS 计算选项
  module: Administer PaaS Compute Options
---

# 09 - 管理 PaaS 计算选项

## 配置 Azure 应用服务计划

在本演示中，将创建并使用 Azure 应用服务计划。

[参考：管理应用服务计划 - Azure 应用服务](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

参考：[在 Azure 应用服务中纵向扩展应用](https://learn.microsoft.com/azure/app-service/manage-scale-up)

参考：[Azure 应用服务中的自动扩缩](https://learn.microsoft.com/azure/app-service/manage-automatic-scaling?tabs=azure-portal)

1. 使用 Azure 门户。 

1. 搜索并选择“应用服务计划” **** 。

1. 创建一个简单的应用服务计划。 讨论需要选择 Windows 还是 Linux。 现在或在后续步骤中讨论定价计划。 

1. 部署新的应用服务计划。 

1. 查看“纵向扩展（应用服务计划）”边栏选项卡。 讨论“开发/测试”计划与“生产”计划之间的差异。  查看功能列表。 

1. 查看“横向扩展（应用服务计划）”边栏选项卡。 查看“手动”和“基于规则”之间的差异。  

## 配置 Azure 应用服务

在本演示中，我们将新建运行 Docker 容器的 Web 应用。  容器会显示一条“欢迎”消息。

[参考：创建 Web 应用](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

在此任务中，我们将创建一个 Azure 应用服务 Web 应用。

1. 使用 Azure 门户。 

1. 搜索并选择“应用服务” **** 。

1. 创建 Web 应用 。

    - 发布：代码。 查看其他选项。
    - 运行时堆栈：.Net。 查看其他选项。
    - 操作系统：Linux

1. 选择“免费 F1”服务计划。

1. 查看 + 创建 Web 应用。 等待资源部署完毕。

1. 在“概述”页上，确保“状态”为“正在运行”  。

1. 选择 URL 并确保加载默认占位符页面。

1. 如果有时间，请浏览“部署槽位”选项。
   
## 配置 Azure 容器实例

在本演示中，我们将在 Azure 门户中使用 Azure 容器实例 (ACI) 创建、配置和部署容器。 ACI 应用程序显示一个静态 HTML 页面，其中包含公共 Microsoft Hello World 图像。 

**参考**：[快速入门 - 将 Docker 容器部署到容器实例](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)

1. 使用 Azure 门户。

1. 搜索并选择“容器实例” **** 。

1. 创建新的容器实例。 

1. 填写“资源组”和“容器名称” 。 

1. 讨论“映像源”选项。 使用快速入门映像。

1. 对于“容器映像”，使用 mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux) 。 此示例 Linux 映像打包了一个用 Node.js 编写的小型 Web 应用，该应用提供静态 HTML 页面。

1. 在“网络”页，为容器指定一个“DNS 名称标签”   。 

1. 将其他所有设置保留为默认设置，然后选择“查看 + 创建”。

1. 等待资源部署完毕。

1. 在资源的“概述”页上，确保“状态”为“正在运行”  。

1. 导航到容器实例的 FQDN，并确保显示欢迎页。 

注意：若要避免产生额外费用，请删除资源。 

## 配置 Azure 容器应用

在本演示中，我们将创建并使用 Azure 容器应用。 

**参考**：[快速入门：使用 Azure 门户部署自己的第一个容器应用](https://learn.microsoft.com/azure/container-apps/quickstart-portal)

1. 搜索并选择“容器应用”。

1. 提供“项目详细信息”并创建容器应用环境 。

1. 查看并创建容器应用。

1. 使用应用程序 URL 链接查看自己的应用程序。

1. 验证浏览器是否显示“欢迎使用 Azure 容器应用”消息。 






