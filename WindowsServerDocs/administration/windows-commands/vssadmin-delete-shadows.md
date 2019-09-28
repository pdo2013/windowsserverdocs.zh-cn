---
title: Vssadmin 删除阴影
description: Vssadmin 删除 shadows 命令的说明。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9779da98ecb43245fe206390d9b70471f15d706e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362612"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin 删除阴影

>适用于：Windows 10，Windows 8.1，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows Server 2008

删除指定卷的卷影副本。

## <a name="syntax"></a>语法

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---|---|
|/for = \<ForVolumeSpec >|指定将删除的卷影副本。|
|/oldest|仅删除最旧的卷影副本。|
|/all|删除指定卷的所有卷影副本。|
|/shadow = \<ShadowID >|删除由 ShadowID 指定的卷影副本。 若要获取卷影副本 ID，请使用**vssadmin list shadows**命令。 输入卷影副本 ID 时，请使用以下格式，其中每个*X*表示十六进制字符：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX （XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX）|
|/quiet|指定命令在运行时不显示消息。|

## <a name="remarks"></a>备注

只能删除具有客户端可访问的类型的卷影副本。

## <a name="examples"></a>示例

若要删除最旧的卷影副本，请输入以下命令：

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>其他参考

* [命令行语法关键字](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [List](vssadmin.md)
* [Vssadmin list shadows](vssadmin-list-shadows.md)