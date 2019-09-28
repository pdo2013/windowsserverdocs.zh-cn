---
title: ksetup： setcomputerpassword
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d1d3742476385eb770c9cb5c798c1f6ab27c74f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374944"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup： setcomputerpassword



设置本地计算机的密码。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Password >|使用提供的密码设置本地计算机上的计算机帐户。</br>只能使用具有管理权限的帐户来设置密码。 密码长度可以是1到156个字母数字或特殊字符。|

## <a name="remarks"></a>备注

此命令仅影响计算机帐户。

您必须重新启动计算机才能使密码更改生效。

计算机帐户密码不会在注册表中显示，也不会显示为**ksetup**命令的输出。

## <a name="BKMK_Examples"></a>示例

将本地计算机上的计算机帐户密码从 IPops897 更改为 IPop $ 897！。
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [命令行语法项](command-line-syntax-key.md)