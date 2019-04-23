---
title: subst
description: 了解如何将路径相关联的驱动器号。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 858195de89ca8661cf47c25b6cf9b519cc4efbf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858068"
---
# <a name="subst"></a>subst



将路径关联的驱动器号。 如果使用不带参数， **subst**实际上显示的虚拟驱动器的名称。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive1 >:|指定你想要分配路径的虚拟驱动器。|
|[\<Drive2>:]\<Path>|指定的物理驱动器和你想要分配到的虚拟驱动器的路径。|
|/d|删除被替换的 （虚拟） 驱动器。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   以下命令不起作用，因此不应在中指定的驱动器上**subst**命令：

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   *Drive1*参数必须是在由指定的范围内**lastdrive**命令。 如果不是， **subst**显示以下错误消息：

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>示例

若要创建的虚拟驱动器 Z B:\User\Betty\Forms 的路径，请键入：
```
subst z: b:\user\betty\forms 
```
而不键入完整路径，可以通过键入后接一个冒号，如下所示的虚拟驱动器号到达此目录：
```
z: 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)