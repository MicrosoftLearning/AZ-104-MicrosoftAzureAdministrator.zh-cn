---
lab:
  title: 实验室 04：实现虚拟网络
  module: Implement Virtual Networking
---

# 实验室 04 - 实现虚拟网络

## 实验室简介

此实验室是专注于虚拟网络的三个实验室中的第一个。 在本实验室中，你将了解虚拟网络和子网的基础知识。 了解如何使用网络安全组和应用程序安全组保护网络。 你还将了解 DNS 区域和记录。 

本实验室需要 Azure 订阅。 订阅类型可能会影响此实验室中功能的可用性。 可以更改区域，但步骤是使用 **美国东部** 区域编写的。

## 预计用时：50 分钟

## 实验室方案 

你的全球组织计划实施虚拟网络。 直接目标是容纳所有现有资源。 但组织正处于增长阶段，因此希望确保为该增长提供额外的容量。

**CoreServicesVnet** 虚拟网络拥有最多的资源。 预计会有大量增长，因此该虚拟网络需要较大的地址空间。

**ManufacturingVnet** 虚拟网络包含用于运营制造设施的系统。 组织预计其系统要从大量内部连接设备中检索数据。 

## 交互式实验室模拟

你可能会发现几个交互式实验室模拟可用于本主题。 通过模拟，可按照自己的节奏点击浏览类似的方案。 交互式模拟与本实验室之间存在差异，但许多核心概念是相同的。 不需要 Azure 订阅。 

+ [保护网络流量](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013)。 创建虚拟机、虚拟网络和网络安全组。 添加网络安全组规则以允许和禁止流量。
  
+ [创建简单的虚拟网络](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204)。 创建包含两台虚拟机的虚拟网络。 演示虚拟机可以通信。 

+ [在 Azure 中设计和实现虚拟网络](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure)。 创建资源组并创建包含子网的虚拟网络。  

+ [实现虚拟网络](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208)。 创建和配置虚拟网络、部署虚拟机、配置网络安全组和配置 Azure DNS。

## 体系结构关系图

![网络布局](../media/az104-lab04-architecture.png)

这些虚拟网络和子网的构建方式既可以容纳现有资源，也能满足预计的增长。 让我们来创建这些虚拟网络和子网，为网络基础结构奠定基础。

>**你是否知道？**：避免 IP 地址范围重叠，以减少问题并简化故障排除是一种良好做法。 重叠问题会影响整个网络，无论是在云中还是在本地。 许多组织设计了企业范围的 IP 寻址方案，以避免重叠并规划未来增长。

## 工作技能

+ 任务 1：使用门户创建包含子网的虚拟网络。
+ 任务 2：使用模板创建虚拟网络和子网。
+ 任务 3：创建和配置应用程序安全组与网络安全组之间的通信。
+ 任务 4：配置公共和专用 Azure DNS 区域。
  
## 任务 1：使用门户创建包含子网的虚拟网络

组织计划大量增加核心服务。 在此任务中，你将创建虚拟网络和关联的子网，以容纳现有资源和规划的增长。 在此任务中，你将使用 Azure 门户。 

1. 登录 **Azure 门户** - `https://portal.azure.com`。
   
1. 搜索并选择 `Virtual Networks`。

1. 在“虚拟网络”页上选择“创建”。

1. 完成 CoreServicesVnet 的“基本信息”选项卡****。  

    |  **选项**         | **值**            |
    | ------------------ | -------------------- |
    | 资源组     | `az104-rg4`（如有必要，请新建一个） |
    | 名称               | `CoreServicesVnet`     |
    | 区域             | （美国）**美国东部**         |

1. 移动到“IP 地址”选项卡****。

    |  **选项**         | **值**            |
    | ------------------ | -------------------- |
    | IPv4 地址空间 | 将预填充的 IPv4 地址空间替换为 `10.20.0.0/16`（分隔条目）  |

1. 选择“+添加子网”。 完成每个子网的名称和地址信息。 请务必为每个新子网选择“添加”****。 请务必在创建其他子网之前或之后删除默认子网。

    | **子网**             | **选项**           | **值**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | 子网名称          | `SharedServicesSubnet`   |
    |                        | 开始地址     | `10.20.10.0`          |
    |                        | 大小                 | `/24` |
    | DatabaseSubnet         | 子网名称          | `DatabaseSubnet`         |
    |                        | 开始地址     | `10.20.20.0`        |
    |                        | 大小                 | `/24` |

    >**注意：** 每个虚拟网络必须具有至少一个子网。 注意，将始终保留五个 IP 地址，因此请在规划中考虑到这一点。 

1. 若要完成创建 CoreServicesVnet 及其关联的子网，请选择“审阅并创建”。

1. 验证配置是否通过验证，然后选择“创建”。

1. 等待虚拟网络部署，然后选择“转到资源”****。

1. 利用一分钟时间验证“地址空间”和“子网”********。 注意你在“设置”边栏选项卡中的其他设置****。 

1. 在“自动化”部分中，选择“导出模板”，然后等待模板生成。********

1. **下载** 模板。

1. 在本地计算机上导航到“下载”文件夹，然后“全部提取”已下载 zip 文件中的文件。******** 

1. 在继续操作之前，请确保具有 **template.json** 文件。 你将使用此模板在下一个任务中创建 ManufacturingVnet。 
 
## 任务 2：使用模板创建虚拟网络和子网

在此任务中，你将创建 ManufacturingVnet 虚拟网络和关联的子网。 组织预计制造业部门将会增长，因此将根据预期增长调整子网的大小。 对于此任务，请使用模板创建资源。 

1. 找到在上一个任务中导出的 **template.json** 文件。 它应位于“下载”文件夹中。****

1. 使用所选编辑器编辑文件。 许多编辑器具有 *更改全部事件* 功能。 如果使用 Visual Studio Code，请确保在 **受信任的窗口** 中工作，而不是处于 **受限模式** 下。 请参阅体系结构关系图来验证详细信息。 

### 更改 ManufacturingVnet 虚拟网络

1. 将 **CoreServicesVnet** 的所有事件替换为 `ManufacturingVnet`。 

1. 将 **10.20.0.0** 的所有事件替换为 `10.30.0.0`。 

### 更改 ManufacturingVnet 子网

1. 将 **SharedServicesSubnet** 的所有事件更改为 `SensorSubnet1`。

1. 将 **10.20.10.0/24** 的所有事件更改为 `10.30.20.0/24`。

1. 将 **DatabaseSubnet** 的所有事件更改为 `SensorSubnet2`。

1. 将 **10.20.20.0/24** 的所有事件更改为 `10.30.21.0/24`。

1. 通读文件并确保所有内容正确无误。 将体系结构关系图用于资源名称和 IP 地址。 

1. 务必保存你的更改。

>**注意：** 实验室文件目录中存在一个已完成的模板文件。 

### 更改参数文件

1. 查找在上一个任务中导出的 parameters.json 文件****。 它应位于“下载”文件夹中。****

1. 使用所选编辑器编辑文件。

1. 将 **CoreServicesVnet**的一个事件替换为 `ManufacturingVnet`。

1. **保存**所做更改。
   
### 部署自定义模板

1. 在门户中，搜索并选择 `Deploy a custom template`。

1. 选择“在编辑器中生成自己的模板”，然后选择“加载文件”。********

1. 选择包含“制造更改”的 **templates.json** 文件，然后选择“**保存**”。

1. 选择“**编辑参数**”，然后选择“**加载文件**”。

1. 选择包含制造更改的 **templates.json** 文件，然后选择“**保存**”。

1. 确保已选择资源组 **az104-rg4**。 

1. 选择“查看 + 创建”，然后选择“创建” 。

1. 等待模板部署，然后确认（在门户中）是否已创建制造业虚拟网络和子网。

>**注意：** 如果必须进行多次部署，则可能会发现一些资源已成功完成，并且部署失败。 可以手动移除这些资源，然后重试。 
   
## 任务 3：创建和配置应用程序安全组与网络安全组之间的通信

在此任务中，我们将创建应用程序安全组和网络安全组。 NSG 将具有允许来自 ASG 的流量的入站安全规则。 NSG 还将具有拒绝访问 Internet 的出站规则。 

### 创建应用程序安全组 (ASG)

1. 在 Azure 门户中，搜索并选择`Application security groups`。

1. 单击“创建”并提供基本信息。****

    | 设置 | 值 |
    | -- | -- |
    | 订阅 | *用户的订阅* |
    | 资源组 | **az104-rg4** |
    | 名称 | `asg-web` |
    | 区域 | **美国东部**  |

1. 单击“查看 + 创建”****，然后在验证后单击“创建”****。

>**备注：** 此时会将 ASG 与虚拟机相关联。 这些计算机将受到在下一个任务中创建的入站 NSG 规则的影响。  

### 创建网络安全组并将其与 CoreServicesVnet 相关联

1. 在 Azure 门户中，搜索并选择`Network security groups`。

1. 选择“+ 创建”，并在“基本信息”选项卡上提供信息。******** 

    | 设置 | 值 |
    | -- | -- |
    | 订阅 | *用户的订阅* |
    | 资源组 | **az104-rg4** |
    | 名称 | `myNSGSecure` |
    | 区域 | **美国东部**  |

1. 单击“查看 + 创建”****，然后在验证后单击“创建”****。

1. 部署 NSG 后，单击“转到资源”。****

1. 在“设置”下，单击“子网”，然后单击“关联”。************

    | 设置 | 值 |
    | -- | -- |
    | 虚拟网络 | **CoreServicesVnet (az104-rg4)** |
    | 子网 | **SharedServicesSubnet** |

1. 单击“确定”以保存关联****。

### 配置入站安全规则以允许 ASG 流量

1. 继续使用 NSG。 在“设置”区域中，选择“入站安全规则”。********

1. 查看默认入站规则。 请注意，仅允许其他虚拟网络和负载均衡器访问。

1. 选择 **+ 添加**。

1. 在“添加入站安全规则”边栏选项卡上，使用以下信息添加入站端口规则。**** 此规则允许 ASG 流量。 完成后，选择“添加”。****

    | 设置 | 值 |
    | -- | -- |
    | Source | **应用程序安全组** |
    | 源应用程序安全组 | **asg-web** |
    | 源端口范围 |  * |
    | 目标 | **任意** |
    | 服务 | **自定义**（请注意其他选择） |
    | 目标端口范围 | **80,443** |
    | 协议 | **TCP** |
    | 操作 | **允许** |
    | 优先级 | **100** |
    | 名称 | `AllowASG` |

### 配置拒绝 Internet 访问的出站 NSG 规则

1. 创建入站 NSG 规则后，选择“出站安全规则”。**** 

1. 请注意 **AllowInternetOutboundRule** 规则。 另请注意，无法删除规则，并且优先级为 65001。

1. **** 选择“+ 添加”，然后配置拒绝访问 Internet 的出站规则。 完成后，选择“添加”。****

    | 设置 | 值 |
    | -- | -- |
    | 源 | **任意** |
    | 源端口范围 |  * |
    | 目标 | **服务标记** |
    | 目标服务标记 | **Internet** |
    | 服务 | **自定义** |
    | 目标端口范围 | **8080** |
    | 协议 | **任意** |
    | 操作 | **拒绝** |
    | 优先级 | 4096**** |
    | 名称 | **DenyAnyCustom8080Outbound** |


## 任务 4：配置公共和专用 Azure DNS 区域

在此任务中，你将创建和配置公共和专用 DNS 区域。 

### 配置公用 DNS 区域

可以将 Azure DNS 配置为解析公共域中的主机名。 例如，如果从域名注册机构购买了 contoso.xyz 域名，则可配置 Azure DNS 来托管 `contoso.com` 域，并将 www.contoso.xyz 解析为 Web 服务器或 Web 应用的 IP 地址。

1. 在门户中，搜索并选择 `DNS zones`。

1. 选择“+ 新建”。

1. 配置“基本信息”选项卡。****

    | 属性 | 值    |
    |:---------|:---------|
    | 订阅 | 选择订阅 |
    | 资源组 | az-104-rg4**** |
    | 名称 | `contoso.com`（如果保留，请调整名称） |
    | 区域 |**美国东部**（查看信息图标） |

1. 选择“查看 + 创建”，然后选择“创建”。********
   
1. 等待 DNS 区域部署，然后选择“转到资源”****。

1. 在“概述”边栏选项卡上，可以看到分配给该区域的四个 Azure DNS 名称服务器的名称。**** **复制** 其中一个名称服务器地址。 下一步会用到。 
  
1. 展开“**DNS 管理**”边栏选项卡，然后选择“**记录集**”。 单击“+添加”****。 

    | 属性 | 值    |
    |:---------|:---------|
    | 名称 | **www** |
    | 类型 | **A** |
    | TTL | **1** |
    | IP 地址 | 10.1.1.4 |

>**注意：** 在实际方案中，你会输入 Web 服务器的公共 IP 地址。

1. 选择“**添加**”并验证域是否具有名为 **www** 的 A 记录集。

1. 打开命令提示符，并运行以下命令。 如果已更改域名，请进行调整。 

   ```sh
   nslookup www.contoso.com <name server name>
   ```
1. 验证主机名 www.contoso.com 是否解析为你提供的 IP 地址。 这可确认名称解析是否工作正常。

### 配置专用 DNS 区域

专用 DNS 区域会在虚拟网络中提供名称解析服务。 只能从专用 DNS 区域链接到的虚拟网络访问专用 DNS 区域，而无法通过 Internet 进行访问。 

1. 在门户中，搜索并选择 `Private dns zones`。

1. 选择“+ 新建”。

1. 在创建专用 DNS 区域的“基本信息”**** 选项卡上，输入下表中列出的信息：

    | 属性 | 值    |
    |:---------|:---------|
    | 订阅 | 选择订阅 |
    | 资源组 | az-104-rg4**** |
    | 名称 | `private.contoso.com`（如果必须重命名，请调整） |
    | 区域 |**美国东部** |

1. 选择“查看 + 创建”，然后选择“创建”。********
   
1. 等待 DNS 区域部署，然后选择“转到资源”****。

1. 请注意，“概述”边栏选项卡上不提供名称服务器记录。**** 

1. 展开“**DNS 管理**”边栏选项卡，然后选择“**虚拟网络链接**”。 配置链接。 

    | 属性 | Value    |
    |:---------|:---------|
    | 链接名称 | `manufacturing-link` |
    | 虚拟网络 | `ManufacturingVnet` |

1. 选择“**创建**”并等待链接创建。 

1. 在“**DNS 管理**”边栏选项卡中，选择“**+ 记录集**”。 现在，将为需要专用名称解析支持的每台虚拟机添加一条记录。

    | 属性 | 值    |
    |:---------|:---------|
    | 名称 | **sensorvm** |
    | 类型 | **A** |
    | TTL | **1** |
    | IP 地址 | 10.1.1.4 |

 >**注意：** 在实际方案中，你将输入特定制造业虚拟机的 IP 地址。

## 清理资源

如果使用自己的订阅，需要一点时间删除实验室资源****。 这将确保资源得到释放，并将成本降至最低。 删除实验室资源的最简单方法是删除实验室资源组。 

+ 在 Azure 门户中，选择资源组，选择“删除资源组”，输入资源组名称，然后单击“删除”************。
+ `Remove-AzResourceGroup -Name resourceGroupName`（使用 Azure PowerShell）。
+ `az group delete --name resourceGroupName`（使用 CLI）。

## 使用 Copilot 扩展学习

Copilot 可帮助你了解如何使用 Azure 脚本工具。 Copilot 还可以帮助了解实验室中未涵盖的领域或需要更多信息的领域。 打开 Edge 浏览器并选择“Copilot”（右上角）或导航到*copilot.microsoft.com*。 花几分钟时间尝试这些提示。
+ 在 Azure 中部署和配置虚拟网络时，共享前 10 个最佳做法。
+ 我如何使用 Azure PowerShell 和 Azure CLI 命令创建具有公共 IP 地址和一个子网的虚拟网络。 
+ 介绍 Azure 网络安全组入站和出站规则及其使用方式。
+ Azure 网络安全组和 Azure 应用程序安全组之间有什么区别？ 共享什么时候使用这些组的示例。 
+ 提供有关如何排查在 Azure 上部署网络时遇到的任何网络问题的分步指南。 此外，分享用于故障排除的每一步的思维过程。

## 通过自定进度的培训了解详细信息

+ [Azure 虚拟网络的简介](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/)。 设计和实现核心 Azure 网络基础结构，例如虚拟网络、公共和专用 IP、DNS、虚拟网络对等互连、路由和 Azure 虚拟 NAT。
+ [设计 IP 寻址方案](https://learn.microsoft.com/training/modules/design-ip-addressing-for-azure/)。 确定 Azure 和本地虚拟网络的专用和公共 IP 寻址功能。
+ [使用网络安全组和服务终结点来保护和隔离对 Azure 资源的访问](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/)。 网络安全组和服务终结点能帮助保护虚拟机和 Azure 服务，使其免遭未经授权的网络访问。
+ [在 Azure DNS 上托管域](https://learn.microsoft.com/training/modules/host-domain-azure-dns/)。 为域名创建 DNS 区域。 创建 DNS 记录，以将域映射到 IP 地址。 测试域名是否解析为 Web 服务器。
  
## 关键结论

恭喜你完成本实验室的内容。 下面是本模块的主要收获。 

+ 虚拟网络是你自己的网络在云中的表示形式。 
+ 设计虚拟网络时，避免 IP 地址范围重叠是一种良好做法。 这将减少问题并简化故障排除。
+ 子网是虚拟网络中某个范围内的 IP 地址。 出于组织安全性考虑，可将一个虚拟网络划分为多个子网。
+ 网络安全组包含允许或拒绝网络流量的安全规则。 可以根据需要自定义默认传入和传出规则。
+ 应用程序安全组用于保护具有常见功能的服务器组，例如 Web 服务器或数据库服务器。
+ Azure DNS 是 DNS 域的托管服务，用于提供名称解析。 可以将 Azure DNS 配置为解析公共域中的主机名。  还可以使用专用 DNS 区域将 DNS 名称分配给 Azure 虚拟网络中的虚拟机 (VM)。
