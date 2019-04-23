---
title: 隐藏
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fe9cb5dfd8ae6c71fdc72ddc1e8421229f98f5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837468"
---
# <a name="unexpose"></a>隐藏



使用公开的卷影副本 unexposes**公开**命令。 可以通过其卷影 ID、 驱动器号、 共享或装入点指定公开的卷影副本。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ShadowID >|Unexposes 根据给定的卷影 ID 指定的卷影副本|
|\<驱动器： >|Unexposes 与指定的驱动器号 （例如，驱动器 P） 相关联的卷影副本。|
|\<Share>|Unexposes 与指定共享相关联的卷影复制 (例如， \\ \\ *MachineName*\)。|
|\<MountPoint>|Unexposes 与指定的装载点相关联的卷影复制 (例如，C:\shadowcopy\)。|

## <a name="remarks"></a>备注

-   可以使用现有别名或环境变量来代替*ShadowID*。 使用**添加**不带参数，以查看现有别名。

## <a name="BKMK_examples"></a>示例

若要隐藏驱动器 P 与关联的卷影副本，请键入：
```
unexpose P:
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)