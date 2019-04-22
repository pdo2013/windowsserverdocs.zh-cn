---
title: 更新 ServerFiles 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ec96e2ba9aea14ed9a203dabbb697187736b33a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817438"
---
# <a name="the-update-serverfiles-command"></a>更新 ServerFiles 命令



通过使用最新的文件存储在服务器的 %Windir%\System32\RemInst 文件夹中的更新 REMINST 共享文件夹中的文件。 若要确保你的 Windows 部署服务安装的有效性，您应运行此命令一次每个服务器升级后，安装 service pack 或更新到 Windows 部署服务文件。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|

## <a name="BKMK_examples"></a>示例

若要更新的文件，请键入以下项之一：
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)