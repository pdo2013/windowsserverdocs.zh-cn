---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: Fsutil volume
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c4496cfec94823ae177bc6de4fac83dc977fb61d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376705"
---
# <a name="fsutil-volume"></a>Fsutil volume
>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10，Windows Server 2012 R2，Windows 8.1，Windows Server 2012，Windows 8，Windows Server 2008 R2，Windows 7

卸载卷，或查询硬盘驱动器，确定硬盘驱动器上当前可用的可用空间量，或使用特定群集的文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil volume [allocationreport] <VolumePath>
fsutil volume [diskfree] <VolumePath>
fsutil volume [dismount] <VolumePath>
fsutil volume [filelayout] <VolumePath> <fileid>
fsutil volume [list]
fsutil volume [querycluster] <VolumePath> <Cluster> [<Cluster>] … …
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|allocationreport|显示有关在给定卷上如何使用存储的信息。|
|\<VolumePath >|指定驱动器号（后跟冒号）。|
|diskfree|查询硬盘驱动器以确定其上的可用空间量。|
|卸载|卸载卷。|
|filelayout|显示给定文件的 NTFS 元数据。|
|\<fileid >|指定文件 id。|
|list|列出系统上的所有卷。|
|querycluster|查找使用指定群集的文件。 可以指定包含**querycluster**参数的多个群集。<br /><br />此参数适用于：Windows Server 2008 R2 和 Windows 7。|
|\<cluster >|指定逻辑群集号（LCN）。|

## <a name="BKMK_examples"></a>示例
若要显示分配的群集报告，请键入：

```
fsutil volume allocationreport C:
```

若要卸除驱动器 C 上的卷，请键入：

```
fsutil volume dismount c:
```

若要查询驱动器 C 上卷的可用空间量，请键入：

```
fsutil volume diskfree c:
```

若要显示有关指定文件的所有信息，请键入：

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

若要列出磁盘上的卷，请键入：

```
fsutil volume list
```

若要在驱动器 C 上查找使用由逻辑群集号50和0x2000 指定的群集的文件，请键入：

```
fsutil volume querycluster C: 50 0x2000
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS 的工作方式](https://go.microsoft.com/fwlink/?LinkId=183396)


