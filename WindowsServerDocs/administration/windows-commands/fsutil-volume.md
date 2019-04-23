---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: fsutil 卷
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: a8576dce4be639a516f8898e78bb6db12c91e171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882448"
---
# <a name="fsutil-volume"></a>fsutil 卷
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

卸除卷，或查询来确定多少可用空间是在硬盘驱动器上当前可用或哪些文件正在使用某个特定分类硬盘驱动器。

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
|allocationreport|显示有关如何在给定卷上使用存储的信息。|
|\<VolumePath>|指定 （后跟一个冒号） 的驱动器号。|
|diskfree|查询硬盘驱动器上以确定其上的可用空间量。|
|卸载|卸除卷。|
|filelayout|显示给定文件的 NTFS 元数据。|
|\<fileid>|指定的文件 id。|
|列表|列出所有系统上的卷。|
|querycluster|查找哪些文件使用指定的群集。 您可以指定多个群集**querycluster**参数。<br /><br />此参数适用于：Windows Server 2008 R2 和 Windows 7。|
|\<cluster>|指定逻辑群集数 (LCN)。|

## <a name="BKMK_examples"></a>示例
若要显示已分配的群集报表，请键入：

```
fsutil volume allocationreport C:
```

若要卸载驱动器 C 上的卷，请键入：

```
fsutil volume dismount c:
```

若要查询的驱动器 C 上的卷的可用空间量，请键入：

```
fsutil volume diskfree c:
```

若要显示有关指定的文件的所有信息，请键入：

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

若要列出在磁盘上的卷，请键入：

```
fsutil volume list
```

若要查找使用指定的逻辑群集数字 50 和 0x2000，驱动器 C 上的群集的文件类型：

```
fsutil volume querycluster C: 50 0x2000
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS 的工作原理](https://go.microsoft.com/fwlink/?LinkId=183396)


