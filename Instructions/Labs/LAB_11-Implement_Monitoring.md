---
lab:
  title: 11 - 实现监视
  module: Administer Monitoring
---

# <a name="lab-11---implement-monitoring"></a>实验室 11 - 实现监视
# <a name="student-lab-manual"></a>学生实验室手册

## <a name="lab-scenario"></a>实验室方案

You need to evaluate Azure functionality that would provide insight into performance and configuration of Azure resources, focusing in particular on Azure virtual machines. To accomplish this, you intend to examine the capabilities of Azure Monitor, including Log Analytics.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>目标

在此实验中，将执行以下操作：

+ 任务 1：预配实验室环境
+ 任务 2：注册 Microsoft.Insights 和 Microsoft.AlertsManagement 资源提供程序
+ 任务 3：创建和配置 Azure Log Analytics 工作区和基于 Azure 自动化的解决方案
+ 任务 4：查看 Azure 虚拟机的默认监视设置
+ 任务 5：配置 Azure 虚拟机诊断设置
+ 任务 6：审阅 Azure Monitor 功能
+ 任务 7：查看 Azure Log Analytics 功能

## <a name="estimated-timing-45-minutes"></a>预计用时：45 分钟

## <a name="instructions"></a>说明

### <a name="exercise-1"></a>练习 1

#### <a name="task-1-provision-the-lab-environment"></a>任务 1：预配实验室环境

在此任务中，你将部署用于测试监视方案的虚拟机。

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 在 Azure 门户中，单击 Azure 门户右上方的图标，打开 Azure Cloud Shell。

1. 如果系统提示选择“Bash”或“PowerShell”，请选择“PowerShell”  。

    >**注意**：如果这是你第一次启动 Cloud Shell，并看到消息“未装载任何存储”，请选择你将在本实验室中使用的订阅，然后选择“创建存储”  。

1. 在 Cloud Shell 窗格的工具栏中，单击“上传/下载文件”图标，在下拉菜单中，单击“上传”，然后将文件 \\Allfiles\\Labs\\11\\az104-11-vm-template.json 和 \\Allfiles\\Labs\\11\\az104-11-vm-parameters.json 上传到 Cloud Shell 主目录中   。

1. Edit the Parameters file you just uploaded and change the password. If you need help editing the file in the Shell please ask your instructor for assistance. As a best practice, secrets, like passwords, should be more securely stored in the Key Vault. 

1. 在 Cloud Shell 窗格中，运行以下命令以创建托管虚拟机的资源组（将 `[Azure_region]` 占位符替换为你打算在其中部署 Azure 虚拟机的 Azure 区域的名称）：

    >**注意**：[确保在引用的工作区映射文档](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)中，选择列为 **“Log Analytics 工作区区域”** 的一个区域

   ```powershell
   $location = '[Azure_region]'

   $rgName = 'az104-11-rg0'

   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. 在“Cloud Shell”窗格中运行以下命令，以创建第一个虚拟网络，并使用上传的模板和参数文件将虚拟机部署到其中：

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-11-vm-template.json `
      -TemplateParameterFile $HOME/az104-11-vm-parameters.json `
      -AsJob
   ```

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not wait for the deployment to complete but instead proceed to the next task. The deployment should take about 3 minutes.

#### <a name="task-2-register-the-microsoftinsights-and-microsoftalertsmanagement-resource-providers"></a>任务 2：注册 Microsoft.Insights 和 Microsoft.AlertsManagement 资源提供程序。

1. 在 Cloud Shell 窗格中，运行以下命令以注册 Microsoft.Insights 和 Microsoft.AlertsManagement 资源提供程序。

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

1. 最小化 Cloud Shell 窗格（但不要将其关闭）。

#### <a name="task-3-create-and-configure-an-azure-log-analytics-workspace-and-azure-automation-based-solutions"></a>任务 3：创建和配置 Azure Log Analytics 工作区和基于 Azure 自动化的解决方案

在此任务中，你将创建和配置 Azure Log Analytics 工作区和基于 Azure 自动化的解决方案

1. 在 Azure 门户中，搜索并选择“Log Analytics 工作区”，然后在“Log Analytics 工作区”边栏选项卡上，选择“+ 创建”  。

1. 在“创建 Log Analytics 工作区”边栏选项卡的“基本设置”选项卡中，输入以下设置，单击“查看 + 创建”，然后单击“创建”   ：

    | 设置 | 值 |
    | --- | --- |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | 新资源组名称 az104-11-rg1 |
    | Log Analytics 工作区 | 任何唯一名称 |
    | 区域 | 在上一个任务中将虚拟机部署到的 Azure 区域的名称 |

    >注意：请确保指定在上一个任务中向其中部署了虚拟机的同一区域。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. The deployment should take about 1 minute.

1. 在 Azure 门户中，搜索并选择“自动化帐户”，然后在“自动化帐户”边栏选项卡上，选择“+ 创建”  。

1. 在“创建自动化帐户”边栏选项卡上，指定以下设置，然后在验证时单击“查看 + 创建”，再单击“创建”  ：

    | 设置 | 值 |
    | --- | --- |
    | 自动化帐户名称 | 任何唯一名称 |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | az104-11-rg1 |
    | 区域 | 根据[工作区映射文档](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)确定的 Azure 区域的名称 |

    >**注意**：务必根据[工作区映射文档](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)指定 Azure 区域

    >你需要评估 Azure 功能，以深入了解 Azure 资源的性能和配置，尤其要关注 Azure 虚拟机。

1. 单击“转到资源”。

1. 在“自动化帐户”边栏选项卡上，单击“配置管理”部分中的“清单”。

1. 在“清单”窗格中的“Log Analytics 工作区”下拉列表中，选择你之前在此任务中创建的 Log Analytics 工作区，然后单击“启用”。

    >若要实现此目的，你打算检查 Azure Monitor 的功能，包括 Log Analytics。

    >备注：这还会自动安装“更改跟踪”解决方案 。

1. 在“自动化帐户”边栏选项卡上的“更新管理”部分，单击“更新管理”，然后单击“启用”。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the installation to complete. This might take about 5 minutes.

#### <a name="task-4-review-default-monitoring-settings-of-azure-virtual-machines"></a>任务 4：查看 Azure 虚拟机的默认监视设置

在此任务中将查看 Azure 虚拟机的默认监视设置

1. 在 Azure 门户中，搜索并选择“虚拟机”，然后在“虚拟机”边栏选项卡上，单击“az104-11-vm0”。

1. 在“az104-11-vm0”边栏选项卡上，单击“监视”部分中的“指标”。

1. 在“az104-11-vm0 \| 指标”边栏选项卡中，请注意，在默认图表中唯一可用的“指标命名空间”是“虚拟机主机”  。

    >                **注意：** 我们提供 **[交互式实验室模拟](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017)** ，让你能以自己的节奏点击浏览实验室。

1. 在“指标”下拉列表中，查看可用指标的列表。

    >注意：此列表包含可从虚拟机主机收集的一系列 CPU、磁盘和网络相关指标，无需访问来宾级指标。

1. 在“指标”下拉列表中，选择“CPU 百分比”，在“聚合”下拉列表中，选择“平均”，然后查看生成的图表。

#### <a name="task-5-configure-azure-virtual-machine-diagnostic-settings"></a>任务 5：配置 Azure 虚拟机诊断设置

在此任务中，将配置 Azure 虚拟机诊断设置。

1. 在“az104-11-vm0”边栏选项卡上，单击“监视”部分的“诊断设置”。

1. 在“az104-11-vm0 \| 诊断设置”边栏选项卡的“概述”选项卡中，单击“启用来宾级别的监视”  。

    >你可能会发现交互式模拟与托管实验室之间存在细微差异，但演示的核心概念和思想是相同的。

1. 切换到“az104-11-vm0 \| 诊断设置”边栏选项卡上的“性能计数器”选项卡并查看可用的计数器 。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: By default, CPU, memory, disk, and network counters are enabled. You can switch to the <bpt id="p1">**</bpt>Custom<ept id="p1">**</ept> view for more detailed listing.

1. 切换到“az104-11-vm0 \| 诊断设置”边栏选项卡的“日志”选项卡，并查看可用的事件日志集合选项 。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: By default, log collection includes critical, error, and warning entries from the Application Log and System log, as well as Audit failure entries from the Security log. Here as well you can switch to the <bpt id="p1">**</bpt>Custom<ept id="p1">**</ept> view for more detailed configuration settings.

1. 在“az104-11-vm0”边栏选项卡上的“监视”部分，单击“Log Analytics 代理”，然后单击“启用”。   

1. 在“az104-11-vm0 - 日志”边栏选项卡上，确保在“选择 Log Analytics 工作区”下拉列表中选择了你之前在本实验室中创建的 Log Analytics 工作区，然后单击“启用”。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not wait for the operation to complete but instead proceed to the next step. The operation might take about 5 minutes.

1. 在“az104-11-vm0 \| 日志”边栏选项卡的“监视”部分中，单击“指标”  。

1. 在“az104-11-vm0 \| 指标”边栏选项卡的默认图表上，可以看到此时“指标命名空间”下拉列表中除了包含“虚拟机主机”条目外，还包含了“来宾(经典)”条目   。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This is expected, since you enabled guest-level diagnostic settings. You also have the option to <bpt id="p1">**</bpt>Enable new guest memory metrics<ept id="p1">**</ept>.

1. 在“指标命名空间”下拉列表中，选择“来宾(经典)”条目。

1. 在“指标”下拉列表中，查看可用指标的列表。

    >注意：此列表包括仅依赖于主机级监视时不可用的其他来宾级指标。

1. 在“指标”下拉列表中，选择“内存\\可用字节”，在“聚合”下拉列表中，选择“最大”，然后查看生成的图表   。

#### <a name="task-6-review-azure-monitor-functionality"></a>任务 6：审阅 Azure Monitor 功能

1. 在 Azure 门户中，搜索并选择“监视”，并在“监视 \| 概述”边栏选项卡中单击“指标”  。

1. 在“选择范围”边栏选项卡的“浏览”选项卡上，导航到“az104-11-rg0”资源组，将其展开，选中该资源组内“az104-11-vm0”虚拟机条目旁边的复选框，然后单击“应用”。

    >**注意**：这为你提供了与“az104-11-vm0 - 指标”边栏选项卡相同的视图和选项。

1. 在“指标”下拉列表中，选择“CPU 百分比”，在“聚合”下拉列表中，选择“平均”，然后查看生成的图表。

1. 在“监视 \| 指标”边栏选项卡上，在“az104-11-vm0 的平均 CPU 百分比”窗格中单击“新建预警规则”  。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Creating an alert rule from Metrics is not supported for metrics from the Guest (classic) metric namespace. This can be accomplished by using Azure Resource Manager templates, as described in the document <bpt id="p1">[</bpt>Send Guest OS metrics to the Azure Monitor metric store using a Resource Manager template for a Windows virtual machine<ept id="p1">](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm)</ept>

1. 在“创建警报规则”边栏选项卡的“条件”部分，单击现有条件条目。

1. 在“配置信号逻辑”边栏选项卡的信号列表中的“警报逻辑”部分，指定以下设置（将其他设置保留为默认值），然后单击“完成”：

    | 设置 | 值 |
    | --- | --- |
    | 阈值 | **静态** |
    | 运算符 | 大于 |
    | 聚合类型 | **平均值** |
    | 阈值 | **2** |
    | 聚合粒度（期限） | **1 分钟** |
    | 评估频率 | **每 1 分钟** |

1. 单击“下一步:操作 >”，在“创建预警规则”边栏选项卡的“操作组”部分，单击“+ 创建操作组”按钮  。

1. 在“创建操作组”边栏选项卡的“基本信息”选项卡上，指定以下设置（其他设置保留默认值）并选择“下一步:   通知 >”：

    | 设置 | 值 |
    | --- | --- |
    | 订阅 | 你在此实验室中使用的 Azure 订阅的名称 |
    | 资源组 | az104-11-rg1 |
    | 操作组名称 | az104-11-ag1 |
    | 显示名称 | az104-11-ag1 |

1. On the <bpt id="p1">**</bpt>Notifications<ept id="p1">**</ept> tab of the <bpt id="p2">**</bpt>Create an action group<ept id="p2">**</ept> blade, in the <bpt id="p3">**</bpt>Notification type<ept id="p3">**</ept> drop-down list, select <bpt id="p4">**</bpt>Email/SMS message/Push/Voice<ept id="p4">**</ept>. In the <bpt id="p1">**</bpt>Name<ept id="p1">**</ept> text box, type <bpt id="p2">**</bpt>admin email<ept id="p2">**</ept>. Click the <bpt id="p1">**</bpt>Edit details<ept id="p1">**</ept> (pencil) icon.

1. 在“电子邮件/短信/推送/语音”边栏选项卡上选中“电子邮件”复选框，在“电子邮件”文本框中键入你的电子邮件地址，将其他设置保留默认值，单击“确定”，然后返回到“创建操作组”边栏选项卡的“通知”选项卡，选择“下一步:       操作 >”。

1. 在“创建操作组”边栏选项卡的“操作”选项卡上，查看“操作类型”下拉列表中显示的项而不进行任何更改，然后选择“查看 + 创建”   。

1. 在“创建操作组”边栏选项卡的“查看 + 创建”选项卡中，选择“创建”。

1. 返回到“创建预警规则”边栏选项卡，单击“下一步:  详细信息 >”，在“预警规则详细信息”部分中，指定以下设置（其他设置保留默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 预警规则名称 | **CPU 百分比高于测试阈值** |
    | 预警规则说明 | **CPU 百分比高于测试阈值** |
    | 严重性 | **Sev 3** |
    | 创建时启用 | **是** |

1. 单击“查看 + 创建”，然后在“查看 + 创建”选项卡上，单击“创建”  。

    >注意：指标警报规则可能需要 10 分钟才能激活。

1. 在 Azure 门户中，搜索并选择“虚拟机”，然后在“虚拟机”边栏选项卡上，单击“az104-11-vm0”。

1. 在“az104-11-vm0”边栏选项卡上单击“连接”，在下拉菜单中单击“RDP”，在“使用 RDP 连接”边栏选项卡上单击“下载 RDP 文件”，并按照提示启动远程桌面会话    。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

    >**注意**：连接到目标虚拟机时，可以忽略任何警告提示。

1. 出现提示时，请使用用户名“Student”和密码从参数文件登录。

1. 在远程桌面会话中，单击“启动”，展开“Windows 系统”文件夹，然后单击“命令提示符”。

1. 在命令提示符中，运行以下命令以触发 az104-11-vm0 Azure VM 的 CPU 利用率提高：

   ```sh
   for /l %a in (0,0,1) do echo a
   ```

    >**注意**：这将启动无限循环，从而使 CPU 利用率超过新创建的警报规则的阈值。

1. 在实验室计算机上，使远程桌面会话保持打开状态，然后切换回显示 Azure 门户的浏览器窗口。

1. 在 Azure 门户中，导航回到“监视器”边栏选项卡，并单击“警报”。

1. 记下“严重性级别 3”警报的数量，然后单击“严重性级别 3”行 。

    >**注意**：可能需要等待几分钟，然后单击“刷新”。

1. 在“所有警报”边栏选项卡上，查看生成的警报。

#### <a name="task-7-review-azure-log-analytics-functionality"></a>任务 7：查看 Azure Log Analytics 功能

1. 在 Azure 门户中，导航回到“监视器”边栏选项卡，单击“日志”。

    >**注意**：如果这是你第一次访问 Log Analytics，则需要单击“开始使用”。

1. 如有必要，请单击“选择范围”，在“选择范围”边栏选项卡上选择“最近”选项卡，然后选择“az104-11-vm0”并单击“应用”。    

1. 在查询窗口中，粘贴以下查询并单击“运行”，然后查看生成的图表：

   ```sh
   // Virtual Machine available memory
   // Chart the VM's available memory over the last hour.
   InsightsMetrics
   | where TimeGenerated > ago(1h)
   | where Name == "AvailableMB"
   | project TimeGenerated, Name, Val
   | render timechart
   ```

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The query should not have any errors (indicated by red blocks on the right scroll bar). If the query will not paste without errors directly from the instructions, paste the query code into a text editor such as Notepad, and then copy and paste it into the query window from there.


1. 单击工具栏中的“查询”，在“查询”窗格中找到“跟踪 VM 可用性”磁贴，双击该磁贴以填充查询窗口，单击该磁贴中的“运行”命令按钮，然后查看结果   。

1. 在“新建查询 1”选项卡上，选择“表”标头，并查看“虚拟机”部分中的表列表  。

    >注意：多个表的名称与之前在此实验室中安装的解决方案相对应。

1. 将鼠标悬停在“VMComputer”条目上，然后单击“查看预览数据”图标 。

1. 如有任何可用数据，请在“更新”窗格中单击“在编辑器中使用” 。

    >注意：可能需要等待几分钟，更新数据才可用。

#### <a name="clean-up-resources"></a>清理资源

><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. 在 Azure 门户的“Cloud Shell”窗格中打开“PowerShell”会话。

1. 运行以下命令，列出在本模块各实验室中创建的所有资源组：

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*'
   ```

1. 通过运行以下命令，删除在此模块的实验室中创建的所有资源组：

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注意**：该命令以异步方式执行（由 -AsJob 参数决定），因此，虽然你可以随后立即在同一个 PowerShell 会话中运行另一个 PowerShell 命令，但需要几分钟才能实际删除资源组。

#### <a name="review"></a>审阅

在此实验室中，你执行了以下操作：

+ 预配实验室环境
+ 创建和配置 Azure Log Analytics 工作区和基于 Azure 自动化的解决方案
+ 查看 Azure 虚拟机的默认监视设置
+ 配置 Azure 虚拟机诊断设置
+ 查看 Azure Monitor 功能
+ 查看 Azure Log Analytics 功能
