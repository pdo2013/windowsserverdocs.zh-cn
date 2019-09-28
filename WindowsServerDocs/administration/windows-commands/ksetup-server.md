---
title: ksetup：服务器
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: dd05fd294640c63e633b7b866307197ae6770476
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374959"
---
# <a name="ksetupserver"></a>ksetup：服务器



允许您为运行 Windows 操作系统的计算机指定一个名称，以便使用**ksetup**进行的更改将更新目标计算机。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ServerName >|配置将对其有效的完整计算机名称，例如 IPops897.corp.contoso.com。</br>如果指定了不完整的完全限定的域计算机名称，则该命令将失败。|

## <a name="remarks"></a>备注

无法删除目标服务器名称;您只能将其更改回本地计算机名称，这是默认值。

目标服务器名称存储在**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**中的注册表中。 不能使用**ksetup**报告它。

## <a name="BKMK_Examples"></a>示例

使**ksetup**配置在连接到 Contoso 域的 IPops897 计算机上生效：
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [命令行语法项](command-line-syntax-key.md)