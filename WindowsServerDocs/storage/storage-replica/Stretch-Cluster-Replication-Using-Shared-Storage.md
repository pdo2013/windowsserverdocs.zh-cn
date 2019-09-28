---
title: 使用共享存储拉伸群集复制
ms.prod: windows-server
manager: eldenc
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 6c5b9431-ede3-4438-8cf5-a0091a8633b0
ms.openlocfilehash: 654b4aea135c360f5fc5f59fdf85627fe8dd4cc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402970"
---
# <a name="stretch-cluster-replication-using-shared-storage"></a>使用共享存储拉伸群集复制

>适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

在此评估示例中，将在单个拉伸群集中配置这些计算机及其存储，其中两个节点共享一组存储，两个节点共享另一组存储，然后复制保持两组存储在群集中进行镜像，以允许立即故障转移。 这些节点及其存储应位于单独的物理站点（尽管这不是必需的）。 将 Hyper-V 和文件服务器群集创建为示例方案具有单独的步骤。  

> [!IMPORTANT]  
> 在此评估中，不同站点中的服务器必须能够通过网络与其他服务器通信，但是不具有与其他站点的共享存储的物理连接。 此方案不会利用存储空间直通。  

## <a name="terms"></a>术语  
此演练使用以下环境作为示例：  

-   四台服务器（分别名为 **SR-SRV01**、**SR-SRV02**、**SR-SRV03** 和 **SR-SRV04**）形成名为 **SR-SRVCLUS** 的单个群集。  

-   表示两个不同数据中心的一对逻辑“站点”，一个名为 **Redmond**，而另一个名为 **Bellevue**。  

> [!NOTE]  
> 可以仅使用两个节点，每个站点中一个节点。 但是，仅使用两台服务器无法执行站点内故障转移。 最多可使用 64 个节点。

![图示显示 Redmond 的两个节点，这两个节点复制了 Bellevue 站点中同一个群集的两个节点](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_StretchClusterExample.png)  

@NO__T 0FIGURE 1：Stretch 群集中的存储复制 @ no__t-0  

## <a name="prerequisites"></a>先决条件  
-   Active Directory 域服务林（无需运行 Windows Server 2016）。  
-   2-64 运行 Windows Server 2019 或 Windows Server 2016 Datacenter Edition 的服务器。 如果你运行的是 Windows Server 2019，则可以改为使用标准版，如果你只是复制一个最大为 2 TB 的卷。 
-   两组共享存储，使用 SAS JBOD（例如与存储空间配合使用）、光纤通道 SAN、共享 VHDX 或 iSCSI 目标。 存储应包含 HDD 和 SSD 媒体的组合，且必须支持永久保留。 使每个存储集仅对两台服务器可用（非对称）。  
-   每个存储集必须允许至少创建两个虚拟磁盘，一个用于复制的数据，另一个用于日志。 物理存储在所有数据磁盘上的扇区大小必须相同。 物理存储在所有日志磁盘上的扇区大小必须相同。  
-   每个服务器上必须具有至少一个用于同步复制的 1GbE 连接，但最好是 RDMA。   
-   至少 2 GB 的 RAM 且每台服务器具有两个内核。 对于更多的虚拟机，需要更多的内存和内核。  
-   合适的防火墙和路由器规则，以允许所有节点之间的 ICMP、SMB（端口 445 以及用于 SMB 直通的 5445）和 WS-MAN（端口 5985）双向通信。  
-   服务器间的网络具有足够的带宽，以包含 IO 写入工作负载和平均值为 5 毫秒的往返行程延迟（对于同步复制）。 异步复制没有延迟建议。  
-   复制的存储不能位于包含 Windows 操作系统文件夹的驱动器上。

许多这些要求都可通过使用 `Test-SRTopology` cmdlet 来确定。 如果将存储副本或存储副本管理工具功能安装在至少一台服务器上，则会获取此工具的访问权限。 无需将存储副本配置为使用此工具，只需安装 cmdlet。 以下步骤中包含更多信息。  

## <a name="provision-operating-system-features-roles-storage-and-network"></a>设置操作系统、功能、角色、存储和网络  

1.  使用带有桌面体验的服务器核心或服务器安装选项，在所有服务器节点上安装 Windows Server。  
    > [!IMPORTANT]
    > 从现在开始，始终以域用户（所有服务器上的内置管理员组的成员）身份登录。 在图形服务器安装或在 Windows 10 计算机上运行时，请始终提升你的 PowerShell 和 CMD 命令提示符。

2.  添加网络信息并将节点联接到域，然后对其重启。  
    > [!NOTE]
    > 目前为止，本指南假定你在拉伸群集中有两对供使用的服务器。 WAN 或 LAN 网络将服务器和属于物理或逻辑站点的服务器分隔开。 本指南将 **SR-SRV01** 和 **SR-SRV02** 视为处于站点 Redmond 中，将 **SR-SRV03** 和 **SR-SRV04** 视为处于站点 **Bellevue** 中。  

3.  将第一组共享 JBOD 存储机箱、共享 VHDX、iSCSI 目标或 FC SAN 连接到站点 **Redmond** 中的服务器。  

4.  将第二组存储连接到站点 **Bellevue**.中的服务器。  

5.  根据需要，在所有四个节点上均安装最新的供应商存储和机箱固件及驱动程序、最新的供应商 HBA 驱动程序、最新的供应商 BIOS/UEFI 固件、最新的供应商网络驱动程序和最新的母板芯片组驱动程序。 根据需要重启节点。  

    > [!NOTE]
    > 请查看配置共享存储和网络硬件的硬件供应商文档。  

6.  确保服务器的 BIOS/UEFI 设置启用高性能，例如禁用 C-State、设置 QPI 速度、启用 NUMA 和设置最高内存频率。 确保 Windows Server 中的电源管理设置为高性能。 根据需要重启。  

7.  配置角色，如下所示：  

    -   **图形方法**  

        运行 **ServerManager.exe**，并通过单击“**管理**”和“**添加服务器**”来添加所有服务器节点。  

        > [!IMPORTANT]
        > 在每个节点上安装**故障转移群集**和**存储副本**角色和功能，并对其重启。 如果计划使用其他角色（如 Hyper-V、文件服务器等），也可以立即安装它们。  

    -   **使用 Windows PowerShell 方法**  

        在 **SR-SRV04** 或远程管理计算机上，在 Windows PowerShell 控制台中运行以下命令以在四个节点上为拉伸群集安装所需功能和角色，并对其重启：  

        ```PowerShell  
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'  

        $Servers | foreach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }  

        ```  

        有关这些步骤的详细信息，请参阅[安装或卸载角色、角色服务或功能](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)。  


8. 配置存储，如下所示：  

    > [!IMPORTANT]  
    > -   必须在每个机箱上创建两个卷：一个用于数据，另一个用于日志。  
    > -   必须将日志和数据磁盘初始化为 GPT，而非 MBR。  
    > -   两个数据卷的大小必须相同。  
    > -   两个日志卷的大小应相同。  
    > -   所有复制的数据磁盘的扇区大小必须相同。  
    > -   所有日志磁盘的扇区大小必须相同。  
    > -   日志卷应使用基于闪存的存储和高性能复原设置。 Microsoft 建议，日志存储应比数据存储速度快。 日志卷不得用于其他工作负荷。 
    > -   数据磁盘可使用 HDD、SSD 或分层组合，并可使用镜像或奇偶校验空间或 RAID 1 或 10，或者使用 RAID 5 或 RAID 50。  
    > -  日志卷默认至少必须为 9 GB，但可以根据日志需求增加或减小。  
    > - 卷必须使用 NTFS 或 ReFS 格式。
    > - 文件服务器角色仅对运行 Test-SRTopology 是必要的，因为它将打开必要的防火墙端口。  

    -   **对于 JBOD 机箱：**  

        1.  确保每组配对的服务器节点只能看到该站点的存储机箱（即非对称存储），且 SAS 连接已正确配置。  

        2.  使用 Windows PowerShell 或服务器管理器，按照[在独立服务器上部署存储空间](../storage-spaces/deploy-standalone-storage-spaces.md)中提供的**步骤 1 至 3** 使用存储空间来配置存储。  

    -   **对于 iSCSI 存储：**  

        1.  确保每组配对的服务器节点只能看到该站点的存储机箱（即非对称存储）。 如果使用 iSCSI，则应使用多个网络适配器。  

        2.  使用供应商文档预配存储。 如果使用基于 Windows 的 iSCSI 目标，请查阅 [iSCSI 目标块存储方法](../iscsi/iscsi-target-server.md)。  

    -   **对于 FC SAN 存储：**  

        1.  确保每组配对的服务器节点只能看到该站点的存储机箱（即非对称存储），并且已对主机进行正确的分区。  

        2.  使用供应商文档预配存储。  

## <a name="configure-a-hyper-v-failover-cluster-or-a-file-server-for-a-general-use-cluster"></a>为常规使用群集配置 Hyper-V 故障转移群集或文件服务器

设置服务器节点后，下一步是创建以下群集类型之一：  
*  [Hyper-v 故障转移群集](#BKMK_HyperV)  
*  [一般使用群集的文件服务器](#BKMK_FileServer)  

### <a name="BKMK_HyperV"></a>配置 Hyper-v 故障转移群集  

>[!NOTE]
> 如果你想要创建文件服务器群集，而非 Hyper-V 群集，请跳过此部分，并转到[为常规使用群集配置文件服务器](#BKMK_FileServer)部分。  

现在可以创建常规故障转移群集。 配置、验证和测试完成后，将使用存储副本对其拉伸。 你可以直接在群集节点上或从包含 Windows Server 远程服务器管理工具的远程管理计算机执行以下所有步骤。  

#### <a name="graphical-method"></a>图形方法  

1. 运行 **cluadmin.msc**。  

2. 验证计划群集并分析结果以确保可以继续。  

   > [!NOTE]  
   > 由于使用非对称存储，你在群集验证过程中应该会遇到存储错误。  

3. 创建 Hyper-V 计算群集。 确保群集名称为 15 个字符或更少。 以下使用的示例为 SR-SRVCLUS。 如果节点将驻留在不同的子网中，则必须为每个子网创建群集名称的 IP 地址，并使用 "OR" 依赖关系。  有关详细信息，请参阅[为多子网群集配置 IP 地址和依赖项–第 III 部分](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698)。  

4. 配置文件共享见证或云见证，以在站点丢失事件中提供仲裁。  

   > [!NOTE]  
   > WIndows Server 现在包含基于云（Azure）的见证的选项。 你可以选择此仲裁选项来替代文件共享见证。  

   > [!WARNING]  
   > 有关仲裁配置的详细信息，请参阅 [在 Windows Server 2012 故障转移群集指南的见证配置中配置和管理仲裁](https://technet.microsoft.com/library/jj612870.aspx)。 有关 `Set-ClusterQuorum` cmdlet 上的详细信息，请参阅 [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum)。  

5. 查看 [在 Windows Server 2012 中的 HYPER-V 群集的网络建议](https://technet.microsoft.com/library/dn550728.aspx)，并确保以最佳方式配置了群集网络。  

6. 将 Redmond 站点中的一个磁盘添加到群集 CSV。 若要执行此操作，右键单击“**存储**”部分的“**磁盘**”节点中的一个源磁盘，然后单击“**添加到群集共享卷**”。  

7. 使用[部署 Hyper-V 群集](https://technet.microsoft.com/library/jj863389.aspx)指南，按照 **Redmond** 站点中的步骤 7 至 10 仅创建测试虚拟机，以确保在第一个测试站点中共享存储的两个节点内的群集能正常运行。  

8. 如果要创建两个节点的拉伸群集，则必须在继续之前添加所有存储。 为此，请使用群集节点上的管理权限打开一个 PowerShell 会话，并运行以下命令：`Get-ClusterAvailableDisk -All | Add-ClusterDisk`。

   这在 Windows Server 2016 中是按设计行为。

9. 启动 Windows PowerShell，并使用 `Test-SRTopology` cmdlet，以确定是否满足存储副本的所有要求。  

    例如，对具有 **D:** 和 **E:** 卷的每个计划拉伸群集节点进行验证并运行 30 分钟测试：
   1. 将所有可用的存储移动到 **SR-SRV01**。
   2. 在故障转移群集管理器的“**角色**”部分单击“**创建空角色**”。
   3. 将联机存储添加到名为**新建角色**的空角色。
   4. 将所有可用的存储移动到 **SR-SRV03**。
   5. 在故障转移群集管理器的“**角色**”部分单击“**创建空角色**”。
   6. 将空的**新建角色 (2)** 移动到 **SR-SRV03**。
   7. 将联机存储添加到名为**新建角色 (2)** 的空角色。
   8. 现在你已装载了所有具有驱动器字母的存储，并可使用 `Test-SRTopology` 评估群集。

       例如：

           MD c:\temp  

           Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName D: -SourceLogVolumeName E: -DestinationComputerName SR-SRV03 -DestinationVolumeName D: -DestinationLogVolumeName E: -DurationInMinutes 30 -ResultPath c:\temp        

      > [!IMPORTANT]
      > 在评估期间，当在指定源卷上使用无写入 IO 负载的测试服务器时，请考虑添加工作负载，否则 Test-SRTopology 将不会生成有用的报表。 你应该使用与生产类似的工作负载进行测试，以便看到真实的数值和建议的日志大小。 或者，只需在测试期间将一些文件复制到源卷或下载并运行 DISKSPD 以生成写入 IO。 例如，D: 卷的十分钟的较低写入 IO 工作负荷：   
       `Diskspd.exe -c1g -d600 -W5 -C5 -b4k -t2 -o2 -r -w5 -i100 d:\test.dat`  

10. 检查 **TestSrTopologyReport-&lt; date &gt;.html** 报表以确保满足存储副本要求，并记下初始同步时间预测和日志建议。  

      ![屏幕显示复制报告](./media/Stretch-Cluster-Replication-Using-Shared-Storage/SRTestSRTopologyReport.png)

11. 将磁盘返回到可用存储，并删除临时空角色。

12. 一旦满足，即删除测试虚拟机。 向计划源节点添加进一步评估所需的任何真实的测试虚拟机。  

13. 配置拉伸群集站点感知，以便服务器 **SR-SRV01** 和 **SR-SRV02** 处于站点 **Redmond** 中，而 **SR-SRV03** 和 **SR-SRV04** 处于站点 **Bellevue** 中，且 **Redmond** 对源存储和 VM的节点所有权是首选项：  

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  
   
    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  
   
    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"
    ```

    > [!NOTE]
    > Windows Server 2016 中没有使用故障转移群集管理器配置站点感知的选项。  

14. **（可选）** 配置群集网络和 Active Directory 以用于更快的 DNS 站点故障转移。 可以利用 Hyper-V 软件定义的网络、拉伸的 VLAN、网络抽象设备、降低的 DNS TTL 和其他常用技术。

    有关详细信息，请查看 Microsoft Ignite 会话：延伸[故障转移群集和使用 Windows Server vNext 中的存储副本](http://channel9.msdn.com/Events/Ignite/2015/BRK3487)和在[站点之间启用更改通知-如何和为什么？](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx)博客文章。  

15. **（可选）** 配置 VM 复原，以使来宾无需在节点失败时暂停过久。 相反，他们在 10 秒内故障转移到新的复制源存储。  

    ```PowerShell  
    (Get-Cluster).ResiliencyDefaultPeriod=10  
    ```  

    > [!NOTE]
    >  Windows Server 2016 中没有使用故障转移群集管理器配置 VM 复原的选项。  

#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  

1. 测试计划群集并分析结果以确保可以继续：  

   ```PowerShell  
   Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04  
   ```  

   > [!NOTE]
   >  由于使用非对称存储，你在群集验证过程中应该会遇到存储错误。  

2. 为常规使用存储群集创建文件服务器（必须指定群集将使用的静态 IP 地址）。 确保群集名称为 15 个字符或更少。  如果节点位于不同子网中，则必须使用 "OR" 依赖关系创建其他站点的 IP 地址。 有关详细信息，请参阅[为多子网群集配置 IP 地址和依赖项–第 III 部分](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698)。
   ```PowerShell  
   New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here>  
   Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”
   Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
   ```  

3. 在指向托管在域控制器或某些其他独立服务器上的共享的群集中配置文件共享见证或云 (Azure) 见证。 例如：  

   ```PowerShell  
   Set-ClusterQuorum -FileShareWitness \\someserver\someshare  
   ```  

   > [!NOTE]
   > WIndows Server 现在包含基于云（Azure）的见证的选项。 你可以选择此仲裁选项来替代文件共享见证。  
    
   有关仲裁配置的详细信息，请参阅 [在 Windows Server 2012 故障转移群集指南的见证配置中配置和管理仲裁](https://technet.microsoft.com/library/jj612870.aspx)。 有关 `Set-ClusterQuorum` cmdlet 上的详细信息，请参阅 [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum)。  

4. 查看 [在 Windows Server 2012 中的 HYPER-V 群集的网络建议](https://technet.microsoft.com/library/dn550728.aspx)，并确保以最佳方式配置了群集网络。  

5. 如果要创建两个节点的拉伸群集，则必须在继续之前添加所有存储。 为此，请使用群集节点上的管理权限打开一个 PowerShell 会话，并运行以下命令：`Get-ClusterAvailableDisk -All | Add-ClusterDisk`。

   这在 Windows Server 2016 中是按设计行为。

6. 使用[部署 Hyper-V 群集](https://technet.microsoft.com/library/jj863389.aspx)指南，按照 **Redmond** 站点中的步骤 7 至 10 仅创建测试虚拟机，以确保在第一个测试站点中共享存储的两个节点内的群集能正常运行。  

7. 一旦满足，即删除测试 VM。 向计划源节点添加进一步评估所需的任何真实的测试虚拟机。  

8. 配置拉伸群集站点感知以便服务器 **SR-SRV01** 和 **SR-SRV02** 处于站点 **Redmond** 中，而 **SR-SRV03** 和 **SR-SRV04** 处于站点 **Bellevue** 中，且 **Redmond** 对源存储和虚拟机的节点所有权是首选项：  

   ```PowerShell  
   New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

   New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

   Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
   Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
   Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
   Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

   (Get-Cluster).PreferredSite="Seattle"  
   ```  

9. **（可选）** 配置群集网络和 Active Directory 以用于更快的 DNS 站点故障转移。 可以利用 Hyper-V 软件定义的网络、拉伸的 VLAN、网络抽象设备、降低的 DNS TTL 和其他常用技术。  

   有关详细信息，请查看 Microsoft Ignite 会话：[使用 Windows Server vNext 中的存储副本拉伸故障转移群集](http://channel9.msdn.com/Events/Ignite/2015/BRK3487)和[启用更改通知，并在站点之间启用更改通知](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx)。  

10. **（可选）** 配置 VM 复原，以使来宾无需在节点失败时暂停时间过长。 相反，他们在 10 秒内故障转移到新的复制源存储。  

    ```PowerShell  
    (Get-Cluster).ResiliencyDefaultPeriod=10  
    ```  

    > [!NOTE]  
    > Windows Server 2016 中没有使用故障转移群集管理器配置 VM 复原的选项。  



### <a name="BKMK_FileServer"></a>为常规使用群集配置文件服务器  

>[!NOTE]
> 如果你已配置了[配置 Hyper-V 故障转移群集](#BKMK_HyperV)中所述的 Hyper-V 故障转移群集，则跳过此部分。  

现在可以创建常规故障转移群集。 配置、验证和测试完成后，将使用存储副本对其拉伸。 你可以直接在群集节点上或从包含 Windows Server 远程服务器管理工具的远程管理计算机执行以下所有步骤。  

#### <a name="graphical-method"></a>图形方法  

1. 运行 cluadmin.msc。  

2. 验证计划群集并分析结果以确保可以继续。  
   >[!NOTE]
   >由于使用非对称存储，你在群集验证过程中应该会遇到存储错误。   
3. 创建用于常规使用存储群集的文件服务器。 确保群集名称为 15 个字符或更少。 以下使用的示例为 SR-SRVCLUS。  如果节点将驻留在不同的子网中，则必须为每个子网创建群集名称的 IP 地址，并使用 "OR" 依赖关系。  有关详细信息，请参阅[为多子网群集配置 IP 地址和依赖项–第 III 部分](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698)。  

4. 配置文件共享见证或云见证，以在站点丢失事件中提供仲裁。  
   >[!NOTE]
   > WIndows Server 现在包含基于云（Azure）的见证的选项。 你可以选择此仲裁选项来替代文件共享见证。                                                                                                                                                                             
   >[!NOTE]
   >  有关仲裁配置的详细信息，请参阅 [在 Windows Server 2012 故障转移群集指南的见证配置中配置和管理仲裁](https://technet.microsoft.com/library/jj612870.aspx)。 有关 Set-ClusterQuorum cmdlet 上的详细信息，请参阅 [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum)。 

5. 如果要创建两个节点的拉伸群集，则必须在继续之前添加所有存储。 为此，请使用群集节点上的管理权限打开一个 PowerShell 会话，并运行以下命令：`Get-ClusterAvailableDisk -All | Add-ClusterDisk`。

   这在 Windows Server 2016 中是按设计行为。

6. 确保以最佳方式配置了群集网络。  
    >[!NOTE]
    > 在继续下一步骤前，必须在所有节点上都安装文件服务器角色。   |  

7. 在“**角色**”下，单击“**配置角色**”。 查看“**开始之前**”，然后单击“**下一步**”。  

8. 选择“**文件服务器**”，然后单击“**下一步**”。  

9. 将“**供常规使用的文件服务器**”保留为选中状态，然后单击“**下一步**”。  

10. 提供“**客户端访问点**”名称（15 个字符或更少），然后单击“**下一步**”。  

11. 选择一个磁盘作为数据卷，然后单击“**下一步**”。  

12. 查看你的设置，然后单击“**下一步**”。 单击 **“完成”** 。  

13. 右键单击你的新文件服务器角色，然后单击“**添加文件共享**”。 继续执行向导以配置共享。  

14. 可选：添加另一个使用此站点中的其他存储的文件服务器角色。  

15. 配置拉伸群集站点感知以便服务器 SR-SRV01 和 SR-SRV02 处于站点 Redmond 中，而 SR-SRV03 和 SR-SRV04 处于站点 Bellevue 中，且 Redmond 对源存储和 VM的节点所有权是首选项：  

    ```PowerShell  
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"  
    ```  

      >[!NOTE]
      > Windows Server 2016 中没有使用故障转移群集管理器配置站点感知的选项。  

16. （可选）配置群集网络和 Active Directory 以用于更快的 DNS 站点故障转移。 可以利用拉伸的 VLAN、网络抽象设备、降低的 DNS TTL 和其他常用技术。  

有关详细信息，请查看 Microsoft Ignite 会话 [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](http://channel9.msdn.com/events/ignite/2015/brk3487)（在 Windows Server vNext 中拉伸故障转移群集和使用存储副本），以及 [Enable Change Notifications between Sites - How and Why?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx)（在站点间启用更改通知 - 操作方法和原因）。    

#### <a name="powershell-method"></a>PowerShell 方法

1. 测试计划群集并分析结果以确保可以继续：    

    ```PowerShell
    Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04
    ```

    > [!NOTE]
    >  由于使用非对称存储，你在群集验证过程中应该会遇到存储错误。   

2.  创建 Hyper-V 计算群集（必须指定群集将使用的你自己的静态 IP 地址）。 确保群集名称为 15 个字符或更少。  如果节点位于不同子网中，则必须使用 "OR" 依赖关系创建其他站点的 IP 地址。 有关详细信息，请参阅[为多子网群集配置 IP 地址和依赖项–第 III 部分](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698)。  

    ```PowerShell
    New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here> 

    Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”

    Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
    ```


3. 在指向托管在域控制器或某些其他独立服务器上的共享的群集中配置文件共享见证或云 (Azure) 见证。 例如：  

    ```PowerShell
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare
    ```

    >[!NOTE]
    > Windows Server 现在包含使用 Azure 的云见证的选项。 你可以选择此仲裁选项来替代文件共享见证。  

   有关仲裁配置的详细信息，请参阅[了解群集和池仲裁](../storage-spaces/understand-quorum.md)。 有关 Set-ClusterQuorum cmdlet 上的详细信息，请参阅 [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum)。

4.  如果要创建两个节点的拉伸群集，则必须在继续之前添加所有存储。 为此，请使用群集节点上的管理权限打开一个 PowerShell 会话，并运行以下命令：`Get-ClusterAvailableDisk -All | Add-ClusterDisk`。

    这在 Windows Server 2016 中是按设计行为。

5. 确保以最佳方式配置了群集网络。  

6.  配置文件服务器角色。 例如：

    ```PowerShell  
    Get-ClusterResource  
    Add-ClusterFileServerRole -Name SR-CLU-FS2 -Storage "Cluster Disk 4"  

    MD e:\share01  

    New-SmbShare -Name Share01 -Path f:\share01 -ContinuouslyAvailable $false  
    ```

7. 配置拉伸群集站点感知，以便服务器 SR-SRV01 和 SR-SRV02 处于站点 Redmond 中，而 SR-SRV03 和 SR-SRV04 处于站点 Bellevue 中，且 Redmond 对源存储和虚拟机的节点所有权是首选项：  

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"  
    ```

8.  （可选）配置群集网络和 Active Directory 以用于更快的 DNS 站点故障转移。 可以利用拉伸的 VLAN、网络抽象设备、降低的 DNS TTL 和其他常用技术。  
    
    有关详细信息，请查看 Microsoft Ignite 会话 [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](http://channel9.msdn.com/events/ignite/2015/brk3487)（在 Windows Server vNext 中拉伸故障转移群集和使用存储副本），以及 [Enable Change Notifications between Sites - How and Why?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx)（在站点间启用更改通知 - 操作方法和原因）。

### <a name="configure-a-stretch-cluster"></a>配置拉伸群集  
现在将使用故障转移群集管理器或 Windows PowerShell 来配置拉伸群集。 你可以直接在群集节点上或从包含 Windows Server 远程服务器管理工具的远程管理计算机执行以下所有步骤。  

#### <a name="failover-cluster-manager-method"></a>故障转移群集管理器方法  

1.  对于 Hyper-V 工作负载，在想要复制数据的节点上，将源数据磁盘从可用磁盘添加到群集共享卷（如果还未配置）。 不要添加所有磁盘；只需添加单个磁盘。 此时，因为这是非对称存储，一半的磁盘将显示离线。  
如果复制物理磁盘资源 (PDR) 工作负载（如文件服务器）供常规使用，则表示已有准备就绪的角色附加磁盘。  

    ![屏幕显示故障转移群集管理器](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_OnlineDisks2.png)  

2.  右键单击 CSV 磁盘或角色附加磁盘，单击“**复制**”，然后单击“**启用**”。  

3.  选择适当的目标数据卷，然后单击“**下一步**”。 显示的目标磁盘将具有一个与所选的源磁盘大小相同的卷。 在这些向导对话框间移动时，可用的存储会根据需要自动在后台移动并进入联机状态。  

    ![屏幕显示“配置存储副本”向导的“选择目标磁盘”页面](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_SelectDestinationDataDisk2.png)  

4.  选择适当的源日志磁盘，然后单击“**下一步**”。 源日志卷应处于使用 SSD 或相似的快速媒体的磁盘上，而不是处于旋转磁盘上。  

5.  选择适当的目标日志卷，然后单击“下一步”。 显示的目标日志磁盘将具有一个与所选的源日志磁盘卷大小相同的卷。  

6.  如果目标卷不包含来自源服务器的数据的以前副本，则将“**覆盖卷**”值保留为“**覆盖目标卷**”。 如果目标的确包含类似数据，则从近期备份或以前的复制中，选择“**种子目标磁盘**”，然后单击“**下一步**”。  

7.  若计划使用零 RPO 复制，则将“**复制模式**”值保留为“**同步复制**”。 如果计划在主站点节点上的更高延迟网络拉伸群集或需要更低的 IO 延迟，则将其更改为“**异步复制**”。  
8.  如果并未计划稍后在复制组中使用写入顺序和额外的磁盘对，则将“**一致性组**”值保留为“**最高性能**”。 如果计划向此复制组添加更多磁盘且需要保证的写入顺序，请选择“**启用写入顺序**”，然后单击“**下一步**”。  

9.  单击“**下一步**”以配置复制和拉伸群集结构。  

    ![屏幕显示“配置存储副本”向导的“选择确认”页面](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ConfigureSR2.png)  

10. 在“摘要”屏幕中备注完成对话框结果。 可以在 Web 浏览器中查看报表。  

11. 此时，已配置群集的两个部分之间的存储副本合作关系，但复制正在进行。 有多种方法可以通过图形工具查看复制状态。  

    1.  使用“**复制角色**”列和“**复制**”选项卡。完成初始同步后，源和目标磁盘的复制状态将为“**连续复制**”。   

        ![屏幕显示故障转移群集管理器中磁盘的“复制”选项卡](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ReplicationDetails2.png)  

    2.  启动 **eventvwr.exe**。  

        1.  在源服务器上，导航到**应用程序和服务 \ Microsoft \ Windows \ StorageReplica \ 管理员**并检查事件 5015、5002、5004、1237、5001 和 2200。  

        2.  在目标服务器上，导航到**应用程序和服务 \ Microsoft \ Windows \ StorageReplica \ 操作**并等待事件 1215。 此事件会显示复制的字节数和所用的时间。 例如：  

            ```  
            Log Name:      Microsoft-Windows-StorageReplica/Operational  
            Source:        Microsoft-Windows-StorageReplica  
            Date:          4/6/2016 4:52:23 PM  
            Event ID:      1215  
            Task Category: (1)  
            Level:         Information  
            Keywords:      (1)  
            User:          SYSTEM  
            Computer:      SR-SRV03.Threshold.nttest.microsoft.com  
            Description:  
            Block copy completed for replica.  

            ReplicationGroupName: Replication 2  
            ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}  
            ReplicaName: \\?\Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}\  
            ReplicaId: {00000000-0000-0000-0000-000000000000}  
            End LSN in bitmap:   
            LogGeneration: {00000000-0000-0000-0000-000000000000}  
            LogFileId: 0  
            CLSFLsn: 0xFFFFFFFF  
            Number of Bytes Recovered: 68583161856  
            Elapsed Time (ms): 140  
            ```  

        3.  在目标服务器上，导航到**应用程序和服务 \ Microsoft \ Windows \ StorageReplica \ 管理员**并检查事件 5009、1237、5001、5015、5005 和 2200 以了解处理进程。 在该序列中不应有错误的警告。 将有许多 1237 事件；这些表示进度。  

            > [!WARNING]
            > 初始同步完成前，CPU 和内存使用率可能比平时高。  

#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  

1.  确保使用提升的管理员帐户运行 Powershell 控制台。  
2.  仅将源数据存储作为 CSV 添加到群集。 若要获取可用磁盘的大小、分区和卷布局，请使用以下命令：  

    ```PowerShell  
    Move-ClusterGroup -Name "available storage" -Node sr-srv01  

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }  
    $DiskResources | foreach {  
        $resource = $_  
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid  

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |  
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining  
    } | FT -AutoSize  

    Move-ClusterGroup -Name "available storage" -Node sr-srv03  

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }  
    $DiskResources | foreach {  
        $resource = $_  
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid  

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |  
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining  
    } | FT -AutoSize  
    ```  

4.  将正确的磁盘设置为 CSV，如下：  

    ```PowerShell  
    Add-ClusterSharedVolume -Name "Cluster Disk 4"  
    Get-ClusterSharedVolume  
    Move-ClusterSharedVolume -Name "Cluster Disk 4" -Node sr-srv01  
    ```  

5.  配置拉伸群集，指定以下：  

    -   源和目标节点（源数据是 CSV 磁盘，而所有其他磁盘则不是）。  

    -   源和目标复制组名称。  

    -   分区大小匹配的源和目标磁盘。  

    -   源和目标日志卷，其中有足够的可用空间来包含两个磁盘上的日志大小，且存储为 SSD 或类似的快速媒体。  

    -   源和目标日志卷，其中有足够的可用空间来包含两个磁盘上的日志大小，且存储为 SSD 或类似的快速媒体。  

    -   日志大小。  

    -   源日志卷应处于使用 SSD 或相似的快速媒体的磁盘上，而不是处于旋转磁盘上。  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv01 -SourceRGName rg01 -SourceVolumeName "C:\ClusterStorage\Volume1" -SourceLogVolumeName e: -DestinationComputerName sr-srv03 -DestinationRGName rg02 -DestinationVolumeName d: -DestinationLogVolumeName e:  
    ```  

    > [!NOTE]  
    > 还可使用每个站点中的一个节点上的 `New-SRGroup` 和 `New-SRPartnership` 以分阶段创建复制，而非一次性创建复制。  

6.  确定复制进度。  

    1.  在源服务器上，运行以下命令并检查事件 5015、5002、5004、1237、5001 和2200：  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  在目标服务器上，运行以下命令以查看显示合作关系创建的存储副本事件。 此事件会显示复制的字节数和所用的时间。 例如：  

            Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  

            TimeCreated  : 4/6/2016 4:52:23 PM  
            ProviderName : Microsoft-Windows-StorageReplica  
            Id           : 1215  
            Message      : Block copy completed for replica.  

               ReplicationGroupName: Replication 2  
               ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}  
               ReplicaName: ?Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}  
               ReplicaId: {00000000-0000-0000-0000-000000000000}  
               End LSN in bitmap:   
               LogGeneration: {00000000-0000-0000-0000-000000000000}  
               LogFileId: 0  
               CLSFLsn: 0xFFFFFFFF  
               Number of Bytes Recovered: 68583161856  
               Elapsed Time (ms): 140  

    3.  在目标服务器上，运行以下命令并检查事件 5009、1237、5001、5015、5005 和 2200 以了解处理进度。 在该序列中不应有错误的警告。 将有许多 1237 事件；这些表示进度。  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

    4.  或者，副本的目标服务器组规定要复制的剩余字节数，且可通过 PowerShell 查询。 例如：  

        ```PowerShell  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        作为进度示例（它将不会终止）：  

        ```PowerShell  
        while($true) {  

         $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

7.  若要在拉伸群集内获取复制源和目标状态，请使用 `Get-SRGroup` 和 `Get-SRPartnership` 查看拉伸群集中的复制的配置状态。  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```  

### <a name="manage-stretched-cluster-replication"></a>管理拉伸群集复制  
现在将管理和运行拉伸群集。 你可以直接在群集节点上或从包含 Windows Server 远程服务器管理工具的远程管理计算机执行以下所有步骤。  

#### <a name="graphical-tools-method"></a>图形工具方法  

1.  使用故障转移群集管理器确定复制的当前源和目标及其状态。  

2.  若要测量复制性能，在源和目标节点上均运行 **Perfmon.exe**。  

    1.  在目标节点上：  

        1.  为数据卷添加**存储副本统计信息**对象及其所有性能计数器。  

        2.  检查结果。  

    2.  在源节点上：  

        1.  为数据卷添加“**存储副本统计信息**”和“**存储副本分区 I/O 统计信息**”对象及其所有性能计数器（后者仅对当前源服务器上的数据可用）。  

        2.  检查结果。  

3.  若要更改拉伸群集内的复制源和目标，请使用以下方法：  

    1.  在同一站点中的节点间移动源复制：右键单击源 CSV，依次单击“**移动存储**”和“**选择节点**”，然后选择同一站点中的节点。 如果对角色分配的磁盘使用非 CSV 存储，则移动角色。  

    2.  将源复制从一个站点移动到其他站点：右键单击源 CSV，依次单击“**移动存储**”和“**选择节点**”，然后选择其他站点中的节点。 如果配置首选站点，则可使用最佳可能的节点以始终将源存储移动到首选站点中的节点。 如果对角色分配的磁盘使用非 CSV 存储，则移动角色。  

    3.  执行从一个站点到其他站点的计划故障转移复制方向：使用 **ServerManager.exe** 或 **SConfig** 关闭一个站点中的两个节点。  

    4.  执行从一个站点到其他站点的未计划故障转移复制方向：切断一个站点中的两个节点的电源。  

        > [!NOTE]
        > 在 Windows Server 2016 中，可能需要使用故障转移群集管理器或 Move-ClusterGroup，在节点重新联机后将目标磁盘手动移回另一个站点。  

        > [!NOTE]
        > 存储副本用于卸除目标卷。 这是设计使然。  

4.  若要更改默认的 8 GB 日志大小，请右键单击源和目标日志磁盘，单击 "**复制日志**" 选项卡，然后更改两个磁盘上的大小以进行匹配。  

    > [!NOTE]  
    > 默认日志大小为 8 GB。 根据 `Test-SRTopology` cmdlet 的结果，你可能决定使用具有较高值或较低值的 `-LogSizeInBytes`。  

5.  若要向现有的复制组添加另一对复制磁盘，必须确保在可用的存储中至少具有一个额外的磁盘。 然后可以右键单击源磁盘并选择“**添加复制合作关系**”。  
    > [!NOTE]  
    > 需要可用存储中额外的“虚拟”磁盘是因为回归，而非有意为之。 故障转移群集管理器以前支持正常添加更多磁盘，在以后的版本中也会继续支持。  


6.  若要删除现有的复制，请执行以下操作：  

    1.  启动 **cluadmin.msc**。  

    2.  右键单击源 CSV 磁盘，然后依次单击“**复制**”和“**删除**”。 接受警告提示。  

    3.  （可选）从 CSV 删除存储以将其返回到可用存储，供进一步测试。  

        > [!NOTE]  
        > 返回到可用存储后，可能需要使用 **DiskMgmt.msc** 或 **ServerManager.exe** 以将后驱动器字母添加到卷。  

#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  

1.  使用 **Get-SRGroup** 和 **(Get-SRGroup).Replicas** 确定复制的当前源和目标及其状态。  

2.  若要测量复制性能，请在源和目标节点上均使用 `Get-Counter`。 计数器名称为：  

    -   \Storage Replica Partition I/O Statistics(*)\Number of times flush paused  

    -   \Storage Replica Partition I/O Statistics(*)\Number of pending flush I/O  

    -   \Storage Replica Partition I/O Statistics(*)\Number of requests for last log write  

    -   \Storage Replica Partition I/O Statistics(*)\Avg.Flush Queue Length  

    -   \Storage Replica Partition I/O Statistics(*)\Current Flush Queue Length  

    -   \Storage Replica Partition I/O Statistics(*)\Number of Application Write Requests  

    -   \Storage Replica Partition I/O Statistics(*)\Avg.Number of requests per log write  

    -   \Storage Replica Partition I/O Statistics(*)\Avg.App Write Latency  

    -   \Storage Replica Partition I/O Statistics(*)\Avg.App Read Latency  

    -   \Storage Replica Statistics(*)\Target RPO  

    -   \Storage Replica Statistics(*)\Current RPO  

    -   \Storage Replica Statistics(*)\Avg.Log Queue Length  

    -   \Storage Replica Statistics(*)\Current Log Queue Length  

    -   \Storage Replica Statistics(*)\Total Bytes Received  

    -   \Storage Replica Statistics(*)\Total Bytes Sent  

    -   \Storage Replica Statistics(*)\Avg.Network Send Latency  

    -   \Storage Replica Statistics(*)\Replication State  

    -   \Storage Replica Statistics(*)\Avg.Message Round Trip Latency  

    -   \Storage Replica Statistics(*)\Last Recovery Elapsed Time  

    -   \Storage Replica Statistics(*)\Number of Flushed Recovery Transactions  

    -   \Storage Replica Statistics(*)\Number of Recovery Transactions  

    -   \Storage Replica Statistics(*)\Number of Flushed Replication Transactions  

    -   \Storage Replica Statistics(*)\Number of Replication Transactions  

    -   \Storage Replica Statistics(*)\Max Log Sequence Number  

    -   \Storage Replica Statistics(*)\Number of Messages Received  

    -   \Storage Replica Statistics(*)\Number of Messages Sent  

    有关 Windows PowerShell 中的性能计数器的详细信息，请参阅 [Get-Counter](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Diagnostics/Get-Counter)。  

3.  若要更改拉伸群集内的复制源和目标，请使用以下方法：  

    1.  若要将复制源从一个节点移动到 **Redmond** 站点中的其他节点，使用 Move-ClusterSharedVolume cmdlet 移动 CSV 资源。  

        ```PowerShell  
        Get-ClusterSharedVolume | fl *  
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv02  
        ```  

    2.  若要将复制方向从一个站点移动到其他“计划”站点，请使用 **Move-ClusterSharedVolume** cmdlet 移动 CSV 资源。  

        ```PowerShell 
        Get-ClusterSharedVolume | fl *  
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv04  
        ```  

        该操作也将为其他站点和节点移动适当的日志和数据。  

    3.  执行从一个站点到其他站点的未计划故障转移复制方向：切断一个站点中的两个节点的电源。  

        > [!NOTE]  
        > 存储副本用于卸除目标卷。 这是设计使然。  

4.  若要更改默认的 8 GB 日志大小，请在源和目标存储副本组上使用**get-srgroup** 。   例如，若要将所有日志设置为 2 GB：  

    ```PowerShell  
    Get-SRGroup | Set-SRGroup -LogSizeInBytes 2GB  
    Get-SRGroup  
    ```  

5.  若要向现有的复制组添加另一对复制磁盘，必须确保在可用的存储中至少具有一个额外的磁盘。 然后可以右键单击源磁盘并选择“添加复制合作关系”。  
       >[!NOTE]
       >需要可用存储中额外的“虚拟”磁盘是因为回归，而非有意为之。 故障转移群集管理器以前支持正常添加更多磁盘，在以后的版本中也会继续支持。  

    将 **Set-SRPartnership** cmdlet 与 **-SourceAddVolumePartnership** 和 **-DestinationAddVolumePartnership** 参数结合使用。  
6.  若要删除复制，请在任意节点上使用 `Get-SRGroup`、Get-`SRPartnership`、`Remove-SRGroup` 和 `Remove-SRPartnership`。  

    ```PowerShell  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

    > [!NOTE]
    > 如果使用远程管理计算机，则需要向这些 cmdlet 指定群集名称，并提供两个 RG 名称。  

### <a name="related-topics"></a>相关主题  
- [存储副本概述](storage-replica-overview.md)  
- [服务器到服务器存储复制](server-to-server-storage-replication.md)  
- [群集到群集存储复制](cluster-to-cluster-storage-replication.md)  
- [存储副本：已知问题](storage-replica-known-issues.md) 
- [存储副本：常见问题解答](storage-replica-frequently-asked-questions.md)  

## <a name="see-also"></a>请参阅  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Windows Server 2016 中的存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)
