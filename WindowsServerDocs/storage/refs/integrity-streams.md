---
title: ReFS 完整性流
description: ''
author: gawatu
ms.author: jgerend
manager: dmoss
ms.date: 10/16/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.assetid: 1f1215cd-404f-42f2-b55f-3888294d8a1f
ms.openlocfilehash: 11f0a696fb843f5cd8b4a7ff3318c28d6c1adeb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871338"
---
# <a name="refs-integrity-streams"></a>ReFS 完整性流
>适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012，Windows Server （半年频道），Windows 10

完整性流是 ReFS 中的可选功能，可使用校验和验证和维护数据完整性。 尽管 ReFS 始终将校验和用于元数据，但默认情况下，ReFS 不会生成或验证文件数据的校验和。 完整性流是一项可让用户将校验和用于文件数据的可选功能。 启用完整性流后，ReFS 可以清楚地确定数据有效还是已损坏。 此外，ReFS 和存储空间还可以一起自动更正损坏的元数据和数据。

## <a name="how-it-works"></a>工作原理 

可为单独的文件、目录或整个卷启用完整性流，并且随时可以切换完整性流设置。 此外，针对文件和目录的完整性流设置继承自其父目录。 

启用完整性流后，ReFS 将在该文件的元数据中创建和保留针对指定文件的校验和。 此校验和支持 ReFS 在访问数据前验证它的完整性。 返回启用了完整性流的任何数据前，ReFS 将首先计算其校验和：

![计算校验和文件数据](media/compute-checksum.gif)

然后将此校验和与包含在文件元数据中的校验和进行比较。 如果校验和都匹配，则数据标记为有效并返回给用户。 如果校验和不匹配，则数据损坏。 卷的复原确定 ReFS 响应损坏的方式：

- 如果在非复原的简单空间或裸机驱动器上装载了 ReFS，ReFS 无需返回损坏的数据即可向用户返回错误。 
- 如果 ReFS 在复原镜像或奇偶校验空间上装载，ReFS 将尝试更正损坏。 
    - 如果该尝试成功，ReFS 将应用更正写入来还原数据完整性，然后将有效数据返回应用程序。 应用程序依旧不会意识到任何损坏。
    - 如果该尝试不成功，ReFS 将返回一个错误。 

ReFS 将在系统事件日志中记录所有损坏，并且日志将反映损坏是否已修复。 

![纠正写还原数据的完整性](media/corrective-write.gif)

## <a name="performance"></a>性能 

尽管完整性流为系统提供更全面的数据完整性，它也会产生性能成本。 以下是造成这一现象的不同原因：
- 如果启用了完整性流，所有写入操作将成为根据写入进行分配的操作。 尽管这可以避免所有读取-修改-写入瓶颈，这是因为 ReFS 不需要读取或修改任何现有数据，但数据经常会碎片化，延迟读取时间。 
- 根据系统的工作负载和基础存储，对校验和进行计算和验证的计算成本可能导致 IO 延迟增加。 

因为完整性流会带来性能成本，我们建议在性能需求较高的系统上使完整性流保持禁用状态。 

## <a name="integrity-scrubber"></a>完整性清理器

如上所述，ReFS 在访问任何数据前将自动验证数据的完整性。 此外，ReFS 还使用了后台清理器，这可以让 ReFS 验证不经常访问的数据。 此清理器会定期扫描卷，从而识别潜在损坏，然后主动触发任何损坏数据的修复。

  >[!NOTE]
  >数据完整性清理器只能在启用完整性流的情况下验证文件数据。

默认情况下，清理器每四周运行一次，但可在“Microsoft\Windows\数据完整性扫描”下的“任务计划程序”中配置时间间隔。 

## <a name="examples"></a>示例
若要监视和更改文件数据完整性设置，ReFS 使用 **Get-FileIntegrity** 和 **Set-FileIntegrity** cmdlet。

### <a name="get-fileintegrity"></a>Get-FileIntegrity
若要查看文件数据是否启用了完整性流，请使用 **Get-FileIntegrity** cmdlet。 

```PowerShell
PS C:\> Get-FileIntegrity -FileName 'C:\Docs\TextDocument.txt'
```

还可使用 **Get-Item** cmdlet 获取指定目录中所有文件的完整性流设置。 

```PowerShell
PS C:\> Get-Item -Path 'C:\Docs\*' | Get-FileIntegrity
```

### <a name="set-fileintegrity"></a>Set-FileIntegrity
若要启用/禁用文件数据的完整性流，请使用 **Set-FileIntegrity** cmdlet。 

```PowerShell
PS C:\> Set-FileIntegrity -FileName 'H:\Docs\TextDocument.txt' -Enable $True
```

还可使用 **Get-Item** cmdlet 设置指定文件夹中所有文件的完整性流设置。 

```PowerShell
PS C:\> Get-Item -Path 'H\Docs\*' | Set-FileIntegrity -Enable $True 
```

**Set-FileIntegrity** cmdlet 也可直接在卷和目录上使用。 

```PowerShell
PS C:\> Set-FileIntegrity H:\ -Enable $True
PS C:\> Set-FileIntegrity H:\Docs -Enable $True
```

## <a name="see-also"></a>请参阅

-   [ReFS 概述](refs-overview.md)
-   [ReFS 块克隆](block-cloning.md)
-   [存储空间直通概述](../storage-spaces/storage-spaces-direct-overview.md)
