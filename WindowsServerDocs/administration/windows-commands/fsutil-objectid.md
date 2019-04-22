---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: Fsutil objectid
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2f5887f20e2c36ec7dcfcd6f4e920b5273c6c60c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813738"
---
# <a name="fsutil-objectid"></a>Fsutil objectid
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理对象标识符 (Oid) 是由分布式链接跟踪 (DLT) 客户端服务和文件复制服务 (FRS) 用于跟踪文件、 目录和链接之类的其他对象的内部对象。 对象标识符对大多数程序不可见，但应永远不会修改。

> [!CAUTION]
> 不要删除、 设置，或以其他方式修改的对象标识符。 删除或设置对象标识符可以导致丢失数据的文件，直至并包括整个卷的数据的部分。 此外，您可能会导致不良行为的分布式链接跟踪 (DLT) 客户端服务和文件复制服务 (FRS)。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil objectid [create] <FileName>
fsutil objectid [delete] <FileName>
fsutil objectid [query] <FileName>
fsutil objectid [set] <ObjectID> <BirthVolumeID> <BirthObjectID> <DomainID> <FileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|创建|如果指定的文件中不具有，创建的对象标识符。 如果文件已包含的对象标识符，此子命令等同于**查询**子命令。|
|“删除”|删除的对象标识符。|
|查询|查询的对象标识符。|
|设置|设置对象标识符。|
|\<ObjectID>|设置特定于文件的长度为 16 字节十六进制标识符保证在卷中是唯一的。 对象标识符由分布式链接跟踪 (DLT) 客户端服务和文件复制服务 (FRS) 用于标识文件。|
|\<BirthVolumeID>|指示的卷的文件被定位时它第一次获得对象标识符。 此值是由 DLT 客户端服务的 16 字节十六进制标识符。|
|\<BirthObjectID>|指示文件的原始对象标识符 ( *ObjectID*移动文件时可能会更改)。 此值是由 DLT 客户端服务的 16 字节十六进制标识符。|
|\<DomainID>|16 字节的十六进制标识符。 此值当前未使用，必须设置为全零。|
|\<FileName>|指定的文件包括文件名和扩展名，例如 C:\documents\filename.txt 的完整路径。|

## <a name="remarks"></a>备注

-   具有对象标识符的任何文件还具有出生卷标识符、 出生对象标识符以及域标识符。 当您移动一个文件时，可能会更改的对象标识符，但出生卷和出生对象标识符保持不变。 此行为使 Windows 操作系统可始终找到文件，无论移动的位置。

## <a name="BKMK_examples"></a>示例
若要创建的对象标识符，请键入：

`fsutil objectid create c:\temp\sample.txt`

若要删除的对象标识符，请键入：

`fsutil objectid delete c:\temp\sample.txt`

若要查询的对象标识符，请键入：

`fsutil objectid query c:\temp\sample.txt`

若要设置的对象标识符，请键入：

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


