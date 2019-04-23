---
title: regsvr32
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87d9291755ddb4484e85248cb01ad78b01a25965
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889988"
---
# <a name="regsvr32"></a>regsvr32



将.dll 文件注册为在注册表中的命令组成。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/u|取消注册服务器。|
|/s|运行**Regsvr32**而不会显示消息。|
|/n|运行**Regsvr32**而不调用**DllRegisterServer**。 (需要 **/i**参数。)|
|/i:\<cmdline>|将传递一个可选的命令行字符串 (*cmdline*) 到**DllInstall**。 如果结合使用此参数 **/u**参数，它将调用**DllUninstall**。|
|\<DllName>|将注册的.dll 文件的名称。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要注册 Active Directory 架构.dll 文件，请键入：
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)