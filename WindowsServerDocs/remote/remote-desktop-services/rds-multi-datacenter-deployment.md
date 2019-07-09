---
title: Azure 中的异地冗余 RDS 数据中心
description: 了解如何创建使用多个数据中心的 RDS 部署，以跨多个地理位置提供高可用性。
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66222795"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>为灾难恢复创建异地冗余、多数据中心的 RDS 部署

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

可以利用 Azure 中的多个数据中心为远程桌面服务部署启用灾难恢复。 与使用单个 Azure 区域（例如，西欧）中的数据中心的标准高可用 RDS 部署（如[远程桌面服务体系结构](desktop-hosting-logical-architecture.md)中所述）不同，多数据中心部署使用多个地理位置中的数据中心，从而提高了部署的可用性 - 一个 Azure 数据中心可能不可用，但多个区域不太可能同时关闭。 通过部署异地冗余 RDS 体系结构，可以在整个区域发生灾难性故障时启用故障转移。

可以按照以下说明，利用 Microsoft Azure 基础结构服务和 RDS 通过 [Microsoft 服务提供商许可协议 (SPLA) 计划](https://www.microsoft.com/hosting/licensing/splabenefits.aspx)向多个租户交付异地冗余桌面托管服务和订阅者访问许可证 (SAL)。 还可以按照以下步骤，使用[通过软件保障提供的 RDS 用户 CAL 扩展权限](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf)为你自己的员工创建异地冗余托管服务。

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>可实现高可用性的逻辑体系结构 - 单个和多个区域
下图显示了单个 Azure 区域中高可用部署的体系结构：

![单个 Azure 区域中的高可用部署](media/rds-ha-single-region.png)

部署由三层组成：

- Azure 服务 - Azure 管理界面，包括 Azure 门户和 API，以及公共网络服务，如 DNS 和公共 IP 寻址。
- 桌面托管服务 - 虚拟机、网络、存储、Azure 服务和 Windows Server 角色服务
- Azure Fabric - 运行 Hyper-V 角色的 Windows Server 操作系统，用于虚拟化物理服务器、存储单元、网络交换机和路由器。 借助 Azure Fabric，可以独立于底层硬件创建 VM、网络、存储和应用程序。


下面是与之形成对比的使用多个 Azure 数据中心的部署的体系结构：

![使用多个 Azure 区域的 RDS 部署](media/rds-ha-multi-region.png)

整个 RDS 部署在第二个 Azure 区域中复制，以创建异地冗余部署。 此体系结构使用主动-被动模型，一次只运行一个 RDS 部署。 VNet 到 VNet 连接允许这两个环境相互通信。 RDS 部署基于单个 Active Directory 林/域，AD 服务器跨两个部署进行复制，这意味着用户可以使用相同的凭据登录任一部署。 用户配置文件磁盘 (UPD) 中存储的用户设置和数据存储在双节点群集存储空间直通横向扩展文件服务器 (SOFS) 中。 第二个相同的存储空间直通群集部署在第二个（被动）区域中，存储副本用于将用户配置文件从主动部署复制到被动部署中。 Azure 流量管理器用于自动将最终用户定向到当前处于活动状态的任一部署 - 从最终用户的角度来看，他们使用单个 URL 访问部署，并且不知道最终使用哪个区域。


你*可以*在每个区域创建一个非高可用 RDS 部署，但即使在一个区域中重新启动单个 VM，也会发生故障转移，从而增加发生故障转移的可能性以及相关的性能影响。

## <a name="deployment-steps"></a>部署步骤
在 Azure 中创建以下资源，以创建异地冗余多数据中心 RDS 部署：

1. 两个不同 Azure 区域中的两个资源组。 例如 RG A（主动部署，RG 代表“资源组”）和 RG B（被动部署）。
2. RG A 中的高可用 Active Directory 部署。可以使用[“使用 2 个域控制器新建 AD 域”模板](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/)来创建部署。
3. RG A 中的高可用 RDS 部署。使用[利用现有 Active Directory 的 RDS 场部署](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)模板创建基本 RDS 部署，然后按照[远程桌面服务 - 高可用性](rds-plan-high-availability.md)中的信息配置其他 RDS 组件，以实现高可用性。
4. RG B 中的 VNet - 确保使用的地址空间与 RG A 中的部署不重叠。
5. 两个资源组之间的 [VNet 到 VNet 连接](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)。
6. RG B 中的可用性集中的两个 AD 虚拟机 - 确保 VM 名称与 RG A 中的 AD VM 不同。在单个可用性集中部署两个 Windows Server 2016 VM，安装 Active Directory 域服务角色，然后将它们提升至步骤 1 中创建的域中的域控制器。
7. RG B 中的第二个高可用 RDS 部署。 
   1. 再次使用[利用现有 Active Directory 的 RDS 场部署](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)模板，但这次进行以下更改。 （若要自定义该模板，请在库中选择它，单击“部署到 Azure”，然后单击“编辑模板”   。）
      1. 调整 DNS 服务器专用 IP 的地址空间，以对应 RG B 中的 VNet。 
      
         在变量中搜索“dnsServerPrivateIp”。 编辑默认 IP (10.0.0.4)，以对应在 RG B 的 VNet 中定义的地址空间。
   
      2. 编辑计算机名称，使其不与 RG A 的部署中的计算机名称冲突。
      
         在模板的“资源”部分找到 VM  。 更改“osProfile”下的“computerName”字段   。 例如，“gateway”可以变为“gateway **-b**”；“[concat('rdsh-', copyIndex())]”可以变为“[concat('rdsh-b-', copyIndex())]”，“broker”可以变为“broker **-b**”。
      
         （也可以在运行模板后手动更改 VM 的名称。）
   2. 与上面的步骤 3 一样，使用[远程桌面服务 - 高可用性](rds-plan-high-availability.md)中的信息来配置其他 RDS 组件，以实现高可用性。
8. 具有跨两个部署的存储副本的存储空间直通横向扩展文件服务器。 使用 [PowerShell 脚本](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts)在各资源组中部署[模板](https://github.com/robotechredmond/301-s2d-sr-dr-md)。

   > [!NOTE]
   > 可以手动预配存储（而不是使用 PowerShell 脚本和模板）： 
   >1. 在 RG A 中部署[双节点存储空间直通 SOFS](rds-storage-spaces-direct-deployment.md) 以存储用户配置文件磁盘 (UPD)。
   >2. 在 RG B 中部署第二个相同的存储空间直通 SOFS - 确保在每个群集中使用相同的存储量。
   >3. 在两者之间设置[使用异步复制的存储副本](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

### <a name="enable-upds"></a>启用 UPD
存储副本将数据从源卷（与主/主动部署关联）复制到目标卷（与辅助/被动部署关联）。 根据设计，目标群集显示为“联机(不能访问)”- 存储副本会卸除目标卷及其驱动器号或装入点  。 这意味着通过提供文件共享路径为辅助部署启用 UPD 将失败，因为未装载卷。 

想深入了解如何管理复制？ 请查看[群集到群集存储复制](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

若要在两个部署上都启用 UPD，请执行以下操作：

1. 运行 [Set-RDSessionCollectionConfiguration cmdlet](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration) 为主（主动）部署启用用户配置文件磁盘 - 提供源卷（在部署步骤中的步骤 7 中创建）上文件共享的路径。
2. 反转存储副本方向，使目标卷变成源卷（这将装载卷并使其可供辅助部署访问）。 可以运行 **Set-SRPartnership** cmdlet 来执行此操作。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. 在辅助（被动）部署中启用用户配置文件磁盘。 使用与在步骤 1 中对主部署执行的相同步骤。
4. 再次反转存储副本方向，因此，原始源卷再次成为 SR 合作关系中的源卷，主部署可以访问文件共享。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Azure 流量管理器 

创建 [Azure 流量管理器](/azure/traffic-manager/traffic-manager-overview)配置文件，并确保选择“优先级”路由方法  。 将两个终结点设置为每个部署的公共 IP 地址。 在“配置”下，将协议更改为 HTTPS（而不是 HTTP），将端口更改为 443（而不是 80）  。 记下“DNS 生存时间”，并根据故障转移需求进行相应设置  。 

请注意，流量管理器要求终结点返回“200 正常”来响应 GET 请求，以便标记为“正常运行”。 根据 RDS 模板创建的 publicIP 对象将起作用，但不添加路径附录。 相反，你可以为最终用户提供追加了“/RDWeb”的流量管理器 URL，例如：```http://deployment.trafficmanager.net/RDWeb```

通过部署路由方法为“优先级”的 Azure 流量管理器，可以防止最终用户在主动部署正常运行时访问被动部署。 如果最终用户访问被动部署，而存储副本方向尚未切换为进行故障转移，则用户登录会在该部署尝试访问被动存储空间直通群集上的文件共享并且失败时挂起 - 最终，该部署会放弃访问，并为用户提供临时配置文件。  

### <a name="deallocate-vms-to-save-resources"></a>解除分配 VM 以节省资源 
配置完这两个部署后，可以选择关闭并解除分配辅助 RDS 基础结构和 RDSH VM，以节省这些 VM 的成本。 存储空间直通 SOFS 和 AD 服务器 VM 必须始终在辅助/被动部署中运行，才能实现用户帐户和配置文件同步。  

发生故障转移时，需要启动已解除分配的 VM。 这种部署配置具有成本较低的优点，但代价是故障转移时间较长。 如果主动部署中发生灾难性故障，则必须手动启动被动部署，或者需要一个自动化脚本来检测故障并自动启动被动部署。 在任一情况下，都需要几分钟时间才能使被动部署运行并且可供用户登录，从而会导致服务中断一段时间。 此中断时间取决于启动 RDS 基础结构和 RDSH VM 所需的时间（通常为 2-4 分钟，前提是 VM 并行启动而不是按顺序启动），以及将被动群集联机的时间（取决于群集的大小，每个节点 2 个磁盘的双节点群集通常要 2-4 分钟）。 

### <a name="active-directory"></a>Active Directory 
每个部署中的 Active Directory 服务器都是同一林/域中的副本。 Active Directory 具有内置的同步协议，可使四个域控制器保持同步。但是，可能存在一些延迟，因为如果向其中一个 AD 服务器添加新用户，可能需要一些时间跨两个部署中的所有 AD 服务器进行复制。 因此，请务必警告用户在加入域后不要立即尝试登录。 

### <a name="rd-license-server"></a>RD 许可证服务器 
为有权访问异地冗余部署的每个指定用户提供[每用户 RD CAL](rds-client-access-license.md)。 在主动部署中的两个 RD 许可证服务器之间均匀分配每用户 CAL。 然后，将这些 CAL 复制到被动部署中的两个 RD 许可证服务器。 由于 CAL 在主动部署和被动部署之间是重复的，因此在任何给定时间，都只能有一个部署处于可供用户连接的活动状态；否则就违反了许可协议。  

### <a name="image-management"></a>映像管理 
在更新 RDSH 映像以提供软件更新或新应用程序时，需要分别更新每个部署中的 RDSH 集合，以便在两个部署间保持共同的用户体验。 可以使用[“更新 RDSH 集合”模板](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)，但请注意，必须运行被动部署的 RDS 基础结构和 RDSH VM 才能运行该模板。 

## <a name="failover"></a>故障转移

对于主动-被动部署，故障转移要求启动辅助部署的 VM。 可以手动或使用自动化脚本执行此操作。 如果存储空间直通 SOFS 发生灾难性故障转移，请更改存储副本合作关系方向，使目标卷变成源卷。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

可以在[群集到群集存储复制](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)中了解详细信息。

Azure 流量管理器会自动识别出主部署失败且辅助部署正常运行（在 RG B 中，RD 网关中的 VM 已启动），并将用户流量定向到辅助部署。 用户可以使用相同的流量管理器 URL 继续处理其远程资源，从而享受一致的体验。 请注意，客户端 DNS 缓存不会更新在 Azure 流量管理器配置中设置的 TTL 持续时间的相应记录。

### <a name="test-failover"></a>测试故障转移
在存储副本合作关系中，一次只能有一个卷（源）处于活动状态。 这意味着，当切换 SR 合作关系方向时，主部署 (RG A) 中的卷将变成复制目标，因此将被隐藏。 因此，任何连接到 RG A 的用户都无法再访问存储在 RG A 中 SOFS 上的 UPD。 

若要在测试故障转移的同时允许用户继续登录，请执行以下操作：
1. 在 RG B 中启动基础结构 VM 和 RDSH VM。
2. 切换 SR 合作关系方向（cluster-b-s2d-c 变成源卷）。
3. 在 Azure 流量管理器配置文件中[禁用 RG A 的终结点](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint)，以强制 ATM 将流量定向到 RG B。或者，使用 PowerShell 脚本：

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B 现在是主动主部署。 若要切换回 RG A 作为主部署，请执行以下操作：

1. 切换 SR 合作关系方向（cluster-a-s2d-c 变成源卷）：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. 在 Azure 流量管理器配置文件中重新启用 RG A 的终结点：

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>本地部署注意事项

虽然本地部署无法使用本文中引用的 Azure 快速入门模板，但你可以手动实施所有基础结构角色。 在成本不受 Azure 消耗驱动的本地部署中，请考虑使用主动-主动模型，以加快故障转移。

可以将 Azure 流量管理器与本地终结点结合使用，但它需要 Azure 订阅。 或者，对于提供给最终用户的 DNS，为它们提供一条 CNAME 记录，该记录直接将用户定向到主部署。 进行故障转移时，可修改该 DNS CNAME 记录以重定向到辅助部署。 通过这种方式，最终用户只需使用一个 URL，就像使用 Azure 流量管理器一样将用户定向到合适的部署。 

如果对创建本地到 Azure 站点模型感兴趣，请考虑使用 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)。
