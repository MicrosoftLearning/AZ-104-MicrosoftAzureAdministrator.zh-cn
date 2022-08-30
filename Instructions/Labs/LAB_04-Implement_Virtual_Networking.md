---
lab:
  title: 04 - 实现虚拟网络
  module: Module 04 - Virtual Networking
---

# <a name="lab-04---implement-virtual-networking"></a>实验室 04 - 实现虚拟网络

# <a name="student-lab-manual"></a>学生实验室手册

## <a name="lab-scenario"></a>实验室方案

You need to explore Azure virtual networking capabilities. To start, you plan to create a virtual network in Azure that will host a couple of Azure virtual machines. Since you intend to implement network-based segmentation, you will deploy them into different subnets of the virtual network. You also want to make sure that their private and public IP addresses will not change over time. To comply with Contoso security requirements, you need to protect public endpoints of Azure virtual machines accessible from Internet. Finally, you need to implement DNS name resolution for Azure virtual machines both within the virtual network and from Internet.

## <a name="objectives"></a>目标

在此实验中，将执行以下操作：

+ 任务 1：创建和配置虚拟网络
+ 任务 2：将虚拟机部署到虚拟网络中
+ 任务 3：配置 Azure VM 的专用 IP 地址和公共 IP 地址
+ 任务 4：配置网络安全组
+ 任务 5：配置 Azure DNS 以进行内部名称解析
+ 任务 6：配置 Azure DNS 以进行外部名称解析

## <a name="estimated-timing-40-minutes"></a>预计用时：40 分钟

## <a name="architecture-diagram"></a>体系结构关系图

![image](../media/lab04.png)

## <a name="instructions"></a>说明

### <a name="exercise-1"></a>练习 1

#### <a name="task-1-create-and-configure-a-virtual-network"></a>任务 1：创建和配置虚拟网络

在此任务中，你将使用 Azure 门户创建一个包含多个子网的虚拟网络

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 在 Azure 门户中，搜索并选择“虚拟网络”，然后在“虚拟网络”边栏选项卡中单击“+ 创建”  。

1. 创建一个虚拟网络，设置如下（其他设置保留默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 订阅 | 将在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | 新资源组名称 az104-04-rg1 |
    | 名称 | **az104-04-vnet1** |
    | 区域 | 本实验室将使用的订阅中可用的任何 Azure 区域的名称 |

1. 单击“下一步: IP 地址”并输入以下值

    | 设置 | 值 |
    | --- | --- |
    | IPv4 地址空间 | **10.40.0.0/20** |

1. 单击“+ 添加子网”，输入以下值，然后单击“添加” 

    | 设置 | 值 |
    | --- | --- |
    | 子网名称 | subnet0 |
    | 子网地址范围 | **10.40.0.0/24** |

1. Accept the defaults and click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> Wait for the virtual network to be provisioned. This should take less than a minute.

1. 单击“前往资源”

1. 在“az104-04-vnet1”虚拟网络边栏选项卡中，单击“子网”，然后单击“+ 子网”。

1. 创建一个子网，设置如下（其他则保留其默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 名称 | **subnet1** |
    | 地址范围（CIDR 块） | **10.40.1.0/24** |
    | 网络安全组 | **无** |
    | 路由表 | **无** |

1. 单击“保存” 

#### <a name="task-2-deploy-virtual-machines-into-the-virtual-network"></a>任务 2：将虚拟机部署到虚拟网络中

在此任务中，你将使用 ARM 模板将 Azure 虚拟机部署到虚拟网络的不同子网中

1. 在 Azure 门户中，单击 Azure 门户右上方的图标，打开 Azure Cloud Shell。

1. 如果系统提示选择“Bash”或“PowerShell”，请选择“PowerShell”  。

    >**注意**：如果这是你第一次启动 Cloud Shell，并看到消息“未装载任何存储”，请选择你将在本实验室中使用的订阅，然后选择“创建存储”  。

1. 在 Cloud Shell 窗格的工具栏中，单击“上传/下载文件”图标，在下拉菜单中，单击“上传”，然后将文件 \\Allfiles\\Labs\\04\\az104-04-vms-loop-template.json 和 \\Allfiles\\Labs\\04\\az104-04-vms-loop-parameters.json 上传到 Cloud Shell 主目录中   。

    >**注意**：你可能需要分别上传各个文件。

1. Edit the Parameters file, and change the password. If you need help editing the file in the Shell please ask your instructor for assistance. As a best practice, secrets, like passwords, should be more securely stored in the Key Vault. 

1. 在 Cloud Shell 窗格中，使用模板和参数文件运行以下命令，以部署两台虚拟机：

   ```powershell
   $rgName = 'az104-04-rg1'

   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-04-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-04-vms-loop-parameters.json
   ```

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This method of deploying ARM templates uses Azure PowerShell. You can perform the same task by running the equivalent Azure CLI command <bpt id="p1">**</bpt>az deployment create<ept id="p1">**</ept> (for more information, refer to <bpt id="p2">[</bpt>Deploy resources with Resource Manager templates and Azure CLI<ept id="p2">](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-cli)</ept>.

    >你需要探索 Azure 虚拟网络功能。

    >**注意**：如果遇到提示 VM 大小不可用的错误，请向讲师寻求帮助并尝试以下步骤：
    > 1. 单击 CloudShell 中的 `{}` 按钮，从左侧栏中选择“az104-04-vms-loop-parameters.json”，并记下 `vmSize` 参数值。
    > 1. 首先，你计划在 Azure 中创建一个将托管几个 Azure 虚拟机的虚拟网络。
    > 1. 由于你打算实现基于网络的分段，因此会将这些虚拟机部署到虚拟网络的不同子网中。
    > 1. 将 `vmSize` 参数的值替换为刚运行的命令返回的一个值。
    > 1. 你还希望确保其专用 IP 地址和公共 IP 地址不会随着时间的推移而发生变化。

1. 关闭 Cloud Shell 窗格。

#### <a name="task-3-configure-private-and-public-ip-addresses-of-azure-vms"></a>任务 3：配置 Azure VM 的专用 IP 地址和公共 IP 地址

在此任务中，你将配置分配到 Azure 虚拟机网络接口的公共 IP 地址和专用 IP 地址的静态分配。

   >**注意**：专用 IP 地址和公共 IP 地址实际上分配到网络接口，而这些接口又附加到 Azure 虚拟机，但是，改为引用分配给 Azure VM 的 IP 地址相当普遍。

1. 在 Azure 门户中，搜索并选择“资源组”，然后在“资源组”边栏选项卡中单击“az104-04-rg1”。

1. 在“az104-04-rg1”资源组边栏选项卡的资源列表中，单击“az104-04-vnet1”。

1. 在“az104-04-vnet1”虚拟网络边栏选项卡中，查看“已连接设备”部分，并验证“az104-04-nic0”和“az104-04-nic1”这两个网络接口是否已连接到虚拟网络。

1. 单击“az104-04-nic0”，然后在“az104-04-nic0”边栏选项卡中单击“IP 配置”。

    >**注意**：请验证当前是否将“ipconfig1”设置为动态专用 IP 地址。

1. 在 IP 配置列表中，单击“ipconfig1”。

1. 在“ipconfig1”边栏选项卡上的“公共 IP 地址设置”部分，选择“关联”，单击“+ 新建”，指定以下设置，然后单击“确定”    ：

    | 设置 | 值 |
    | --- | --- |
    | 名称 | **az104-04-pip0** |
    | SKU | **标准** |

1. 在“ipconfig1”边栏选项卡中，将“分配”设置为“静态”，将“IP 地址”的默认值设置为“10.40.0.4”    。

1. 为了符合 Contoso 的安全要求，你需要保护可从 Internet 访问的 Azure 虚拟机公共终结点。

1. 导航回“az104-04-vnet1”边栏选项卡

1. 单击“az104-04-nic1”，然后在“az104-04-nic1”边栏选项卡中单击“IP 配置”  。

    >**注意**：请验证当前是否将“ipconfig1”设置为动态专用 IP 地址。

1. 在 IP 配置列表中，单击“ipconfig1”。

1. 在“ipconfig1”边栏选项卡上的“公共 IP 地址设置”部分，选择“关联”，单击“+ 新建”，指定以下设置，然后单击“确定”    ：

    | 设置 | 值 |
    | --- | --- |
    | 名称 | **az104-04-pip1** |
    | SKU | **标准** |

1. 在“ipconfig1”边栏选项卡中，将“分配”设置为“静态”，将“IP 地址”的默认值设置为“10.40.1.4”    。

1. 返回“ipconfig1”边栏选项卡，保存更改。

1. 导航回“az104-04-rg1”资源组边栏选项卡，在资源列表中单击“az104-04-vm0”，并记下“az104-04-vm0”虚拟机边栏选项卡中的公共 IP 地址条目。

1. 导航回“az104-04-rg1”资源组边栏选项卡，在资源列表中单击“az104-04-vm1”，并记下“az104-04-vm1”虚拟机边栏选项卡中的公共 IP 地址条目  。

    >**注意**：本实验室的最后一个任务中需要这两个 IP 地址。

#### <a name="task-4-configure-network-security-groups"></a>任务 4：配置网络安全组

在此任务中，你将配置网络安全组，以允许对 Azure 虚拟机的受限连接。

1. 在 Azure 门户中，导航回“az104-04-rg1”资源组边栏选项卡，然后在资源列表中单击“az104-04-vm0”。

1. 在“az104-04-vm0”概述边栏选项卡上单击“连接”，在下拉菜单中单击“RDP”，在“使用 RDP 连接”边栏选项卡上单击“使用公共 IP 地址下载 RDP 文件”，然后按照提示启动远程桌面会话    。

1. 请注意，连接尝试失败。

    >最后，你需要为虚拟网络和 Internet 内的 Azure 虚拟机实现 DNS 名称解析。

1. 停止 az104-04-vm0 和 az104-04-vm1 虚拟机 。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This is done for lab expediency. If the virtual machines are running when a network security group is attached to their network interface, it can can take over 30 minutes for the attachment to take effect. Once the network security group has been created and attached, the virtual machines will be restarted, and the attachment will be in effect immediately.

1. 在 Azure 门户中，搜索并选择“网络安全组”，然后在“网络安全组”边栏选项卡中单击“+ 创建”  。

1. 创建一个网络安全组，设置如下（其他设置保留默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | **az104-04-rg1** |
    | 名称 | **az104-04-nsg01** |
    | 区域 | 本实验室中用于部署所有其他资源的 Azure 区域的名称 |

1. Click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> to submit your deployment.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. This should take about 2 minutes.

1. 在“部署”边栏选项卡上，单击“前往资源”以打开“az104-04-nsg01”网络安全组边栏选项卡 。

1. 在“az104-04-nsg01”网络安全组边栏选项卡中的“设置”部分，单击“入站安全规则”。

1. 添加一个入站规则，设置如下（其他设置保留默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 源 | **任意** |
    | 源端口范围 | * |
    | 目标 | **任意** |
    | 服务 | **RDP** |
    | 操作 | **允许** |
    | 优先级 | **300** |
    | 名称 | **AllowRDPInBound** |

1. 在“az104-04-nsg01”网络安全组边栏选项卡的“设置”部分，单击“网络接口”，然后单击“+ 关联”。

1. 将“az104-04-nsg01”网络安全组与“az104-04-nic0”和“az104-04-nic1”网络接口关联。

    >**注意**：新创建的网络安全组中的规则可能需要最多 5 分钟才能应用到网络接口卡。

1. 启动 az104-04-vm0 和 az104-04-vm1 虚拟机 。

1. 导航回“az104-04-vm0”虚拟机边栏选项卡。

    >**注意**：在后续步骤中，你将验证你能成功连接到目标虚拟机。

1. 在“az104-04-vm0”边栏选项卡上依次单击“连接”和“RDP”，在“使用 RDP 连接”边栏选项卡上单击“使用公共 IP 地址下载 RDP 文件”，然后按照提示启动远程桌面会话    。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

    >**注意**：连接到目标虚拟机时，可以忽略任何警告提示。

1. 出现提示时，请使用参数文件中的用户和密码登录。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Leave the Remote Desktop session open. You will need it in the next task.

#### <a name="task-5-configure-azure-dns-for-internal-name-resolution"></a>任务 5：配置 Azure DNS 以进行内部名称解析

在此任务中，你将使用 Azure 专用 DNS 区域在虚拟网络中配置 DNS 名称解析。

1. 在 Azure 门户中，搜索并选择“专用 DNS 区域”，然后在“专用 DNS 区域”边栏选项卡中单击“+ 创建”  。

1. 创建一个专用 DNS 区域，设置如下（其他设置保留默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | **az104-04-rg1** |
    | 名称 | **contoso.org** |

1. Click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the private DNS zone to be created. This should take about 2 minutes.

1. 单击“前往资源”以打开“contoso.org DNS”专用区域边栏选项卡 。

1. 在“contoso.org”专用 DNS 区域边栏选项卡的“设置”部分，单击“虚拟网络链接”

1. 单击“+ 添加”，使用以下设置创建虚拟网络链接（其他设置保留默认值）：：

    | 设置 | 值 |
    | --- | --- |
    | 链接名称 | **az104-04-vnet1-link** |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 虚拟网络 | **az104-04-vnet1** |
    | 启用自动注册 | enabled |

1. 单击" **确定**"。

    ><bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> Wait for the virtual network link to be created. This should take less than 1 minute.

1. 在 contoso.org 专用 DNS 区域边栏选项卡上，在边栏中，单击“概述”

1. 验证“az104-04-vm0”和“az104-04-vm1”的DNS 记录是否在记录集列表中显示为“自动注册”。

    >**注意：** 如果未列出记录集，你可能需要等待几分钟并刷新页面。

1. 将远程桌面会话切换到 az104-04-vm0，右键单击“开始”按钮，然后在右键菜单中单击“Windows PowerShell (管理员)”。

1. 在 Windows PowerShell 控制台窗口中，运行以下命令以测试新创建的专用 DNS 区域中的内部名称解析：

   ```powershell
   nslookup az104-04-vm0.contoso.org
   nslookup az104-04-vm1.contoso.org
   ```

1. 验证命令的输出是否包含 az104-04-vm1 的专用 IP 地址 (10.40.1.4) 。

#### <a name="task-6-configure-azure-dns-for-external-name-resolution"></a>任务 6：配置 Azure DNS 以进行外部名称解析

此任务将使用 Azure 公用 DNS 区域配置外部 DNS 名称解析。

1. 在 Web 浏览器中，打开一个新选项卡并导航到 <https://www.godaddy.com/domains/domain-name-search>。

1. 使用域名搜索来标识未使用的域名。

1. 在 Azure 门户中，搜索并选择“DNS 区域”，然后在“DNS 区域”边栏选项卡中单击“+ 创建”  。

1. 使用以下设置（将其他设置保留为默认值）创建 DNS 区域：

    | 设置 | 值 |
    | --- | --- |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | **az104-04-rg1** |
    | 名称 | 之前在此任务中标识的 DNS 域名 |

1. Click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the DNS zone to be created. This should take about 2 minutes.

1. 单击“前往资源”，打开新创建的 DNS 区域的边栏选项卡。

1. 在“DNS 区域”边栏选项卡中，单击“+ 记录集”。

1. 添加具有以下设置的记录集（保留其他设置为默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 名称 | **az104-04-vm0** |
    | 类型 | **A** |
    | 别名记录集 | **是** |
    | TTL | **1** |
    | TTL 单位 | **小时数** |
    | IP 地址 | 在本实验室的第三个练习中标识的 az104-04-vm0 的公用 IP 地址 |

1. 单击 **“确定”**

1. 在“DNS 区域”边栏选项卡中，单击“+ 记录集”。

1. 添加具有以下设置的记录集（保留其他设置为默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 名称 | **az104-04-vm1** |
    | 类型 | **A** |
    | 别名记录集 | **是** |
    | TTL | **1** |
    | TTL 单位 | **小时数** |
    | IP 地址 | 在本实验室的第三个练习中标识的 az104-04-vm1 的公共 IP 地址 |

1. 单击 **“确定”**

1. 在“DNS 区域”边栏选项卡中，请记下“名称服务器 1”条目的名称。

1. 在 Azure 门户中，通过单击 Azure 门户右上方的图标，在 Cloud Shell 中打开“PowerShell”会话 。

1. 在 Cloud Shell 窗格中，运行以下命令以测试新创建的 DNS 区域中“az104-04-vm0”DNS 记录集的外部名称解析（将占位符 `[Name server 1]` 替换为你之前在本任务中记下的“名称服务器 1”的名称，将占位符 `[domain name]` 替换为你之前在本任务中创建的 DNS 域的名称） ：

   ```powershell
   nslookup az104-04-vm0.[domain name] [Name server 1]
   ```

1. 验证命令输出是否包含 az104-04-vm0 的公共 IP 地址。

1. 在 Cloud Shell 窗格中，运行以下命令以测试新创建的 DNS 区域中“az104-04-vm1”DNS 记录集的外部名称解析（将占位符 `[Name server 1]` 替换为你之前在本任务中记下的“名称服务器 1”的名称，将占位符 `[domain name]` 替换为你之前在本任务中创建的 DNS 域的名称） ：

   ```powershell
   nslookup az104-04-vm1.[domain name] [Name server 1]
   ```

1. 验证命令输出是否包含 az104-04-vm1 的公共 IP 地址。

#### <a name="clean-up-resources"></a>清理资源

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

1. 运行以下命令，列出在本模块各实验室中创建的所有资源组：

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*'
   ```

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。

#### <a name="review"></a>审阅

在此实验室中，你执行了以下操作：

+ 创建和配置虚拟网络
+ 将虚拟机部署到虚拟网络中
+ 配置 Azure VM 的专用 IP 地址和公共 IP 地址
+ 配置网络安全组
+ 配置 Azure DNS 以进行内部名称解析
+ 配置 Azure DNS 以进行外部名称解析
