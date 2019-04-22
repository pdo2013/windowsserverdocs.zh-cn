---
title: 使用存储空间直通的性能历史记录的脚本编写
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816318"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>使用 PowerShell 和存储空间直通的性能历史记录的脚本编写

> 适用于：Windows Server Insider Preview 生成 17692 及更高版本

在 Windows Server 2019[存储空间直通](storage-spaces-direct-overview.md)记录和存储大量[性能历史记录](performance-history.md)虚拟机、 服务器、 驱动器、 卷、 网络适配器和的详细信息。 性能历史记录是易于查询和处理在 PowerShell 中的，因此可以快速转化*原始数据*到*正确答案*到问题，例如：

1. 是否有任何 CPU 峰值过去一周？
2. 任何物理磁盘暴露异常延迟？
3. 哪些 Vm 现在正在使用的大部分存储 IOPS？
4. 我的网络带宽达到饱和？
5. 此卷将何时运行可用空间不足？
6. 在上个月中的 Vm 使用内存最多？

`Get-ClusterPerf` Cmdlet 生成的脚本。 它接受来自等 cmdlet 的输入`Get-VM`或`Get-PhysicalDisk`由管道来处理关联，并且您可以通过管道传递其输出到等的实用程序 cmdlet `Sort-Object`， `Where-Object`，和`Measure-Object`快速编写功能强大的查询。

**本主题提供，并介绍了 6 回答上述 6 的问题的示例脚本。** 它们在呈现的模式可用于查找峰值、 查找平均值、 绘制趋势线、 检测和的详细信息，跨多个数据和时间段运行离群值。 提供免费的起始代码，以便复制、 扩展和重复使用它们。

   > [!NOTE]
   > 为简洁起见，示例脚本省略您所料的高质量 PowerShell 代码的错误处理等内容。 它们主要面向灵感和教育版而不是生产使用。

## <a name="sample-1-cpu-i-see-you"></a>示例 1：CPU，我看到你 ！

此示例使用`ClusterNode.Cpu.Usage`中的系列`LastWeek`时间范围以显示群集中的最大值 （"高使用标记"），每个服务器的最小和平均 CPU 使用率。 它还将执行简单的四分位数分析，以显示多少小时 CPU 的使用量超过了 25%、 50%，并在最近 8 天的 75%。

### <a name="screenshot"></a>屏幕截图

在下面的屏幕截图，我们看到*Server 02*有无法解释的峰值过去一周：

![PowerShell 屏幕截图](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>工作原理

从输出`Get-ClusterPerf`管道可以很好地到内置`Measure-Object`cmdlet，我们只需指定`Value`属性。 使用其`-Maximum`， `-Minimum`，并`-Average`标志，`Measure-Object`为我们提供了前三个列几乎为免费。 若要执行的四分位数分析，我们可以通过管道传递给`Where-Object`，但计算多少个值是`-Gt`（大于） 25、 50 或 75。 最后一步是使用 beautify`Format-Hours`和`Format-Percent`helper 函数 – 肯定可选。

### <a name="script"></a>脚本

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

## <a name="sample-2-fire-fire-latency-outlier"></a>示例 2：火灾、 火灾、 延迟离群值

此示例使用`PhysicalDisk.Latency.Average`中的系列`LastHour`时间范围内查找统计离群值，定义为具有每小时的平均延迟时间超过 + 3σ 驱动器 （三个标准偏差） 高于总体平均值。

   > [!IMPORTANT]
   > 为简洁起见，此脚本未实现对此有所防护低方差，不会处理部分缺失数据，不会区分由模型或固件，等等。请执行良好的判断并不依赖于此脚本仅以确定是否要更换硬盘。 它仅供教学使用此处提供。

### <a name="screenshot"></a>屏幕截图

在下面的屏幕截图，我们看到有任何离群值：

![PowerShell 屏幕截图](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>工作原理

首先，我们通过检查排除空闲或几乎空闲驱动器`PhysicalDisk.Iops.Total`一直`-Gt 1`。 为每个活动的 HDD，我们将其`LastHour`时间范围内，组成 360 度量在 10 秒的间隔，为`Measure-Object -Average`过去一小时内获取其平均延迟时间。 这将提高我们填充设置。

我们实现[众所周知公式](http://www.mathsisfun.com/data/standard-deviation.html)若要查找平均值`μ`偏差和标准偏差`σ`填充。 对于每个活动的 HDD，我们比较其平均延迟时间与填充平均值并除以标准偏差。 我们保留原始值，以便我们可以`Sort-Object`我们的结果，但使用`Format-Latency`和`Format-StandardDeviation`帮助程序函数来 beautify 什么我们将展示 – 肯定可选。

如果有任何驱动器是多个 + 3σ，我们`Write-Host`以红色; 如果不熟悉，显示为绿色。

### <a name="script"></a>脚本

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

## <a name="sample-3-noisy-neighbor-thats-write"></a>示例 3：干扰性邻居？ 这是写入 ！

性能历史记录可以回答有关*稍后再试*、 过。 新的度量值是实时可用，每隔 10 秒。 此示例使用`VHD.Iops.Total`中的系列`MostRecent`时间范围来标识最繁忙 （有人会说"繁忙"） 使用最大存储 IOPS，跨群集中每个主机的虚拟机，并显示的读/写细分其活动。

### <a name="screenshot"></a>屏幕截图

在下面的屏幕截图，我们将看到列出前 10 个虚拟机存储活动：

![PowerShell 屏幕截图](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>工作原理

与不同`Get-PhysicalDisk`，则`Get-VM`cmdlet 并没有能够识别群集，它仅返回本地服务器上的 Vm。 若要从每个服务器中并行查询，我们将在我们调用`Invoke-Command (Get-ClusterNode).Name { ... }`。 为每个 VM，我们得到`VHD.Iops.Total`， `VHD.Iops.Read`，和`VHD.Iops.Write`度量值。 通过不指定`-TimeFrame`参数，我们获取`MostRecent`为每个单个数据点。

   > [!TIP]
   > 这些系列反映到所有其 VHD/VHDX 文件的此 VM 的活动的总和。 这是一个示例，其中性能历史记录正在自动聚合为我们。 若要获取的每个 VHD/VHDX 明细，你可以通过管道传递个人`Get-VHD`到`Get-ClusterPerf`而不是 VM。

每个服务器的结果一起作为`$Output`，我们可`Sort-Object`，然后`Select-Object -First 10`。 请注意，`Invoke-Command`修饰结果与`PsComputerName`属性，指示它们原来所在的其中我们可以打印知道 VM 正在运行。

### <a name="script"></a>脚本

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

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>示例 4：正如他们所说，"25-将是最佳人选新的 10 千兆"

此示例使用`NetAdapter.Bandwidth.Total`中的系列`LastDay`时间范围内查找网络饱和度的符号定义为 > 90%的理论最大带宽。 对于群集中每个网络适配器，它比较观察到的带宽使用率最高到其规定的链接速度的最后一天。

### <a name="screenshot"></a>屏幕截图

在下面的屏幕截图，我们看到一个*Fabrikam NX 4 Pro #2*达到高峰的最后一天中：

![PowerShell 屏幕截图](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>工作原理

我们重复我们`Invoke-Command`到上述技巧`Get-NetAdapter`上的每个服务器和到管道`Get-ClusterPerf`。 此过程中，我们获取两个相关属性： 其`LinkSpeed`字符串，如"10 Gbps"和其原始`Speed`10000000000 等的整数。 我们使用`Measure-Object`从最后一天中获取的平均值和峰值 (提醒： 在每个测量`LastDay`时间范围表示 5 分钟) 乘以每个字节，若要获取的苹果对象比较的 8 位。

   > [!NOTE]
   > 某些供应商，如 Chelsio，包括远程直接内存访问 (RDMA) 活动中的其*网络适配器*性能计数器，使其包含在`NetAdapter.Bandwidth.Total`系列。 其他人，如 Mellanox，可能不允许。 如果你的供应商不会只需添加`NetAdapter.Bandwidth.RDMA.Total`此脚本的版本中的序列。

### <a name="script"></a>脚本

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

## <a name="sample-5-make-storage-trendy-again"></a>示例 5：使存储用户再次使用 ！

若要查看宏趋势，性能历史记录将保留最多 1 年。 此示例使用`Volume.Size.Available`中的系列`LastYear`时间范围来确定存储填满的速率和估计值时将填满。

### <a name="screenshot"></a>屏幕截图

在下面的屏幕截图，我们看到*备份*卷添加每天大约 15 GB:

![PowerShell 屏幕截图](media/performance-history/Show-StorageTrend.png)

按照这个频率，它将在另一个 42 天内达到其容量。

### <a name="how-it-works"></a>工作原理

`LastYear`时间范围内具有每天创建一个数据点。 尽管仅一定需要两个点，以适应趋势线，但在实践中最好要求的详细信息，如 14 天。 我们使用`Select-Object -Last 14`若要设置的数组 *（x，y）* 点，对于*x* [1，14] 范围内。 使用这些点，我们实现简单[线性最小二乘法算法](http://mathworld.wolfram.com/LeastSquaresFitting.html)若要查找`$A`并`$B`的参数化的最佳行*y = ax + b*。 欢迎使用高中从头重新。

划分的卷`SizeRemaining`属性的趋势 (增量的斜率`$A`) 可以让我们粗略估计存储增长情况的当前速率为多少天，直到该卷已满。 `Format-Bytes`， `Format-Trend`，和`Format-Days`帮助器函数 beautify 输出。

   > [!IMPORTANT]
   > 此估计值是线性的并且仅基于最新的 14 每日度量值。 存在更复杂且准确的技术。 请执行良好的判断并不依赖于此脚本仅以确定是否扩展你的存储投资。 它仅供教学使用此处提供。

### <a name="script"></a>脚本

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

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>示例 6:您可以运行占用了大量内存，但不能隐藏

因为性能历史记录收集和集中存储对于整个群集，则永远不会需要将拼结起来从不同的计算机，无论数据很多时候 Vm 主机之间移动。 此示例使用`VM.Memory.Assigned`中的系列`LastMonth`时间范围来确定最近 35 天内使用了最大内存的虚拟机。

### <a name="screenshot"></a>屏幕截图

在下面的屏幕截图，我们将看到列出前 10 个虚拟机按内存使用上个月：

![PowerShell 屏幕截图](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>工作原理

我们重复我们`Invoke-Command`技巧，为更高版本，引入了`Get-VM`每个服务器上。 我们使用`Measure-Object -Average`来获取每月平均为每个 VM，则`Sort-Object`跟`Select-Object -First 10`获取我们排行榜。 (或者，也许是我们*最想*列表？)

### <a name="script"></a>脚本

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

就这么简单！ 希望这些示例启发您并帮助你开始入门。 使用存储空间直通的性能历史记录和功能强大，脚本编写友好`Get-ClusterPerf`cmdlet，你有权提出 – 和回答问题 ！ – 复杂问题管理和监视 Windows Server 2019 基础结构。

## <a name="see-also"></a>请参阅

- [Windows PowerShell 入门](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [存储空间直通概述](storage-spaces-direct-overview.md)
- [性能历史记录](performance-history.md)
