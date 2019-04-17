---
title: 脚本和存储空间直通性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 78ecb64cac789751abf9fd3f55b4a1fcbbe4dad2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "8843105"
---
# 脚本与 PowerShell 和存储空间直通性能历史记录

> 适用于： Windows Server Insider Preview 版本 17692 及更高版本

在 Windows Server 2019，[存储空间直通](storage-spaces-direct-overview.md)记录，并将存储广泛的[性能历史](performance-history.md)记录的虚拟机、 服务器、 驱动器、 卷、 网络适配器和详细信息。 性能历史记录是易于查询和 PowerShell 中的过程，因此你可以快速从*原始数据*转到*实际解答*以下问题：

1. 是否有任何 CPU 峰值上周？
2. 任何物理磁盘出现异常延迟？
3. 哪些 Vm 立即使用最存储 IOPS？
4. 我的网络带宽被饱和？
5. 何时此卷将可用空间之外运行？
6. 在过去一个月，哪些虚拟机使用的大部分内存？

`Get-ClusterPerf` Cmdlet 生成脚本。 它将接受来自 cmdlet 类似的输入`Get-VM`或`Get-PhysicalDisk`按管道来处理关联，并且你可以管道其输出到所示的实用程序 cmdlet `Sort-Object`， `Where-Object`，并`Measure-Object`快速撰写功能强大的查询。

**本主题提供，并且介绍了 6 个回答 6 上面的示例脚本。** 它们提供可用于查找山峰、 查找平均值、 绘制趋势线、 运行 outlier 检测和详细信息，跨多个数据和时间范围的模式。 为他们提供作为要复制、 扩展和重复使用免费的起始代码。

   > [!NOTE]
   > 为简便起见，示例脚本省略诸如你期望的高质量 PowerShell 代码的错误处理。 它们主要用于灵感和教育版而不是生产使用。

## 示例 1: CPU，查看你 ！

此示例使用`ClusterNode.Cpu.Usage`从一系列`LastWeek`以显示最大值 （"高位线"），为每个服务器的最小和平均 CPU 使用率群集中的时间范围。 它还执行简单到四分之一分析，以显示多少小时 CPU 使用率已超过 25%50%，并在最后一个 8 天内的 75%。

### 屏幕截图

在下面的屏幕截图，我们看到*服务器 02*拥有的未知的峰值上周：

![PowerShell 的屏幕截图](media/performance-history/Show-CpuMinMaxAvg.png)

### 工作原理

从输出`Get-ClusterPerf`开到内置管道`Measure-Object`cmdlet，我们只需指定`Value`属性。 使用其`-Maximum`， `-Minimum`，并`-Average`标志，`Measure-Object`为我们提供的前三个列几乎为免费。 要执行此到四分之一分析，我们可以通过管道传递到`Where-Object`并计算其数量多少个值是`-Gt`（大于） 25、 50 或 75。 最后一步是使用 beautify`Format-Hours`和`Format-Percent`帮助程序函数 – 当然可选。

### 脚本

下面是该脚本：

```
Function Format-Hours {
    Param (
        $RawValue
    )
    # Weekly timeframe has frequency 15 minutes = 4 points per hour
    [Math]::Round($RawValue/4)
}

Function Format-Percent {
    Param (
        $RawValue
    )
    [String][Math]::Round($RawValue) + " " + "%"
}

$Output = Get-ClusterNode | ForEach-Object {
    $Data = $_ | Get-ClusterPerf -ClusterNodeSeriesName "ClusterNode.Cpu.Usage" -TimeFrame "LastWeek"

    $Measure = $Data | Measure-Object -Property Value -Minimum -Maximum -Average
    $Min = $Measure.Minimum
    $Max = $Measure.Maximum
    $Avg = $Measure.Average

    [PsCustomObject]@{
        "ClusterNode"    = $_.Name
        "MinCpuObserved" = Format-Percent $Min
        "MaxCpuObserved" = Format-Percent $Max
        "AvgCpuObserved" = Format-Percent $Avg
        "HrsOver25%"     = Format-Hours ($Data | Where-Object Value -Gt 25).Length
        "HrsOver50%"     = Format-Hours ($Data | Where-Object Value -Gt 50).Length
        "HrsOver75%"     = Format-Hours ($Data | Where-Object Value -Gt 75).Length
    }
}

$Output | Sort-Object ClusterNode | Format-Table
```

## 示例 2： 触发，射击、 延迟 outlier

此示例使用`PhysicalDisk.Latency.Average`从一系列`LastHour`时间范围以查找统计离群值，定义为具有每小时平均延迟超出 + 3σ 驱动器 （三个标准偏差） 高于填充平均值。

   > [!IMPORTANT]
   > 为简便起见，此脚本并不实现防止低差异，不会处理部分的丢失数据，通过模型或固件等没有区别。请试验良好判断并不依赖于此脚本来确定是否要替换硬盘。 它仅供教学使用此处显示。

### 屏幕截图

在下面的屏幕截图，我们看到有没有离群值：

![PowerShell 的屏幕截图](media/performance-history/Show-LatencyOutlierHDD.png)

### 工作原理

首先，我们通过检查排除处于空闲状态或空闲几乎驱动器`PhysicalDisk.Iops.Total`是完全一致地`-Gt 1`。 每个活动的 hdd，我们管道其`LastHour`组成 360 测量在 10 秒的间隔，到的时间范围`Measure-Object -Average`以在过去一小时中获取其平均延迟。 这会给成员设置。

我们实现[众所周知公式](http://www.mathsisfun.com/data/standard-deviation.html)查找平均值`μ`和标准偏差`σ`中的样本。 对于每个活动的 HDD，我们比较的总体平均到其平均延迟并除以标准偏差。 我们将保留原始值，以便可以找出`Sort-Object`我们结果，但使用`Format-Latency`和`Format-StandardDeviation`帮助程序函数以 beautify 什么，我们将显示 – 当然可选。

如果任何驱动器是多个 + 3σ，我们`Write-Host`以红色;如果不是，为绿色。

### 脚本

下面是该脚本：

```
Function Format-Latency {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("s", "ms", "μs", "ns") # Petabits, just in case!
    Do { $RawValue *= 1000 ; $i++ } While ( $RawValue -Lt 1 )
    # Return
    [String][Math]::Round($RawValue, 2) + " " + $Labels[$i]
}

Function Format-StandardDeviation {
    Param (
        $RawValue
    )
    If ($RawValue -Gt 0) {
        $Sign = "+"
    }
    Else {
        $Sign = "-"
    }
    # Return
    $Sign + [String][Math]::Round([Math]::Abs($RawValue), 2) + "σ"
}

$HDD = Get-StorageSubSystem Cluster* | Get-PhysicalDisk | Where-Object MediaType -Eq HDD

$Output = $HDD | ForEach-Object {

    $Iops = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Iops.Total" -TimeFrame "LastHour"
    $AvgIops = ($Iops | Measure-Object -Property Value -Average).Average

    If ($AvgIops -Gt 1) { # Exclude idle or nearly idle drives

        $Latency = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Latency.Average" -TimeFrame "LastHour"
        $AvgLatency = ($Latency | Measure-Object -Property Value -Average).Average

        [PsCustomObject]@{
            "FriendlyName"  = $_.FriendlyName
            "SerialNumber"  = $_.SerialNumber
            "MediaType"     = $_.MediaType
            "AvgLatencyPopulation" = $null # Set below
            "AvgLatencyThisHDD"    = Format-Latency $AvgLatency
            "RawAvgLatencyThisHDD" = $AvgLatency
            "Deviation"            = $null # Set below
            "RawDeviation"         = $null # Set below
        }
    }
}

If ($Output.Length -Ge 3) { # Minimum population requirement

    # Find mean μ and standard deviation σ
    $μ = ($Output | Measure-Object -Property RawAvgLatencyThisHDD -Average).Average
    $d = $Output | ForEach-Object { ($_.RawAvgLatencyThisHDD - $μ) * ($_.RawAvgLatencyThisHDD - $μ) }
    $σ = [Math]::Sqrt(($d | Measure-Object -Sum).Sum / $Output.Length)

    $FoundOutlier = $False

    $Output | ForEach-Object {
        $Deviation = ($_.RawAvgLatencyThisHDD - $μ) / $σ
        $_.AvgLatencyPopulation = Format-Latency $μ
        $_.Deviation = Format-StandardDeviation $Deviation
        $_.RawDeviation = $Deviation
        # If distribution is Normal, expect >99% within 3σ
        If ($Deviation -Gt 3) {
            $FoundOutlier = $True
        }
    }

    If ($FoundOutlier) {
        Write-Host -BackgroundColor Black -ForegroundColor Red "Oh no! There's an HDD significantly slower than the others."
    }
    Else {
        Write-Host -BackgroundColor Black -ForegroundColor Green "Good news! No outlier found."
    }

    $Output | Sort-Object RawDeviation -Descending | Format-Table FriendlyName, SerialNumber, MediaType, AvgLatencyPopulation, AvgLatencyThisHDD, Deviation

}
Else {
    Write-Warning "There aren't enough active drives to look for outliers right now."
}
```

## 示例 3： 干扰邻居？ 这是写入 ！

性能历史记录可以太回答有关*现在*，问题。 新的测量值是实时可用，每隔 10 秒。 此示例使用`VHD.Iops.Total`从一系列`MostRecent`标识繁忙 （一些可能说出"noisiest"） 的时间使用最大 IOPS，跨存储群集，并显示其活动的读/写细分内容中的每个主机的虚拟机。

### 屏幕截图

在下面的屏幕截图，我们看到前 10 个虚拟机的存储活动：

![PowerShell 的屏幕截图](media/performance-history/Show-TopIopsVMs.png)

### 工作原理

与`Get-PhysicalDisk`、 `Get-VM` cmdlet 不是群集感知-它只返回在本地服务器上的虚拟机。 若要查询从并行每个服务器，我们我们将调用包装在`Invoke-Command (Get-ClusterNode).Name { ... }`。 对于每个虚拟机，我们获取`VHD.Iops.Total`， `VHD.Iops.Read`，并`VHD.Iops.Write`度量值。 通过不指定`-TimeFrame`参数，我们获取`MostRecent`单一每个数据点。

   > [!TIP]
   > 这些系列反映所有 VHD/VHDX 文件到此虚拟机的活动的总和。 这是一个示例，其中性能历史记录正在自动聚合为我们。 若要获取每个的 VHD/VHDX 细分，你无法管道个人`Get-VHD`到`Get-ClusterPerf`而不是虚拟机。

每个服务器的结果汇集在一起的作为`$Output`，我们可以`Sort-Object`，然后`Select-Object -First 10`。 请注意，`Invoke-Command`修饰结果和`PsComputerName`属性，指示他们来自何处，我们可以打印了解 VM 运行所在的位置。

### 脚本

下面是该脚本：

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Iops {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = (" ", "K", "M", "B", "T") # Thousands, millions, billions, trillions...
        Do { if($RawValue -Gt 1000){$RawValue /= 1000 ; $i++ } } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-VM | ForEach-Object {
        $IopsTotal = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Total"
        $IopsRead  = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Read"
        $IopsWrite = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Write"
        [PsCustomObject]@{
            "VM" = $_.Name
            "IopsTotal" = Format-Iops $IopsTotal.Value
            "IopsRead"  = Format-Iops $IopsRead.Value
            "IopsWrite" = Format-Iops $IopsWrite.Value
            "RawIopsTotal" = $IopsTotal.Value # For sorting...
        }
    }
}

$Output | Sort-Object RawIopsTotal -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, IopsTotal, IopsRead, IopsWrite
```

## 示例 4： 他们所说，如"25 千兆是新的 10 千兆"

此示例使用`NetAdapter.Bandwidth.Total`从一系列`LastDay`时间范围以查找网络饱和度的迹象定义为 > 90%的理论上最大带宽。 对于群集中每个网络适配器，它在到其规定的链接速度的最后一天中比较最高的观察到的带宽使用量。

### 屏幕截图

在下面的屏幕截图，我们看到一个*Fabrikam NX 4 Pro #2*中的最后一天最高达到：

![PowerShell 的屏幕截图](media/performance-history/Show-NetworkSaturation.png)

### 工作原理

我们重复我们`Invoke-Command`诀窍上述到`Get-NetAdapter`上的每个服务器和管道到`Get-ClusterPerf`。 同时，我们抓取两个相关的属性： 其`LinkSpeed`字符串，如"10 Gbps"，并且其原始`Speed`10000000000 类似的整数。 我们使用`Measure-Object`若要获得的最后一天的平均和峰值 (提醒： 中的每个测量`LastDay`的时间范围表示 5 分钟) 和乘以每个字节，以获取苹果到苹果比较 8 位。

   > [!NOTE]
   > 某些供应商，如 Chelsio，包括远程直接内存访问 (RDMA) 活动中它们*网络适配器*的性能计数器，所以它包含在`NetAdapter.Bandwidth.Total`系列。 其他人，如 Mellanox，可能不允许。 如果你的供应商不，只需添加`NetAdapter.Bandwidth.RDMA.Total`此脚本的版本中的一系列。

### 脚本

下面是该脚本：

```
$Output = Invoke-Command (Get-ClusterNode).Name {

    Function Format-BitsPerSec {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("bps", "kbps", "Mbps", "Gbps", "Tbps", "Pbps") # Petabits, just in case!
        Do { $RawValue /= 1000 ; $i++ } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-NetAdapter | ForEach-Object {

        $Inbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Inbound" -TimeFrame "LastDay"
        $Outbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Outbound" -TimeFrame "LastDay"

        If ($Inbound -Or $Outbound) {

            $InterfaceDescription = $_.InterfaceDescription
            $LinkSpeed = $_.LinkSpeed
    
            $MeasureInbound = $Inbound | Measure-Object -Property Value -Maximum
            $MaxInbound = $MeasureInbound.Maximum * 8 # Multiply to bits/sec
    
            $MeasureOutbound = $Outbound | Measure-Object -Property Value -Maximum
            $MaxOutbound = $MeasureOutbound.Maximum * 8 # Multiply to bits/sec
    
            $Saturated = $False
    
            # Speed property is Int, e.g. 10000000000
            If (($MaxInbound -Gt (0.90 * $_.Speed)) -Or ($MaxOutbound -Gt (0.90 * $_.Speed))) {
                $Saturated = $True
                Write-Warning "In the last day, adapter '$InterfaceDescription' on server '$Env:ComputerName' exceeded 90% of its '$LinkSpeed' theoretical maximum bandwidth. In general, network saturation leads to higher latency and diminished reliability. Not good!"
            }
    
            [PsCustomObject]@{
                "NetAdapter"  = $InterfaceDescription
                "LinkSpeed"   = $LinkSpeed
                "MaxInbound"  = Format-BitsPerSec $MaxInbound
                "MaxOutbound" = Format-BitsPerSec $MaxOutbound
                "Saturated"   = $Saturated
            }
        }
    }
}

$Output | Sort-Object PsComputerName, InterfaceDescription | Format-Table PsComputerName, NetAdapter, LinkSpeed, MaxInbound, MaxOutbound, Saturated
```

## 示例 5： 使存储用户再次 ！

若要查看宏趋势，性能历史记录可保留最多为 1 年。 此示例使用`Volume.Size.Available`从一系列`LastYear`来确定存储填满的速率和估计何时完整的时间范围。

### 屏幕截图

在下面的屏幕截图，我们看到*备份*卷添加大约每日 15 GB:

![PowerShell 的屏幕截图](media/performance-history/Show-StorageTrend.png)

此速率，它将在另一个 42 天达到其容量。

### 工作原理

`LastYear`时间范围内有每天个数据点。 你仅严格需要两个点以适应趋势线，尽管实际上最好要求的详细信息，如 14 天。 我们使用`Select-Object -Last 14`若要设置的 *（x，y）* 点， *x* [1，14] 范围中的数组。 具有这些点，我们实现简单[线性最小二乘算法](http://mathworld.wolfram.com/LeastSquaresFitting.html)来查找`$A`和`$B`的参数化最适合的一行*y = ax + b*。 欢迎使用高学校遍布整个再次。

划分卷的`SizeRemaining`通过趋势的属性 (斜率`$A`) 让我们 crudely 存储增长的当前费率估计多少天，直到该卷已满。 `Format-Bytes`， `Format-Trend`，并`Format-Days`帮助程序函数 beautify 输出。

   > [!IMPORTANT]
   > 此评估是线性和仅基于最新的 14 每日度量值。 存在更复杂且准确的技术。 请试验良好判断并不依赖于此脚本来确定是否要在扩展存储投资。 它仅供教学使用此处显示。

### 脚本

下面是该脚本：

```

Function Format-Bytes {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
    Do { $RawValue /= 1024 ; $i++ } While ( $RawValue -Gt 1024 )
    # Return
    [String][Math]::Round($RawValue) + " " + $Labels[$i]
}

Function Format-Trend {
    Param (
        $RawValue
    )
    If ($RawValue -Eq 0) {
        "0"
    }
    Else {
        If ($RawValue -Gt 0) {
            $Sign = "+"
        }
        Else {
            $Sign = "-"
        }
        # Return
        $Sign + $(Format-Bytes [Math]::Abs($RawValue)) + "/day"
    }
}

Function Format-Days {
    Param (
        $RawValue
    )
    [Math]::Round($RawValue)
}

$CSV = Get-Volume | Where-Object FileSystem -Like "*CSV*"

$Output = $CSV | ForEach-Object {

    $N = 14 # Require 14 days of history

    $Data = $_ | Get-ClusterPerf -VolumeSeriesName "Volume.Size.Available" -TimeFrame "LastYear" | Sort-Object Time | Select-Object -Last $N

    If ($Data.Length -Ge $N) {

        # Last N days as (x, y) points
        $PointsXY = @()
        1..$N | ForEach-Object {
            $PointsXY += [PsCustomObject]@{ "X" = $_ ; "Y" = $Data[$_-1].Value }
        }

        # Linear (y = ax + b) least squares algorithm
        $MeanX = ($PointsXY | Measure-Object -Property X -Average).Average
        $MeanY = ($PointsXY | Measure-Object -Property Y -Average).Average
        $XX = $PointsXY | ForEach-Object { $_.X * $_.X }
        $XY = $PointsXY | ForEach-Object { $_.X * $_.Y }
        $SSXX = ($XX | Measure-Object -Sum).Sum - $N * $MeanX * $MeanX
        $SSXY = ($XY | Measure-Object -Sum).Sum - $N * $MeanX * $MeanY
        $A = ($SSXY / $SSXX)
        $B = ($MeanY - $A * $MeanX)
        $RawTrend = -$A # Flip to get daily increase in Used (vs decrease in Remaining)
        $Trend = Format-Trend $RawTrend

        If ($RawTrend -Gt 0) {
            $DaysToFull = Format-Days ($_.SizeRemaining / $RawTrend)
        }
        Else {
            $DaysToFull = "-"
        }
    }
    Else {
        $Trend = "InsufficientHistory"
        $DaysToFull = "-"
    }

    [PsCustomObject]@{
        "Volume"     = $_.FileSystemLabel
        "Size"       = Format-Bytes ($_.Size)
        "Used"       = Format-Bytes ($_.Size - $_.SizeRemaining)
        "Trend"      = $Trend
        "DaysToFull" = $DaysToFull
    }
}

$Output | Format-Table
```

## 示例 6： 内存扰乱，你可以运行，但不是能隐藏

由于性能历史记录收集和存储集中整个群集，你永远不需要缝合数据从不同的计算机，无需考虑如何多次 Vm 之间移动的主机。 此示例使用`VM.Memory.Assigned`从一系列`LastMonth`标识过去 35 天内占用了大部分内存虚拟机的时间。

### 屏幕截图

在下面的屏幕截图，我们通过内存使用量过去一个月看到列出前 10 个虚拟机：

![PowerShell 的屏幕截图](media/performance-history/Show-TopMemoryVMs.png)

### 工作原理

我们重复我们`Invoke-Command`这一轮，到上述引入了`Get-VM`每台服务器上。 我们使用`Measure-Object -Average`为每个虚拟机，然后获取每月的平均`Sort-Object`跟`Select-Object -First 10`以获取我们排行榜。 （或者，也许是我们*最需要*list?）

### 脚本

下面是该脚本：

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Bytes {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        Do { if( $RawValue -Gt 1024 ){ $RawValue /= 1024 ; $i++ } } While ( $RawValue -Gt 1024 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }
    
    Get-VM | ForEach-Object {
        $Data = $_ | Get-ClusterPerf -VMSeriesName "VM.Memory.Assigned" -TimeFrame "LastMonth"
        If ($Data) {
            $AvgMemoryUsage = ($Data | Measure-Object -Property Value -Average).Average
            [PsCustomObject]@{
                "VM" = $_.Name
                "AvgMemoryUsage" = Format-Bytes $AvgMemoryUsage.Value
                "RawAvgMemoryUsage" = $AvgMemoryUsage.Value # For sorting...
            }
        }
    }
}

$Output | Sort-Object RawAvgMemoryUsage -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, AvgMemoryUsage
```

就这么简单！ 希望这些样本激发出，并帮助你开始。 使用存储空间直通性能历史记录和强大，脚本友好`Get-ClusterPerf`cmdlet，你有权提出 – 和回答问题 ！ – 复杂问题在管理和监视你的 Windows Server 2019 基础结构。

## 另请参阅

- [开始使用 Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [存储空间直通概述](storage-spaces-direct-overview.md)
- [性能历史记录](performance-history.md)
