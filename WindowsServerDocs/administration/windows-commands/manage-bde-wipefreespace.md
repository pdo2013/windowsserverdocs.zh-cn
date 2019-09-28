---
title: manage-bde WipeFreeSpace
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 35d5f5fe079485d35fa412502bec745a136fbce9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373780"
---
# <a name="manage-bde-wipefreespace"></a>manage-bde.exeWipeFreeSpace



擦除卷上的可用空间，删除空间中可能存在的任何数据碎片。 在使用 "仅使用的空间" 加密方法进行加密的卷上运行此命令可提供与 "全卷加密" 加密方法相同的保护级别。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive >|表示驱动器号后跟冒号、卷 GUID 路径或装入的卷。|
|-Cancel|取消正在进行的擦除可用空间。|
|-computername|指定 Manage-bde.exe 将用于修改另一台计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。|
|\<名称 >|表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|在命令提示符下显示 brief Help。|
|-help 或-h|在命令提示符下显示完整的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-w**命令创建擦除驱动器 C 上的可用空间。
```
manage-bde -w C:
```
下面的示例演示如何使用带有 **-cancel**参数的 **-w**命令取消擦除驱动器 C 上的可用空间。
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)