---
demo:
  title: 演示 07：管理 Azure 存储
  module: Administer Azure Storage
---


# 07 - 管理 Azure 存储

## 配置存储帐户

在本演示中，我们将创建一个存储帐户。

参考：[创建存储帐户](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. 使用 Azure 门户。

1. 查看存储帐户的用途。 
   
1. 搜索并选择“存储帐户”。 
 
1. 创建基本存储帐户。 

    - 讨论有关命名存储帐户的要求，以及名称在 Azure 中唯一的必要性。 

    - 查看不同的存储类型。 例如，常规用途 v2。 

    - 查看访问层选择。 例如，冷层和热层。 

    - 其他选项卡和设置将在其他演示中介绍。 

1. 创建存储帐户并等待资源部署。 


## 配置 Blob 存储

在本演示中，我们将探索 Blob 存储。

注意：这些步骤需要一个存储帐户 。

参考：[快速入门：上传、下载和列出 Blob](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. 前往 Azure 门户中的存储帐户。

1. 查看 Blob 存储的用途。 

1. 创建 Blob 容器。 查看容器的访问级别。 例如，专用（没有匿名访问）。 

1. 将 blob 上传到容器。 如果你有时间，可查看高级设置。 例如，Blob 类型和 Blob 大小。 

## 配置存储安全性

在本演示中，我们将创建一个共享访问签名。

注意：本演示需要包含 Blob 容器和已上传文件的存储帐户。 

参考：[为存储容器创建 SAS 令牌](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. 选择要保护的 Blob 或文件。 

1. 生成共享访问签名 (SAS)。 查看权限、开始和到期时间以及允许的协议。

1. 使用 SAS URL 确保显示资源。 


## 配置 Azure 文件存储 

在此次演示中，我们将使用文件共享和快照。

注意：这些步骤需要一个存储帐户 。

参考：[管理 Azure 文件共享的快速入门](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. 查看文件共享的用途。 

1. 访问存储帐户并单击“文件” **** 。

1. 创建文件共享。 查看配额、上传文件以及添加目录以组织信息。 

1. 创建文件共享快照。 查看何时使用快照，以及它们与备份的不同之处。 如果有时间，可上传文件、拍摄快照、删除文件并还原快照。


## 存储工具（可选）

在本演示中，我们将介绍几种常见的 Azure 存储工具。 

参考：[存储资源管理器入门](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. 安装存储资源管理器或使用存储浏览器。

1. 查看如何浏览和创建存储资源。 例如，添加 Blob 容器。 

参考：[使用 AzCopy v10 将数据复制或移到 Azure 存储](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. 讨论何时使用 AzCopy。 查看帮助 azcopy /?。

1. 向下滚动“示例”部分 ****  。 如果有时间，请尝试任何示例。 
    



