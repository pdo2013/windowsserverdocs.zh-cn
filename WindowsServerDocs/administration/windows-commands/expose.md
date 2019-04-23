---
title: 公开
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51cc744bc2b61862ed05ca2e7d0aaa8f70d38692
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886658"
---
# <a name="expose"></a>公开



公开为驱动器号、 共享或装入点的永久性卷影副本。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|ShadowID|指定你想要公开的卷影副本的卷影 ID。|
|\<驱动器： >|公开为驱动器号 （例如，p:） 指定的卷影副本。|
|\<Share>|公开指定的卷影副本在一个共享 (例如， \\ \\ *MachineName*\)。|
|\<MountPoint>|公开指定的卷影复制到装入点 (例如，C:\shadowcopy\)。|

## <a name="remarks"></a>备注

-   可以使用现有别名或环境变量来代替*ShadowID*。 使用**添加**不带参数，以查看现有别名。

## <a name="BKMK_examples"></a>示例

若要公开与 VSS_SHADOW_1 环境变量关联作为驱动器 X 的永久性卷影副本，请键入：
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)