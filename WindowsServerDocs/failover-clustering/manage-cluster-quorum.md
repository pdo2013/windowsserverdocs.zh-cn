---
title: 在故障转移群集中配置和管理仲裁
description: 有关如何在 Windows Server 故障转移群集中管理群集仲裁的详细信息。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03e155cb9d30bc32da407f0d9ae915308f31494a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361023"
---
# <a name="configure-and-manage-quorum"></a>配置和管理仲裁

>适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题提供了在 Windows Server 故障转移群集中配置和管理仲裁的背景和步骤。

## <a name="understanding-quorum"></a>了解仲裁

群集的仲裁由投票元素的数量确定，这些投票元素必须是活动群集成员身份的一部分以供该群集正确启动或继续运行。 有关更详细的说明，请参阅[了解群集和池仲裁文档](../storage/storage-spaces/understand-quorum.md)。

## <a name="quorum-configuration-options"></a>仲裁配置选项

Windows Server 中的仲裁模型非常灵活。 如果需要修改群集的仲裁配置，可以使用配置群集仲裁向导或故障转移群集 Windows PowerShell cmdlet。 有关配置仲裁的步骤和注意事项，请参阅本主题后面的 [配置群集仲裁](#configure-the-cluster-quorum) 。

下表列出了配置群集仲裁向导中提供的三个仲裁配置选项。

| 选项  |描述  |
| --------- | ---------|
| “使用典型设置”     |  该群集自动将投票分配到每个节点并且动态管理节点投票。 如果它适用于你的群集，并且有可用的群集共享存储，该群集将选择磁盘见证。 建议在大多数情况下使用此选项，因为群集软件会自动选择可为群集提供最高可用性的仲裁和见证配置。       |
| “添加或更改仲裁见证”     |   你可以添加、更改或删除见证资源。 可以配置文件共享或磁盘见证。 该群集自动将投票分配到每个节点并且动态管理节点投票。      |
| “高级仲裁配置和见证选择”     | 应仅当你具有有关配置仲裁的特定于应用程序或特定于站点的要求时才选择该选项。 可以修改仲裁见证、添加或删除节点投票，以及选择该群集是否动态管理节点投票。 默认情况下，将投票分配给所有节点，并动态管理节点投票。        |

根据你选择的仲裁配置选项和特定设置，将在以下仲裁模式之一中配置群集：

| 模式  | 描述  |
| --------- | ---------|
| 多数节点（无见证）     |   仅节点具有投票。 没有配置任何仲裁见证。 群集仲裁是活动群集成员身份中的多数投票节点。      |
| 带有见证的多数节点（磁盘或文件共享）     |   节点具有投票。 此外，仲裁见证有一票。 群集仲裁是活动群集成员身份中的多数投票节点以及一个见证投票。 仲裁见证可以是指定的磁盘见证或文件共享见证。 
| 无多数（仅磁盘见证）     | 没有节点具有投票。 仅磁盘见证有一票。 <br>群集仲裁由磁盘见证的状态确定。 通常，不建议使用此模式，并且不应该选择它，因为它为群集创建了单点故障。       |

以下小节将向你介绍高级仲裁配置设置的详细信息。

### <a name="witness-configuration"></a>见证配置

作为一般规则，当你配置仲裁时，群集中的投票元素应为奇数。 因此，如果群集包含偶数个投票节点，则应配置磁盘见证或文件共享见证。 群集将能够维持一个额外节点关闭。 此外，如果一半的群集节点同时关闭或断开连接，则添加见证投票会使该群集继续运行。

如果所有节点都可以看到该磁盘，则通常建议使用磁盘见证。 当你需要考虑使用复制存储的多站点灾难恢复时，建议使用文件共享见证。 仅当存储供应商支持从所有站点到复制存储的读写访问时，才可以使用复制存储配置磁盘见证。 <strong>*存储空间直通不支持磁盘见证*</strong>。

下表提供了有关仲裁见证类型的其他信息和注意事项。

| 见证类型  | 描述  | 要求和建议  |
| ---------    |---------        |---------                        |
| 磁盘见证     |  <ul><li> 存储群集数据库副本的专用 LUN</li><li> 对具有共享（非复制）存储的群集最有用</li>       |  <ul><li>LUN 的大小必须至少有 512 MB</li><li> 必须专用于群集而且不分配给群集角色</li><li> 必须包括在群集存储中并且通过存储验证测试</li><li> 不可以是群集共享卷 (CSV) 磁盘</li><li> 具有单个卷的基本磁盘</li><li> 不需要有驱动器号</li><li> 可以使用 NTFS 或 ReFS 格式化</li><li> 可以使用容错功能的硬件 RAID 有选择性地进行配置</li><li> 应从备份和防病毒扫描中排除</li><li> 存储空间直通不支持磁盘见证</li>|
| 文件共享见证     | <ul><li>在运行 Windows Server 的文件服务器上配置的 SMB 文件共享</li><li> 不存储群集数据库的副本</li><li> 仅维护 witness.log 文件中的群集信息</li><li> 对具有复制存储的多站点群集最有用 </li>       |  <ul><li>必须至少具有 5 MB 的可用空间</li><li> 必须专用于单个群集而不用于存储用户或应用程序数据</li><li> 必须对群集名称的计算机对象启用写入权限</li></ul><br>以下是有关托管文件共享见证的文件服务器的其他注意事项：<ul><li>可以使用多个群集的文件共享见证配置单个文件服务器。</li><li> 文件服务器必须位于与群集工作负载分开的站点上。 如果站点到站点网络通信丢失，则允许向任何群集站点提供均等的生存机会。 如果文件服务器位于同一站点上，则该站点成为主站点，并且它是可以访问文件共享的唯一站点。</li><li> 如果虚拟机未托管在使用文件共享见证的同一群集上，则文件服务器可以在虚拟机上运行。</li><li> 为了获得高可用性，可在单独的故障转移群集上配置文件服务器。 </li>      |
| 云见证     |  <ul><li>存储在 Azure blob 存储中的见证文件</li><li> 当群集中的所有服务器都具有可靠的 Internet 连接时，建议使用此设置。</li>      |  请参阅[部署云见证](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。       |

### <a name="node-vote-assignment"></a>节点投票分配

作为高级仲裁配置选项，你可以选择根据每个节点分配或删除仲裁投票。 默认情况下，所有节点都分配了投票。 无论投票分配的情况如何，所有节点都可继续在群集中正常工作、接收群集数据库更新，并且可以托管应用程序。

你可能希望从某些灾难恢复配置中的节点删除投票。 例如，在多站点群集中，你可以从备份站点中的节点删除投票，以便这些节点不会影响仲裁计算。 对于跨站点的手动故障转移，仅建议使用此配置。 有关详细信息，请参阅本主题后面的[灾难恢复配置的仲裁注意事项](#quorum-considerations-for-disaster-recovery-configurations)。

可以通过使用[Start-clusternode](https://technet.microsoft.com/library/hh847268.aspx)Windows PowerShell cmdlet 查找群集节点的**NodeWeight** common 属性来验证节点的已配置投票。 值为 0 指示该节点没有配置仲裁投票。 值为 1 指示已分配该节点的仲裁投票，并且它由该群集管理。 有关节点投票管理的详细信息，请参阅本主题后面的 [动态仲裁管理](#dynamic-quorum-management) 。

通过使用“验证群集仲裁”验证测试，可以验证所有群集节点的投票分配。

#### <a name="additional-considerations-for-node-vote-assignment"></a>节点投票分配的其他注意事项

  - 不建议用节点投票分配来强制执行奇数个投票节点。 相反，你应该配置磁盘见证或文件共享见证。 有关详细信息，请参阅本主题后面的[见证配置](#witness-configuration)。
  - 如果启用动态仲裁管理，则只有配置为分配了节点投票的节点才可以动态分配或删除其投票。 有关详细信息，请参阅本主题后面的[动态仲裁管理](#dynamic-quorum-management)。

### <a name="dynamic-quorum-management"></a>动态仲裁管理

在 Windows Server 2012 中，作为高级仲裁配置选项，你可以选择启用群集的动态仲裁管理。 有关动态仲裁工作原理的更多详细信息，请参阅[此说明](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior)。

借助动态仲裁管理，群集还有可能在最后一个未出现故障的群集节点上运行。 通过动态调整多数仲裁要求，该群集可以针对单个节点维持相继节点关闭。

通过使用[Start-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) Windows PowerShell cmdlet，可以通过群集节点的**DynamicWeight** common 属性验证节点的群集分配的动态投票。 “0”值表示该节点没有仲裁投票。 值为 1 指示该节点具有仲裁投票。

通过使用“验证群集仲裁”验证测试，可以验证所有群集节点的投票分配。

#### <a name="additional-considerations-for-dynamic-quorum-management"></a>动态仲裁管理的其他注意事项

- 动态仲裁管理不允许群集维持大多数投票成员同时出现故障。 若要继续运行，在节点关闭或发生故障时，该群集必须始终具有多数仲裁。

- 如果你已经明确地删除节点的投票，则群集无法动态添加或删除该投票。
- 启用存储空间直通时，群集只能支持两个节点故障。 "[池仲裁" 部分](../storage/storage-spaces/understand-quorum.md)中对此进行了详细介绍

## <a name="general-recommendations-for-quorum-configuration"></a>仲裁配置的常规建议

群集软件基于配置的节点数和共享存储的可用性自动为新群集配置仲裁。 通常这是最适合该群集的仲裁配置。 但是，在将群集投入生产之前，最好是在创建该群集后查看仲裁配置。 若要查看详细的群集仲裁配置，你可以使用验证配置向导或[测试群集](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)Windows PowerShell cmdlet 来运行 "**验证仲裁配置**" 测试。 在故障转移群集管理器中，基本仲裁配置显示在所选群集的摘要信息中，或者你可以查看有关运行[Set-clusterquorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) Windows PowerShell 时返回的仲裁资源的信息。cmdlet.

你可以随时运行“验证仲裁配置”测试，以验证该仲裁配置是否最适合你的群集。 测试输出指示是否建议对仲裁配置进行更改以及最适合的设置。 如果建议进行更改，你可以使用配置群集仲裁向导来应用建议的设置。

群集投入生产后，不更改仲裁配置，除非你已确定该更改适合你的群集。 在以下情况下，你可能想要考虑更改仲裁配置：

- 添加或逐出节点
- 添加或删除存储
- 长期节点或见证失败
- 在多站点灾难恢复方案中恢复群集

有关验证故障转移群集的详细信息，请参阅[验证故障转移群集的硬件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

## <a name="configure-the-cluster-quorum"></a>配置群集仲裁

你可以通过使用故障转移群集管理器或故障转移群集 Windows PowerShell cmdlet 来配置群集仲裁设置。

> [!IMPORTANT]
> 通常，最好使用由配置群集仲裁向导推荐的仲裁配置。 仅在你确定该更改适合你的群集时，我们才建议自定义仲裁配置。 有关详细信息，请参阅本主题中的 [仲裁配置的常规建议](#general-recommendations-for-quorum-configuration) 。

### <a name="configure-the-cluster-quorum-settings"></a>配置群集仲裁设置

每个群集服务器上的本地“Administrators”组的成员身份或同等身份是完成此过程所需的最低权限。 此外，你使用的帐户必须为域用户帐户。

> [!NOTE]
> 你可以更改群集仲裁配置，而无需停止该群集或使群集资源脱机。

### <a name="change-the-quorum-configuration-in-a-failover-cluster-by-using-failover-cluster-manager"></a>使用故障转移群集管理器更改故障转移群集中的仲裁配置

1. 在故障转移群集管理器中，选择或指定你想要更改的群集。
2. 选择群集后，在 "**操作**" 下，选择 "**更多操作**"，然后选择 "**配置群集仲裁设置**"。 将出现配置群集仲裁向导。 选择“**下一步**”。
3. 在“选择仲裁配置选项” 页面上，选择三个配置选项之一，并且完成该选项的步骤。 配置仲裁设置之前，你可以查看你的选择。 有关选项的详细信息，请参阅本主题前面的[了解仲裁](#understanding-quorum)。

    - 若要允许群集自动重置最适合当前群集配置的仲裁设置，请选择 "**使用典型设置**"，然后完成该向导。
    - 若要添加或更改仲裁见证，请选择 **"添加或更改仲裁见证**"，然后完成以下步骤。 有关配置仲裁见证的信息和注意事项，请参阅本主题前面的[见证配置](#witness-configuration)。

      1. 在“选择仲裁见证” 页面上，选中一个选项以配置磁盘见证或文件共享见证。 该向导指示建议你的群集使用的见证选择选项。

          > [!NOTE]
          > 你还可以选择“不配置仲裁见证”，然后完成该向导。 如果你在群集中的投票节点为偶数，则这可能不是推荐的配置。

      2. 如果你选择该选项来配置磁盘见证，则在“配置存储见证” 页面上，选择你想要分配为磁盘见证的存储卷，然后完成该向导。
      3. 如果你选择该选项来配置文件共享见证，则在“配置文件共享见证”页面上，键入或浏览到将用作见证资源的文件共享，然后完成该向导。

    - 若要配置仲裁管理设置和添加或更改仲裁见证，请选择 "**高级仲裁配置和见证选择**"，然后完成以下步骤。 有关高级仲裁配置设置的信息和注意事项，请参阅本主题前面的[节点投票分配](#node-vote-assignment)和[动态仲裁管理](#dynamic-quorum-management)。

      1. 在“选择投票配置”页面上，选择一个选项以将投票权分配给节点。 默认情况下，所有节点都分配了投票。 但是，对于某些方案，你可以仅将投票分配给节点的子集。

          > [!NOTE]
          > 你还可以选择“无节点”。 通常不建议使用此选项，因为它不允许节点参与仲裁投票，并且它需要配置见证磁盘。 此磁盘见证将成为群集的单点故障。

      2. 在“配置仲裁管理”页面上，可以启用或禁用“允许群集动态管理节点投票的分配情况”选项。 选择此选项通常会增加群集的可用性。 默认情况下，该选项处于启用状态，并且强烈建议不要禁用此选项。 此选项允许群集在出现故障的情况（在此选项处于禁用状态时不会出现此情况）下继续运行。
      3. 在“选择仲裁见证” 页面上，选中一个选项以配置磁盘见证或文件共享见证。 该向导指示建议你的群集使用的见证选择选项。

          > [!NOTE]
          > 你还可以选择“不配置仲裁见证”，然后完成该向导。 如果你在群集中的投票节点为偶数，则这可能不是推荐的配置。

      4. 如果你选择该选项来配置磁盘见证，则在“配置存储见证” 页面上，选择你想要分配为磁盘见证的存储卷，然后完成该向导。
      5. 如果你选择该选项来配置文件共享见证，则在“配置文件共享见证”页面上，键入或浏览到将用作见证资源的文件共享，然后完成该向导。

4. 选择“**下一步**”。 在出现的确认页面上确认你的选择，然后选择 "**下一步**"。

运行向导并出现 "**摘要**" 页面后，如果要查看向导执行的任务的报告，请选择 "**查看报告**"。 最新的报告将保留在<em>systemroot</em> **\\Cluster @ no__t-3Reports**文件夹中，名称为**为 quorumconfiguration.mht**。

> [!NOTE]
> 配置群集仲裁之后，我们建议你运行“验证仲裁配置” 测试以验证更新的仲裁设置。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 等效命令

下面的示例演示如何使用[set-clusterquorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) cmdlet 和其他 Windows PowerShell cmdlet 来配置群集仲裁。

以下示例将群集 *CONTOSO-FC1* 上的仲裁配置更改为没有仲裁见证的简单多数节点配置。

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

以下示例将本地群集上的仲裁配置更改为具有见证配置的多数节点。 将名为 *Cluster Disk 2* 的磁盘资源配置为见证磁盘。

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

以下示例将本地群集上的仲裁配置更改为具有见证配置的多数节点。 名为 *\\ @ no__t-2CONTOSO-FS @ no__t-3fsw*的文件共享资源配置为文件共享见证。

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

以下示例从本地群集上的节点 *ContosoFCNode1* 中删除仲裁投票。

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

以下示例将仲裁投票添加到本地群集上的节点 *ContosoFCNode1*。

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

以下示例启用群集 **CONTOSO-FC1** 的 *DynamicQuorum* 属性（如果该属性之前处于禁用状态）：

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## <a name="recover-a-cluster-by-starting-without-quorum"></a>在没有仲裁的情况下通过启动恢复群集

将不会启动没有足够仲裁投票的群集。 作为第一步，你应该始终确认群集仲裁配置，并且调查为什么该群集不再具有仲裁。 如果有已停止响应的节点，或者如果在多站点群集中无法访问主站点，则可能发生此情况。 确定了群集失败的根本原因后，你可以使用本部分中所述的恢复步骤。

> [!NOTE]
> * 如果因为丢失仲裁而停止群集服务，则事件 ID 1177 将出现在系统日志中。
> * 始终有必要调查丢失群集仲裁的原因。
> * 最好始终使节点或仲裁见证进入正常运行状态（加入群集），而不是在没有仲裁的情况下启动该群集。

### <a name="force-start-cluster-nodes"></a>强制启动群集节点

在你确定不能通过使节点或仲裁见证进入正常运行状态恢复群集之后，有必要强制启动该群集。 强制启动群集将覆盖群集仲裁配置设置，并且在“ForceQuorum”模式下启动群集。

当群集不具有仲裁时，在多站点群集中强制启动该群集可能特别有用。 请考虑具有包含单独的本地主要和备份站点 *SiteA* 和 *SiteB* 的群集的灾难恢复方案。 如果在 *SiteA* 中有真正的灾难，则该站点可能需要大量的时间才能恢复到联机状态。 你可能想要强制 *SiteB* 恢复联机，即使它不具有仲裁也是如此。

当在“ForceQuorum”模式下启动群集时，以及在它重新获得足够的仲裁投票后，该群集将自动退出强制状态，并且正常工作。 因此，通常没必要再次启动群集。 如果群集失去一个节点并且失去仲裁，则因为它不再处于强制状态，所以它将再次脱机。 如果它没有仲裁，则需要强制群集在没有仲裁的情况下启动。

> [!IMPORTANT]
> * 强制启动群集后，管理员可完全控制该群集。
> * 群集使用节点上的群集配置，在该节点上强制启动该群集，并将其复制到可用的所有其他节点。
> * 如果你在没有仲裁的情况下强制启动群集，则将忽略所有仲裁配置设置，同时将该群集保留在“ForceQuorum”模式下。 这包括特定节点投票分配和动态仲裁管理设置。

### <a name="prevent-quorum-on-remaining-cluster-nodes"></a>阻止剩余群集节点上的仲裁

在节点上强制启动该群集后，必须使用要阻止仲裁的某个设置启动群集中的所有剩余节点。 使用阻止仲裁的设置启动的节点指示群集服务加入现有运行群集（而非形成新的群集实例）。 这阻止剩余节点形成包含两个竞争实例的拆分群集。

在你强制启动备份站点 *SiteB*上的群集后，当你需要恢复某些多站点灾难恢复方案中的群集时，有必要这样做。 若要将强制启动的群集加入 *SiteB* 中，需要使用阻止的仲裁启动主站点 *SiteA* 中的节点。

> [!IMPORTANT]
> 在节点上强制启动群集后，我们建议你始终使用阻止的仲裁启动剩余节点。

下面介绍了如何通过故障转移群集管理器恢复群集：

1. 在故障转移群集管理器中，选择或指定你想要恢复的群集。
2. 选择群集后，在 "**操作**" 下选择 "**强制群集启动**"。

    故障转移群集管理器在可访问的所有节点上强制启动群集。 启动时，该群集使用当前的群集配置。

> [!NOTE]
> * 若要在包含要使用的群集配置的特定节点上强制启动群集，必须使用 Windows PowerShell cmdlet 或等效的命令行工具，如此过程后面所示。 
> * 如果你使用故障转移群集管理器连接到强制启动的群集，并且使用“启动群集服务” 操作启动节点，则使用阻止仲裁的设置自动启动该节点。

#### <a name="windows-powershell-equivalent-commands-start-clusternode"></a>Windows PowerShell 等效命令（Start-clusternode）

以下示例显示了如何使用 **Start-ClusterNode** cmdlet 在节点 *ContosoFCNode1*上强制启动群集。

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

或者，你可以在该节点上本地键入以下命令：

```PowerShell
Net Start ClusSvc /FQ
```

以下示例显示了如何使用 **Start-ClusterNode** cmdlet 启动群集服务，该服务具有在节点 *ContosoFCNode1*上阻止的仲裁。

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

或者，你可以在该节点上本地键入以下命令：

```PowerShell
Net Start ClusSvc /PQ
```

## <a name="quorum-considerations-for-disaster-recovery-configurations"></a>灾难恢复配置的仲裁注意事项

本部分概括了灾难恢复部署中两个多站点群集配置的特征和仲裁配置。 仲裁配置指南有所不同，具体取决于针对站点之间的工作负载，是需要自动故障转移还是手动故障转移。 通常，你的配置取决于存在于你的组织中的服务级别协议 (SLA)，用于在站点上发生故障或恢复时提供并支持群集工作负载。

### <a name="automatic-failover"></a>自动故障转移

在此配置中，群集包含可以托管群集角色的两个或多个站点。 如果在任何站点上发生故障，则群集角色预期会自动故障转移到剩余站点。 因此，必须配置群集仲裁，以便任何站点都可以维持完整的站点故障。

下表总结了有关此配置的注意事项和建议。

| 项目  | 描述  |
| ---------| ---------|
| 每个站点的节点投票数     | 应该相等       |
| 节点投票分配     |  因为所有节点都同等重要，所以不应删除节点投票       |
| 动态仲裁管理     |   应该启用      |
| 见证配置     |  建议使用文件共享见证，该见证配置在从群集站点分开的站点中       |
| 工作负载     |  可以在任何站点上配置工作负载       |

#### <a name="additional-considerations-for-automatic-failover"></a>自动故障转移的其他注意事项

- 有必要在单独的站点中配置文件共享见证，以给每个站点均等的生存机会。 有关详细信息，请参阅本主题前面的[见证配置](#witness-configuration)。

### <a name="manual-failover"></a>手动故障转移

在此配置中，群集包含主站点 *SiteA*和备份（恢复）站点 *SiteB*。 群集角色托管在 *SiteA*上。 由于群集仲裁配置，如果在 *SiteA* 中的所有节点上出现故障，则群集停止运行。 在此情况下，管理员必须手动将群集服务故障转移到 *SiteB* ，并且执行附加步骤来恢复该群集。

下表总结了有关此配置的注意事项和建议。

| 项目   |描述  |
| ---------| ---------|
| 每个站点的节点投票数     |  <ul><li> 不应从主站点 **SiteA** 的节点中删除节点投票</li><li>应从备份站点 **SiteB** 的节点中删除节点投票</li><li>如果长期中断发生在 **SiteA** 上，必须将投票分配给 **SiteB** 上的节点，以便在该站点上启用多数仲裁作为恢复的一部分</li>       |
| 动态仲裁管理     |  应该启用       |
| 见证配置     |  <ul><li>如果在 **SiteA** 上有偶数个节点，则配置一个见证</li><li>如果需要见证，则配置仅在 **SiteA** 中的节点可以访问的文件共享见证或磁盘见证（有时称为非对称磁盘见证）</li>       |
| 工作负载     |  使用首选的所有者以保持工作负载在 **SiteA** 的节点上运行       |

#### <a name="additional-considerations-for-manual-failover"></a>手动故障转移的其他注意事项

- 只有 *SiteA* 上的节点最初使用仲裁投票进行配置。 必需确保 *SiteB* 上的节点的状态不会影响群集仲裁。
- 恢复步骤可能有所不同，具体取决于 *SiteA* 维持的是临时故障，还是长期故障。

## <a name="more-information"></a>详细信息

* [故障转移群集](failover-clustering.md)
* [故障转移群集 Windows PowerShell cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [了解群集和池仲裁](../storage/storage-spaces/understand-quorum.md)