---
title: ServerFiles 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 93eeb0deaa527921db35f4ab955d2ccc46b57d7a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385848"
---
# <a name="the-update-serverfiles-command"></a>ServerFiles 命令



使用存储在服务器的%Windir%\System32\RemInst 文件夹中的最新文件更新 REMINST 共享文件夹中的文件。 若要确保 Windows 部署服务安装的有效性，应在每次服务器升级后运行此命令一次，Service Pack 安装或更新 Windows 部署服务文件。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[/Server： @no__t 名称 >]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|

## <a name="BKMK_examples"></a>示例

若要更新文件，请键入下列内容之一：
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)