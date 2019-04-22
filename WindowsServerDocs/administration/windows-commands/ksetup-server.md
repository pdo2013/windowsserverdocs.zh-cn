---
title: ksetup:server
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f370d4dede278e1facdda829503ea3793502b9e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814568"
---
# <a name="ksetupserver"></a>ksetup:server



允许您指定的名称，以便通过使用所做的更改在运行 Windows 操作系统的计算机**ksetup**将更新目标计算机。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ServerName>|完整的计算机名称的配置将能有效，如 IPops897.corp.contoso.com。</br>如果不完整的完全限定指定域的计算机名称，该命令将失败。|

## <a name="remarks"></a>备注

无法删除目标的服务器名称;仅可以返回到本地计算机名称，这是默认设置来更改它。

目标服务器名称存储在注册表中**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**。 它不通过使用报告**ksetup**。

## <a name="BKMK_Examples"></a>示例

使你**ksetup**有效连接到 Contoso 域的 IPops897 计算机上的配置：
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [命令行语法解答](command-line-syntax-key.md)