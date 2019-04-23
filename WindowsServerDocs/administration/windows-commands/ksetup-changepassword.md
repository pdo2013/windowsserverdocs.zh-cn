---
title: ksetup:changepassword
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f629c6c7930777583df38f5af900ed380ec60f9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878528"
---
# <a name="ksetupchangepassword"></a>ksetup:changepassword



使用密钥分发中心 (KDC) 密码 (kpasswd) 值来更改登录的用户的密码。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<OldPasswd>|状态已登录用户的现有密码。|
|\<NewPasswd>|状态已登录用户的新密码。|

## <a name="remarks"></a>备注

此命令使用 KDC 密码 (kpasswd) 值，若要更改登录的用户的密码。 Kpasswd，如果设置，将显示在输出中通过运行**ksetup /dumpstate**命令。

用户的新密码必须满足此计算机设置的所有密码要求。

如果在当前域中找不到用户帐户，系统将要求您提供的用户帐户所在的域名。

如果你想要强制在下次登录时更改密码，此命令将允许使用星号 （*），因此将提示用户输入新密码。

命令的输出会通知你的成功或失败状态。

## <a name="BKMK_Examples"></a>示例

更改当前已登录到此域中的此计算机的用户的密码：
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
更改当前登录 Contoso 域中的用户的密码：
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
强制当前登录的用户下次登录时更改密码：
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)