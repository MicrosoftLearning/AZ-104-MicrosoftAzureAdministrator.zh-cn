---
demo:
  title: 演示 05：管理站点间连接
  module: Administer Intersite Connectivity
---

# 05 - 管理站点间连接

## 配置 VNet 对等互连

注意：在本演示中，你将需要两个虚拟网络。 

参考：[使用 VNet 对等互连连接虚拟网络 - 教程](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**配置第一个虚拟网络的 VNet 对等互连**

1. 在 Azure 门户中，选择第一个虚拟网络 **** 。 查看对等互连的值。 

1. 在“设置”下，选择“对等互连”和“+ 添加新对等互连” ****  **** 。

1. 配置对等互连第二个虚拟网络。 使用信息图标查看不同的设置。 

1. 对等互连完成后，查看对等互连状态。 

**确认第二个虚拟网络的 VNet 对等互连**

1. 在 Azure 门户中，选择第二个虚拟网络 ****

1. 在“设置”下，选择“对等互连” ****  **** 。

1. 可以看到，一个对等互连已自动创建。 请注意，“对等互连状态”为“已连接” ****   **** 。

1. 在实验室中，学生将创建对等互连并测试虚拟机之间的连接。 

## 配置网络路由和终结点

在本演示中，你将学习如何创建路由表、定义自定义路由以及将路由与子网相关联。

注意：本演示需要一个具有至少一个子网的虚拟网络。 

参考：[路由网络流量 - 教程 - Azure 门户](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)

**创建路由表**

1. 如果有时间，可查看教程图。 说明需要创建用户定义的路由的原因。 

1. 访问 Azure 门户。

1. 搜索并选择“路由表”。**** 讨论何时应使用传播网关路由。 

1. 创建路由表，说明任何不常见的设置。 

1. 等待要部署的新路由表。

**添加路由**

1.  选择新路由表，然后选择“路由” **** 。

1.  创建新的路由。 讨论可用的不同跃点类型。 

1.  创建新路由并等待资源部署完成。
 
**将路由表关联到子网**

1.  前往希望与路由表相关联的子网。

1.  选择“路由表”，然后选择新的路由表 **** 。 

1.  保存更改 。

