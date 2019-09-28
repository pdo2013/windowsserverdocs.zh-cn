---
title: manage-bde 状态
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 235db54ef2361c0e95c66b15a15be7f188fb74d9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373864"
---
# <a name="manage-bde-status"></a>manage-bde：状态



提供有关计算机上所有驱动器的下列信息：是否受 BitLocker 保护：
-   Size
-   BitLocker 版本
-   转换状态
-   加密百分比
-   加密方法
-   保护状态
-   锁定状态
-   标识字段
-   密钥保护程序

有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde -status [<Drive>] [-protectionaserrorlevel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive >|表示驱动器号后跟一个冒号。|
|-protectionaserrorlevel|使 manage-bde 命令行工具在卷受保护时发送返回代码0，并在卷未受保护时返回 1;最常见的批处理脚本用于确定驱动器是否受 BitLocker 保护。 你还可以使用 **-p**作为此命令的缩写形式。|
|-computername|指定 Manage-bde.exe 将用于修改另一台计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。|
|\<名称 >|表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|在命令提示符下显示 brief Help。|
|-help 或-h|在命令提示符下显示完整的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-status**命令显示驱动器 C 的状态。
```
manage-bde –status C:
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)