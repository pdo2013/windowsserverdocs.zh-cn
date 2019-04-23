---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: fsutil 修复
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 18c06b1f426105b098a5dc7e992b1e3becd3a4ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846678"
---
# <a name="fsutil-repair"></a>fsutil 修复
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理和监视 NTFS 自修复修复操作。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil repair [enumerate] <volumepath> [<LogName>]
fsutil repair [initiate] <VolumePath> <FileReference>
fsutil repair [query] <VolumePath>
fsutil repair [set] <VolumePath> <Flags>
fsutil repair [wait][<WaitType>] <VolumePath>

```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|枚举|枚举的卷损坏日志的项。|
|\<volumepath>|指定的卷作为驱动器名称后接一个冒号。|
|\<LogName>|$ 损坏-已确认损坏卷中的集。<br />$ 验证的一组潜在的、 未验证卷中的损坏。|
|启动|启动自愈 NTFS。|
|\<FileReference>|指定的 NTFS 卷特定文件 ID （文件参考编号）。 文件引用包含文件的段数量。|
|查询|查询自愈 NTFS 卷的状态。|
|设置|设置卷的自我修复的状态。|
|\<标志 >|指定要设置的数据量的自我修复状态时使用的修复方法。<br /><br />**标志**参数可以设置为三个值：<br /><br />-   **0x01**:启用常规修复。<br />-   **0x09**:有关可能数据丢失，而无需修复会发出警告。<br />-   **0x00**:禁用 NTFS 自修复修复操作。|
|状态|查询的系统或给定卷的损坏状态。|
|等待|等待 repair(s) 才能完成。 如果 NTFS 已检测到它正在其执行修复的卷上的问题，此选项将允许系统等待，直到它在运行任何待执行的脚本之前，修复已完成。|
|[WaitType {0&#124;1}]|指示是否要等待当前修复以完成或等待完成的所有修复。 *WaitType*可设置为以下值：<br /><br />-   **0**:等待完成的所有修复。 （默认值）<br />-   **1**:等待当前的修复，才能完成。|

## <a name="remarks"></a>备注

-   自愈 NTFS 会尝试更正损坏的 NTFS 文件系统处于联机状态，而无需**Chkdsk.exe**运行。 Windows Server 2008 中引入了此功能。 有关详细信息，请参阅[自愈 NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)。

## <a name="BKMK_examples"></a>示例

若要枚举的卷已确认损坏问题，请键入：

```
fsutil repair enumerate C: $Corrupt 
```

若要启用自我修复修复驱动器 C 上的，请键入：

```
fsutil repair set c: 1
```

若要禁用自愈修复驱动器 C 上的，请键入：

```
fsutil repair set c: 0
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[自愈 NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)


