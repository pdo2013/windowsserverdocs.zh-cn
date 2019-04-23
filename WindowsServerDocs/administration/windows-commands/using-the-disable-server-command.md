---
title: 使用禁用服务器命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b229146206c1fbe6ce8b6f585b2ff9b50ae6104
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853028"
---
# <a name="using-the-disable-server-command"></a>使用禁用服务器命令



禁用 Windows 部署服务服务器的所有服务。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /Disable-Server [/Server:<Server name>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|

## <a name="BKMK_examples"></a>示例

若要禁用服务器，请运行以下项之一：
```
WDSUTIL /Disable-Server
WDSUTIL /Verbose /Disable-Server /Server:MyWDSServer
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

