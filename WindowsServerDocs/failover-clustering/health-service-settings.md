---
title: "运行状况服务设置"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 569cf7ba30fd3f993394efd3735a56d116c067e0
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-settings"></a>运行状况服务设置
> 适用于 Windows Server 2016

运行状况服务是 Windows Server 2016，以便改进的日常监视和运营群集运行存储空间直通体验中的新增功能。

作为设置公开的许多参数这控制的运行状况服务的行为。 你可以修改这些 tune 故障或操作，快速、 打开/关闭某些行为等。

使用以下 PowerShell cmdlet 设置或修改设置。

### <a name="usage"></a>使用情况

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name <SettingName> -Value <Value>  
```

#### <a name="example"></a>示例

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.Volume.CapacityThreshold.Warning" -Value 70
```

### <a name="common-settings"></a>常见设置

一些常见修改后的设置下面列出，以及它们的默认值。

#### <a name="volume-capacity-threshold"></a>音量容量 Threshold

```
"System.Storage.Volume.CapacityThreshold.Enabled"  = True
"System.Storage.Volume.CapacityThreshold.Warning"  = 80
"System.Storage.Volume.CapacityThreshold.Critical" = 90
```

#### <a name="pool-reserve-capacity-threshold"></a>池预订容量 Threshold

```
"System.Storage.StoragePool.CheckPoolReserveCapacity.Enabled" = True
```

#### <a name="physical-disk-lifecycle"></a>物理磁盘生命周期

```
"System.Storage.PhysicalDisk.AutoPool.Enabled"                             = True
"System.Storage.PhysicalDisk.AutoRetire.OnLostCommunication.Enabled"       = True
"System.Storage.PhysicalDisk.AutoRetire.OnUnresponsive.Enabled"            = True
"System.Storage.PhysicalDisk.AutoRetire.DelayMs"                           = 900000 (i.e. 15 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountResetIntervalSeconds" = 360 (i.e. 60 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountAllowed"              = 3
```

#### <a name="supported-components-document"></a>受支持的组件文档

查看以前的一部分。

#### <a name="firmware-rollout"></a>固件 Rollout

```
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.SingleDrive.Enabled"       = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled"           = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelaySeconds"  = 604800 (i.e. 7 days)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.ShortDelaySeconds" = 86400 (i.e. 1 day)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelayCount"    = 1
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.FailureTolerance"  = 3
```

#### <a name="platform--quiescence"></a>平台 / Quiescence

```
"Platform.Quiescence.MinDelaySeconds" = 120 (i.e. 2 minutes)
"Platform.Quiescence.MaxDelaySeconds" = 420 (i.e. 7 minutes)
```

#### <a name="metrics"></a>指标

```
"System.Reports.ReportingPeriodSeconds" = 1
```

#### <a name="debugging"></a>调试

```
"System.LogLevel" = 4
```

## <a name="see-also"></a>请参阅

- [Windows Server 2016 中的运行状况服务](health-service-overview.md)
- [直接在 Windows Server 2016 的存储空间](../storage/storage-spaces/storage-spaces-direct-overview.md)
