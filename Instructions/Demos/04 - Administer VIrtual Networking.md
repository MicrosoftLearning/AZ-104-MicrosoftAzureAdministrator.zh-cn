---
demo:
  title: 演示 04：管理虚拟网络
  module: Administer Virtual Networking
---

# 04 - 管理虚拟网络

## 配置虚拟网络

在本演示中，你将创建虚拟网络。

参考：[快速入门：创建虚拟网络 - Azure 门户](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## 在门户中创建虚拟网络

1.  登录到 Azure 门户并搜索“虚拟网络” **** 。

1.  创建虚拟网络，说明基本设置。 确保至少创建了一个子网。 

1.  验证是否已创建虚拟网络。

## 配置网络安全组

在本演示中，你将探索 NSG 和服务终结点。

参考：[限制对 PaaS 资源的访问 - 教程 - Azure 门户](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

**创建网络安全组**

1. 访问 Azure 门户。

1. 搜索并选择“网络安全组” **** 。

1. 创建一个 NSG，说明设置。 
 
1. 等待部署新 NSG。

**探索入站和出站规则**

1. 选择新 NSG。

1. 讨论 NSG 如何与子网或网络接口相关联。

1. 讨论目标入站和出站规则。  

1. 查看默认的入站和出站规则。 

1. 创建新规则，说明设置。 专门讨论服务选择（如 HTTPS）和优先级设置。 

## 配置 Azure DNS

在本演示中，你将探索 Azure DNS。

参考：[教程：托管域和子域 - Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**创建 DNS 区域**

1. 访问 Azure 门户。

1. 搜索 DNS 区域服务 ****  。

1. 创建 DNS 区域并说明该区域的用途。 对于名称，可以使用 contoso.internal.com。

1.  请等待 DNS 区域创建完毕。 可能需要刷新页面。 ****  

**添加 DNS 记录集**

参考：[教程：创建别名记录以引用区域资源记录](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. 创建区域后，选择“+记录集” **** 。

1. 使用“类型”下拉列表查看各种记录类型。 ****   查看如何使用不同的记录类型。 请注意当你选择不同记录类型时记录信息如何变化。

1. 创建 A 记录作为示例。 

