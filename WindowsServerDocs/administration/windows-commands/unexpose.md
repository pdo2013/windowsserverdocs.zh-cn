---
title: 隐藏
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e10126739ef82b060e271e9b804a77658b5ec82
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392262"
---
# <a name="unexpose"></a>隐藏



unexposes 使用**公开**命令公开的卷影副本。 公开的卷影副本可通过其影子 ID、驱动器号、共享或装入点来指定。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ShadowID >|Unexposes 指定的阴影 ID 指定的卷影副本。|
|\<Drive： >|Unexposes 与指定驱动器号（例如，drive P）关联的卷影副本。|
|\<Share >|Unexposes 与指定共享关联的卷影副本（例如 \\ @ no__t-1*MachineName*\)。|
|\<MountPoint >|Unexposes 与指定装入点关联的卷影副本（例如，C:\shadowcopy @ no__t-0。|

## <a name="remarks"></a>备注

-   您可以使用现有的别名或环境变量来代替*ShadowID*。 使用**add**而不使用参数查看现有别名。

## <a name="BKMK_examples"></a>示例

若要隐藏与 Drive P 关联的卷影副本，请键入：
```
unexpose P:
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)