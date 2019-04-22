---
title: 存储空间直通的故障排除
description: 了解如何排查存储空间直通部署。
keywords: 存储空间
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: ecf3cb5703a90976dce15abbd0c9fdd1d4aa24ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812628"
---
# <a name="troubleshoot-storage-spaces-direct"></a>排查存储空间直通

使用以下信息来排查存储空间直通部署。

一般情况下，使用以下步骤启动：

1. 确认 SSD 制造商/型号经过认证的 Windows Server 2016 使用 Windows Server 目录。 与供应商确认存储空间直通支持的驱动器。
2. 检查任何故障的驱动器的存储。 使用存储管理软件检查驱动器的状态。 如果任意驱动器发生故障，请与供应商联系。 
3. 更新存储和驱动器固件，如有必要。
   请确保在所有节点上安装最新的 Windows 更新。 你可以获取最新的更新从 Windows Server 2016 [ https://aka.ms/update2016 ](https://aka.ms/update2016)。
4. 网络适配器驱动程序和固件更新。
5. 运行群集验证和查看存储空间直通部分，确保正确地报告将用于缓存的驱动器和没有错误。

如果仍遇到问题，请查看下面的方案。

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>虚拟磁盘资源处于无冗余状态
节点的存储空间直通系统意外重新启动出现崩溃或电源故障。 然后，在一个或多个虚拟磁盘可能未进入联机状态，请参阅说明"没有足够冗余信息。"
    
|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|大小| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| 确定|  正常| True|  10 TB|  节点 01.conto...|
|Disk3         |Mirror                 |确定                          |正常       |True            |10 TB | 节点 01.conto...|
|Disk2         |Mirror                 |未冗余               |Unhealthy     |True            |10 TB | 节点 01.conto...|
|Disk1         |Mirror                 |{无冗余，维护}  |Unhealthy     |True            |10 TB | 节点 01.conto...| 

此外之后尝试使虚拟磁盘，以下信息是群集日志 (DiskRecoveryAction) 中记录。  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

**否冗余操作状态**的磁盘故障，或者如果系统无法访问虚拟磁盘上的数据可能会出现。 如果在节点上维护期间，在节点上进行重启，则可能出现此问题。
    
若要解决此问题，请按照下列步骤：

1. 从 CSV 删除受影响的虚拟磁盘。 这会将其放在群集中的"Available storage"组，并开始显示为"物理磁盘。"ResourceType

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 在节点上拥有可用存储组，处于无冗余状态的每个磁盘上运行以下命令。 若要标识哪一节点"Available Storage"组是你可以运行以下命令。

   ```powershell
   Get-ClusterGroup
   ```
3. 设置磁盘恢复操作，然后启动磁盘。
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. 修复应自动启动。 等待修复完成。 它可能会进入挂起状态并重新启动。 若要监视进度： 
    - 运行**Get-storagejob**监视修复的状态以及查看完成的。
    - 运行**Get-virtualdisk**并验证是否在空间返回 HealthStatus 的正常运行。
5. 在修复后完成和虚拟磁盘是正常、 更改虚拟磁盘参数返回。

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. 执行脱机然后再联机，再次能够生效 DiskRecoveryAction 的磁盘：

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. 受影响的虚拟磁盘将重新添加到 CSV。

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction**是允许将附加中无需进行任何检查的读写模式下的空间量的重写开关。 该属性，可执行诊断操作到为什么卷不会进入联机状态。 为维护模式非常类似，但你可以处于失败状态的资源上调用它。 它还允许您访问数据，这非常有用，例如"无冗余"中获取访问权限的情况下对任何数据可以并将其复制。 在 2018 年 2 月 22，，更新，KB 4077525 中添加了 DiskRecoveryAction 属性。
    
    
## <a name="detached-status-in-a-cluster"></a>在群集中的已分离的状态 

在运行时**Get-virtualdisk** cmdlet，OperationalStatus 为一个或多个存储空间直通分离虚拟磁盘。 但是，报告的 HealthStatus **Get-physicaldisk** cmdlet 指示所需的所有物理磁盘都处于正常状态。

下面是示例的输出**Get-virtualdisk** cmdlet。

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  大小|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 确定|                  正常|       True|            10 TB|  节点 01.conto...|
|Disk3|         Mirror|                 确定|                  正常|       True|            10 TB|  节点 01.conto...|
|Disk2|         Mirror|                 Detached|            Unknown|       True|            10 TB|  节点 01.conto...|
|Disk1|         Mirror|                 Detached|            Unknown|       True|            10 TB|  节点 01.conto...| 


此外，可能会在节点上记录以下事件：

```
Log Name: Microsoft-Windows-StorageSpaces-Driver/Operational
Source: Microsoft-Windows-StorageSpaces-Driver 
Event ID: 311 
Level: Error
User: SYSTEM 
Computer: Node#.contoso.local 
Description: Virtual disk {GUID} requires a data integrity scan.  

Data on the disk is out-of-sync and a data integrity scan is required. 

To start the scan, run the following command:   
Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask                
             
Once you have resolved the condition listed above, you can online the disk by using the following commands in PowerShell:   

Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsReadOnly $false 
Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsOffline  $false
------------------------------------------------------------

Log Name: System
Source: Microsoft-Windows-ReFS 
Event ID: 134
Level: Error 
User: SYSTEM
Computer: Node#.contoso.local 
Description: The file system was unable to write metadata to the media backing volume <VolumeId>. A write failed with status "A device which does not exist was specified." ReFS will take the volume offline. It may be mounted again automatically.
------------------------------------------------------------
Log Name: Microsoft-Windows-ReFS/Operational
Source: Microsoft-Windows-ReFS 
Event ID: 5 
Level: Error 
User: SYSTEM 
Computer: Node#.contoso.local 
Description: ReFS failed to mount the volume. 
Context: 0xffffbb89f53f4180 
Error: A device which does not exist was specified.
Volume GUID:{00000000-0000-0000-0000-000000000000} 
DeviceName: 
Volume Name:
``` 

**分离操作状态**下可能发生脏区域跟踪 (DRT) 日志已满。 存储空间使用脏区域跟踪 (DRT) 为镜像空间确保出现电源故障时，对元数据的任何正在进行更新会记录来确保可以重做或撤消操作以将存储空间恢复到一个灵活的存储空间和电源恢复和系统重新启动后时的一致状态。 DRT 日志已满时，如果虚拟磁盘不能使联机直到同步并刷新 DRT 元数据。 此过程需要运行完全扫描，这可能需要几个小时才能完成。
    
若要解决此问题，请按照下列步骤：
1. 从 CSV 删除受影响的虚拟磁盘。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 在未联机的每个磁盘上运行以下命令。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. 在其中已分离的卷处于联机状态的每个节点上运行以下命令。 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   应在其分离的卷处于联机状态的所有节点上启动此任务。 修复应自动启动。 等待修复完成。 它可能会进入挂起状态并重新启动。 若要监视进度： 
   - 运行**Get-storagejob**监视修复的状态以及查看完成的。
   -  运行**Get-virtualdisk**并验证是否在空间返回 HealthStatus 的正常运行。
    - "数据完整性扫描有关崩溃恢复"任务不会显示为存储作业，并且没有任何进度指示器。 如果任务显示为正在运行，它正在运行。 操作完成时，它将显示已完成。
    
       此外，您可以通过使用以下 cmdlet 查看正在运行的计划任务的状态： 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. 只要完成"数据完整性扫描有关崩溃恢复"，修复完成和虚拟磁盘都是正常、 更改虚拟磁盘参数返回。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. 受影响的虚拟磁盘将重新添加到 CSV。    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
**DiskRunChkdsk 值 7**用于附加空间卷，并在只读模式下具有的分区。 这样，空格自己发现和自行修复触发修复。 修复将运行后会自动装载。 它还允许您访问数据，这非常有用，若要获取对你可以复制任何数据的访问。 对于某些错误条件，如 DRT 日志已满，您需要运行故障恢复计划任务的数据完整性扫描。
    
**崩溃恢复任务的数据完整性扫描**用于同步和清除完整脏区域跟踪 (DRT) 日志。 此任务将花费几个小时才能完成。 "数据完整性扫描有关崩溃恢复"任务不会显示为存储作业，并且没有任何进度指示器。 如果任务显示为正在运行，它正在运行。 操作完成时，它将显示为已完成。 如果取消该任务或重新启动一个节点运行此任务时，该任务需要从头重新开始。

有关详细信息，请参阅[故障排除存储空间直通的运行状况和操作状态](storage-spaces-states.md)。
    
## <a name="event-5120-with-statusiotimeout-c00000b5"></a>STATUS_IO_TIMEOUT c00000b5 5120 事件 

>[!重要} 若要减少应用使用修补程序更新时遇到以下症状的可能性，建议使用下面的过程存储维护模式下安装[2018 年 10 月 18 日，Windows Server 2016 的累积更新](https://support.microsoft.com/help/4462928)或更高版本时节点当前已安装已在中发布的 Windows Server 2016 累积更新[2018 年 5 月 8](https://support.microsoft.com/help/4103723)到[2018 年 10 月 9 日](https://support.microsoft.com/help/KB4462917)。

重新启动具有累积更新从发布的 Windows Server 2016 上的节点后，可能会收到事件 5120 STATUS_IO_TIMEOUT c00000b5 [KB 4103723 2018 年 5 月 8 日](https://support.microsoft.com/help/4103723)到[KB 44629172018年10月9日](https://support.microsoft.com/help/4462917)安装。

重新启动节点时，事件 5120 系统事件日志中记录，包括以下的错误代码之一：

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

记录事件 5120 时，会生成实时转储以收集调试信息可能会导致其他症状或产生性能影响。 生成实时转储创建短暂暂停以便拍摄快照的内存来写入转储文件。 具有大量内存，并且是在高负荷下的系统可能会导致节点超出群集成员身份删除，并还会导致以下的事件 1135，要记入日志。

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

存储空间直通群集内部的 SMB 网络会话添加 SMB 弹性处理 8，2018 年 5 月累积更新中引入了更改。 这样做是为了提高暂时网络故障的复原能力并提高 RoCE 如何处理网络拥塞。

重新启动节点时，这些改进会无意中增加 SMB 连接尝试重新连接时的超时和超时等待。 这些问题可能会影响系统的压力。 在计划外停机期间的最多 60 秒的 IO 暂停还观察到系统等待超时的连接时。

若要解决此问题，请安装[2018 年 10 月 18 日，Windows Server 2016 的累积更新](https://support.microsoft.com/help/4462928)或更高版本。

*请注意*此更新将对齐 CSV 超时值具有 SMB 连接超时值，若要解决此问题。 它不实现更改后，若要禁用的解决方法部分中所述的实时转储生成。
    
### <a name="shutdown-process-flow"></a>关闭处理流程：

1. 运行 Get-virtualdisk cmdlet，并确保 HealthStatus 值正常。
2. 通过运行以下 cmdlet 来排出该节点：

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. 通过运行以下 cmdlet 将磁盘置于存储维护模式中的该节点： 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. 运行**Get-physicaldisk** cmdlet，并确保 OperationalStatus 值处于维护模式。
5. 运行**重新启动计算机**cmdlet 来重新启动节点。
6. 节点重新启动后，请删除该节点上的磁盘存储维护模式通过运行以下 cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. 通过运行以下 cmdlet 恢复该节点：

   ```powershell
   Resume-ClusterNode
   ```
8. 运行以下 cmdlet 检查重新同步作业的状态：

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>禁用实时转储
若要缓解实时转储生成具有大量内存，并且是在高负荷下的系统上的效果，可能会另外要禁用实时转储生成。 下面提供了三个选项。

>[!Caution]
>此过程可能会阻止 Microsoft 支持团队可能需要调查此问题的诊断信息的集合。 支持代理可能需要要求你重新启用实时转储生成基于特定的故障排除方案。

有两种方法来禁用实时转储，如下所述。

#### <a name="method-1-recommended-in-this-scenario"></a>方法 1 （建议在此方案中）
若要完全禁用所有转储，包括实时转储整个系统，执行以下步骤：

1. 创建以下注册表项：HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. 在新下**ForceDumpsDisabled**密钥、 创建作为 GuardedHost，REG_DWORD 属性，然后将其值设置为 0x10000000 处。
3. 适用于每个群集节点的新注册表项。

>[!NOTE]
>您必须重新启动计算机，以 nregistry 更改才会生效。

此注册表项设置实时后，创建转储将失败，并且生成"STATUS_NOT_SUPPORTED"错误。

#### <a name="method-2"></a>Method 2
默认情况下，Windows 错误报告将允许每个每 7 天的报表类型只有一个 LiveDump 和只有 1 LiveDump 每台计算机每 5 天。 您可以通过设置以下的注册表项，以仅允许在计算机上一个 LiveDump，永久更改的。
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*请注意*必须重新启动计算机以使更改生效。

### <a name="method-3"></a>方法 3
若要禁用的 （例如当记录事件 5120） 实时转储生成群集，请运行以下 cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
此 cmdlet 会立即影响而无需重新启动计算机的所有群集节点上。
    
## <a name="slow-io-performance"></a>IO 性能缓慢

如果你看到的 IO 性能降低，检查是否缓存已启用存储空间直通配置中。 

有两种方法来检查： 
     
 
1. 使用群集日志。 所选文本编辑器中打开群集日志并搜索"[=== SBL 磁盘 ===]。" 这将在生成日志的节点上的磁盘的列表。 

     启用缓存的磁盘的示例：请注意此处的状态是 CacheDiskStateInitializedAndBound，此处没有存在的 GUID。 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    缓存未启用：这里我们可以看到存在没有 guid 和状态是 CacheDiskStateNonHybrid。 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    缓存未启用：默认情况下不启用的所有磁盘都时的相同类型的大小写。 这里我们可以看到存在没有 guid 和状态是 CacheDiskStateIneligibleDataPartition。 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. 使用 Get-PhysicalDisk.xml 从 SDDCDiagnosticInfo
    1. 打开 XML 文件使用"$d = 导入 Clixml GetPhysicalDisk.XML"
    2. 运行"ipmo 存储"
    3. 运行"$d"。 请注意，使用情况自动选择，不日志是你将看到如下输出： 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| 用法| 大小|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   确定|                正常|      自动选择 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    确定|                正常| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  确定|                正常| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| 确定|    正常| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|确定|       正常| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| 确定|      正常| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| 确定|      正常|自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| 确定|  正常| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| 确定| 正常| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| 确定|正常| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| 确定| 正常| 自动选择 |1.82 TB|
    
## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>如何破坏现有的群集，以便可以再次使用相同的磁盘

在存储空间直通群集中，一次将禁用存储空间直通和使用中所述的清理过程[清洗驱动器](deploy-storage-spaces-direct.md#step-31-clean-drives)，群集的存储池仍处于脱机状态，并从删除运行状况服务群集。

下一步是删除虚拟存储池： 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

现在，如果你运行**Get-physicaldisk**在任何节点，你将看到已在池中的所有磁盘。 例如，在使用 4 节点的群集使用 4 个 SAS 磁盘，100 GB，每个提供给每个节点的实验室。 在这种情况下，禁用存储空间直通后，删除 SBL （存储总线层） 但离开筛选器中，如果在运行**Get-physicaldisk**，它应该报告中排除本地 OS 磁盘的 4 个磁盘。 而是它将改为报告 16。 这是相同的群集中的所有节点。 在运行时**Get-disk**命令中，您将看到本地附加的磁盘编号为 0、 1、 2 等，如以下示例输出所示：

|编号| 友好名称| 序列号|HealthStatus|OperationalStatus|总大小| 分区形式|
|-|-|-|-|-|-|-|-|
|0|Msft 虚拟...  ||正常 | 联机|  127 GB| GPT|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
|1|Msft 虚拟...||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
|2|Msft 虚拟...||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
|4|Msft 虚拟...||正常| 脱机| 100 GB| RAW|
|3|Msft 虚拟...||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|
||Msft 虚拟... ||正常| 脱机| 100 GB| RAW|

    
## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>有关"不受支持的媒体类型"错误消息时创建的存储空间直通群集使用 Enable-ClusterS2D  

在运行时，可能会看到类似于以下的错误**Enable-ClusterS2D** cmdlet:

![方案 6 错误消息](media/troubleshooting/scenario-error-message.png)

若要解决此问题，请确保在 HBA 模式下配置 HBA 适配器。 没有 HBA 都应在 RAID 模式下进行配置。  
    
## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>在等待直到 SBL 磁盘会显示还是 27%，Enable-clusterstoragespacesdirect 挂起

您将看到验证报告中的以下信息：

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 
    
  
问题在于与 HPE SAS 扩展器卡位于之间的磁盘和 HBA 卡。 SAS 扩展器创建第一个驱动器连接到扩展器和扩展器本身之间的重复 ID。  中已解决此[HPE 智能数组控制器 SAS 扩展器固件：4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).
    
## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Intel SSD DC P4600 序列具有非唯一 NGUID
您可能会看到问题 Intel SSD DC P4600 系列设备似乎会报告类似 16 字节 NGUID 如 0100000001000000E4D25C000014E214 或 0100000001000000E4D25C0000EEE214 下面的示例中的多个命名空间。

|uniqueid| deviceid |MediaType| BusType| 序列号| 大小|canpool| friendlyname| OperationalStatus|
|-|-|-|-|-|-|-|-|-
|5000CCA251D12E30| 0| HDD| SAS| 7PKR197G|                  10000831348736 |False|HGST| HUH721010AL4200| 确定|
|eui.0100000001000000E4D25C000014E214 |4|SSD| NVMe|   0100_0000_0100_0000_E4D2_5C00_0014_E214.|1600321314816|True| INTEL| SSDPE2KE016T7|  确定|
|eui.0100000001000000E4D25C000014E214 |5|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_0014_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  确定|
|eui.0100000001000000E4D25C0000EEE214| 6|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  确定|
|eui.0100000001000000E4D25C0000EEE214| 7|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  确定|

若要解决此问题，请更新到最新版本的 Intel 驱动器上的固件。  固件版本从 2018 年 5 月 QDV101B1 已知要解决此问题。

[2018 年 5 月版本的 Intel SSD 数据中心 Tool](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf)包括固件更新、 QDV101B1，Intel SSD DC P4600 序列。

    
## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>物理磁盘"正常"和操作状态是"从池中删除" 

在 Windows Server 2016 存储空间直通群集中，可能会看到一个或多个物理磁盘的 HealthStatus 为"正常"OperationalStatus 时"（删除从池中，确定）。" 

"从池中删除"时，将意向设置**Remove-physicaldisk**是调用，但存储在运行状况，以维护状态，并让恢复，如果删除操作失败。 您可以手动更改 OperationalStatus 为正常使用以下方法之一：

- 从池中删除物理磁盘，然后重新添加它。
- 运行[清除 PhysicalDiskHealthData.ps1 脚本](https://go.microsoft.com/fwlink/?linkid=2034205)清除目的。 (下载。TXT 文件。 你将需要将其保存为。PS1 文件，然后才能运行。）

下面是一些示例来演示如何运行脚本：

- 使用**SerialNumber**参数来指定所需设置为正常磁盘。 你可以获取从序列号**WMI MSFT_PhysicalDisk**或**Get-physicaldisk**。 （我们将只使用 0 秒的序列号下面。）
   
   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- 使用**UniqueId**参数来指定该磁盘 (再次从**WMI MSFT_PhysicalDisk**或**Get-physicaldisk**)。

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>文件复制速度缓慢

您可能会看到使用文件资源管理器将大 VHD 复制到的虚拟磁盘的问题-文件复制所花的时间比预期。

使用文件资源管理器，Robocopy 或 Xcopy 将大 VHD 复制到的虚拟磁盘不是建议的方法因为这将导致慢于预期的性能。 复制过程不会通过存储空间直通堆栈，它更低时位于存储堆栈，并改为的作用类似于本地的复制过程。

如果你想要测试存储空间直通的性能，我们建议使用 VMFleet 和 Diskspd 到负载测试和压力测试服务器来获取基准线和设置的存储空间直通的性能期望。

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>预期节点的重新启动期间会看到其余节点的事件。 

则可以安全地忽略这些事件： 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

如果您在运行 Azure Vm，则可以忽略此事件：

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 
    
## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>性能变慢或"通信中断，""IO 错误"、"分离"或"否冗余"错误，对于使用 Intel P3x00 NVMe 设备的部署

我们发现会影响某些存储空间直通的用户使用基于 Intel P3x00 系列 NVM Express (NVMe) 在"维护版本 8。"之前的固件版本的设备的硬件的关键问题 

>[!NOTE]
> 各个 Oem 可能基于 Intel P3x00 系列使用唯一的固件版本字符串的 NVMe 设备的设备。 有关最新的固件版本的详细信息，请与您的 OEM 联系。

如果使用的基于 Intel P3x00 系列 NVMe 设备在部署中的硬件，我们建议您立即应用的最新的固件 (至少维护版本 8)。 这[Microsoft 支持文章](https://support.microsoft.com/en-us/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda)提供了有关此问题的其他信息。 
