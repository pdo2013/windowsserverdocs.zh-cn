---
title: 存储空间直通疑难解答
description: 了解如何解决存储空间直通部署。
keywords: 存储空间
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: c7c9573732ff1cf5a998588b1aec81915c227ee2
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256930"
---
# 疑难解答存储空间直通

使用以下信息来解决存储空间直通部署。

一般情况下，开始菜单的以下步骤：

1. 确认已针对使用在 Windows Server 目录的 Windows Server 2016 认证 SSD 制造商/型号。 与供应商，确认驱动器受支持的存储空间直通。
2. 检查存储的任何故障驱动器。 存储管理软件用于检查驱动器的状态。 如果有任何驱动器出现故障，适用于你的供应商。 
3. 更新存储，如有必要驱动器固件。
   确保在所有节点上安装了最新的 Windows 更新。 你可以获取从 Windows Server 2016 的最新的更新[https://aka.ms/update2016](https://aka.ms/update2016)。
4. 更新网络适配器驱动程序和固件。
5. 运行群集验证和查看存储空间直通部分，请确保正确报告将用于缓存的驱动器和任何错误。

如果仍遇到问题，请查看下面的方案。

## 虚拟磁盘资源是在无冗余状态
存储空间直通系统中的节点意外重新启动由于崩溃或电源故障。 然后，一个或多个虚拟磁盘可能不进入联机状态，并且你看到的描述"没有足够冗余信息"。
    
|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|大小| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| 镜像| “确定”|  Healthy| True|  10 TB|  节点 01.conto...|
|Disk3         |镜像                 |“确定”                          |Healthy       |True            |10 TB | 节点 01.conto...|
|Disk2         |镜像                 |没有冗余               |Unhealthy     |True            |10 TB | 节点 01.conto...|
|Disk1         |镜像                 |{无冗余，维护}  |Unhealthy     |True            |10 TB | 节点 01.conto...| 

此外后尝试使虚拟磁盘，以下信息是群集日志 (DiskRecoveryAction) 中记录。  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

如果磁盘失败，或者如果系统无法访问虚拟磁盘上的数据，则可能会发生**否冗余操作状态**。 如果在节点在节点上的维护期间发生重启，则可能会发生此问题。
    
若要解决此问题，请按照以下步骤操作：

1. 从 CSV 删除受影响的虚拟磁盘。 这会将它们放在群集中的"可用的存储"组并开始菜单显示为"物理磁盘。"的资源类型

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 在节点上的可用的存储组，在无冗余状态中每个磁盘上运行以下命令。 若要确定哪个节点的"可用的存储"组位于你可以运行以下命令。

   ```powershell
   Get-ClusterGroup
   ```
3. 将磁盘恢复操作设置，然后启动磁盘。
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. 修复应自动启动。 等待修复完成。 它可能会进入挂起状态，并重新开始。 若要监视进度： 
    - 运行**获取 StorageJob**监视修复的状态并查看完成后。
    - 运行**获取 VirtualDisk**并验证空间返回 HealthStatus 的正常运行。
5. 在修复后完成和虚拟磁盘是正常，后退虚拟磁盘参数的更改。

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. 执行脱机，则再次能够生效 DiskRecoveryAction 的磁盘：

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. 将受影响的虚拟磁盘添加回 CSV。

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction**是替代交换机，允许将附加而无需任何检查的读写模式中的空间卷。 该属性可以做诊断到为什么卷将不会进入联机状态。 它是非常类似于维护模式，但可调用该上失败状态中的资源。 它还允许你访问数据，这可能很有帮助的情况下，如"无冗余，"可从何处访问任何数据可以并将其复制。 在 22，2018 年 2 月，更新，KB 4077525 月添加了 DiskRecoveryAction 属性。
    
    
## 在群集中分离的状态 

在运行时**获取 VirtualDisk** cmdlet，被分离的一个或多个存储空间直通的虚拟磁盘 OperationalStatus。 但是，通过**Get-physicaldisk** cmdlet 报告 HealthStatus 指示所有物理磁盘处于正常运行状态。

下面是来自**Get VirtualDisk** cmdlet 输出的示例。

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  大小|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         镜像|                 “确定”|                  Healthy|       True|            10 TB|  节点 01.conto...|
|Disk3|         镜像|                 “确定”|                  Healthy|       True|            10 TB|  节点 01.conto...|
|Disk2|         镜像|                 分离|            Unknown|       True|            10 TB|  节点 01.conto...|
|Disk1|         镜像|                 分离|            Unknown|       True|            10 TB|  节点 01.conto...| 


此外，可能在节点上记录的以下事件：

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

如果跟踪 (DRT) 的更新区域日志已满，则可能会发生**分离操作的状态**。 存储空间使用已更新区域的镜像空间跟踪 (DRT) 以确保的电源故障发生时，对元数据的任何传输的数据更新记录以确保存储空间可以重做或撤消操作以将存储空间恢复到的灵活和一致时还原电源和系统重新启动后的状态。 如果 DRT 日志已满，虚拟磁盘无法将联机直到 DRT 元数据同步并刷新。 此过程需要运行完全扫描，这可能需要几个小时才能完成。
    
若要解决此问题，请按照以下步骤操作：
1. 从 CSV 删除受影响的虚拟磁盘。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 不即将上线每个磁盘上运行以下命令。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. 在其中分离的卷处于联机状态的每个节点上运行以下命令。 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   此任务应在其分离的卷处于联机状态的所有节点上启动。 修复应自动启动。 等待修复完成。 它可能会进入挂起状态，并重新开始。 若要监视进度： 
   - 运行**获取 StorageJob**监视修复的状态并查看完成后。
   -  运行**获取 VirtualDisk**并验证空间返回 HealthStatus 的正常运行。
    - "数据完整性扫描的崩溃恢复"是一任务，不会显示为存储作业，并且没有任何进度指示器。 如果该任务会显示为正在运行，它正在运行。 完成后，它将显示已完成。
    
       此外，你可以通过使用以下 cmdlet 查看正在运行的计划任务的状态： 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. 只要"数据完整性扫描的崩溃恢复"完成后，修复完成和虚拟磁盘是正常，后退虚拟磁盘参数的更改。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. 将受影响的虚拟磁盘添加回 CSV。    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
**DiskRunChkdsk 值 7**用于附加空间卷和只读模式中具有的分区。 这使空间自发现和自我康复通过触发修复。 修复将运行一次自动安装。 它还允许你访问数据，这会很有用获取你可以复制任何数据的访问权限。 对于某些故障情况，如完整 DRT 日志中，你需要运行故障恢复计划任务数据完整性扫描。
    
**数据完整性扫描崩溃恢复任务**用于同步并清除完整已更新的区域 (DRT) 跟踪日志。 此任务可能需要几个小时才能完成。 "数据完整性扫描的崩溃恢复"是一任务，不会显示为存储作业，并且没有任何进度指示器。 如果该任务会显示为正在运行，它正在运行。 完成后，它将显示为已完成。 如果取消的任务或重启节点运行此任务时，该任务将需要重新从头开始。

有关详细信息，请参阅[疑难解答存储空间直通的运行状况和操作状态](storage-spaces-states.md)。
    
## 事件 5120 与 STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> 若要减少应用的修复更新时遇到这些问题的机会，建议使用下面的存储维护模式过程安装[2018 年 10 月 18，适用于 Windows Server 2016 的累积更新](https://support.microsoft.com/help/4462928)或更高版本当节点当前安装已发布的 Windows Server 2016 累积更新从[2018 年 5 月 8 日](https://support.microsoft.com/help/4103723)至[2018 年 10 月 9 日](https://support.microsoft.com/help/KB4462917)。

重启累积更新中[可能 8，2018 KB 4103723](https://support.microsoft.com/help/4103723)发布到[2018 年 10 月 9 日 KB 4462917](https://support.microsoft.com/help/4462917)安装 Windows Server 2016 上的节点后，可能会具有 STATUS_IO_TIMEOUT c00000b5 事件 5120。

重新启动节点时，事件 5120 系统事件日志中记录，包括以下错误代码之一：

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

记录事件 5120 后，生成的实时转储收集调试信息可能会导致其他症状或产生性能影响。 生成的实时转储创建简短暂停能够执行的内存转储文件写入快照。 系统具有大量内存并压力可能会导致节点退出群集成员身份，并且还会导致下面的事件 1135，若要将事件记录。

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

在 2018 年 5 月 8 日，累积更新的存储空间直通群集内 SMB 网络会话添加 SMB 复原处理引入了更改。 This was done to improve resiliency to transient network failures and improve how RoCE handles network congestion.

这些改进还无意中增加了超时 SMB 连接尝试重新连接时和超时将等待重启节点时。 这些问题可能会影响下压力的系统。 在非计划停机时间，最多 60 秒的 IO 暂停也已观测到时系统等待连接到超时。

若要解决此问题，安装[2018 年 10 月 18 日，适用于 Windows Server 2016 的累积更新](https://support.microsoft.com/help/4462928)或更高版本。

*注意*此更新将对齐与 SMB 连接超时，若要解决此问题的 CSV 超时。 它不实现更改禁用实时转储生成解决方法一部分中所述。
    
### 关闭流程：

1. 运行获取 VirtualDisk cmdlet，并确保 HealthStatus 值正常。
2. 通过运行以下 cmdlet 清空节点：

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. 将磁盘放在存储维护模式下的节点上，通过运行以下 cmdlet: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. 运行**Get-physicaldisk** cmdlet，并确保 OperationalStatus 值中维护模式。
5. 运行**重启计算机**cmdlet 来重启节点。
6. 节点重新启动后，请删除该节点上的磁盘存储维护模式通过运行以下 cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. 通过运行以下 cmdlet 恢复节点：

   ```powershell
   Resume-ClusterNode
   ```
8. 通过运行以下 cmdlet 检查重新同步作业的状态：

   ```powershell
   Get-StorageJob
   ```

### 禁用实时转储
若要缓解实时转储生成具有大量内存并压力的系统上的效果，你可能会另外想要禁用实时转储生成。 三个选项如下所示。

>[!Caution]
>此过程可阻止 Microsoft 支持人员可能需要要调查此问题的诊断信息的集合。 必须支持代理可能会要求你重新启用实时转储生成基于特定故障排除方案。

有两种方法可以禁用实时转储，如下所述。

#### 方法 1 （在此方案中推荐）
若要完全禁用所有转储，包括实时转储系统范围内，请按照下列步骤：

1. 创建以下注册表项： HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. 在新的**ForceDumpsDisabled**项下创建 GuardedHost，为 REG_DWORD 属性，然后将其值设置为 0x10000000。
3. 适用于每个群集节点的新的注册表项。

>[!NOTE]
>你需要重新启动计算机 nregistry 才能使更改生效。

此注册表项设置、 实时后创建转储将失败并生成"STATUS_NOT_SUPPORTED"错误。

#### 方法 2
默认情况下，Windows 错误报告将允许只有一个 LiveDump 每个每 7 天的报告类型和每台计算机每 5 天只有 1 个 LiveDump。 你可以通过设置以下注册表项以仅允许在计算机上一个 LiveDump，永久更改它。
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*注意*你必须重新启动计算机以使更改生效。

### 方法 3
若要禁用群集生成的实时转储 （如登录事件 5120 时），请运行以下 cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
此 cmdlet 具有而无需重新启动计算机的所有群集节点上立即生效。
    
## IO 性能降低

如果看到 IO 性能降低，请检查缓存是否已启用存储空间直通配置。 

有两种方法来检查： 
     
 
1. 使用群集日志。 选择的文本编辑器中打开群集日志，然后搜索"[=== SBL 磁盘 ===]。" 这将是磁盘的节点生成日志上的列表。 

     缓存已启用磁盘示例： 此处请注意，状态 CacheDiskStateInitializedAndBound，此处没有存在一个 GUID。 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    无法启用缓存： 我们可以在此处看到不没有存在任何 GUID 和状态是 CacheDiskStateNonHybrid。 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    无法启用缓存： 当所有磁盘都是同一类型的情况下不启用默认情况下。 这里我们可以看到不没有存在任何 GUID 和状态是 CacheDiskStateIneligibleDataPartition。 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. 使用获取 PhysicalDisk.xml 从 SDDCDiagnosticInfo
    1. 打开 XML 文件中使用"$d = 导入 Clixml GetPhysicalDisk.XML"
    2. 运行"ipmo 存储"
    3. 运行"$d"。 请注意，使用自动选择，不日志你将看到输出如下： 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| 用途| 大小|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   “确定”|                Healthy|      自动选择 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    “确定”|                Healthy| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  “确定”|                Healthy| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| “确定”|    Healthy| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|“确定”|       Healthy| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| “确定”|      Healthy| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| “确定”|      Healthy|自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| “确定”|  Healthy| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| “确定”| Healthy| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| “确定”|Healthy| 自动选择| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| “确定”| Healthy| 自动选择 |1.82 TB|
    
## 如何销毁现有群集，以便可以再次使用相同的磁盘

在存储空间直通群集中，禁用存储空间直通并使用[全新的驱动器](deploy-storage-spaces-direct.md#step-31-clean-drives)中所述的清理过程后，群集的存储池仍保持在脱机状态，并从群集中删除运行状况服务。

下一步是删除虚拟存储池： 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

现在，如果在任何节点上运行**Get-physicaldisk** ，你将看到已在池中的所有磁盘。 例如，在实验室的 4 节点的群集具有 4 个 SAS 磁盘，100 GB，每个呈现给每个节点。 在此情况下，存储空间直通处于禁用状态，这会删除 SBL （存储总线层），但会让筛选器中，如果你运行**Get-physicaldisk**后，它应该报告不包括本地的操作系统磁盘的 4 个磁盘。 而是改为报告 16。 这是相同的群集中的所有节点。 当运行**获取磁盘**命令时，你将看到本地连接的磁盘编号为 0、 1、 2 等等，如此示例输出中所示：

|数字| 友好名称| 序列号|HealthStatus|OperationalStatus|总大小| 分区样式|
|-|-|-|-|-|-|-|-|
|0|Msft 虚拟...  ||Healthy | Online|  127 GB| GPT|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
|1|Msft 虚拟...||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
|2|Msft 虚拟...||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
|4|Msft 虚拟...||Healthy| 脱机| 100 GB| 原始|
|3|Msft 虚拟...||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|
||Msft 虚拟... ||Healthy| 脱机| 100 GB| 原始|

    
## 有关"不支持的媒体类型"的错误消息时创建存储空间直通群集中使用 Enable-clusters2d  

当你运行**Enable-clusters2d** cmdlet 时，你可能会看到错误与此类似：

![场景 6 错误消息](media/troubleshooting/scenario-error-message.png)

若要解决此问题，请确保在 HBA 模式下配置 HBA 适配器。 没有 HBA 应在 RAID 模式下配置。  
    
## Enable-clusterstoragespacesdirect 挂起等待 SBL 磁盘会显示或为 27%

你将看到验证报告中的以下信息：

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 
    
  
磁盘和 HBA 卡之间存在于 HPE SAS 扩展器卡是问题。 SAS 扩展器之后创建连接到扩展器和扩展器本身的第一个驱动器之间重复的 ID。  这已在解决[HPE 智能数组控制器 SAS 扩展器固件： 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3)。
    
## Intel SSD DC P4600 系列具有非唯一 NGUID
你可能会看到问题的 Intel SSD DC P4600 系列设备似乎报告类似 16 字节 NGUID 如 0100000001000000E4D25C000014E214 或 0100000001000000E4D25C0000EEE214 下面的示例中的多个命名空间。

|uniqueid| deviceid |MediaType| BusType| 序列号| 大小|canpool| friendlyname| OperationalStatus|
|-|-|-|-|-|-|-|-|-
|5000CCA251D12E30| 0| HDD| SAS| 7PKR197G|                  10000831348736 |False|HGST| HUH721010AL4200| “确定”|
|eui.0100000001000000E4D25C000014E214 |4|SSD| NVMe|   0100_0000_0100_0000_E4D2_5C00_0014_E214。|1600321314816|True| INTEL| SSDPE2KE016T7|  “确定”|
|eui.0100000001000000E4D25C000014E214 |5|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_0014_E214。|  1600321314816|    True| INTEL| SSDPE2KE016T7|  “确定”|
|eui.0100000001000000E4D25C0000EEE214| 6|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214。|  1600321314816|    True| INTEL| SSDPE2KE016T7|  “确定”|
|eui.0100000001000000E4D25C0000EEE214| 7|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214。|  1600321314816|    True| INTEL| SSDPE2KE016T7|  “确定”|

若要解决此问题，更新到最新版本的 Intel 驱动器上的固件。  固件版本已知从 2018 年 5 月 QDV101B1 要解决此问题。

[2018 年 5 月发布的 Intel SSD 数据中心工具](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf)包括固件更新，QDV101B1，Intel SSD DC P4600 系列。

    
## 物理磁盘"正常"和操作状态是"删除从池" 

在 Windows Server 2016 存储空间直通群集中，你可能会为一个或多个物理磁盘 HealthStatus 看到为"正常"OperationalStatus 时"（删除从池，确定）。" 

"从池中删除"意图设置当**删除物理磁盘**调用，但存储在运行状况维护状态，并允许恢复，如果删除操作失败时。 可以手动更改 OperationalStatus 为正常使用以下方法之一：

- 从该池，删除物理磁盘，然后重新添加它。
- 运行[清除 PhysicalDiskHealthData.ps1 脚本](https://go.microsoft.com/fwlink/?linkid=2034205)来清除意图。 (可供下载作为。TXT 文件。 你将需要将它保存为。PS1 文件之前你可以运行它。）

下面是一些示例展示了如何运行脚本：

- **序列号**参数用于指定你需要将设置为正常的磁盘。 你可以从**WMI MSFT_PhysicalDisk**或**Get-physicaldisk**获取序列号。 （我们将只使用存在的序列号下面。）
   
   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- 使用**UniqueId**参数指定 （再次从**WMI MSFT_PhysicalDisk**或**Get-physicaldisk**） 的磁盘。

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## 文件复制速度慢

你可能会看到使用文件资源管理器将大型的 VHD 复制到虚拟磁盘的问题-文件副本时间超出预期。

使用文件资源管理器，Robocopy 或 Xcopy 将大型的 VHD 复制到虚拟磁盘不是推荐的方法为因为这将导致低于预期的性能。 复制过程未变为在存储空间直通堆栈，越短越位于存储堆栈，并改为充当本地复制过程。

如果你想要测试存储空间直通的性能，我们建议使用 VMFleet 和 Diskspd 到加载和压力测试的服务器获取基本行和设置的存储空间直通性能的期望。

## 预期的节点在重新启动期间你会看到其他节点的事件。 

它是安全地忽略这些事件： 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

如果你运行的 Azure 虚拟机，则可以忽略此事件：

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 
    
## 性能降低或"通信中断，""IO 错误"，"分离，"或"否冗余"错误的使用 Intel P3x00 NVMe 设备的部署

我们已确定影响某些使用基于 Intel P3x00 设备系列的 NVM Express (NVMe) 使用"维护发布 8。"之前的固件版本的硬件的存储空间直通用户的关键问题 

>[!NOTE]
> 单个 Oem 可能有基于 Intel P3x00 设备系列的 NVMe 使用唯一的固件版本字符串的设备。 有关最新的固件版本的详细信息，请联系你的 OEM。

如果你使用的硬件基于 Intel P3x00 系列 NVMe 设备的部署中，我们建议你立即应用的最新的固件 (至少维护版本 8)。 此[Microsoft 支持文章](https://support.microsoft.com/en-us/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda)提供有关此问题的其他信息。 
