---
title: 通过存储空间直通收集诊断数据
description: 了解存储空间直通的数据收集工具，并提供有关如何运行和使用它们的特定示例。
keywords: 存储空间，数据收集，故障排除，事件通道，SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: 67f35e3afa8e9eafabe7b22eb60cc85c7be6cb23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402872"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>通过存储空间直通收集诊断数据

> 适用于：Windows Server 2019、Windows Server 2016

可以使用各种诊断工具来收集对存储空间直通和故障转移群集进行故障排除所需的数据。 在本文中，我们将重点介绍**SDDCDiagnosticInfo** -一个触摸工具，它将收集所有相关信息来帮助你诊断群集。

假设日志和**SDDCDiagnosticInfo**的其他信息是密集的，下面提供的故障排除信息将有助于排查已经升级的高级问题，并可能需要将数据发送到Microsoft 的会审。

## <a name="installing-get-sddcdiagnosticinfo"></a>安装 SDDCDiagnosticInfo

**SDDCDiagnosticInfo** PowerShell cmdlet （也称为 **PCStorageDiagnosticInfo**（以前称为**StorageHealth**）可用于收集日志，并为故障转移群集（群集、资源、网络、节点）、存储空间（物理磁盘、存储设备、虚拟磁盘）、群集共享卷、SMB 文件共享和重复数据删除。 

有两种安装脚本的方法，这两种方法都是下面的轮廓。

### <a name="powershell-gallery"></a>PowerShell 库

[PowerShell 库](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo)是 GitHub 存储库的快照。 请注意，从 PowerShell 库安装项需要最新版本的 PowerShellGet 模块，该模块在 Windows 10、Windows Management Framework （WMF）5.0 或基于 MSI 的安装程序（适用于 PowerShell 3 和4）中提供。

可以通过使用管理员权限在 PowerShell 中运行以下命令来安装该模块：

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

[GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/)存储库是该模块的最新版本，因为我们不断地循环访问。 若要从 GitHub 安装模块，请从[存档](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip)下载最新的模块，并将 PrivateCloud. DiagnosticInfo 目录解压缩到 ```$env:PSModulePath``` 指向的正确 PowerShell 模块路径

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

如果需要将此模块置于脱机群集上，请下载 zip，将其移动到目标服务器节点，然后安装该模块。

## <a name="gathering-logs"></a>正在收集日志

启用事件通道并完成安装过程后，可以使用模块中的 SDDCDiagnosticInfo PowerShell cmdlet 获取：

- 报告存储运行状况，以及有关不正常组件的详细信息
- 按池、卷和删除重复的卷的存储容量报告
- 所有群集节点中的事件日志和摘要错误报告

假设你的存储群集名称为 *"CLUS01"。*

针对远程存储群集执行：

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

在群集存储节点上本地执行：

``` PowerShell
Get-SDDCDiagnosticInfo
```

若要将结果保存到指定文件夹：

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

下面是在实际群集上的外观示例：

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

如您所见，脚本还将验证当前群集状态

![数据收集 powershell 屏幕截图](media/data-collection/CollectData.png)

正如您所看到的，所有数据都被写入 SDDCDiagTemp 文件夹

![文件资源管理器屏幕截图中的数据](media/data-collection/CollectDataFolder.png)

脚本完成后，它将在用户目录中创建 ZIP

![powershell 屏幕截图中的数据 zip](media/data-collection/CollectDataResult.png)

让我们将报表生成到文本文件中

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

下面是[示例报表](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt)和[示例 zip](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP)的链接。

若要在 Windows 管理中心中查看此信息（从版本1812开始），请导航到 "*诊断*" 选项卡。如以下屏幕截图所示，可以 

- 安装诊断工具
- 更新它们（如果它们已过期）， 
- 计划每日诊断运行（这对你的系统具有较小的影响，通常会在后台 < 5 分钟，而不会占用超过群集的500mb）
- 查看以前收集的诊断信息，前提是你需要为其提供支持或进行分析。

![wac 诊断屏幕快照](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>SDDCDiagnosticInfo 输出

以下是 SDDCDiagnosticInfo 的压缩输出中包含的文件。

### <a name="health-summary-report"></a>运行状况摘要报表

运行状况摘要报表另存为：
- 0_CloudHealthSummary

此文件是在分析所有收集的数据后生成的，旨在提供系统的快速摘要。 它包含：

- 系统信息
- 存储运行状况概述（节点数、联机资源、群集共享卷联机、不正常组件等）
- 有关不正常组件的详细信息（脱机、失败或联机挂起的群集资源）
- 固件和驱动程序信息
- 池、物理磁盘和卷详细信息
- 存储性能（收集性能计数器）

此报告将不断更新以包含更多有用信息。 有关最新信息，请参阅[GITHUB 自述文件](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md)。

### <a name="logs-and-xml-files"></a>日志和 XML 文件

脚本运行各种日志收集脚本，并将输出保存为 xml 文件。 收集群集和运行状况日志、系统信息（MSInfo32）、未筛选的事件日志（故障转移群集、拆装诊断、hyper-v、存储空间等）以及存储诊断信息（操作日志）。 有关收集的信息的最新信息，请参阅[GITHUB 自述文件（我们收集的内容）](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include)。

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>如何从 PCStorageDiagnosticInfo 使用 XML 文件
你可以使用**PCStorageDiagnosticInfo** cmdlet 收集的数据中提供的 XML 文件中的数据。 这些文件包含有关虚拟磁盘、物理磁盘、基本群集信息和其他 PowerShell 相关输出的信息。 

若要查看这些输出的结果，请打开 PowerShell 窗口，然后运行以下步骤。 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>接下来应该怎么办？
用于分析 SDDC 系统运行状况的大量改进和新的 cmdlet。
通过在[此处](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues)归档问题来提供你想要查看的信息。 此外，还可以通过提交[拉取请求](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls)来随意提供对脚本的有用更改。
