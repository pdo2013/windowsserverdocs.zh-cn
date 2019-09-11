---
title: 服务器到服务器存储复制
description: 如何在 Windows Server 中为服务器到服务器的复制设置和使用存储副本，包括 Windows 管理中心和 PowerShell。
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 61881b52-ee6a-4c8e-85d3-702ab8a2bd8c
ms.openlocfilehash: fccdb8547ff27083ce943892842c2e2d05e5ace8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865288"
---
# <a name="server-to-server-storage-replication-with-storage-replica"></a>具有存储副本的服务器到服务器存储复制

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

可以使用存储副本配置两个同步数据的服务器，以便每个服务器都具有相同卷的相同副本。 本主题提供了此服务器到服务器复制配置的一些背景知识，以及如何对其进行设置和管理环境。

若要管理存储副本，可以使用[Windows 管理中心](../../manage/windows-admin-center/overview.md)或 PowerShell。

下面是在 Windows 管理中心中使用存储副本的概述视频。
> [!video https://www.microsoft.com/videoplayer/embed/3aa09fd4-867b-45e9-953e-064008468c4b?autoplay=false]


## <a name="prerequisites"></a>先决条件  

* Active Directory 域服务林（无需运行 Windows Server 2016）。  
* 两台运行 Windows Server 2019 或 Windows Server 2016 Datacenter Edition 的服务器。 如果你运行的是 Windows Server 2019，则可以改为使用标准版，如果你只是复制一个最大为 2 TB 的卷。  
* 两个使用 SAS JBOD、光纤通道 SAN、iSCSI 目标或本地 SCSI/SATA 存储的存储集。 存储需包含 HDD 和 SSD 媒体的组合。 将每个存储设置为仅对每个服务器可用（没有共享的访问）。  
* 每个存储集必须允许至少创建两个虚拟磁盘，一个用于复制的数据，另一个用于日志。 物理存储在所有数据磁盘上的扇区大小必须相同。 物理存储在所有日志磁盘上的扇区大小必须相同。  
* 每个服务器上必须具有至少一个用于同步复制的以太网/TCP 连接，但最好是 RDMA。   
* 合适的防火墙和路由器规则，以允许所有节点之间的 ICMP、SMB（端口 445 以及用于 SMB 直通的 5445）和 WS-MAN（端口 5985）双向通信。  
* 服务器间的网络具有足够的带宽，以包含 IO 写入工作负载和平均值为 5 毫秒的往返行程延迟（对于同步复制）。 异步复制没有延迟建议。<br>
如果要在本地服务器和 Azure Vm 之间进行复制，则必须在本地服务器和 Azure Vm 之间创建网络链接。 为此，请使用[快速路由](#add-azure-vm-expressroute)、[站点到站点 VPN 网关连接](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)，或在 AZURE vm 中安装 VPN 软件，以将其与本地网络连接。
* 复制的存储不能位于包含 Windows 操作系统文件夹的驱动器上。

> [!IMPORTANT]
>  在此方案中，每个服务器应位于不同的物理或逻辑站点中。 每个服务器必须能够通过网络相互通信。  


很多这些要求都可通过使用 `Test-SRTopology cmdlet` 确定。 如果将存储副本或存储副本管理工具功能安装在至少一台服务器上，则会获取此工具的访问权限。 无需将存储副本配置为使用此工具，只需安装 cmdlet。 以下步骤中包含更多信息。  

## <a name="windows-admin-center-requirements"></a>Windows 管理中心要求

若要将存储副本和 Windows 管理中心一起使用，需要以下各项：

| 系统                        | 操作系统                                            | 所需的     |
|-------------------------------|-------------------------------------------------------------|------------------|
| 两个服务器 <br>（任何本地硬件、Vm 和云 Vm 混合，包括 Azure Vm）| Windows Server 2019、Windows Server 2016 或 Windows Server （半年频道） | 存储副本  |
| 一台 PC                     | Windows 10                                                  | Windows Admin Center |

> [!NOTE]
> 目前不能在服务器上使用 Windows 管理中心来管理存储副本。

## <a name="terms"></a>术语  
此演练使用以下环境作为示例：  

-   两个服务器，名为 **SR-SRV05** 和 **SR-SRV06**。  

-   表示两个不同数据中心的一对逻辑“站点”，一个名为 **Redmond**，另一个名为 **Bellevue**。  

![显示使用构建 5 中的服务器复制构建 9 中的服务器的关系图](media/Server-to-Server-Storage-Replication/Storage_SR_ServertoServer.png)  

**图 1：服务器到服务器的复制**  

## <a name="step-1-install-and-configure-windows-admin-center-on-your-pc"></a>步骤 1：在电脑上安装和配置 Windows 管理中心

如果你使用 Windows 管理中心来管理存储副本，请使用以下步骤来准备你的电脑以管理存储副本。
1. 下载并安装[Windows 管理中心](../../manage/windows-admin-center/overview.md)。
2. 下载并安装[远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。
    - 如果使用的是 Windows 10 版本1809或更高版本，请安装 "RSAT：Windows PowerShell 的存储副本模块 "按需功能"。
3. 通过选择 "**开始**" 按钮，键入 " **powershell**"，右键单击 " **Windows PowerShell"，** 然后选择 "以**管理员身份运行**"，以管理员身份打开 PowerShell 会话。
4. 输入以下命令以在本地计算机上启用 WS-MANAGEMENT 协议，并在客户端上设置远程管理的默认配置。

    ```PowerShell
    winrm quickconfig
    ```

5. 键入**Y**启用 winrm 服务并启用 Winrm 防火墙例外。

## <a name="provision-os"></a>步骤2：设置操作系统、功能、角色、存储和网络

1.  使用 Windows Server **（桌面体验）** 的安装类型在两个服务器节点上安装 windows server。 
 
    若要通过 ExpressRoute 使用连接到网络的 Azure VM，请参阅[添加通过 expressroute 连接到网络的 AZURE vm](#add-azure-vm-expressroute)。

3.  添加网络信息，将服务器加入到与 Windows 10 管理电脑相同的域（如果你使用的是 Windows 10 管理电脑），然后重新启动服务器。  

    > [!NOTE]
    > 从现在开始，始终以域用户（所有服务器上的内置管理员组的成员）身份登录。 在图形服务器安装或在 Windows 10 计算机上运行时，请始终提升你的 PowerShell 和 CMD 命令提示符。  

3.  将第一组 JBOD 存储机箱、iSCSI 目标、FC SAN 或本地固定磁盘（DAS）存储连接到站点**Redmond**中的服务器。  

4.  将第二组存储连接到站点**Bellevue**中的服务器。  

5.  根据需要，在两个节点上均安装最新的供应商存储和机箱固件及驱动程序、最新的供应商 HBA 驱动程序、最新的供应商 BIOS/UEFI 固件、最新的供应商网络驱动程序和最新的母板芯片组驱动程序。 根据需要重启节点。  

    > [!NOTE]
    > 请查看配置共享存储和网络硬件的硬件供应商文档。  

6.  确保服务器的 BIOS/UEFI 设置启用高性能，例如禁用 C-State、设置 QPI 速度、启用 NUMA 和设置最高内存频率。 确保 Windows Server 中的电源管理设置为高性能。 根据需要重启。  

7.  配置角色，如下所示：  

    -   **Windows 管理中心方法**
        1. 在 Windows 管理中心中，导航到 "服务器管理器"，然后选择其中一个服务器。
        2. 导航到 "**角色" & 功能**"。
        3. 选择 "**功能** > " "**存储副本**"，然后单击 "**安装**"。
        4. 在另一台服务器上重复此操作。
    -   **服务器管理器方法**  

        1.  运行 **ServerManager.exe** 并创建服务器组，添加所有服务器节点。  

        2.  在每个节点上安装**文件服务器**和**存储副本**角色和功能，并对其重启。  

    -   **Windows PowerShell 方法**  

        在 SR-SRV06 或远程管理计算机上，在 Windows PowerShell 控制台中运行以下命令以安装所需功能和角色，并对其重启：  

        ```powershell  
        $Servers = 'SR-SRV05','SR-SRV06'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        有关这些步骤的详细信息，请参阅[安装或卸载角色、角色服务或功能](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)  

8.  配置存储，如下所示：  

    > [!IMPORTANT]  
    > -   必须在每个机箱上创建两个卷：一个用于数据，另一个用于日志。  
    > -   必须将日志和数据磁盘初始化为 GPT，而非 MBR。  
    > -   两个数据卷的大小必须相同。  
    > -   两个日志卷的大小应相同。  
    > -   所有复制的数据磁盘的扇区大小必须相同。  
    > -   所有日志磁盘的扇区大小必须相同。  
    > -   日志卷应使用基于闪存的存储，如 SSD。 Microsoft 建议日志存储应比数据存储速度快。 日志卷不得用于其他工作负荷。
    > -   数据磁盘可使用 HDD、SSD 或分层组合，并可使用镜像或奇偶校验空间或 RAID 1 或 10，或者使用 RAID 5 或 RAID 50。  
    > -   默认情况下，日志卷必须至少为 9 GB，但可以根据日志需求设置为更大或更小。  
    > -   文件服务器角色仅对运行 Test-SRTopology 是必要的，因为它将打开必要的防火墙端口。
    
    - **对于 JBOD 机箱：**  

        1.  确保每个服务器只能看到该站点的存储机箱，且 SAS 连接已正确配置。  

        2.  使用 Windows PowerShell 或服务器管理器，按照[在独立服务器上部署存储空间](../storage-spaces/deploy-standalone-storage-spaces.md)中提供的**步骤 1 至 3** 使用存储空间来配置存储。  

    - **对于 iSCSI 存储：**  

        1.  确保每个群集都只能看到该站点的存储机箱。 如果使用 iSCSI，则应使用多个网络适配器。    

        2.  使用供应商文档预配存储。 如果使用基于 Windows 的 iSCSI 目标，请查阅 [iSCSI 目标块存储方法](../iscsi/iscsi-target-server.md)。  

    - **对于 FC SAN 存储：**  

        1.  确保每个群集只能看到该站点的存储机箱，且对主机进行了正确的分区。   

        2.  使用供应商文档预配存储。  

    - **对于本地固定磁盘存储：**  

        -   确保存储不包含系统卷、页面文件或转储文件。  

        -   使用供应商文档预配存储。  

9. 启动 Windows PowerShell，并使用 **Test-SRTopology** cmdlet 确定是否满足所有存储副本要求。 可以在仅要求模式下使用 cmdlet 以用于快速测试，也可以在长时间运行的性能评估模式下使用。  

    例如，对具有 **F:** 和 **G:** 卷的每个计划节点进行验证并运行 30 分钟测试：  
        
    ```PowerShell
    MD c:\temp  

    Test-SRTopology -SourceComputerName SR-SRV05 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV06 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp  
    ```

    > [!IMPORTANT]
      > 在评估期间，当在指定源卷上使用无写入 IO 负载的测试服务器时，请考虑添加工作负载，否则它将不会生成有用的报表。 你应该使用与生产类似的工作负载进行测试，以便看到真实的数值和建议的日志大小。 或者，只需在测试期间将一些文件复制到源卷或下载并运行 [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) 以生成写入 IO。 例如，D: 卷的十分钟的较低写入 IO 工作负荷：  
      >
      > `Diskspd.exe -c1g -d600 -W5 -C5 -b8k -t2 -o2 -r -w5 -i100 -j100 d:\test` 

10. 检查图2中所示的**TestSrTopologyReport**报表，以确保满足存储副本要求。  

    ![显示拓扑报告的屏幕](media/Server-to-Server-Storage-Replication/SRTestSRTopologyReport.png)

    **图 2:存储复制拓扑报表**

## <a name="step-3-set-up-server-to-server-replication"></a>步骤 3：设置服务器到服务器复制
### <a name="using-windows-admin-center"></a>使用 Windows 管理中心

1. 添加源服务器。
    1. 选择 "**添加**" 按钮。
    2. 选择 "**添加服务器连接**"。
    3. 键入服务器的名称，然后选择 "**提交**"。
2. 在 "**所有连接**" 页上，选择源服务器。
3. 从 "工具" 面板中选择**存储副本**。
4. 选择 "**新建**" 以创建新的合作关系。
5. 提供合作关系的详细信息，然后选择 "**创建**"。 <br>
   ![新的合作关系屏幕显示了合作关系详细信息，如 8 GB 日志大小。](media/Storage-Replica-UI/Honolulu_SR_Create_Partnership.png)

    **图 3:创建新的合作关系**

> [!NOTE]
> 从 Windows 管理中心的存储副本中删除合作关系不会删除复制组名称。

### <a name="using-windows-powershell"></a>使用 Windows PowerShell

现在可使用 Windows PowerShell 配置服务器到服务器复制。 必须直接在节点上或从包含 Windows Server 远程服务器管理工具的远程管理计算机执行以下所有步骤。  

1. 确保以管理员身份使用提升的 Powershell 控制台。  
2. 配置服务器到服务器复制、指定源和目标磁盘、源和目标日志、源和目标节点以及日志大小。  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv05 -SourceRGName rg01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName sr-srv06 -DestinationRGName rg02 -DestinationVolumeName f: -DestinationLogVolumeName g:  
    ```  

   输出:
   ```PowerShell
   DestinationComputerName : SR-SRV06
   DestinationRGName       : rg02
   SourceComputerName      : SR-SRV05
   PSComputerName          :
   ```

    > [!IMPORTANT]
    > 默认日志大小为 8 GB。 根据 `Test-SRTopology` cmdlet 的结果，可以决定使用具有较高值或较低值的 LogSizeInBytes。  

2.  若要获取复制源和目标状态，请使用 `Get-SRGroup` 和 `Get-SRPartnership`，如下所示：  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```
    输出:

    ```PowerShell
    CurrentLsn             : 0
    DataVolume             : F:\
    LastInSyncTime         :
    LastKnownPrimaryLsn    : 1
    LastOutOfSyncTime      :
    NumOfBytesRecovered    : 37731958784
    NumOfBytesRemaining    : 30851203072
    PartitionId            : c3999f10-dbc9-4a8e-8f9c-dd2ee6ef3e9f
    PartitionSize          : 68583161856
    ReplicationMode        : synchronous
    ReplicationStatus      : InitialBlockCopy
    PSComputerName         :
    ```

3.  确定复制进度，如下所示：  

    1.  在源服务器上，运行以下命令并检查事件 5015、5002、5004、1237、5001 和2200：  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  在目标服务器上，运行以下命令以查看显示合作关系创建的存储副本事件。 此事件会显示复制的字节数和所用的时间。 例如：  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  
        ```
        下面是一些示例输出：
        ```
        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  

        ReplicationGroupName: rg02  
        ReplicationGroupId: {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
        ReplicaName: f:\  
        ReplicaId: {00000000-0000-0000-0000-000000000000}  
        End LSN in bitmap:   
        LogGeneration: {00000000-0000-0000-0000-000000000000}  
        LogFileId: 0  
        CLSFLsn: 0xFFFFFFFF  
        Number of Bytes Recovered: 68583161856  
        Elapsed Time (ms): 117  
        ```  

        > [!NOTE]
        > 存储复制卸除目标卷及其驱动器字母或装入点。 这是设计使然。  

    3.  或者，副本的目标服务器组始终规定要复制的剩余字节数，且可通过 PowerShell 查询。 例如：  

        ```PowerShell  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        作为进度示例（它将不会终止）：  

        ```PowerShell  
        while($true) {  

         $v = (Get-SRGroup -Name "RG02").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

    4.  在目标服务器上，运行以下命令并检查事件 5009、1237、5001、5015、5005 和 2200 以了解处理进度。 在该序列中不应有错误的警告。 将有许多 1237 事件；这些表示进度。  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

## <a name="step-4-manage-replication"></a>步骤 4：管理复制

现在，可以管理并运行从服务器复制到服务器的基础结构。 可以直接在节点上或从包含 Windows Server 远程服务器管理工具的远程管理计算机执行以下所有步骤。  

1.  使用 `Get-SRPartnership` 和 `Get-SRGroup` 确定复制的当前源和目标及其状态。  

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

3.  要从一个站点移动复制方向，请使用 `Set-SRPartnership` cmdlet。  

    ```PowerShell  
    Set-SRPartnership -NewSourceComputerName sr-srv06 -SourceRGName rg02 -DestinationComputerName sr-srv05 -DestinationRGName rg01  
    ```  

    > [!WARNING]  
    > Windows Server 在初始同步正在进行时阻止角色切换，因为如果在允许初始复制完成前尝试进行切换，则会导致数据丢失。 在初始同步完成之前，请不要强制切换方向。  

    检查事件日志以查看复制方向的更改和恢复恢复模式发生，然后进行协调。 写入 IO 然后可以写入到新的源服务器所拥有的存储。 更改复制方向将阻止在以前的源计算机上写入 IO。  

4.  若要删除复制，请在每个节点上使用 `Get-SRGroup`、`Get-SRPartnership`、`Remove-SRGroup` 和 `Remove-SRPartnership`。 确保仅在当前复制源上（而非目标服务器上）运行 `Remove-SRPartnership` cmdlet。 在两个服务器上均运行 `Remove-Group`。 例如，若要从两个服务器删除全部复制：  

    ```powershell
    Get-SRPartnership  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

## <a name="replacing-dfs-replication-with-storage-replica"></a>将 DFS 复制替换为存储副本  
许多 Microsoft 客户将 DFS 复制部署为非结构化用户数据的灾难恢复解决方案，如主文件夹和部门共享。 DFS 复制已在 Windows Server 2003 R2 中以及所有更高版本的操作系统中提供，可在低带宽网络上运行，非常适用于具有多个节点的高延迟和低更改环境。 但是，DFS 复制作为数据复制解决方案具有明显的局限性：  
* 它不复制正在使用的文件或打开的文件。  
* 它不同步复制。  
* 它的异步复制延迟时间可能是数分钟、数小时或者甚至数天。  
* 它依赖数据库（该数据库在电源中断后可能需要进行较长时间的一致性检查）。  
* 它通常配置为多主机，使更改可以在这两个方向上流动，这可能会覆盖较新的数据。  

存储副本没有以上任何这些限制。 但是，它的确有某些限制，使其在一些环境中没有那么大的吸引力：  

* 它只允许卷之间的一对一复制。 可以在多个服务器之间复制不同的卷。  
* 尽管它支持异步复制，但它不是为低带宽、高延迟网络设计的。  
* 当复制正在进行时，它不允许用户访问目标上受保护的数据  

如果没有这些阻止因素，存储复制可以将 DFS 复制服务器替换为较新的技术。   
在较高级别上，此过程：  

1. 在两台服务器上安装 Windows Server 并配置存储。 这可能意味着升级现有的服务器组或干净安装。  
2. 确保想要复制的任何数据在一个或多个数据卷上，而不是在 C: 驱动器上。   
   a.  也可以在其他服务器上生成种子数据以节省时间（使用备份或文件副本，以及使用精简设置的存储。 无需完全匹配类似元数据的安全性（这与 DFS 复制不同）。  
3. 在源服务器上共享数据，并使其可通过 DFS 命名空间访问。 如果服务器名称更改为灾难站点中的名称，请确保用户仍可对其访问，这一点很重要。  
   a.  可以在目标服务器上创建匹配共享（在正常操作中将不可用），   
   b.  不要将目标服务器添加到 DFS 命名空间命名空间，或者如果这样做，请确保已禁用其所有文件夹目标。  
4. 启用存储副本复制并完成初始同步。复制可以为同步或异步。   
   a.  但是，为了保证目标服务器上的 IO 数据一致性，建议同步。   
   b.  我们强烈建议启用卷影副本并使用 VSSADMIN 或其他所选工具定期拍摄快照。 这将保证应用程序将其数据文件一致地转储到磁盘。 如果出现灾难，可以从目标服务器（可能以异步的方式被部分复制）上的快照恢复文件。 快照随文件复制。  
5. 灾难发生前正常运行。  
6. 将目标服务器切换到新的源，这会向用户显示其复制的卷。  
7. 如果使用同步复制，则无需进行数据恢复，除非在源服务器丢失期间，用户使用的是在没有事务保护的情况下写入数据的应用程序（这未考虑复制）。 如果使用异步复制，则更有必要使用 VSS 快照装载，但请考虑在所有应用程序一致的快照中使用 VSS。  
8. 将服务器及其共享添加为 DFS 命名空间文件夹目标。   
9. 然后，用户可以访问其数据。  

   > [!NOTE]
   > 灾难恢复计划是一个复杂的主题，需要极其注意细节。 强烈建议创建 Runbook 并进行年度实时故障转移钻取。 当实际灾难发生时，混乱将占据主导，而有经验的人员可能都很忙碌。  

## <a name="add-azure-vm-expressroute"></a>添加通过 ExpressRoute 连接到网络的 Azure VM

1. [在 Azure 门户中创建 ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager)。<br>在批准 ExpressRoute 后，会向订阅添加一个资源组-导航到 "**资源组**" 以查看此新组。 记下虚拟网络名称。
![显示随 ExpressRoute 一起添加的资源组的 Azure 门户](media/Server-to-Server-Storage-Replication/express-route-resource-group.png)
    
    **图4：与 ExpressRoute 关联的资源-记下虚拟网络名称**
1. [创建新的资源组](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)。
1. [添加网络安全组](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)。 创建它时，选择与你创建的 ExpressRoute 关联的订阅 ID，并选择你刚创建的资源组。
<br><br>将所需的任何入站和出站安全规则添加到网络安全组。 例如，你可能想要允许远程桌面访问 VM。
1. 使用以下设置[创建 AZURE VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) （如图5所示）：
    - **公共 IP 地址**：无
    - **虚拟网络**:从添加了 ExpressRoute 的资源组中选择记下的虚拟网络。
    - **网络安全组（防火墙）** ：选择之前创建的网络安全组。
    ![创建显示 ExpressRoute 网络设置](media/Server-to-Server-Storage-Replication/azure-vm-express-route.png)
    **的虚拟机图5：选择 ExpressRoute 网络设置时创建 VM**
1. 创建 VM 后，请参阅[步骤2：设置操作系统、功能、角色、存储和网络](#provision-os)。


## <a name="related-topics"></a>相关主题  
- [存储副本概述](storage-replica-overview.md)  
- [使用共享存储拉伸群集复制](stretch-cluster-replication-using-shared-storage.md)  
- [群集到群集存储复制](cluster-to-cluster-storage-replication.md)
- [存储副本：已知问题](storage-replica-known-issues.md)  
- [存储副本：常见问题解答](storage-replica-frequently-asked-questions.md)
- [Windows Server 2016 中的存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)  
