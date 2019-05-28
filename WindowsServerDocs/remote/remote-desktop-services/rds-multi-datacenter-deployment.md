---
title: 在 Azure 中的异地冗余 RDS 数据中心
description: 了解如何创建使用多个数据中心提供高可用性跨地理位置的 RDS 部署。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61c36528-cf47-4af0-83c1-a883f79a73a5
author: haley-rowland
ms.author: elizapo
ms.date: 06/14/2017
manager: dongill
ms.openlocfilehash: 2d12062f302c28a8124e0aa49af7f441e77ffe33
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222795"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>创建异地冗余的多个数据中心进行灾难恢复的 RDS 部署

>适用于：Windows Server （半年频道），Windows Server 2019，Windows Server 2016

通过利用 Azure 中的多个数据中心，可以为远程桌面服务部署启用灾难恢复。 与标准的高可用性 RDS 部署不同 (如中所述[远程桌面服务体系结构](desktop-hosting-logical-architecture.md))，它在单个 Azure 区域 （例如，欧洲西部） 中使用的数据中心，多个数据中心部署使用数据在多个地理位置，从而提高部署的一个 Azure 数据中心的可用性中心可能不可用，但不太可能多个区域会停机一次。 通过部署异地冗余的 RDS 体系结构，您可以在整个区域发生灾难性故障的故障转移。

可以使用下面的说明向多个租户通过利用 Microsoft Azure 基础结构服务和 RDS 提供托管服务和订阅者访问许可证 (Sal) 的异地冗余桌面[Microsoft 服务提供程序许可协议 (SPLA) 计划](https://www.microsoft.com/hosting/licensing/splabenefits.aspx)。 此外可以使用以下步骤来创建你自己的员工使用的异地冗余的托管服务[RDS 用户 Cal 扩展通过软件保障实现的权限](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf)。

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>逻辑体系结构以实现高可用性的单个和多个区域
下图显示在单个 Azure 区域的高度可用的部署的体系结构：

![在单个 Azure 区域中高度可用的部署](media/rds-ha-single-region.png)

部署由三个层组成：

- Azure 服务-Azure 管理界面，包括 Azure 门户和 Api，以及公共网络服务，例如 DNS 和公共 IP 地址。
- 桌面托管服务-虚拟机、 网络、 存储、 Azure 服务和 Windows Server 角色服务
- Azure 结构-Windows Server 操作系统运行的 HYPER-V 角色，用于虚拟化物理服务器、 存储单元、 网络交换机和路由器。 使用 Azure Fabric，可以创建 Vm、 网络、 存储和应用程序从底层硬件独立。


相比之下，下面是使用多个 Azure 数据中心的部署的体系结构：

![使用多个 Azure 区域的 RDS 部署](media/rds-ha-multi-region.png)

若要创建异地冗余部署的第二个 Azure 区域中复制整个 RDS 部署。 此体系结构使用主动-被动模型，其中只有一个 RDS 部署正在运行一次。 VNet 到 VNet 连接允许与彼此通信的两种环境。 RDS 部署基于单个 Active Directory 林/域，并在两个部署之间复制 AD 服务器、 含义用户可以登录到两个部署使用相同的凭据。 双节点群集存储空间直通的横向扩展文件服务器 (SOFS) 上存储用户设置和数据存储在用户配置文件磁盘 (UPD)。 第二个相同的存储空间直通群集部署在第二个 （被动） 区域，并使用存储副本将从活动的用户配置文件复制到被动部署。 Azure 流量管理器用于将自动定向到任何部署的最终用户当前处于活动状态-从最终用户角度来看，它们访问部署使用一个 URL，并不知道最终用掉的哪个区域。


您*无法*在每个区域中创建的非高度可用的 RDS 部署，但如果即使单个 VM 重新启动，在一个区域中，会发生故障转移，从而增加的故障转移发生的可能性关联的性能影响。

## <a name="deployment-steps"></a>部署步骤
在 Azure 创建的异地冗余的多个数据中心 RDS 部署中创建以下资源：

1. 两个资源组中两个单独的 Azure 区域。 例如 RG A （活动部署，RG 代表"资源组"） 和 RG B （被动部署）。
2. RG A 中高度可用的 Active Directory 部署可以使用[新的 AD 域使用 2 个域控制器模板](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/)创建部署。
3. 在 RG A.使用高度可用的 RDS 部署[RDS 服务器场部署使用现有的 active directory](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)模板以创建基本的 RDS 部署中，然后按照中的信息[远程桌面服务-高可用性](rds-plan-high-availability.md)配置以实现高可用性的其他 RDS 组件。
4. 请务必使用不重叠 RG A 中的部署一个地址空间 RG B-中的 VNet
5. 一个[VNet 到 VNet 连接](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)之间的两个资源组。
6. 在 RG B-中的可用性集的两个 AD 虚拟机，请确保 VM 名称都是不同于两个 Windows Server 2016 Vm 中为单个可用性集、 安装 Active Directory 域服务角色，然后将它们升级至域 cont RG A.部署中的 AD 虚拟机在步骤 1 中创建的域中刷。
7. RG B.中的第二个高可用性 RDS 部署 
   1. 使用[RDS 服务器场部署使用现有的 active directory](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)模板，但这次进行以下更改。 (若要自定义模板，请选择库中，单击**部署到 Azure** ，然后**编辑模板**。)
      1. 调整 DNS 服务器的专用 IP 以对应 RG B.中的 VNet 的地址空间 
      
         搜索"dnsServerPrivateIp"在变量中。 编辑对应于 RG B.中的 VNet 中定义的地址空间的默认 IP (10.0.0.4)
   
      2. 编辑计算机名称，以便它们不会与 RG A 中部署中的冲突
      
         查找中的虚拟机**资源**模板部分。 更改**computerName**字段下**osProfile**。 例如，"网关"可能会成为"网关 **-b**";"[concat ('rdsh-'，copyIndex())]"可能会变得"[concat （'rdsh-b-、 copyIndex())]"、 和"中转站"可能会变得"broker **-b**"。
      
         （您还可以更改 Vm 的名称手动运行模板后。）
   2. 如步骤 3 更高版本中，使用中的信息[远程桌面服务的高可用性](rds-plan-high-availability.md)配置以实现高可用性的其他 RDS 组件。
8. 存储空间直通横向扩展文件服务器存储副本的两种部署上。 使用[PowerShell 脚本](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts)部署[模板](https://github.com/robotechredmond/301-s2d-sr-dr-md)跨资源组。

   > [!NOTE]
   > 可以预配存储手动 （而不是使用 PowerShell 脚本和模板）： 
   >1. 部署[双节点存储空间直通 SOFS](rds-storage-spaces-direct-deployment.md) RG A 来存储你的用户配置文件磁盘 (Upd) 中。
   >2. 将第二个、 相同存储空间直通 SOFS RG B 中进行部署-请确保在每个群集中使用相同的存储量。
   >3. 设置[与异步复制的存储副本](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)两者之间。

### <a name="enable-upds"></a>启用 Upd
存储副本 （辅助/被动部署相关联） 的目标卷 （与主/主动部署相关联） 的源卷从复制数据。 根据设计，显示为目标群集**联机 （无访问权限）** -存储副本卸除目标卷及其驱动器号或者装入点。 这意味着，通过提供文件共享路径为辅助部署启用 Upd 将失败，因为未装入的卷。 

想要深入了解如何管理复制？ 请查看[群集到群集存储复制](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

若要启用对这两个部署的 Upd，请执行以下操作：

1. 运行[集 RDSessionCollectionConfiguration cmdlet](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration)若要启用主 （活动） 部署的用户配置文件磁盘提供文件共享的路径 （它在步骤 7 中部署的步骤中创建） 在源卷上。
2. 反转存储副本方向，以便将目标卷将成为源卷 （这将卷装载，并使其可访问的辅助部署）。 你可以运行**Set-srpartnership** cmdlet 来执行此操作。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. 启用辅助 （被动） 部署中的用户配置文件磁盘。 使用与你对主部署，在步骤 1 中相同的步骤。
4. 反转的存储副本方向，因此原始的源卷再次是 SR 合作关系中的源卷可以访问文件共享，主要的部署。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Azure 流量管理器 

创建[Azure 流量管理器](/azure/traffic-manager/traffic-manager-overview)配置文件，并请务必选择**优先级**路由方法。 将两个终结点设置为每个部署的公共 IP 地址。 下**配置**，将协议更改为 HTTPS （而不是 HTTP) 和端口为 443 （而不是 80)。 请注意**DNS 生存时间**，并将其设置正确，您的故障转移需求。 

请注意，流量管理器需要终结点，以返回 200 OK 响应 GET 请求，以便将其标记为"正常运行。" RDS 的模板创建的 publicIP 对象将起作用，但不是添加路径附录。 相反，您可以最终向用户提供与流量管理器 URL"/ RDWeb"追加，例如： ```http://deployment.trafficmanager.net/RDWeb```

通过将 Azure 流量管理器部署与优先级路由方法，则阻止最终用户访问被动部署功能的活动部署时。 如果最终用户访问被动部署和存储副本方向尚未已切换为故障转移时，用户登录方式挂起如部署尝试，并且无法进行访问被动的存储空间直通群集的最终部署上的文件共享将放弃并为用户提供临时配置文件。  

### <a name="deallocate-vms-to-save-resources"></a>解除分配 Vm 以节省资源 
配置这两个部署后，你可以根据需要关闭并解除分配辅助 RDS 基础结构和 RDSH 虚拟机以在这些虚拟机上节省成本。 存储空间直通 SOFS 和 AD 服务器 Vm 必须始终保持在要启用用户帐户和配置文件同步的辅助/被动部署中运行。  

故障转移时，你将需要启动已解除分配的 Vm。 此部署配置的优点在于，成本更低，但代价是故障转移时间。 如果在活动部署中发生灾难性故障，您必须手动启动被动部署，或你将需要的自动化脚本来检测故障并自动启动被动部署。 在任一情况下，可能需要几分钟才能收到被动部署运行并且可供用户进行登录，从而导致服务中断一段时间。 此停机时间取决于时间量它所需开始 RDS 基础结构和 RDSH 虚拟机 （通常 2-4 分钟，如果 Vm 的启动并行而不是按顺序），以及若要将被动群集联机 （具体取决于群集的大小的时间通常 2 到 4 分钟，每个节点的 2 个磁盘的 2 节点群集)。 

### <a name="active-directory"></a>Active Directory 
每个部署中的 Active Directory 服务器都在同一个林/域的副本。 Active Directory 具有内置的同步协议，以使四个域控制器保持同步。但是，可能有一些延迟，以便如果新用户添加到一台 AD 服务器，可能需要一些时间才能在两个部署中的所有 AD 服务器之间复制。 因此，请确保用户尝试添加到域后立即登录，则发出警告。 

### <a name="rd-license-server"></a>远程桌面许可证服务器 
提供[每个用户 CAL 的 RD](rds-client-access-license.md)为有权访问异地冗余部署每个命名用户。 分发每个用户在活动部署中的两个远程桌面许可证服务器之间均匀分配 Cal。 然后，将复制到被动部署中的两个远程桌面许可证服务器这些 Cal。 Cal 主动和被动部署之间复制的因为在任何给定时间只有一个部署可以处于活动状态的用户;否则，在违反许可协议。  

### <a name="image-management"></a>映像管理 
当更新 RDSH 映像提供软件更新或新的应用程序时，将需要单独更新的 RDSH 集合中每个部署保留这两个部署间的常见的用户体验。 可以使用[更新 RDSH 集合模板](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)，但请注意，被动部署 RDS 基础结构和 RDSH 虚拟机必须运行才能运行该模板。 

## <a name="failover"></a>故障转移

在主动-被动部署的情况下故障转移需要您要启动的辅助部署 Vm。 可以手动或使用自动化脚本执行此操作。 在存储空间直通 SOFS 的灾难性故障转移的情况下更改存储副本合作关系方向，使目标卷将成为源卷。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

您可以了解详细信息[群集到群集存储复制](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

Azure 流量管理器会自动识别主部署失败，并且辅助部署正常运行 （在 RD 网关 Vm 已启动 RG B 中） 和用户将流量定向到辅助部署。 用户可以使用相同的流量管理器 URL 以继续处理其远程资源一致的体验感到满意。 请注意，客户端 DNS 缓存将不会更新该记录的 Azure 流量管理器配置中设置的 TTL 持续时间。

### <a name="test-failover"></a>测试故障转移
存储副本合作关系中只有一个卷 （源） 可以处于活动状态一次。 这意味着在切换 SR 合作关系方向，在主部署 (RG A) 中的卷将成为复制目标，因此隐藏。 因此，连接到 RG 的任何用户将不再有权访问存储在 RG A 中的 SOFS 上其 Upd 

若要同时允许用户继续中的日志记录的测试故障转移：
1. 在 RG B.中启动的基础结构 Vm 和 RDSH 虚拟机
2. 切换 SR 合作关系方向 （群集 b s2d c 将成为源卷）。
3. [禁用的终结点](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint)RG a Azure 流量管理器配置文件中以强制将流量定向到或者 RG B.ATM、 使用 PowerShell 脚本：

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B 现在是活动的主要部署。 若要切换回 RG A 作为主部署：

1. 切换 SR 合作关系方向 （群集的 s2d c 将成为源卷）：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. 重新启用 RG 的 Azure 流量管理器配置文件中的终结点：

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>有关在本地部署的注意事项

虽然在本地部署无法使用本文中引用 Azure 快速入门模板，您可以手动实现基础结构的所有角色。 在本地部署中，成本不由 Azure 使用情况，请考虑使用更快故障转移的主动-主动模型。

可以使用 Azure 流量管理器使用的本地终结点，但需要 Azure 订阅。 或者，对于向最终用户提供的 DNS，给予它们一个 CNAME 记录，只需将用户定向到主部署。 在故障转移的情况下修改重定向到辅助部署的 DNS CNAME 记录。 这种方式，最终用户使用一个 URL，就像使用 Azure 流量管理器，将用户定向到适当的部署的。 

如果想创建一个在内部部署-到-Azure-站点模型，请考虑使用[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)。
