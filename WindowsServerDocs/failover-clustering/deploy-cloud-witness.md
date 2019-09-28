---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: 为故障转移群集部署云见证
ms.prod: windows-server
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: 如何使用 Microsoft Azure 在云中托管 Windows Server 故障转移群集的见证服务器，亦即如何部署云见证。
ms.openlocfilehash: 1f38a1a436cfced8637b743817dc1b3d150f7fa6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369879"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>部署故障转移群集的云见证

> 适用于：Windows Server 2019、Windows Server 2016

Cloud 见证是一种故障转移群集仲裁见证，使用 Microsoft Azure 在群集仲裁上提供投票。 本主题概述了云见证功能、它支持的方案，以及有关如何为故障转移群集配置云见证的说明。

## <a name="CloudWitnessOverview"></a>云见证概述

图1说明了 Windows Server 2016 中的多站点延伸故障转移群集仲裁配置。 在此示例配置（图1）中，有2个数据中心（称为站点）中有2个节点。 请注意，群集有可能跨越2个以上的数据中心。 此外，每个数据中心还可以有2个以上的节点。 此设置中的典型群集仲裁配置（自动故障转移 SLA）为每个节点提供投票。 向仲裁见证提供一项额外的投票，以允许群集继续运行，即使其中一个数据中心遇到电源中断也是如此。 数学计算非常简单-有5个投票，并且需要3个投票才能使群集保持运行。  

![第三个站点中有2个节点的文件共享见证](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "文件共享见证")  
**图 1：使用文件共享见证作为仲裁见证服务器 @ no__t-0  

如果某个数据中心发生断电，为其他数据中心的群集提供平等的机会来使其保持运行状态，则建议在两个数据中心以外的位置托管仲裁见证。 这通常意味着需要第三个单独的数据中心（站点）来承载一个文件服务器，该文件服务器将作为仲裁见证（文件共享见证）使用的文件共享。  

大多数组织没有第三个单独的数据中心，它将承载支持文件共享见证的文件服务器。 这意味着，组织主要将文件服务器托管在两个数据中心中的一个数据中心，该数据中心通过扩展将数据中心作为主数据中心。 在主数据中心发生电源中断的情况下，群集将会关闭，因为另一数据中心只有2个投票，这是所需的仲裁大多数投票。 对于具有第三个单独数据中心以托管文件服务器的客户，维护支持文件共享见证的高可用文件服务器的开销会很大。 如果在公有云中托管的虚拟机的文件服务器适用于在来宾 OS 中运行的文件共享见证，则在安装 & 维护的同时，将会产生很大的开销。  

Cloud 见证是一种新的故障转移群集仲裁见证，它利用 Microsoft Azure 作为仲裁点（图2）。 它使用 Azure Blob 存储来读取/写入 Blob 文件，该文件随后用作拆分分辨率的仲裁点。  

这种方法有很多好处：
1. 利用 Microsoft Azure （无需第三个单独的数据中心）。  
2. 使用标准可用的 Azure Blob 存储（公有云中托管的虚拟机无额外的维护开销）。  
3. 同一 Azure 存储帐户可用于多个群集（每个群集一个 blob 文件; 用作 blob 文件名的群集唯一 id）。  
4. $Cost 存储帐户（每个 blob 文件写入非常小的数据，仅当群集节点的状态发生更改时才更新 blob 文件）。  
5. 内置的云见证资源类型。  

@no__t 0Diagram 将云见证作为仲裁见证服务器的多站点拉伸群集（no__t）  
**图 2:使用云见证作为仲裁见证服务器的多站点延伸群集 @ no__t-0  

如图2所示，没有需要的第三个单独的站点。 与任何其他仲裁见证一样，云见证会获得投票，并且可以参与仲裁计算。  

## <a name="CloudWitnessSupportedScenarios"></a>云见证：单个见证服务器的支持方案
如果你有一个故障转移群集部署（其中所有节点都可以访问 internet），则建议你将云见证配置为仲裁见证资源。  

支持使用云见证作为仲裁见证的某些方案如下：  
- 灾难恢复延伸的多站点群集（参见图2）。  
- 无共享存储的故障转移群集（SQL Always On 等）。  
- 在来宾 OS 内运行的故障转移群集 Microsoft Azure 虚拟机角色（或任何其他公有云）。  
- 在私有云中托管的虚拟机的来宾 OS 内运行的故障转移群集。
- 具有或不具有共享存储的存储群集，如横向扩展文件服务器群集。  
- 小型分支-办公室群集（甚至2个节点的群集）  

从 Windows Server 2012 R2 开始，建议始终配置见证服务器，因为群集会自动管理见证服务器投票，并使用动态仲裁来投票节点。  

## <a name="CloudWitnessSetUp"></a>为群集设置云见证
若要将云见证设置为群集的仲裁见证，请完成以下步骤：
1. 创建要用作云见证的 Azure 存储帐户
2. 将云见证配置为群集的仲裁见证。

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>创建要用作云见证的 Azure 存储帐户

本部分介绍如何创建存储帐户并查看和复制该帐户的终结点 Url 和访问密钥。

若要配置云见证，你必须有一个有效的 Azure 存储帐户，该帐户可用于存储 blob 文件（用于仲裁）。 Cloud 见证在 Microsoft 存储帐户下创建一个众所周知的容器**msft-见证**服务器。 云见证会写入一个 blob 文件，其中包含对应群集的唯一 ID，该 ID 用作此**msft-云-见证**容器下的 blob 文件的文件名。 这意味着，你可以使用同一个 Microsoft Azure 存储帐户为多个不同的群集配置云见证。

当你使用相同的 Azure 存储帐户为多个不同的群集配置云见证时，会自动创建一个**msft-云-见证**容器。 此容器将包含每个群集一个 blob 文件。

### <a name="to-create-an-azure-storage-account"></a>创建 Azure 存储帐户

1. 登录到[Azure 门户](http://portal.azure.com)。
2. 在 "中心" 菜单上，选择 "> 数据 + 存储-> 存储帐户"。
3. 在 "创建存储帐户" 页中，执行以下操作：
    1. 输入存储帐户的名称。
    <br>存储帐户名称必须为 3 到 24 个字符，并且只能包含数字和小写字母。 存储帐户名称在 Azure 中也必须是唯一的。
        
    2. 对于 "**帐户类型**"，选择 "**常规用途**"。
    <br>不能将 Blob 存储帐户用于云见证。
    3. 对于 "**性能**", 请选择 "**标准**"。
    <br>不能将 Azure 高级存储用于云见证。
    2. 对于**复制**，请选择 "**本地冗余存储（LRS）** "。
    <br>故障转移群集使用 blob 文件作为仲裁点，这在读取数据时需要一些一致性保证。 因此，你必须为**复制**类型选择 "**本地冗余存储**"。

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>查看和复制 Azure 存储帐户的存储访问密钥

创建 Microsoft Azure 存储帐户时，它会与自动生成的两个访问密钥关联-主访问密钥和辅助访问密钥。 对于首次创建云见证服务器，请使用**主访问密钥**。 对于要用于云见证的密钥没有任何限制。  

#### <a name="to-view-and-copy-storage-access-keys"></a>查看和复制存储访问密钥

在 Azure 门户中，导航到存储帐户，单击 "**所有设置**"，然后单击 "**访问密钥**" 查看、复制和重新生成帐户访问密钥。 "访问密钥" 边栏选项卡还包含使用主密钥和辅助密钥预配置的连接字符串，你可以复制这些字符串，以便在应用程序中使用（请参阅图4）。

![Snapshot Microsoft Azure @ no__t 中的 "管理访问密钥" 对话框  
**图4：存储访问密钥 @ no__t-0

### <a name="view-and-copy-endpoint-url-links"></a>查看和复制终结点 URL 链接  
创建存储帐户时，将使用以下格式生成以下 Url： `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

云见证始终使用**Blob**作为存储类型。 Azure 使用**core.windows.net**作为终结点。 配置云见证时，可以根据方案将其配置为不同的终结点（例如，中国的 Microsoft Azure datacenter 具有不同的终结点）。  

> [!NOTE]  
> 此终结点 URL 由云见证资源自动生成，并且该 URL 无需额外的配置步骤。  

#### <a name="to-view-and-copy-endpoint-url-links"></a>查看和复制终结点 URL 链接
在 Azure 门户中，导航到存储帐户，单击 "**所有设置**"，然后单击 "**属性**" 以查看和复制终结点 url （参见图5）。  

@no__t 云见证终结点链接 @ no__t-1 的0Snapshot  
**图5：Cloud 见证终结点 URL 链接 @ no__t-0

有关创建和管理 Azure 存储帐户的详细信息，请参阅[关于 Azure 存储帐户](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>将云见证配置为群集的仲裁见证
云见证配置在内置于故障转移群集管理器中的现有仲裁配置向导中进行了良好的集成。  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>将云见证配置为仲裁见证
1. 启动故障转移群集管理器。
2. 右键单击群集->**更多操作** -> **配置群集仲裁设置**（见图6）。 这会启动 "配置群集仲裁向导"。  
    ![Snapshot 故障转移群集管理器 UI @ no__t 中的配置群集仲裁设置 **Figure 6。群集仲裁设置 @ no__t-0

3. 在 "**选择仲裁配置**" 页上，选择 **"选择仲裁见证"** （参见图7）。  

    群集仲裁向导中的 "选择 quotrum 见证" 单选按钮的 @no__t 0Snapshot @ no__t-1  
    **Figure 7。选择仲裁配置 @ no__t-0

4. 在 "**选择仲裁见证**" 页上，选择 "**配置云见证**" （参见图8）。  

    用于选择云见证 @ no__t 的适当单选按钮 @no__t 0Snapshot）  
    **Figure 8。选择仲裁见证 @ no__t-0  

5. 在 "**配置云见证**" 页上，输入以下信息：  
   1. （必选参数）Azure 存储帐户名称。  
   2. （必选参数）与存储帐户相对应的访问密钥。  
       1. 第一次创建时，使用主访问密钥（请参阅图5）  
       2. 旋转主访问密钥时，请使用辅助访问密钥（请参阅图5）  
   3. （可选参数）如果要使用不同的 Azure 服务终结点（例如，中国中的 Microsoft Azure 服务），请更新终结点服务器名称。  

      群集仲裁向导中的 "云见证配置" 窗格的 @no__t 0Snapshot @ no__t-1  
      **Figure 9：配置云见证 @ no__t-0

6. 成功配置云见证后，可以在故障转移群集管理器管理单元中查看新创建的见证服务器资源（请参阅图10）。

    Cloud 见证 @ no__t 的 @no__t 0Successful 配置  
    **Figure 10：成功配置云见证 @ no__t-0

### <a name="configuring-cloud-witness-using-powershell"></a>使用 PowerShell 配置云见证  
现有的 Set-clusterquorum PowerShell 命令具有与云见证相对应的新附加参数。  

你可以使用以下 PowerShell 命令[`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx)配置云见证：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

如果需要使用不同的终结点（极少）：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>云见证的 Azure 存储帐户注意事项  
将云见证配置为故障转移群集的仲裁见证时，请考虑以下事项：
* 你的故障转移群集将生成并安全地存储共享访问安全（SAS）令牌，而不是存储访问密钥。  
* 只要访问密钥保持有效，生成的 SAS 令牌就有效。 旋转主访问密钥时，必须先使用辅助访问密钥更新云见证（在所有使用该存储帐户的群集上），然后再重新生成主访问密钥。  
* 云见证使用 Azure 存储帐户服务的 HTTPS REST 接口。 这意味着需要在所有群集节点上打开 HTTPS 端口。

### <a name="proxy-considerations-with-cloud-witness"></a>云见证的代理注意事项  
Cloud 见证使用 HTTPS （默认端口443）来与 Azure blob 服务建立通信。 确保可通过网络代理访问 HTTPS 端口。

## <a name="see-also"></a>请参阅
- [Windows Server 中故障转移群集的新增功能](whats-new-in-failover-clustering.md)
