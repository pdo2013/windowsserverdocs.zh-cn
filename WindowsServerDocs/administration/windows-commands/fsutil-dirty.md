---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: fsutil 脏
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c308b0497a5a39a25384b22441b733143df8727b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852128"
---
# <a name="fsutil-dirty"></a>fsutil 脏
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

查询或设置卷的非正常位。 当卷的脏设置位，则**autochk**的下次重新启动计算机时自动检查卷中的错误。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil dirty {query | set} <VolumePath>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|查询|查询指定的卷的非正常位。|
|设置|设置指定的卷的非正常位。|
|\<VolumePath>|指定驱动器名称后跟冒号或 GUID 格式如下：**卷 {***GUID***}**。|

## <a name="remarks"></a>备注

-   卷的非正常位指示文件系统可能处于不一致的状态。 可以设置非正常位，原因如下：

    -   卷处于联机状态，它具有未完成的更改。

    -   对卷进行了更改并且所做的更改已提交到磁盘之前在计算机已关闭。

    -   在卷上检测到损坏。

-   如果在计算机重新启动时，设置了非正常位**chkdsk**运行以验证文件系统的完整性并尝试与批量修复任何问题。

## <a name="BKMK_examples"></a>示例
若要查询的驱动器 C 上的坏，请键入：

```
fsutil dirty query c:
```

-   如果该卷已更新，会显示以下输出：

    `Volume C: is dirty`

-   如果卷未更新，会显示以下输出：

    `Volume C: is not dirty`

若要在驱动器 C 上设置了非正常位，请键入：

```
fsutil dirty set C:
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


