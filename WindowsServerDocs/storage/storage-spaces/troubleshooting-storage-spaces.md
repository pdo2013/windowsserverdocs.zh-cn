---
title: 存储空间直通疑难解答
description: 了解如何排查存储空间直通部署问题。
keywords: 存储空间
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7cc5709723b300f46ce108b36501e7ace272cd45
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544563"
---
# <a name="troubleshoot-storage-spaces-direct"></a>排查存储空间直通

> 适用于：Windows Server 2019、Windows Server 2016

使用以下信息来排查存储空间直通部署问题。

一般情况下, 请先执行以下步骤:

1. 确认使用 Windows Server 目录为 Windows Server 2016 和 Windows Server 2019 认证了 SSD 的品牌/型号。 与供应商确认存储空间直通支持驱动器。
2. 检查存储中是否有任何出现故障的驱动器。 使用存储管理软件检查驱动器的状态。 如果有任何驱动器出现故障, 请与供应商合作。 
3. 如有必要, 请更新存储和驱动器固件。
   确保所有节点上都安装了最新的 Windows 更新。 可以从 windows [10 和 Windows server 2016 更新历史记录](https://aka.ms/update2016)以及 windows [10 和 windows server 2019 更新历史记录](https://support.microsoft.com/help/4464619)中的 Windows Server 2019 获取 windows server 2016 的最新更新。
4. 更新网络适配器驱动程序和固件。
5. 运行群集验证并查看存储空间直通部分, 确保缓存用于缓存的驱动器正确且没有错误。

如果仍有问题, 请查看以下方案。

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>虚拟磁盘资源无冗余状态
由于崩溃或电源故障, 存储空间直通系统的节点意外重启。 然后, 一个或多个虚拟磁盘可能未联机, 并且你会看到说明 "没有足够的冗余信息"。

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Size| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| 确定|  正常| True|  10 TB|  Node-01. conto|
|Disk3         |Mirror                 |确定                          |正常       |True            |10 TB | Node-01. conto|
|Disk2         |Mirror                 |无冗余               |Unhealthy     |True            |10 TB | Node-01. conto|
|Disk1         |Mirror                 |{无冗余, InService}  |Unhealthy     |True            |10 TB | Node-01. conto| 

此外, 在尝试使虚拟磁盘联机后, 会在群集日志 (DiskRecoveryAction) 中记录以下信息。  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

如果磁盘出现故障或系统无法访问虚拟磁盘上的数据, 则不会出现**冗余操作状态**。 如果节点上的维护期间节点发生重新启动, 则可能出现此问题。

若要解决此问题, 请执行以下步骤:

1. 从 CSV 中删除受影响的虚拟磁盘。 这会将其放在群集中的 "可用存储" 组中, 并开始显示为 "物理磁盘" 的 ResourceType。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 在拥有可用存储组的节点上, 在处于 "无冗余" 状态的每个磁盘上运行以下命令。 若要确定 "可用存储" 组在哪个节点上, 可以运行以下命令。

   ```powershell
   Get-ClusterGroup
   ```
3. 设置磁盘恢复操作, 然后启动磁盘。
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. 修复应该会自动启动。 等待修复完成。 它可能进入挂起状态并再次开始。 监视进度: 
    - 运行**get-storagejob**以监视修复的状态, 并查看其完成时间。
    - 运行**VirtualDisk**并验证空间是否返回 "正常" 的 HealthStatus。
5. 修复完成并且虚拟磁盘运行正常后, 将虚拟磁盘参数更改回。

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. 使磁盘脱机, 然后再次联机, 使 DiskRecoveryAction 生效:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. 将受影响的虚拟磁盘添加回 CSV。

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction**是允许在读写模式下附加空间卷的替代开关, 无需进行任何检查。 属性使您可以进行诊断, 使卷无法联机的原因。 这与维护模式非常相似, 但你可以对处于失败状态的资源调用它。 它还使您可以访问数据, 这在某些情况下非常有用, 例如 "无冗余", 您可以在其中访问可以复制的任何数据。 DiskRecoveryAction 属性是在2018年2月22日, update, KB 4077525 中添加的。


## <a name="detached-status-in-a-cluster"></a>群集中的已分离状态 

当你运行**VirtualDisk** cmdlet 时, 一个或多个存储空间直通虚拟磁盘的 OperationalStatus 将被分离。 但是, **PhysicalDisk** cmdlet 报告的 HealthStatus 指示所有物理磁盘都处于正常状态。

下面是**VirtualDisk** cmdlet 的输出示例。

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Size|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 确定|                  正常|       True|            10 TB|  Node-01. conto|
|Disk3|         Mirror|                 确定|                  正常|       True|            10 TB|  Node-01. conto|
|Disk2|         Mirror|                 Detached|            Unknown|       True|            10 TB|  Node-01. conto|
|Disk1|         Mirror|                 Detached|            Unknown|       True|            10 TB|  Node-01. conto| 


此外, 还可以在节点上记录以下事件:

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

如果脏区域跟踪 (DRT) 日志已满, 则可能发生**分离的操作状态**。 存储空间为镜像空间使用脏区域跟踪 (DRT), 以确保在发生电源故障时, 对元数据进行的任何正在进行的更新都将记录下来, 以确保存储空间可以重做或撤消操作, 使存储空间恢复为灵活性当电源恢复并且系统恢复正常时, 状态一致。 如果 DRT 日志已满, 则在同步和刷新 DRT 元数据之前, 不能使虚拟磁盘处于联机状态。 此过程需要运行完全扫描, 这可能需要几个小时才能完成。

若要解决此问题, 请执行以下步骤:
1. 从 CSV 中删除受影响的虚拟磁盘。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 在未联机的每个磁盘上运行以下命令。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. 在分离卷处于联机状态的每个节点上运行以下命令。 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   应在已分离卷处于联机状态的所有节点上启动此任务。 修复应该会自动启动。 等待修复完成。 它可能进入挂起状态并再次开始。 监视进度: 
   - 运行**get-storagejob**以监视修复的状态, 并查看其完成时间。
   - 运行**VirtualDisk**并验证空间是否返回 "正常" 的 HealthStatus。
     - "针对崩溃恢复的数据完整性扫描" 是不显示为存储作业的任务, 且没有进度指示器。 如果任务显示为 "正在运行", 则它正在运行。 完成后, 它将显示 "已完成"。

       此外, 你可以使用以下 cmdlet 查看正在运行的计划任务的状态: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. 一旦 "针对崩溃恢复的数据完整性扫描" 完成, 修复完成并且虚拟磁盘运行正常, 将虚拟磁盘参数更改回。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```

5. 使磁盘脱机, 然后再次联机, 使 DiskRecoveryAction 生效:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 

6. 将受影响的虚拟磁盘添加回 CSV。    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   **DiskRunChkdsk 值 7**用于附加空间量, 并使分区处于只读模式。 这样, 便可以通过触发修复来自动发现和自行修复空间。 安装后将自动运行修复。 它还允许您访问数据, 这对于访问可以复制的任何数据都很有帮助。 对于某些故障情况, 如完整的 DRT 日志, 需要运行针对崩溃恢复计划任务的数据完整性扫描。

**针对崩溃恢复任务的数据完整性扫描**用于同步和清除完全脏区域跟踪 (DRT) 日志。 此任务可能需要几个小时才能完成。 "针对崩溃恢复的数据完整性扫描" 是不显示为存储作业的任务, 且没有进度指示器。 如果任务显示为 "正在运行", 则它正在运行。 完成后, 它将显示为 "已完成"。 如果在运行此任务时取消任务或重新启动节点, 任务将需要从头开始。

有关详细信息, 请参阅[存储空间直通运行状况和运行状态疑难解答](storage-spaces-states.md)。

## <a name="event-5120-with-statusiotimeout-c00000b5"></a>事件5120与 STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> **对于 Windows Server 2016:** 若要减少在将更新与修补程序一起应用时遇到这些问题的可能性, 建议使用下面的存储维护模式过程安装[10 月18日2018、Windows Server 2016](https://support.microsoft.com/help/4462928)或更高版本的累积更新当节点当前安装了从[5 月 8 2018 日](https://support.microsoft.com/help/4103723)起发布的 Windows Server 2016 累积更新时[, 2018](https://support.microsoft.com/help/KB4462917)。

使用从5月8日发布的累积更新 (从[5 月8日到 2018 kb 4103723](https://support.microsoft.com/help/4103723)到[10 月9日, 已安装 2018 kb 4462917](https://support.microsoft.com/help/4462917) ) 重启 Windows Server 2016 上的节点后, 你可能会收到事件5120和 STATUS_IO_TIMEOUT c00000b5。

重新启动节点时, 事件5120将记录在系统事件日志中, 并包含以下错误代码之一:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

记录事件5120时, 会生成实时转储, 收集可能会导致其他症状或性能影响的调试信息。 生成实时转储会创建一个短暂的暂停, 以启用内存快照来写入转储文件。 具有大量内存且处于压力下的系统可能导致节点丢弃群集成员身份, 还会导致记录以下事件1135。

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

在5月8日2018到 Windows Server 2016 中引入的更改, 这是为存储空间直通群集内 SMB 网络会话添加 SMB 复原句柄的累积更新。 这样做是为了提高暂时性网络故障的复原能力, 并改进 RoCE 处理网络拥塞的方式。 当 SMB 连接尝试重新连接并在节点重新启动时等待超时时, 这些改进也会无意中增加。 这些问题可能会影响系统的压力。 在计划外停机期间, 系统等待连接超时时, 还会出现 IO 暂停长达60秒的情况。若要解决此问题, 请安装 Windows Server 2016 或更高版本的[2018 年10月18日累积更新](https://support.microsoft.com/help/4462928)。

*注意*此更新将 CSV 超时与 SMB 连接超时保持一致, 以解决此问题。 它不会实现 "解决方法" 一节中提到的禁用实时转储生成的更改。

### <a name="shutdown-process-flow"></a>关闭进程流:

1. 运行 VirtualDisk cmdlet, 并确保 HealthStatus 值正常。
2. 通过运行以下 cmdlet 来排出节点:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. 通过运行以下 cmdlet, 将该节点上的磁盘放入存储维护模式: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. 运行**PhysicalDisk** cmdlet, 并确保 OperationalStatus 值处于维护模式。
5. 运行**重新启动计算机**cmdlet 以重启节点。
6. 节点重启后, 通过运行以下 cmdlet, 从存储维护模式中删除该节点上的磁盘:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. 通过运行以下 cmdlet 恢复节点:

   ```powershell
   Resume-ClusterNode
   ```
8. 通过运行以下 cmdlet 来检查重新同步作业的状态:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>禁用实时转储
若要减轻在具有大量内存并且负载过重的系统上生成实时转储的影响, 你可能还想要禁用实时转储生成。 下面提供了三个选项。

>[!Caution]
>此过程可阻止收集 Microsoft 支持部门可能需要调查此问题的诊断信息。 支持代理可能需要根据特定的故障排除方案, 要求重新启用实时转储生成。

可以通过两种方法禁用实时转储, 如下所述。

#### <a name="method-1-recommended-in-this-scenario"></a>方法 1 (在此方案中建议使用)
若要完全禁用所有转储, 包括实时转储系统, 请执行以下步骤:

1. 创建以下注册表项:HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. 在新的**ForceDumpsDisabled**项下, 创建一个 REG_DWORD 属性作为 GuardedHost, 然后将其值设置为0x10000000。
3. 将新的注册表项应用于每个群集节点。

>[!NOTE]
>必须重新启动计算机, n 更改才能生效。

设置此注册表项后, 将无法创建实时转储, 并生成 "STATUS_NOT_SUPPORTED" 错误。

#### <a name="method-2"></a>Method 2
默认情况下, 对于每个报表类型, Windows 错误报告将每7天只允许一个 LiveDump, 每台计算机只允许5天。 你可以通过将以下注册表项设置为仅允许计算机上的一个 LiveDump 来更改此项。
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*注意*要使更改生效, 必须重新启动计算机。

### <a name="method-3"></a>方法3
若要禁用实时转储的群集生成 (如记录了事件 5120), 请运行以下 cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
此 cmdlet 会立即影响所有群集节点, 而无需重新启动计算机。

## <a name="slow-io-performance"></a>IO 性能缓慢

如果你看到的 IO 性能缓慢, 请检查你的存储空间直通配置是否启用了缓存。 

可以通过两种方式检查: 


1. 使用群集日志。 在所选的文本编辑器中打开群集日志, 并搜索 "[= = = SBL 磁盘 = = =]"。 这将是在其上生成日志的节点上的磁盘的列表。 

     启用缓存的磁盘示例:请注意, 此处的状态为 CacheDiskStateInitializedAndBound, 此时存在 GUID。 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    未启用缓存:这里我们可以看到, 没有 GUID, 并且状态为 CacheDiskStateNonHybrid。 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    未启用缓存:默认情况下, 如果所有磁盘都属于相同的类型, 则不启用。 这里我们可以看到, 没有 GUID, 并且状态为 CacheDiskStateIneligibleDataPartition。 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. 使用 SDDCDiagnosticInfo 中的 Get-PhysicalDisk
    1. 使用 "$d = Export-clixml GetPhysicalDisk 打开 XML 文件
    2. 运行 "ipmo 存储"
    3. 运行 "$d"。 请注意, 使用情况为自动选择, 而不是日记本你将看到如下输出: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| 用法| Size|
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

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>如何销毁现有群集以便可以再次使用相同的磁盘

在存储空间直通群集中, 禁用存储空间直通并使用[清理驱动器](deploy-storage-spaces-direct.md#step-31-clean-drives)中所述的清理过程后, 群集存储池仍会保持脱机状态, 并从群集中删除运行状况服务。

下一步是删除虚拟存储池: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

现在, 如果在任何节点上运行**PhysicalDisk** , 将看到池中的所有磁盘。 例如, 在具有4个 SAS 磁盘的4节点群集的实验室中, 每个节点有100GB。 在这种情况下, 禁用存储空间直通后, 这会删除 SBL (存储总线层), 但会保留筛选器, 如果你运行**PhysicalDisk**, 它应报告4个磁盘, 不包括本地操作系统磁盘。 相反, 它报告了16。 这对于群集中的所有节点都是相同的。 当你运行**磁盘**上的命令时, 你将看到按以下示例输出中所示, 将本地附加的磁盘编号为0、1、2等等。

|编号| 友好名称| 序列号|HealthStatus|OperationalStatus|总大小| 分区形式|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu  ||正常 | Online|  127 GB| GPT|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
|1|Msft Virtu||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
|2|Msft Virtu||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
|4|Msft Virtu||正常| 脱机| 100 GB| 原材料|
|3|Msft Virtu||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|
||Msft Virtu ||正常| 脱机| 100 GB| 原材料|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>使用 Enable-clusters2d 创建存储空间直通群集时出现的有关 "不支持的媒体类型" 的错误消息  

运行**enable-clusters2d** cmdlet 时, 可能会看到类似于下面的错误:

![方案6错误消息](media/troubleshooting/scenario-error-message.png)

若要解决此问题, 请确保 HBA 适配器是在 HBA 模式下配置的。 不应在 RAID 模式下配置 HBA。  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>启用-ClusterStorageSpacesDirect 在 "等待 SBL 磁盘出现" 或 27% 时挂起

验证报告中将显示以下信息:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


此问题的原因在于磁盘和 HBA 卡之间的 HPE SAS 扩展器卡。 SAS 扩展器在连接到扩展器的第一个驱动器与扩展器本身之间创建重复的 ID。  此操作已在 HPE [智能阵列控制器 SAS 扩展器固件中得到解决:4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3)。

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Intel SSD DC P4600 系列具有非唯一的 n
在下面的示例中, 可能会出现一个问题, 即 Intel SSD DC P4600 系列设备似乎为多个命名空间 (如0100000001000000E4D25C000014E214 或 0100000001000000E4D25C0000EEE214) 报告类似的16字节 n。


|               uniqueid               | deviceid | MediaType | BusType |               serialnumber               |      大小      | canpool | 友好 | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| eui. 0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    媒体     |   SSDPE2KE016T7   |
| eui. 0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    媒体     |   SSDPE2KE016T7   |
| eui. 0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    媒体     |   SSDPE2KE016T7   |
| eui. 0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    媒体     |   SSDPE2KE016T7   |

若要解决此问题, 请将 Intel 驱动器上的固件更新到最新版本。  已知固件版本 QDV101B1 可能是2018。

[INTEL Ssd 数据中心工具的2018版](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf)包含适用于 INTEL Ssd DC P4600 系列的固件更新 (QDV101B1)。


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>物理磁盘 "正常", 并且操作状态为 "从池中删除" 

在 Windows Server 2016 存储空间直通群集中, 你可能会看到一个或多个物理磁盘的 HealthStatus 为 "正常", 而 OperationalStatus 为 "(从池中删除, 确定)"。 

当调用**PhysicalDisk**时, "从池中删除" 是一个意向集, 但是在运行时将其存储为维护状态, 并在删除操作失败时允许恢复。 可以通过以下方法之一, 手动将 OperationalStatus 更改为 "正常":

- 从池中删除物理磁盘, 然后将其重新添加。
- 运行[Clear-PhysicalDiskHealthData 脚本](https://go.microsoft.com/fwlink/?linkid=2034205)以清除意向。 (可用于下载。TXT 文件。 需要将其另存为。PS1 文件, 然后才能运行它。)

下面是一些演示如何运行脚本的示例:

- 使用**SerialNumber**参数指定需要设置为 "正常" 的磁盘。 可以从**WMI MSFT_PhysicalDisk**或**PhysicalDisk**获取序列号。 (我们只需将0用于以下序列号。)

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- 使用**UniqueId**参数指定磁盘 (再次从**WMI MSFT_PhysicalDisk**或**PhysicalDisk**)。

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>文件复制速度缓慢

使用文件资源管理器将大型 VHD 复制到虚拟磁盘时, 可能会出现问题-文件复制所用的时间比预期要长。

不建议使用文件资源管理器、Robocopy 或 Xcopy 将大型 VHD 复制到虚拟磁盘, 因为这会导致性能下降。 复制过程不会经历存储空间直通堆栈, 该堆栈在存储堆栈中较低, 而作为本地复制进程。

如果要测试存储空间直通性能, 我们建议使用 VMFleet 和 Diskspd 对服务器进行负载和压力测试, 以获取基准线并设置存储空间直通性能的预期。

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>节点重新启动期间在其余节点上看到的预期事件。 

可以安全地忽略这些事件: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

如果正在运行 Azure Vm, 则可以忽略此事件:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>使用 Intel P3x00 NVMe 设备的部署的性能或 "丢失的通信"、"IO 错误"、"已分离" 或 "无冗余" 错误

我们发现了一个关键问题, 该问题会影响使用基于 Intel P3x00 系列 NVM Express (NVMe) 设备 (在 "维护版本 8" 之前具有固件版本) 的某些存储空间直通用户。 

>[!NOTE]
> 单个 Oem 可能有基于 Intel P3x00 家族 NVMe 设备的设备, 这些设备具有唯一的固件版本字符串。 有关最新固件版本的详细信息, 请与你的 OEM 联系。

如果基于 NVMe 设备的 Intel P3x00 系列使用部署中的硬件, 则建议立即应用最新的可用固件 (至少维护版本 8)。 本[Microsoft 支持部门文章](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda)提供了有关此问题的其他信息。 
