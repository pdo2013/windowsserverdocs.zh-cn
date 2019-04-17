---
title: Vssadmin 列表阴影
description: Vssadmin 列表的说明隐藏命令。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081963"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin 列表阴影

>适用于： Windows 10、 Windows 8.1、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

列出所有现有的指定卷的卷影副本。 如果您使用不带参数的此命令，它指明由**卷影副本集**的顺序中的计算机上显示所有的卷影副本。

## <a name="syntax"></a>语法

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>参数

|参数|描述|
|---|---|
|/ = \ < ForVolumeSpec >|指定将为列出卷影副本的卷。|
|/ 阴影 = \ < ShadowID >|列出了由 ShadowID 指定卷影副本。 要获取的卷影副本 ID，请使用**vssadmin 列表阴影**命令。 键入卷影副本 ID 时，使用以下格式，其中每个*X*代表十六进制字符：<br><br>XXXXXXXX-格式的格式的格式-XXXXXXXXXXXX|

## <a name="additional-references"></a>其他参考

* [命令行语法键](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)