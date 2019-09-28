---
title: regsvr32
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 444af0ccf7c9bbe21c013f32b396997b7cb2e00f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371632"
---
# <a name="regsvr32"></a>regsvr32



将 .dll 文件注册为注册表中的命令组件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/u|注销服务器。|
|/s|运行**Regsvr32**而不显示消息。|
|/n|在不调用**DllRegisterServer**的情况下运行**Regsvr32** 。 （需要 **/i**参数。）|
|/i： \<cmdline >|将可选的命令行字符串（*cmdline*）传递给**DllInstall**。 如果将此参数与 **/u**参数一起使用，则它将调用**DllUninstall**。|
|\<DllName >|要注册的 .dll 文件的名称。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要为 Active Directory 架构注册 .dll，请键入：
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)