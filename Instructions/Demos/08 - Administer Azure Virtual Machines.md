---
demo:
  title: 演示 08：管理 Azure 虚拟机
  module: Administer Azure Virtual Machines
---


# 08 - 管理 Azure 虚拟机

## 演示 - 在门户中创建虚拟机

在本演示中，我们将在门户中创建并访问 Azure 虚拟机。

**参考**

[快速入门 - 在 Azure 门户中创建 Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

[快速入门 - 在 Azure 门户中创建 Linux VM](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)

[使用 Bastion 连接到虚拟机](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

**创建虚拟机**

注意：这些步骤仅涵盖一些虚拟机参数。 可探索和了解其他区域。 可以根据受众创建 Windows 或 Linux 虚拟机。

1. 使用 Azure 门户。

1. 搜索虚拟机。 

1. 创建基本虚拟机。 查看可用性选项、映像和入站规则。

1. 讨论创建安全管理员帐户的重要性。

1. 创建虚拟机并等待资源部署。  

**连接到虚拟机**

1. 可通过多种方式连接到虚拟机。 

1. 对于 Windows 服务器，可以使用 RDP，如快速入门中所示。 

1. 对于 Linux 服务器，可以通过 SSH 连接，如快速入门中所示。 

1. 对于任一服务器，都可以使用 Bastion 服务进行连接（快速入门）。 查看为什么 Bastion 优先于 RDP 或 SSH。 

## 配置虚拟机可用性

在本演示中，我们将了解虚拟机缩放选项。

**参考**

[使用 Azure 门户在规模集中创建虚拟机](https://learn.microsoft.com/azure/virtual-machine-scale-sets/flexible-virtual-machine-scale-sets-portal)

1. 使用 Azure 门户。

1. 搜索并选择“虚拟机规模集”。 

1. 创建虚拟机规模集。 查看虚拟机规模集的用途。 查看统一业务流程模式与灵活业务流程模式之间的差异。 说明你的选择可能会影响缩放选项。 

1. 移动到“缩放”选项卡 **** 。 

1. 查看如何使用手动缩放和横向缩减策略 ****  。 

1. 更改为“自定义”缩放策略。 

1. 查看如何使用 CPU 阈值 (%) 在虚拟机实例中横向扩展和缩减。 

