---
title: 让
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 819484364e8375c4d58e4d022681eedeaa7084ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377279"
---
# <a name="expose"></a>让



将永久性卷影副本作为驱动器号、共享或装入点公开。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|ShadowID|指定要公开的卷影副本的卷影副本 ID。|
|\<Drive： >|以驱动器号（例如，P：）公开指定的卷影副本。|
|\<Share >|公开共享中的指定卷影副本（例如 \\ @ no__t *，@no__t-* 3。|
|\<MountPoint >|向装入点公开指定的卷影副本（例如，C:\shadowcopy @ no__t-0。|

## <a name="remarks"></a>备注

-   您可以使用现有的别名或环境变量来代替*ShadowID*。 使用**add**而不使用参数查看现有别名。

## <a name="BKMK_examples"></a>示例

若要将与 VSS_SHADOW_1 环境变量关联的持久影子副本作为驱动器 X 公开，请键入：
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)