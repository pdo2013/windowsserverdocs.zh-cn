---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: "部署故障转移群集云见证"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 10/2/2017
description: "如何使用 Microsoft Azure 存放在云中的 Windows Server 故障转移群集见证即如何部署云见证。"
ms.openlocfilehash: 564c6668fcc80a8bd1531c05c142996689de8154
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>部署故障转移群集云见证

> 适用于：适用于：Windows Server（半年通道）、Windows Server 2016

云见证是一种新的 Windows Server 2016 未引入故障转移群集仲裁见证。 本主题提供概述云见证功能、方案的支持它，以及有关如何配置云见证故障转移群集运行 Windows Server 2016 的说明进行操作。

## <a name="CloudWitnessOverview"></a>云见证概述

1 图所示多站点拉伸故障转移群集仲裁配置与 Windows Server 2016。 在此示例配置（图 1）中，在 2 数据中心（也称为站点）中有 2 个节点。 请注意，可能会群集跨越 2 个以上的数据中心。 此外，每个 datacenter 可以有 2 个以上节点。 此设置（自动故障转移 SLA）中的典型群集仲裁配置为每个节点投票。 一个额外投票分发给仲裁见证，以允许群集保留的数据中心运行即使任一一个遭遇断电时。 非常简单的数学运算-5 总投票和需要 3 投票群集以使其保持运行。  

![在第三个单独的文件共享见证网站 2 中 2 个节点与其他站点](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "见证文件共享")  
**图 1 部分：使用作为仲裁见证见证文件共享**  

情形断电一个数据中心中，以确保它运行的，其他数据中心中对群集以便等机会建议主机仲裁见证在以外两个数据中心位置。 这通常意味着要求第三个单独的 datacenter（站点）托管文件服务器正在备份文件共享用作主仲裁见证（共享见证文件）。  

大多数企业不具有第三个单独的将举办备份文件共享见证文件服务器的数据中心。 这意味着组织主要存放文件服务器，在两个数据中心，这样的扩展，该数据中心主要数据中心之一。 在方案中的主要数据中心中断电，群集会下跌其他数据中心只有 2 投票这低于 3 投票所需的仲裁大多数当。 有三个单独的 datacenter 托管文件服务器的客户，它是开销，以便保留备份文件共享见证高度可用的文件服务器。 举办有见证运行来宾操作系统中的文件共享文件服务器的虚拟机云中的公用是设置和维护在很大的开销。  

云见证是一种新的仲裁点（图 2）利用 Microsoft Azure 的故障转移群集仲裁见证。 它使用 Azure 斑点存储读取/写入斑点文件，然后用作主情形裂分辨率仲裁点。  

有重大的好处这种方法：
1. 利用 Microsoft Azure（无需第三个单独的 datacenter）。  
2. 使用标准可用 Azure 斑点存储（公共云中托管了虚拟机任何额外维护开销）。  
3. Azure 存储的相同帐户可用于多个群集（群集每一个斑点文件; 群集用作斑点文件名称的唯一 id）。  
4. 到存储帐户（很小数据编写每个斑点文件斑点文件更新仅当群集节点状态发生更改后）非常低上转 $cost。  
5. 内置的云见证资源类型。  

![使用云见证作为仲裁见证多站点拉伸的问题群集的示意图](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**图 2 部分：多站点拉伸使用云见证群集作为仲裁见证**  

图 2 中所示，没有任何所需第三个单独的网站。 云见证，如任何其他仲裁见证获取投票，并且可以参与仲裁计算。  

## <a name="CloudWitnessSupportedScenarios"></a>作为一个见证类型云见证：支持的方案
如果你有故障转移群集部署，所有节点了可以都连接到互联网（通过 Azure 扩展名），建议你将云见证配置为你仲裁见证资源。  

某些受支持的方案用作的云见证仲裁见证如下所示：  
- 灾难恢复拉伸多站点群集（参见 2 图）。  
- 故障转移群集不共享存储（SQL 始终上等）。  
- 故障转移群集来宾操作系统，Microsoft Azure 虚拟机角色（或任何其他公共云）中托管内运行。  
- 运行内来宾操作系统的虚拟机托管专用云霞在故障转移群集。
- 存储在诸如规模文件服务器群集群集使用或共享存储，不。  
- 小分支机构群集（甚至 2 个节点群集）  

开始与 Windows Server 2012 R2，它建议群集自动管理见证投票以及节点投票，与动态仲裁始终配置见证。  

## <a name="CloudWitnessSetUp"></a>对于群集云见证设置
若要设置为你的群集仲裁见证云见证，完成以下步骤：
1. 创建用于云见证 Azure 存储帐户
2. 作为你的群集仲裁见证配置云见证。

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>创建用于云见证 Azure 存储帐户

此部分中介绍了如何创建存储帐户视图和副本端点 Url 和访问该帐户的键。

若要配置云见证，必须具有有效的 Azure 存储帐户，可以用于存储斑点文件（用于仲裁）。 云见证创建一个已知容器**msft-云见证**下存储 Microsoft 帐户。 云见证写入斑点一个文件具有相应群集的唯一 ID 用作文件名称在斑点文件下此**msft-云见证**容器。 这意味着你可以使用相同的 Microsoft Azure 存储帐户配置为多个不同的群集云见证。

当你使用同一 Azure 存储帐户的多个不同的配置云见证群集，单个**msft-云见证**容器获取自动创建。 此容器将包含群集每一个斑点文件。

### <a name="to-create-an-azure-storage-account"></a>若要创建 Azure 存储帐户

1. 登录到[Azure 门户](http://portal.azure.com)。
2. 在中心菜单中，选择新-> 数据 + 存储-> 存储帐户。
3. 在创建存储帐户页面上，执行以下操作：
    1. 输入存储你的帐户的名称。
    <br>存储帐户名称必须之间 3 和 24 个字符，可能包含数字和仅小写字母。 必须在 Azure 唯一存储帐户名。
        
    2. 对于**帐户 kind**、选择**常规用途**。
    <br>不能用于云见证斑点存储帐户。
    3. 对于**性能**、选择**标准**。
    <br>不能用于云见证 Azure 高级版存储。
    2. 对于**复制**、选择**本地冗余存储 (LRS)**。
    <br>故障转移群集斑点文件用作仲裁点，这需要一些一致性保障时读取数据。 两必须选择**本地冗余存储**为**复制**类型。

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>查看和 Azure 存储帐户复制存储访问键

当您创建 Microsoft Azure 存储帐户时，它都与两个访问键自动生成的主要访问键和辅助访问键关联。 云见证第一次创建，使用**主要访问键**。 没有任何限制关于的用于云见证键。  

#### <a name="to-view-and-copy-storage-access-keys"></a>若要查看和复制存储访问键

在 Azure 门户中，转到存储你的帐户中，单击**所有设置**，然后单击**访问键**若要查看，复制和重新生成你帐户的访问键。 访问键刀口还包括预配置的连接字符串使用主要和次要键，您可以复制（请参阅图 4）应用程序中使用。

![在 Microsoft Azure 管理访问键对话框的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**图 4：存储访问键**

### <a name="view-and-copy-endpoint-url-links"></a>查看并复制端点 URL 链接  
创建存储帐户时，下列 Url 生成使用格式： `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

始终使用云见证**斑点**为存储类型。 Azure 使用**。core.windows.net**为端点。 配置云见证，时，可以在您将配置它使用不同的 endpoint 根据你的情况下（例如 Microsoft Azure 数据中心，在中国具有不同的 endpoint）。  

> [!NOTE]  
> 通过云见证资源自动产生端点 URL，并且没有任何额外的步骤，来配置必需的 URL。  

#### <a name="to-view-and-copy-endpoint-url-links"></a>若要查看和复制端点 URL 链接
在 Azure 门户中，转到存储你的帐户中，单击**所有设置**，然后单击**属性**即可查看和复制你的端点 Url（参见 5 图）。  

![云见证端点链接的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**图 5：云见证端点 URL 链接**

有关如何创建和管理 Azure 存储帐户的详细信息，请参阅[关于 Azure 存储帐户](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>作为你的群集仲裁见证配置云见证
云见证配置为在现有仲裁配置向导内置插入故障转移群集管理器内完善集成。  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>若要将云见证配置为仲裁见证
1. 启动故障转移群集管理器。
2. 右键单击-> 群集**更多操作** -> **配置群集仲裁设置**（请参阅图 6）。 这将启动配置群集仲裁向导。  
    ![菜单通往 Configue 群集故障转移群集管理器 UI 中的仲裁设置的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png)**图 6。群集仲裁设置**

3. 在**选择仲裁配置**页上，选择**选择仲裁见证**（参见 7 图）。  

    ![选择 quotrum 见证快照单选群集仲裁向导中的按钮](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **图 7。 选择仲裁配置**

4. 在**选择仲裁见证**页上，选择**配置云见证**（请参阅图 8）。  

    ![若要选择云见证相应的单选按钮的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **图 8。 选择仲裁见证**  

5. 在**配置云见证**页上，输入以下信息：  
    1. （所需的参数）Azure 存储帐户名。  
    2. （所需的参数）对应于存储帐户的访问键。  
        1. 当创建第一次，使用主要访问键（请参阅图 5）  
        2. 当旋转主要的访问键，使用辅助访问键（请参阅图 5）  
    3. （可选参数）如果你打算使用 Azure 服务的不同端点（例如 Microsoft Azure 服务在中国），然后更新端点服务器名称。  

    ![群集仲裁向导中的云见证配置窗格的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **图 9 部分：配置云见证**

6. 在云中见证成功配置，你可以查看新创建的见证资源故障转移群集管理器中贴靠-在（参见 10 图）。

    ![成功配置了云见证](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **图 10：成功配置了云见证**

### <a name="configuring-cloud-witness-using-powershell"></a>配置使用 PowerShell 云见证  
现有 Set-ClusterQuorum PowerShell 命令有新对应于云中见证的其他参数。  

您可以使用云见证配置[`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx)以下 PowerShell 命令：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

以防你需要使用不同的 endpoint（拥有稀世）：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Azure 云见证与的存储帐户注意事项  
在云中见证配置为故障转移群集仲裁见证时，请考虑以下各项：
* 而不是存储访问键，故障转移群集将生成和安全地存储共享访问权限安全 (SAS) 标记。  
* 只要保持有效的访问键，生成的 SAS 令牌才有效。 旋转主要的访问键时，务必首先之前重新生成主要的访问键和次要的访问键更新云见证（在所有你群集正在使用该存储帐户）。  
* 云见证使用 Azure 存储帐户服务 HTTPS 其余界面。 这意味着它需要打开所有群集节点上的 HTTPS 端口。

### <a name="proxy-considerations-with-cloud-witness"></a>使用云见证代理注意事项  
云见证使用 HTTPS（默认端口 443）与 Azure 斑点服务建立通信。 确保 HTTPS 端口通过网络代理易于访问。

## <a name="see-also"></a>请参阅
- [什么是 Windows 服务器在群集故障转移中的新增功能](whats-new-in-failover-clustering.md)
