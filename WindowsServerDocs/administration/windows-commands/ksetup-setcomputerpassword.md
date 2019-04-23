---
title: ksetup:setcomputerpassword
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0679bb9ee429e05c7679411c5493bd21b530ef8e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831538"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup:setcomputerpassword



设置本地计算机的密码。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<密码 >|使用提供的密码在本地计算机上设置的计算机帐户。</br>仅可以通过使用具有管理权限的帐户设置密码。 密码可以介于 1 到 156 的字母数字或特殊字符。|

## <a name="remarks"></a>备注

此命令会影响的计算机帐户。

必须重新启动计算机以使密码更改才会生效。

不会显示计算机帐户密码，在注册表中或作为从输出**ksetup**命令。

## <a name="BKMK_Examples"></a>示例

从 IPops897 的本地计算机上的计算机帐户密码更改为 IPop 美元 897 ！。
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [命令行语法解答](command-line-syntax-key.md)