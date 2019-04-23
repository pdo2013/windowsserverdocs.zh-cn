---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: Fsutil hardlink
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 69474bd1817471176598afba508cd80c8fa1df8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840048"
---
# <a name="fsutil-hardlink"></a>Fsutil hardlink
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

创建现有文件和一个新的文件之间的硬链接。

## <a name="syntax"></a>语法

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|创建|建立之间现有的文件和一个新文件的 NTFS 硬链接。 （NTFS 硬链接是类似于 POSIX 硬链接。）|
|\<NewFileName>|指定你想要创建的硬链接的文件。|
|\<ExistingFileName>|指定你想要创建硬链接的文件。|
|列表|列出了对硬链接*文件名*。<br /><br />此参数适用于：Windows Server 2008 R2 和 Windows 7。|

## <a name="remarks"></a>备注

-   硬链接是一个文件的目录条目。 每个文件可以被视为具有至少一个硬链接。 在 NTFS 卷上的每个文件可以有多个硬链接，因此单个文件可以出现在多个目录 （或甚至具有不同名称的同一目录中）。 由于所有链接引用同一个文件，程序可以打开的任何链接，并修改该文件。 只有在所有链接到它已被都删除后，将从文件系统都删除文件。 创建硬链接后，程序可以使用它像任何其他文件名称。

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


