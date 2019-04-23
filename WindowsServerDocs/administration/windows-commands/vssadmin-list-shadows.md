---
title: List shadows
description: Vssadmin 列表的说明将隐藏命令。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861148"
---
# <a name="vssadmin-list-shadows"></a>List shadows

>适用于：Windows 10 中，Windows 8.1，Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

列出指定卷的所有现有卷影副本。 如果使用此命令不带参数，它显示由指定的顺序计算机上的所有卷影副本**卷影副本设置**。

## <a name="syntax"></a>语法

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---|---|
|/for=\<ForVolumeSpec>|指定卷影副本不会为列出的卷。|
|/shadow=\<ShadowID>|列出了由 ShadowID 指定的卷影副本。 若要获取卷影副本 ID，请使用**list shadows**命令。 当卷影副本 id，使用以下格式，其中每个*X*表示十六进制字符：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>其他参考

* [命令行语法解答](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [vssadmin](vssadmin.md)