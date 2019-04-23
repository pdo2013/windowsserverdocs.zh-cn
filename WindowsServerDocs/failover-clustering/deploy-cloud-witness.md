---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: 部署故障转移群集的云见证
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: 如何使用 Microsoft Azure 来托管在云中的 Windows Server 故障转移群集的见证服务器也称为如何部署云见证。
ms.openlocfilehash: f7e1c84e54f08044a772f06e591588c1add33026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857978"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>部署故障转移群集的云见证

> 适用于：Windows Server 2019，Windows Server 2016 中，Windows Server （半年频道）

云见证是一种类型的使用 Microsoft Azure 提供在群集仲裁投票的故障转移群集仲裁见证。 本主题概述了云见证服务器功能，它支持的方案和有关如何配置云见证服务器故障转移群集的说明。

## <a name="CloudWitnessOverview"></a>云见证服务器概述

图 1 说明了多站点延伸故障转移群集仲裁配置 Windows Server 2016。 在此示例配置 （图 1），2 个数据中心 （也称为站点） 中有 2 个节点。 请注意，很可能跨多个 2 个数据中心的群集。 此外，每个数据中心可以有 2 个以上的节点。 在此安装程序 （自动故障转移的 SLA） 中的典型的群集仲裁配置为每个节点提供了一票。 一个额外的投票提供给仲裁见证，以允许群集保留的数据中心运行甚至如果一个遇到断电。 数学计算很简单-有 5 个总投票，您需要 3 个投票的群集，以使其运行。  

![在第三个单独的文件共享见证站点 2 中的 2 个节点与其他站点](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "文件共享见证")  
**图 1：使用文件共享见证的仲裁见证服务器**  

发生在数据中心中的电源中断时，为群集中其他数据中心，以使其保持运行，以便平等的机会建议来托管两个数据中心之外的某个位置中的仲裁见证服务器。 这通常意味着需要第三个单独的数据中心 （站点） 来承载文件服务器的备份文件共享用作仲裁见证服务器 （文件共享见证）。  

大多数组织没有第三个单独的数据中心将承载文件服务器备份文件共享见证。 这意味着组织主要托管两个数据中心，这通过扩展，使该数据中心的主数据中心之一中的文件服务器。 在方案中主数据中心中的电源中断，如另一个数据中心仅将具有 2 个投票这低于所需的 3 个投票仲裁大多数，群集会向下转。 对于具有第三个单独的数据中心，以托管文件服务器的客户，它是一个开销保持高度可用文件服务器备份文件共享见证。 承载在公有云中的虚拟机具有针对来宾 OS 中运行的文件共享见证的文件服务器是在安装和维护方面很大的开销。  

云见证是一种新型的故障转移群集仲裁见证服务器，可利用 Microsoft Azure 作为仲裁点 （图 2）。 它使用 Azure Blob 存储来读/写的 blob 文件，然后用来作为仲裁点发生裂脑解析时。  

有重大权益的这种方法：
1. 利用 Microsoft Azure （无需第三个单独的数据中心）。  
2. 使用标准的可用 Azure Blob 存储 （托管在公有云中的虚拟机没有额外的维护开销）。  
3. 同一个 Azure 存储帐户可以用于多个群集 （每个群集的一个 blob 文件; 群集用作 blob 文件名称的唯一 id）。  
4. 非常低正在 $cost 到存储帐户 （写入每个 blob 文件，仅当群集节点的状态发生更改后更新的 blob 文件非常小数据）。  
5. 内置的云见证资源类型。  

![说明使用云见证的仲裁见证服务器的多站点延伸的群集的关系图](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**图 2:使用云见证的仲裁见证服务器的多站点外延式的群集**  

图 2 中所示，没有任何第三个单独的站点所需的。 云见证服务器，像任何其他仲裁见证获取投票，并且可以参与仲裁计算。  

## <a name="CloudWitnessSupportedScenarios"></a>云见证：单个见证服务器类型的支持的方案
如果有一个故障转移群集部署，其中的所有节点可以都连接到互联网 （通过 Azure 的扩展），建议将云见证服务器配置为仲裁见证资源。  

在某些情形下支持使用云见证服务器的因为仲裁见证服务器，如下所示：  
- 灾难恢复拉伸多站点群集 （见图 2）。  
- 故障转移群集而不使用共享存储 （SQL 始终上等）。  
- 在托管在 Microsoft Azure 虚拟机角色 （或任何其他公有云） 中的来宾 OS 内运行故障转移群集。  
- 运行在来宾 OS 的虚拟机托管在私有云中的故障转移群集。
- 存储群集使用或不共享存储，如横向扩展文件服务器群集。  
- 小型分支机构群集 （甚至 2 节点群集）  

从 Windows Server 2012 R2 开始，建议始终作为群集自动管理见证投票和节点投票的动态仲裁配置见证服务器。  

## <a name="CloudWitnessSetUp"></a> 设置群集的云见证服务器
若要将设置云见证服务器群集仲裁见证，请完成以下步骤：
1. 创建用于将云见证服务器的 Azure 存储帐户
2. 将云见证服务器配置为群集的仲裁见证。

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>创建用于将云见证服务器的 Azure 存储帐户

本部分介绍如何创建存储帐户并查看和复制终结点 Url 和该帐户的访问密钥。

若要配置云见证服务器，必须具有一个有效的 Azure 存储帐户用于存储 blob 文件 （用于约束性仲裁）。 云见证服务器创建的已知容器**msft 云见证**Microsoft 存储帐户下。 云见证服务器写入单个 blob 文件相对应的群集的唯一 ID 用作依据此 blob 文件的文件名**msft 云见证**容器。 这意味着您可以使用相同 Microsoft Azure 存储帐户来配置云见证服务器的多个不同的群集。

配置云见证服务器的多个不同使用相同的 Azure 存储帐户时的群集，请将单个**msft 云见证**容器会自动创建。 此容器将包含每个群集的一个 blob 文件。

### <a name="to-create-an-azure-storage-account"></a>若要创建 Azure 存储帐户

1. 登录到[Azure 门户](http://portal.azure.com)。
2. 在中心菜单上，选择新-> 数据 + 存储-> 存储帐户。
3. 在创建存储帐户页中，执行以下步骤：
    1. 输入你的存储帐户的名称。
    <br>存储帐户名称必须为 3 到 24 个字符，并且只能包含数字和小写字母。 存储帐户名称还必须在 Azure 中唯一。
        
    2. 有关**帐户类型**，选择**常规用途**。
    <br>Blob 存储帐户不能用于云见证。
    3. 有关**性能**，选择**标准**。
    <br>Azure 高级存储不能用于云见证。
    2. 有关**复制**，选择**本地冗余存储 (LRS)** 。
    <br>故障转移群集作为仲裁点，这需要一些一致性保证读取数据时使用的 blob 文件。 因此您必须选择**本地冗余存储**有关**复制**类型。

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>查看和复制存储访问密钥的 Azure 存储帐户

创建 Microsoft Azure 存储帐户，请时，与两个访问密钥的自动生成的主访问密钥和辅助访问密钥相关联。 对于云见证服务器的第一次创建，使用**主访问密钥**。 没有任何限制有关要用于云见证服务器的密钥。  

#### <a name="to-view-and-copy-storage-access-keys"></a>若要查看和复制存储访问密钥

在 Azure 门户中，导航到你的存储帐户，单击**的所有设置**，然后单击**访问密钥**若要查看、 复制和重新生成帐户访问密钥。 在访问密钥边栏选项卡还包括使用主要和辅助密钥，你可以复制 （请参阅图 4） 在应用程序中使用的预配置的连接字符串。

![Microsoft Azure 中的管理访问密钥对话框的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**图 4:存储访问密钥**

### <a name="view-and-copy-endpoint-url-links"></a>查看和复制终结点的 URL 链接  
当你创建存储帐户时，请使用格式生成以下 Url: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

始终使用云见证**Blob**作为存储类型。 Azure 使用 **。 core.windows.net 组合在一起**作为终结点。 在配置云见证服务器时，就可以配置它与不同的终结点根据你的方案 （例如在中国的 Microsoft Azure 数据中心有一个不同的终结点）。  

> [!NOTE]  
> 云见证资源的自动生成的终结点 URL，并且没有 URL 所需的配置的任何额外的步骤。  

#### <a name="to-view-and-copy-endpoint-url-links"></a>若要查看和复制终结点的 URL 链接
在 Azure 门户中，导航到你的存储帐户，单击**的所有设置**，然后单击**属性**若要查看和复制终结点 Url （请参阅图 5）。  

![云见证服务器终结点链接的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**图 5:云见证服务器终结点的 URL 链接**

有关创建和管理 Azure 存储帐户的详细信息，请参阅[关于 Azure 存储帐户](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>作为群集的仲裁见证配置云见证
云见证服务器配置为生成到故障转移群集管理器中现有的仲裁配置向导中良好地集成。  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>若要将云见证服务器配置为仲裁见证
1. 启动故障转移群集管理器。
2. 右键单击群集->**更多操作** -> **配置群集仲裁设置**（见图 6）。 这将启动配置群集仲裁向导。  
    ![配置群集仲裁设置故障转移群集管理器 UI 中的菜单路径的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png)**图 6。群集仲裁设置**

3. 上**选择的仲裁配置**页上，选择**选择仲裁见证**（请参阅图 7）。  

    ![快照的 select quotrum 见证服务器单选按钮在群集仲裁向导](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **图 7。选择的仲裁配置**

4. 上**选择仲裁见证**页上，选择**配置云见证**（见图 8）。  

    ![相应的单选按钮以选择云见证服务器的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **图 8。选择的仲裁见证服务器**  

5. 上**配置云见证**页上，输入以下信息：  
    1. （所需的参数）Azure 存储帐户名称。  
    2. （所需的参数）对应于存储帐户访问密钥。  
        1. 当第一次创建，请使用主访问密钥 （请参阅图 5）  
        2. 在轮换主访问密钥时，使用辅助访问密钥 （请参阅图 5）  
    3. （可选参数）如果你想要使用不同的 Azure 服务终结点 （例如中国的 Microsoft Azure 服务），然后更新终结点服务器名称。  

    ![群集仲裁向导中云见证配置窗格的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **图 9:配置云见证**

6. 后成功配置了云见证服务器，您可以查看新创建的见证资源在故障转移群集管理器管理单元中 （请参阅图 10）。

    ![成功配置云见证](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **图 10:成功配置云见证**

### <a name="configuring-cloud-witness-using-powershell"></a>使用 PowerShell 配置云见证  
现有 Set-clusterquorum PowerShell 命令具有与云见证服务器相对应的新附加参数。  

您可以使用云见证配置[ `Set-ClusterQuorum` ](https://technet.microsoft.com/library/ee461013.aspx)以下 PowerShell 命令：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

如果您需要使用不同的终结点 （极少见的）：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>使用云见证服务器的 azure 存储帐户注意事项  
在配置云见证服务器作为故障转移群集仲裁见证服务器时，考虑以下方面：
* 而不是存储访问密钥，在故障转移群集将生成并安全地存储共享访问安全 (SAS) 令牌。  
* 只要访问密钥保持有效，生成的 SAS 令牌的有效期。 在轮换主访问密钥时，务必首先在重新生成主访问密钥之前使用辅助访问密钥更新云见证服务器 （所有在群集上使用该存储帐户）。  
* 云见证服务器使用 Azure 存储帐户服务的 HTTPS REST 的接口。 这意味着它需要要在所有群集节点上打开的 HTTPS 端口。

### <a name="proxy-considerations-with-cloud-witness"></a>使用云见证服务器的代理服务器注意事项  
云见证使用 HTTPS （默认端口 443） 建立与 Azure blob 服务的通信。 确保 HTTPS 端口通过网络代理可访问。

## <a name="see-also"></a>请参阅
- [什么是 Windows Server 中故障转移群集中的新增功能](whats-new-in-failover-clustering.md)
