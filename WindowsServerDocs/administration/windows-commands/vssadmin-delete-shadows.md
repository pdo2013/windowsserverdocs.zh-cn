---
title: Vssadmin 删除阴影
description: Vssadmin 删除阴影命令的说明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 83074017e4ae412cf0aec654f6ab5901ad8039e2
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63706619"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin 删除阴影

>适用于：Windows 10 中，Windows 8.1，Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

删除指定的卷的卷影副本。

## <a name="syntax"></a>语法

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---|---|
|/for=\<ForVolumeSpec>|指定将删除的卷的卷影副本。|
|/oldest|删除仅最旧的卷影副本。|
|/all|将删除所有指定的卷的卷影副本。|
|/shadow=\<ShadowID>|删除指定的 ShadowID 卷影副本。 若要获取卷影副本 ID，请使用**list shadows**命令。 当您输入的卷影副本 ID 时，使用以下格式，其中每个*X*表示十六进制字符：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|指定该命令将不显示运行时的消息。|

## <a name="remarks"></a>备注

与客户端可以访问的类型，可以仅删除卷影副本。

## <a name="examples"></a>示例

若要删除最旧的卷影副本的卷 C，输入以下命令：

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>其他参考

* [命令行语法解答](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [vssadmin](vssadmin.md)
* [List shadows](vssadmin-list-shadows.md)