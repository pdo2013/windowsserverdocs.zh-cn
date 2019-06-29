---
title: 收集诊断数据使用存储空间直通
description: 了解存储空间直通数据收集工具，与如何运行，并使用它们的特定示例。
keywords: 存储空间、 数据收集、 故障排除、 事件通道，Get SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: 51cf96fb462b68f2ba01d49642a858430c71e9f5
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469606"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>收集诊断数据使用存储空间直通

> 适用于：Windows Server 2019、Windows Server 2016

有各种诊断工具可以用来收集所需存储空间直通和故障转移群集进行故障排除的数据。 在本文中，我们将重点**Get SDDCDiagnosticInfo** -将收集所有相关的信息，有助于诊断你的群集的一次触摸设备工具。

给定的日志和其他信息的**Get SDDCDiagnosticInfo**是密集，故障排除下面提供的信息，以帮助进行故障排除高级已呈报和的可能的问题需要数据发送给 Microsoft 进行会审。

## <a name="installing-get-sddcdiagnosticinfo"></a>安装 Get SDDCDiagnosticInfo

**Get SDDCDiagnosticInfo** PowerShell cmdlet （也称为 **Get-PCStorageDiagnosticInfo**，之前称为**测试 StorageHealth**) 可用于收集日志并执行故障转移群集 （群集、 资源、 网络、 节点） 的存储空间 （运行状况检查物理磁盘，机箱，虚拟磁盘），群集共享的卷、 SMB 文件共享和重复数据删除。 

有两种方法的安装脚本，这两者都是下面概述了。

### <a name="powershell-gallery"></a>PowerShell 库

[PowerShell 库](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo)是 GitHub 存储库的快照。 请注意，从 PowerShell 库安装项需要 PowerShellGet 模块，它是 Windows 10 中，在 Windows Management Framework (WMF) 5.0 中，或基于 MSI 的安装程序 （适用于 PowerShell 3 和 4） 中可用的最新版本。

可以通过使用管理员权限在 PowerShell 中的运行以下命令来安装该模块：

``` PowerShell
Install-PackageProvider NuGet -Force
Install-Module PrivateCloud.DiagnosticInfo -Force
Import-Module PrivateCloud.DiagnosticInfo -Force
```

若要更新该模块，请在 PowerShell 中运行以下命令：

``` PowerShell
Update-Module PrivateCloud.DiagnosticInfo
```

### <a name="github"></a>GitHub

[GitHub 存储库](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/)是模块的最新版本，因为我们不断循环此处访问。 若要从 GitHub 安装模块，请下载最新的模块从[存档](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip)和提取到正确的 PowerShell 模块路径指向的 PrivateCloud.DiagnosticInfo 目录 ```$env:PSModulePath```

``` PowerShell
# Allowing Tls12 and Tls11 -- e.g. github now requires Tls12
# If this is not set, the Invoke-WebRequest fails with "The request was aborted: Could not create SSL/TLS secure channel."
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$module = 'PrivateCloud.DiagnosticInfo'
Invoke-WebRequest -Uri https://github.com/PowerShell/$module/archive/master.zip -OutFile $env:TEMP\master.zip
Expand-Archive -Path $env:TEMP\master.zip -DestinationPath $env:TEMP -Force
if (Test-Path $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module) {
    rm -Recurse $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module -ErrorAction Stop
    Remove-Module $module -ErrorAction SilentlyContinue
} else {
    Import-Module $module -ErrorAction SilentlyContinue
} 
if (-not ($m = Get-Module $module -ErrorAction SilentlyContinue)) {
    $md = "$env:ProgramFiles\WindowsPowerShell\Modules"
} else {
    $md = (gi $m.ModuleBase -ErrorAction SilentlyContinue).PsParentPath
    Remove-Module $module -ErrorAction SilentlyContinue
    rm -Recurse $m.ModuleBase -ErrorAction Stop
}
cp -Recurse $env:TEMP\$module-master\$module $md -Force -ErrorAction Stop
rm -Recurse $env:TEMP\$module-master,$env:TEMP\master.zip
Import-Module $module -Force

``` 

如果您需要先获取此模块脱机群集上，下载 zip 文件，将其移到目标服务器节点，并安装模块。

## <a name="gathering-logs"></a>正在收集日志

已启用事件通道并完成安装过程之后，可以使用模块中获取 SDDCDiagnosticInfo PowerShell cmdlet 来获取：

- 存储运行状况，再加上不正常的组件的详细信息报表
- 通过池、 卷和已删除重复的卷的存储容量的报告
- 所有群集节点和摘要的错误报告的事件日志

假设你的存储群集具有名称 *"CLUS01"。*

若要执行针对远程存储群集：

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

若要在群集的存储节点上本地执行：

``` PowerShell
Get-SDDCDiagnosticInfo
```

若要将结果保存到指定的文件夹：

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

下面是举例说明了这种情况在真正的群集上：

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

如您所见，脚本还会执行验证的当前群集状态

![数据集合的 powershell 屏幕截图](media/data-collection/CollectData.png)

正如您所看到的所有数据都正在都写入到 SDDCDiagTemp 文件夹

![在文件资源管理器屏幕截图中的数据](media/data-collection/CollectDataFolder.png)

脚本将完成后，将在用户目录中创建 ZIP

![powershell 屏幕截图中的数据 zip](media/data-collection/CollectDataResult.png)

让我们到文本文件中生成报表

```PowerShell
#find the latest diagnostic zip in UserProfile
    $DiagZip=(get-childitem $env:USERPROFILE | where Name -like HealthTest*.zip)
    $LatestDiagPath=($DiagZip | sort lastwritetime | select -First 1).FullName
#expand to temp directory
    New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
    Expand-Archive -Path $LatestDiagPath -DestinationPath D:\SDDCDiagTemp -Force
#generate report and save to text file
    $report=Show-SddcDiagnosticReport -Path D:\SDDCDiagTemp
    $report | out-file d:\SDDCReport.txt
    
```

有关参考，此处有一个指向[示例报表](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt)并[示例 zip](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP)。

若要查看此 Windows Admin Center （及更高版本对版本 1812年） 中，导航到*诊断*选项卡。正如您看到以下屏幕截图中，你可以 

- 安装诊断工具
- 更新它们 （如果它们已过期）， 
- 计划每日诊断运行 (这些具有较低影响你的系统上，通常采用 < 5 分钟在后台，并且不会占用群集上超过 500 mb)
- 如果你需要提供其支持或自行分析，视图以前收集的诊断信息。

![wac 诊断屏幕截图](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Get SDDCDiagnosticInfo 输出

以下是 Get SDDCDiagnosticInfo 压缩输出中包含的文件。

### <a name="health-summary-report"></a>运行状况摘要报表

运行状况摘要报表另存为：
- 0_CloudHealthSummary.log

分析所有数据收集并旨在向您的系统的快速摘要后，会生成此文件。 它包含：

- 系统信息
- 存储运行状况概述 （数向上资源联机，群集共享卷联机的节点、 不正常的组件，等等。）
- 不正常的组件 （包括脱机、 失败或联机挂起的群集资源） 的详细信息
- 固件和驱动程序信息
- 池、 物理磁盘和卷的详细信息
- 存储性能 （收集计数器的性能）

此报表是正在不断更新，以包括更多有用信息。 有关最新信息，请参阅[GitHub 自述文件](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md)。

### <a name="logs-and-xml-files"></a>日志文件和 XML 文件

此脚本运行各种日志收集脚本并将输出保存为 xml 文件。 我们会收集群集和运行状况日志、 系统信息 (MSInfo32)、 未筛选的事件日志 （故障转移群集，磁盘诊断、 hyper-v、 存储空间和的详细信息） 和存储诊断信息 （操作日志）。 有关具体收集的信息的最新信息，请参阅[（什么我们收集） 的 GitHub 自述文件](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include)。

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>如何使用 Get PCStorageDiagnosticInfo 中的 XML 文件
可以使用收集的数据中提供的 XML 文件中的数据**Get PCStorageDiagnosticInfo** cmdlet。 这些文件包含信息的虚拟磁盘、 物理磁盘、 基本的群集信息和其他 PowerShell 相关的输出。 

若要查看这些输出结果，请打开 PowerShell 窗口并运行以下步骤。 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>接下来希望得到什么？
有大量改进和新的 cmdlet 用于分析 SDDC 系统运行状况。
提供有关你想要查看问题的反馈[此处](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues)。 此外，随意好有用更改添加到脚本中，通过提交[拉取请求](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls)。
