---
title: ksetup： removerealm
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11858d8a24d4f125c83b3e4092ac48f336a9ef0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374955"
---
# <a name="ksetupremoverealm"></a>ksetup： removerealm



从注册表中删除指定领域的所有信息。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<RealmName >|领域名称被声明为大写的 DNS 名称，例如 CORP。CONTOSO.COM，在**ksetup**运行时，它将作为默认领域列出。|

## <a name="remarks"></a>备注

领域名称存储在注册表中的两个位置：**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001**和 **\CurrentControlSet\Control\Lsa\Kerberos**。

你无法从域控制器中删除默认领域名称，因为这会重置其 DNS 信息，删除它可能会使域控制器不可用。

## <a name="BKMK_Examples"></a>示例

错误地将本地计算机上的 ".COM" 拼写错误设置为 CORP 的领域名称。CONTOSO.图标
```
ksetup /setrealm CORP.CONTOSO.CON
```
从本地计算机中删除该错误领域名：
```
ksetup /removerealm CORP.CONTOSO.CON
```
通过运行**ksetup**验证删除，并查看输出。

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [命令行语法项](command-line-syntax-key.md)