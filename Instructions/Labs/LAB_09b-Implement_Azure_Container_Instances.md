---
lab:
  title: 09b - 实现 Azure 容器实例
  module: Administer Serverless Computing
---

# <a name="lab-09b---implement-azure-container-instances"></a>实验室 09b - 实现 Azure 容器实例
# <a name="student-lab-manual"></a>学生实验室手册

## <a name="lab-scenario"></a>实验室方案

Contoso wants to find a new platform for its virtualized workloads. You identified a number of container images that can be leveraged to accomplish this objective. Since you want to minimize container management, you plan to evaluate the use of Azure Container Instances for deployment of Docker images.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>目标

在此实验中，将执行以下操作：

- 任务 1：使用 Azure 容器实例部署 Docker 容器映像
- 任务 2：查看 Azure 容器实例的功能

## <a name="estimated-timing-20-minutes"></a>预计用时：20 分钟

## <a name="architecture-diagram"></a>体系结构关系图

![image](../media/lab09b.png)

## <a name="instructions"></a>说明

### <a name="exercise-1"></a>练习 1

#### <a name="task-1-deploy-a-docker-image-by-using-the-azure-container-instance"></a>任务 1：使用 Azure 容器实例部署 Docker 容器映像

在此任务中，你将为 Web 应用程序创建一个新的容器实例。

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 在 Azure 门户中，搜索查找“容器实例”，然后在“容器实例”边栏选项卡中，单击“+ 创建”  。

1. 在“创建容器实例”边栏选项卡的“基本设置”选项卡中，指定以下设置（其他设置保留默认值） ：

    | 设置 | 值 |
    | ---- | ---- |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | 新资源组的名称 **az104-09b-rg1** |
    | 容器名称 | **az104-9b-c1** |
    | 区域 | 可预配 Azure 容器实例的区域的名称 |
    | 映像源 | **快速启动映像** |
    | 映像 | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. 单击“下一步:**网络 >”，同时在“创建容器实例”边栏选项卡的“网络”选项卡，请指定以下设置（将其他设置保留为默认值）**  ：

    | 设置 | 值 |
    | --- | --- |
    | DNS 名称标签 | 任何有效且全局唯一的 DNS 主机名 |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Your container will be publicly reachable at dns-name-label.region.azurecontainer.io. If you receive a <bpt id="p1">**</bpt>DNS name label not available<ept id="p1">**</ept> error message, specify a different value.

1. 单击“下一步:**高级 >”，查看“创建容器实例”边栏选项卡中的“高级”选项卡上的设置，但不要做任何更改，单击“查看 + 创建”，确保验证通过并单击“创建”**    。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. This should take about 3 minutes.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: While you wait, you may be interested in viewing the <bpt id="p2">[</bpt>code behind the sample application<ept id="p2">](https://github.com/Azure-Samples/aci-helloworld)</ept>. To view it, browse the <ph id="ph1">\\</ph>app folder.

#### <a name="task-2-review-the-functionality-of-the-azure-container-instance"></a>任务 2：查看 Azure 容器实例的功能

在此任务中，你将查看容器实例的部署。

1. 在部署边栏选项卡中，单击“前往资源”链接。

1. 在容器实例“概述”边栏选项卡中，验证“状态”是否报告为“正在运行”。

1. 复制容器实例的值“FQDN”，打开一个新的浏览器选项卡，然后导航到相应的 URL。

1. 验证“欢迎使用 Azure 容器实例”的页面是否显示。

1. 关闭新的浏览器选项卡，返回到 Azure 门户，在“容器实例”边栏选项卡的“设置”部分，单击“容器”，然后单击“日志”。

1. 验证你是否看到代表通过在浏览器中显示应用程序生成的 HTTP GET 请求的日志条目。

#### <a name="clean-up-resources"></a>清理资源

>Contoso 希望为其虚拟化工作负荷找到一个新平台。

>你找到了大量可用于实现该目标的容器映像。 

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

1. 运行以下命令，列出在本模块各实验室中创建的所有资源组：

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*'
   ```

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。

#### <a name="review"></a>审阅

在此实验室中，你执行了以下操作：

- 使用 Azure 容器实例部署 Docker 映像
- 查看 Azure 容器实例的功能
