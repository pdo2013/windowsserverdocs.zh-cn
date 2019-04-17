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
ms.openlocfilehash: 71119174c7fc5929eb4e1982183ba0aed7eb1735
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081872"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin 删除阴影

>适用于： Windows 10、 Windows 8.1、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

删除指定的卷的卷影副本。

## <a name="syntax"></a>语法

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>参数

|参数|描述|
|---|---|
|/ = \ < ForVolumeSpec >|指定将删除的卷的卷影副本。|
|/ 最早|删除仅旧的卷影副本。|
|/all|删除所有指定的卷的卷影副本。|
|/ 阴影 = \ < ShadowID >|删除指定 ShadowID 卷影副本。 要获取的卷影副本 ID，请使用**vssadmin 列表阴影**命令。 输入卷影副本 ID 时，使用以下格式，其中每个*X*代表十六进制字符：<br><br>XXXXXXXX-格式的格式的格式-XXXXXXXXXXXX|
|/ 安静|指定该命令不会显示在运行时的邮件。|

## <a name="remarks"></a>备注

您只能删除卷影副本与客户端访问的类型。

## <a name="examples"></a>示例

若要删除最早的卷影副本的卷 C，输入以下命令：

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>其他参考

* [命令行语法键](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin 列表阴影](vssadmin-list-shadows.md)