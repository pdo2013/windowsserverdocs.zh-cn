---
title: 管理 bde WipeFreeSpace
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cf99a9124f78189de223018608d9864e51d7897
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564694"
---
# <a name="manage-bde-wipefreespace"></a>管理-bde:WipeFreeSpace



擦除删除任何可能已存在于的空间中的数据片段的卷上的可用空间。 使用"仅已用空间"加密方法加密的卷上运行此命令可提供相同级别的保护"全卷加密"加密方法。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive>|表示驱动器号后跟一个冒号，卷 GUID 路径或已装入的卷。|
|-Cancel|取消擦除的过程中的可用空间。|
|-computername|指定将使用 bde.exe 来修改在其他计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。|
|\<名称 >|表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|显示在命令提示符下简短帮助。|
|-help 或-h|显示在命令提示符下完成的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-w**命令以创建擦除驱动器 C 上的可用空间
```
manage-bde -w C:
```
下面的示例演示如何使用 **-w**命令 **-取消**参数来取消擦除驱动器 C 上的可用空间
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)