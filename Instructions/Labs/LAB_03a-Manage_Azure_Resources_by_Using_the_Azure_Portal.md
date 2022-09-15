---
lab:
  title: 03a - 使用 Azure 门户管理 Azure 资源
  module: Administer Azure Resources
---

# <a name="lab-03a---manage-azure-resources-by-using-the-azure-portal"></a>实验室 03a - 使用 Azure 门户管理 Azure 资源
# <a name="student-lab-manual"></a>学生实验室手册

## <a name="lab-scenario"></a>实验室方案

You need to explore the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups, including moving resources between resource groups. You also want to explore options for protecting disk resources from being accidentally deleted, while still allowing for modifying their performance characteristics and size.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%204)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>目标

在本实验室中，我们将：

+ 任务 1：创建资源组并将资源部署到资源组
+ 任务 2：在资源组之间移动资源
+ 任务 3：实现和测试资源锁

## <a name="estimated-timing-20-minutes"></a>预计用时：20 分钟

## <a name="architecture-diagram"></a>体系结构关系图

![image](../media/lab03a.png)

## <a name="instructions"></a>说明

### <a name="exercise-1"></a>练习 1

#### <a name="task-1-create-resource-groups-and-deploy-resources-to-resource-groups"></a>任务 1：创建资源组并将资源部署到资源组

在此任务中，你将使用 Azure 门户创建资源组，并在资源组中创建一个磁盘。

1. 登录到 [**Azure 门户**](http://portal.azure.com)。

1. 在 Azure 门户中，搜索并选择“磁盘”，单击“+ 创建”，然后指定以下设置 ：

    |设置|值|
    |---|---|
    |订阅| 创建的资源组所在的 Azure 订阅的名称 |
    |资源组| 新建资源组的名称 az104-03a-rg1 |
    |磁盘名称| **az104-03a-disk1** |
    |区域| **（美国）美国东部** |
    |可用性区域| **无** |
    |源类型| 无  |

    >**注意**：在创建资源时，你可以选择创建新资源组，也可以使用现有资源组。

1. 将磁盘类型和大小分别更改为“标准 HDD”和“32 GiB”。

1. 单击“查看 + 创建”，然后单击“创建”。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait until the disk is created. This should take less than a minute.

#### <a name="task-2-move-resources-between-resource-groups"></a>任务 2：在资源组之间移动资源 

在此任务中，我们会将你在上一个任务中创建的磁盘资源移至新的资源组。 

1. 搜索并选择“资源组”。 

1. 在“资源组”边栏选项卡中，单击表示你在上一任务中创建的“az104-03a-rg1”资源组的条目。

1. 在资源组“概述”边栏选项卡的资源组资源列表中，选择表示新建磁盘的条目，然后单击工具栏中的“移动”，然后在下拉列表中选择“移动到另一个资源组”。

    >**注意**：此方法可以让你同时移动多个资源。 

1. Below the <bpt id="p1">**</bpt>Resource group<ept id="p1">**</ept> text box, click <bpt id="p2">**</bpt>Create new<ept id="p2">**</ept> then type <bpt id="p3">**</bpt>az104-03a-rg2<ept id="p3">**</ept> in the text box. On the Review tab, select the checkbox <bpt id="p1">**</bpt>I understand that tools and scripts associated with moved resources will not work until I update them to use new resource IDs<ept id="p1">**</ept>, and click <bpt id="p2">**</bpt>Move<ept id="p2">**</ept>.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not wait for the move to complete but instead proceed to the next task. The move might take about 10 minutes. You can determine that the operation was completed by monitoring activity log entries of the source or target resource group. Revisit this step once you complete the next task.

#### <a name="task-3-implement-resource-locks"></a>任务 3：实现资源锁

在此任务中，你将把资源锁应用于包含磁盘资源的 Azure 资源组。

1. 在 Azure 门户中，搜索并选择“磁盘”，单击“+ 创建”，然后指定以下设置 ：

    |设置|值|
    |---|---|
    |订阅| 本实验室所用订阅的名称 |
    |资源组| 单击“新建”资源组，并将其命名为 az104-03a-rg3  |
    |磁盘名称| **az104-03a-disk2** |
    |区域| 本实验室中创建的其他资源组所在 Azure 区域的名称 |
    |可用性区域| **无** |
    |源类型| **无** |

1. 将磁盘类型和大小分别设置为“标准 HDD”和“32 GiB”。

1. 单击“查看 + 创建”，然后单击“创建”。

1. 单击“转到资源”。

1. 在磁盘的“概述”页上，单击资源组的名称 az104-03a-rg3。

1. 在“az104-03a-rg3”资源组边栏选项卡中，依次单击“锁”和“+ 添加”，然后指定如下设置  ：

    |设置|值|
    |---|---|
    |锁名称| **az104-03a-delete-lock** |
    |锁类型| **删除** |
    
1. 单击 **“确定”**    

1. 在“az104-03a-rg3”资源组边栏选项卡中，单击“概述”，在资源组资源列表中，选择表示你先前在此任务中创建的磁盘的条目，然后在工具栏中单击“删除”。 

1. 当提示“是否要删除所有选择的资源?”时，在“确认删除”文本框中键入“是”，然后单击“删除”。

1. 你应该会看到一条错误消息，通知删除操作失败。 

    >**注意**：正如错误消息所述，由于在资源组级别上应用了删除锁，所以会出现这种现象。

1. 导航回到“az104-03a-rg3”资源组的资源列表，然后单击表示“az104-03a-disk2”资源的条目。 

1. 你需要探索与预配资源并根据资源组整理资源相关的基本 Azure 管理功能，包括在资源组之间移动资源。

    >**注意**：这很正常，因为资源组级别的锁定仅适用于删除操作。 

#### <a name="clean-up-resources"></a>清理资源

   >你还需要探索可保护磁盘资源不意外遭到删除，同时仍允许修改其性能特征和大小的方案选项。

1. 导航到“az104-03a-rg3”资源组边栏选项卡，显示“锁”边栏选项卡，然后通过单击“删除”锁定条目右侧的“删除”链接移除“az104-03a-delete-lock”锁。

#### <a name="review"></a>审阅

在此实验室中，你执行了以下操作：

- 创建资源组并将资源部署到资源组
- 在资源组之间移动资源
- 实现和测试资源锁
