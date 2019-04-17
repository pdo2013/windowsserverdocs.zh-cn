---
title: "群集到群集存储复制"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
ms.assetid: 834e8542-a67a-4ba0-9841-8a57727ef876
author: nedpyle
ms.date: 10/11/2016
description: "如何通过运行 Windows Server 2016 Datacenter Edition，使用存储副本将一个群集中的卷复制到另一个群集。"
ms.openlocfilehash: 0681f6495390fda9b0c4564bf5516a48652b2c71
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-to-cluster-storage-replication"></a>群集到群集存储复制

> 适用于：Windows Server（半年频道）、Windows Server 2016

Windows Server 2016 Datacenter Edition 现在提供群集到群集复制，包括使用存储空间直通（即，不进行共享、直接附加的存储）复制群集。 管理和配置与服务器到服务器复制类似。  

将在群集到群集配置中配置这些计算机和存储，其中一个群集使用另一群集及其存储组来复制其自身的存储组。 这些节点及其存储应位于单独的物理站点（尽管这不是必需的）。  

Windows Server 2016 Datacenter Edition 中没有可以为群集到群集复制配置存储副本的图形工具，但 Azure Site Recovery 可以在以后配置此方案。

> [!IMPORTANT]
> 在此测试中，四台服务器是示例。 你可以在每个群集中使用任意数量且受 Microsoft 支持的服务器，当前存储空间直通群集使用了 16 个服务器，共享的存储群集使用了 64 个服务器。  
>   
> 本指南不涉及配置存储空间直通。 有关配置存储空间直通的详细信息，请参阅 [Windows Server 2016 中的存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)。  

此演练使用以下环境作为示例：  

-   名为 **SR SRV01** 和 **SR SRV02** 的两个成员服务器稍后会形成一个名为 **SR SRVCLUSA** 的群集。  

-   名为 **SR SRV03** 和 **SR-SRV04** 的两个成员服务器稍后会形成一个名为 **SR-SRVCLUSB** 的群集。  

-   表示两个不同数据中心的一对逻辑“站点”，一个名为 **Redmond**，另一个名为 **Bellevue**。  

![通过使用 Redmond 站点中的群集复制 Bellevue 站点中的群集显示示例环境的关系图](./media/Cluster-to-Cluster-Storage-Replication/SR_ClustertoCluster.png)  

**图 1：群集到群集复制**  

## <a name="prerequisites"></a>必备条件  

* Active Directory 域服务林（无需运行 Windows Server 2016）。  
* 至少有四台服务器（两个群集中各两台服务器）安装了 Windows Server 2016 Datacenter Edition。 支持最多 2 个 64 节点群集。  
* 两组共享存储，使用 SAS JBOD、光纤通道 SAN、共享 VHDX、存储空间直通或 iSCSI 目标。 存储需包含 HDD 和 SSD 媒体的组合。 将每个存储组设置为仅对每个群集可用（群集间没有共享访问）。  
* 每组存储必须允许至少创建两个虚拟磁盘，一个用于复制的数据，而另一个用于日志。 物理存储在所有数据磁盘上的扇区大小必须相同。 物理存储在所有日志磁盘上的扇区大小必须相同。  
* 每个服务器上必须具有至少一个用于同步复制的以太网/TCP 连接，但最好是 RDMA。   
* 合适的防火墙和路由器规则，以允许所有节点之间的 ICMP、SMB（端口 445 以及用于 SMB 直通的 5445）和 WS-MAN（端口 5985）双向通信。  
* 服务器间的网络具有足够的带宽，以包含 IO 写入工作负载和平均值为 5 毫秒的往返行程延迟（对于同步复制）。 异步复制没有延迟建议。  
* 复制的存储不能位于包含 Windows 操作系统文件夹的驱动器上。

许多这些要求都可通过使用 `Test-SRTopology` cmdlet 来确定。 如果将存储副本或存储副本管理工具功能安装在至少一台服务器上，则会获取此工具的访问权限。 无需将存储副本配置为使用此工具，只需安装 cmdlet。 以下步骤中包含更多信息。  

## <a name="step-1-provision-operating-system-features-roles-storage-and-network"></a>步骤 1：预配操作系统、功能、角色、存储和网络

1.  使用 Windows Server 2016 Datacenter**（桌面体验）**的安装类型在四个服务器节点上均安装 Windows Server 2016。 不要选择标准版（如果可用），因为它不包含存储副本。  

2.  添加网络信息并将其添加到域，然后对其重启。  

    > [!IMPORTANT]  
    > 从现在开始，始终以域用户（所有服务器上的内置管理员组的成员）身份登录。 在图形服务器安装或在 Windows 10 计算机上运行时，请始终提升你的 Windows PowerShell 和 CMD 命令提示符。  

3.  将第一组 JBOD 存储机箱、iSCSI 目标、FC SAN 或服务器的本地固定磁盘 (DAS) 存储连接到站点 **Redmond**.中的服务器。  

4.  将第二组存储连接到站点 **Bellevue**.中的服务器。  

5.  根据需要，在所有四个节点上均安装最新的供应商存储和机箱固件及驱动程序、最新的供应商 HBA 驱动程序、最新的供应商 BIOS/UEFI 固件、最新的供应商网络驱动程序和最新的母板芯片组驱动程序。 根据需要重启节点。  

    > [!NOTE]  
    > 请查看配置共享存储和网络硬件的硬件供应商文档。  

6.  确保服务器的 BIOS/UEFI 设置启用高性能，例如禁用 C-State、设置 QPI 速度、启用 NUMA 和设置最高内存频率。 确保 Windows Server 中的电源管理设置为高性能。 根据需要重启。  

7.  配置角色，如下所示：  

    -   **图形方法**  

        1.  运行 **ServerManager.exe** 并创建服务器组，添加所有服务器节点。  

        2.  在每个节点上安装**文件服务器**和**存储副本**角色和功能，并对其重启。  

    -   **Windows PowerShell 方法**  

        在 SR-SRV04 或远程管理计算机上，在 Windows PowerShell 控制台中运行以下命令以在四个节点上为拉伸群集安装所需功能和角色，并对其重启：  

        ```PowerShell
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        有关这些步骤的详细信息，请参阅 [安装或卸载角色、角色服务或功能](http://technet.microsoft.com/library/hh831809.aspx)  

9. 配置存储，如下所示：  

    > [!IMPORTANT]  
    > -   必须在每个机箱上创建两个卷：一个用于数据，另一个用于日志。  
    > -   必须将日志和数据磁盘初始化为 **GPT**，而非 **MBR**。  
    > -   两个数据卷的大小必须相同。  
    > -   两个日志卷的大小应相同。  
    > -   所有复制的数据磁盘的扇区大小必须相同。  
    > -   所有日志磁盘的扇区大小必须相同。  
    > -   日志卷应使用基于闪存的存储，如 SSD。  Microsoft 建议，日志存储应比数据存储速度快。 日志卷不得用于其他工作负荷。
    > -   数据磁盘可使用 HDD、SSD 或分层组合，并可使用镜像或奇偶校验空间或 RAID 1 或 10，或者使用 RAID 5 或 RAID 50。  
    > -   默认情况下，日志卷必须至少为 9 GB，但可以根据日志需求设置为更大或更小。  

    -   **对于 JBOD 存储设备：**  

        1.  确保每个群集只能看到该站点的存储机箱，且 SAS 连接已正确配置。  

        2.  使用 Windows PowerShell 或服务器管理器，按照 [在独立服务器上部署存储空间](http://technet.microsoft.com/library/jj822938.aspx) 中提供的**步骤 1 至 3** 使用存储空间来配置存储。  

    -   **对于 iSCSI 目标存储：**  

        1.  确保每个群集都只能看到该站点的存储机箱。 如果使用 iSCSI，则应使用多个网络适配器。  

        2.  使用供应商文档配置存储。 如果使用基于 Windows 的 iSCSI 目标，请查阅 [iSCSI 目标块存储方法](http://technet.microsoft.com/library/hh848268.aspx)。  

    -   **对于 FC SAN 存储：**  

        1.  确保每个群集只能看到该站点的存储机箱，且对主机进行了正确的分区。  

        2.  使用供应商文档配置存储。  

    -   **对于存储空间直通：**  

        1.  通过部署存储空间直通，确保每个群集只能看到该站点的存储机箱。 (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct) 

        2.  确保 SR 日志卷将始终位于速度最快的闪存存储上，而数据卷位于速度较慢的高容量存储上。

10. 启动 Windows PowerShell，并使用 `Test-SRTopology` cmdlet，以确定是否满足存储副本的所有要求。 可以在仅要求模式下使用 cmdlet 以用于快速测试，也可以在长时间运行的性能评估模式下使用。  
例如，  

   ```PowerShell
   MD c:\temp

   Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV03 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp        
   ```

      > [!IMPORTANT]
      > 在评估期间，当在指定源卷上使用无写入 IO 负载的测试服务器时，请考虑添加工作负载，否则它将不会生成有用的报表。 你应该使用与生产类似的工作负载进行测试，以便看到真实的数值和建议的日志大小。 或者，只需在测试期间将一些文件复制到源卷或下载并运行 [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) 以生成写入 IO。 例如，D: 卷的五分钟的较低写入 IO 工作负载：  
      > `Diskspd.exe -c1g -d300 -W5 -C5 -b8k -t2 -o2 -r -w5 -h d:\test.dat`  

11. 检查 **TestSrTopologyReport.html** 报表以确保符合存储副本要求。  

    ![显示复制拓扑报告结果的屏幕](./media/Cluster-to-Cluster-Storage-Replication/SRTestSRTopologyReport.png)      

## <a name="step-2-configure-two-scale-out-file-server-failover-clusters"></a>步骤 2：配置两个横向扩展文件服务器故障转移群集  
现在可以创建两个常规故障转移群集。 配置、验证和测试完成后，将使用存储副本对其复制。 可以直接在群集节点上或从包含 Windows Server 2016 RSAT 管理工具的远程管理计算机执行以下所有步骤。  

### <a name="graphical-method"></a>图形方法  

1.  在每个站点中对节点运行 **cluadmin.msc**。  

2.  验证计划群集并分析结果以确保可以继续。 以下使用的示例是 **SR SRVCLUSA** 和 **SR SRVCLUSB**。  

3.  创建两个群集。 确保群集名称为 15 个字符或更少。  

4.  配置文件共享见证或云见证。  

    > [!NOTE]  
    > Windows Server 2016 现在包含基于云 (Azure) 的见证的选项。 你可以选择此仲裁选项来替代文件共享见证。  

    > [!WARNING]  
    > 有关仲裁配置的详细信息，请参阅 [在 Windows Server 2012 故障转移群集中配置和管理仲裁](http://technet.microsoft.com/library/jj612870.aspx) 中的**见证配置**部分。 有关 `Set-ClusterQuorum` cmdlet 的详细信息，请参阅 [Set-ClusterQuorum](http://technet.microsoft.com/library/hh847275.aspx)。  

5.  将 **Redmond** 站点中的一个磁盘添加到群集 CSV。 若要执行此操作，右键单击“**存储**”部分的“**磁盘**”节点中的源磁盘，然后单击“**添加到群集共享卷**”。  

6.  使用[配置横向扩展文件服务器](http://technet.microsoft.com/library/hh831718.aspx) 中的说明在两个群集上均创建群集横向扩展文件服务器  

### <a name="windows-powershell-method"></a>Windows PowerShell 方法  

1.  测试计划群集并分析结果以确保可以继续：  

    ```PowerShell
    Test-Cluster SR-SRV01,SR-SRV02  
    Test-Cluster SR-SRV03,SR-SRV04  
    ```  

2.  创建群集（必须为群集指定你的专用静态 IP 地址）。 确保每个群集名称为 15 个字符或更少：  

    ```PowerShell
    New-Cluster -Name SR-SRVCLUSA -Node SR-SRV01,SR-SRV02 -StaticAddress <your IP here>  
    New-Cluster -Name SR-SRVCLUSB -Node SR-SRV03,SR-SRV04 -StaticAddress <your IP here>  
    ```  

3.  在指向托管在域控制器或某些其他独立服务器上的共享的每个群集中配置文件共享见证或云 (Azure) 见证。 例如：  

    ```PowerShell  
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare  
    ```  

    > [!NOTE]  
    > Windows Server 2016 现在包含基于云 (Azure) 的见证选项。 你可以选择此仲裁选项来替代文件共享见证。  

    > [!WARNING]  
    > 有关仲裁配置的详细信息，请参阅 [在 Windows Server 2012 故障转移群集中配置和管理仲裁](http://technet.microsoft.com/library/jj612870.aspx) 中的**见证配置**部分。 有关 `Set-ClusterQuorum` cmdlet 的详细信息，请参阅 [Set-ClusterQuorum](http://technet.microsoft.com/library/hh847275.aspx)。  

4.  使用 [配置横向扩展文件服务器](https://technet.microsoft.com/library/hh831718.aspx) 中的说明在两个群集上均创建群集横向扩展文件服务器  

## <a name="step-3-set-up-cluster-to-cluster-replication-using-windows-powershell"></a>步骤 3：使用 Windows PowerShell 设置群集到群集复制  
现在使用 Windows PowerShell 设置群集到群集复制。 可以直接在节点上或从包含 Windows Server 2016 RSAT 管理工具的远程管理计算机执行以下所有步骤  

1.  通过在第一个群集中的任何节点上运行 **Grant-ClusterAccess** cmdlet 授予第一个群集对其他群集的完全访问权限（或远程授予权限）。  

    ```PowerShell
    Grant-SRAccess -ComputerName SR-SRV01 -Cluster SR-SRVCLUSB  
    ```  

2.  通过在第二个群集中的任何节点上运行 **Grant-ClusterAccess** cmdlet 授予第二个群集对其他群集的完全访问权限（或远程授予权限）。  

    ```PowerShell
    Grant-SRAccess -ComputerName SR-SRV03 -Cluster SR-SRVCLUSA  
    ```  

3.  配置群集到群集复制、指定源和目标磁盘、源和目标日志、源和目标群集名称以及日志大小。 可以在服务器上本地执行此命令，也可以使用远程管理计算机执行。  

    ```powershell  
    New-SRPartnership -SourceComputerName SR-SRVCLUSA -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\Volume2 -SourceLogVolumeName f: -DestinationComputerName SR-SRVCLUSB -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\Volume2 -DestinationLogVolumeName f:  
    ```  

    > [!WARNING]  
    > 默认日志大小为 8 GB。 根据 **Test-SRTopology** cmdlet 的结果，可以决定使用具有较高值或较低值的 **LogSizeInBytes**。  

4.  若要获取复制源和目标状态，请使用 **Get-SRGroup** 和 **Get-SRPartnership**，如下所示：  

    ```powershell
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```  

5.  确定复制进度，如下所示：  

    1.  在源服务器上，运行以下命令并检查事件 5015、5002、5004、1237、5001 和2200：
        
        ```PowerShell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20
        ```
    2.  在目标服务器上，运行以下命令以查看显示合作关系创建的存储副本事件。 此事件会显示复制的字节数和所用的时间。 例如：  
        
        ```powershell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | Format-List
        ```
        下面是输出示例：
        
        ```
        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  
            ReplicationGroupName: rg02  
            ReplicationGroupId:  
            {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
            ReplicaName: f:\  
            ReplicaId: {00000000-0000-0000-0000-000000000000}  
            End LSN in bitmap:  
            LogGeneration: {00000000-0000-0000-0000-000000000000}  
            LogFileId: 0  
            CLSFLsn: 0xFFFFFFFF  
            Number of Bytes Recovered: 68583161856  
            Elapsed Time (seconds): 117  
        ```
    3. 或者，副本的目标服务器组规定要复制的剩余字节数，且可通过 PowerShell 查询。 例如：

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

6. 在目标群集中的目标服务器上，运行以下命令并检查事件 5009、1237、5001、5015、5005 和 2200 以了解处理进度。 在该序列中不应有错误的警告。 将有许多 1237 事件；这些表示进度。  
    
   ```PowerShell
   Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
   ```
   > [!NOTE]  
        > 在复制时，目标群集磁盘将始终显示为**联机（无访问权限）**。  

## <a name="step-4-manage-replication"></a>步骤 4：管理复制

现在将管理并操作群集到群集复制。 可以直接在群集节点上或从包含 Windows Server 2016 RSAT 管理工具的远程管理计算机执行以下所有步骤。  

1.  使用 **Get-ClusterGroup** 或**故障转移群集管理器**确定复制的当前源和目标及其状态。  

2.  若要测量复制性能，请在源和目标节点上均使用 **Get-Counter** cmdlet。 计数器名称为：  

    -   \Storage Replica Partition I/O Statistics(*)\Number of times flush paused  

    -   \Storage Replica Partition I/O Statistics(*)\Number of pending flush I/O  

    -   \Storage Replica Partition I/O Statistics(*)\Number of requests for last log write  

    -   \Storage Replica Partition I/O Statistics(*)\Avg. Flush Queue Length  

    -   \Storage Replica Partition I/O Statistics(*)\Current Flush Queue Length  

    -   \Storage Replica Partition I/O Statistics(*)\Number of Application Write Requests  

    -   \Storage Replica Partition I/O Statistics(*)\Avg. Number of requests per log write  

    -   \Storage Replica Partition I/O Statistics(*)\Avg. App Write Latency  

    -   \Storage Replica Partition I/O Statistics(*)\Avg. App Read Latency  

    -   \Storage Replica Statistics(*)\Target RPO  

    -   \Storage Replica Statistics(*)\Current RPO  

    -   \Storage Replica Statistics(*)\Avg. Log Queue Length  

    -   \Storage Replica Statistics(*)\Current Log Queue Length  

    -   \Storage Replica Statistics(*)\Total Bytes Received  

    -   \Storage Replica Statistics(*)\Total Bytes Sent  

    -   \Storage Replica Statistics(*)\Avg. Network Send Latency  

    -   \Storage Replica Statistics(*)\Replication State  

    -   \Storage Replica Statistics(*)\Avg. Message Round Trip Latency  

    -   \Storage Replica Statistics(*)\Last Recovery Elapsed Time  

    -   \Storage Replica Statistics(*)\Number of Flushed Recovery Transactions  

    -   \Storage Replica Statistics(*)\Number of Recovery Transactions  

    -   \Storage Replica Statistics(*)\Number of Flushed Replication Transactions  

    -   \Storage Replica Statistics(*)\Number of Replication Transactions  

    -   \Storage Replica Statistics(*)\Max Log Sequence Number  

    -   \Storage Replica Statistics(*)\Number of Messages Received  

    -   \Storage Replica Statistics(*)\Number of Messages Sent  

    有关 Windows PowerShell 中的性能计数器的详细信息，请参阅 [Get-Counter](https://technet.microsoft.com/library/hh849685.aspx)。  

3.  要从一个站点移动复制方向，请使用 **Set-SRPartnership** cmdlet。  

    ```PowerShell
    Set-SRPartnership -NewSourceComputerName SR-SRVCLUSB -SourceRGName rg02 -DestinationComputerName SR-SRVCLUSA -DestinationRGName rg01  
    ```  

    > [!NOTE]  
    > Windows Server 2016 会在初始同步进行时阻止角色切换，如果在允许初始复制完成前尝试进行切换，则会导致数据丢失。 在初始同步完成前不要强制切换方向。

    检查事件日志以查看复制方向的更改和恢复恢复模式发生，然后进行协调。 写入 IO 然后可以写入到新的源服务器所拥有的存储。 更改复制方向将阻止在以前的源计算机上写入 IO。  

    > [!NOTE]  
    > 在复制时，目标群集磁盘将始终显示为**联机（无访问权限）**。  

4.  若要更改 Windows Server 2016 中默认的 8 GB 日志大小，请在源和目标存储副本组上均使用 **Set-SRGroup**。  

    > [!IMPORTANT]  
    > 默认日志大小为 8 GB。 根据 **Test-SRTopology** cmdlet 的结果，可以决定使用具有较高值或较低值的 LogSizeInBytes。  

5.  若要删除复制，请在每个群集上使用 **Get-SRGroup**、**Get-SRPartnership**、**Remove-SRGroup** 和 **Remove-SRPartnership**。  

    ```powershell
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

    > [!NOTE]  
    > 存储副本用于卸除目标卷。 这是设计使然。

## <a name="see-also"></a>另请参阅

-   [存储副本概述](storage-replica-overview.md) 
-   [使用共享存储拉伸群集复制](stretch-cluster-replication-using-shared-storage.md)  
-   [服务器到服务器存储复制](server-to-server-storage-replication.md)  
-   [存储副本：已知问题](storage-replica-known-issues.md)  
-   [存储副本：常见问题](storage-replica-frequently-asked-questions.md)  
-   [Windows Server 2016 中的存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)  
