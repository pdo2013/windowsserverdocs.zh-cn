---
title: 管理 bde 解锁
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a86267890449be2048221940e5955e49f30f99f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814598"
---
# <a name="manage-bde-unlock"></a>管理 bde： 解锁



使用恢复密码或恢复密钥解锁受 BitLocker 保护驱动器。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|ReplTest1|描述|
|---------|-----|-----------|
|-recoverypassword||指定将使用恢复密码以解锁驱动器。缩写形式： rp|
||\<密码 >|表示可用于解锁驱动器的恢复密码。|
|-recoverykey||指定外部恢复密钥文件将用于解锁驱动器。 Abbreviation: -rk|
||\<PathToExternalKeyFile>|表示可用于解锁驱动器的外部恢复密钥文件。|
||\<Drive>|表示驱动器号后, 接一个冒号。|
|-certificate||要 unclock 卷的 BitLocker 证书的本地用户证书位于的位置的用户证书存储中。 Abbreviation: -cert|
||<-cf PathToCertificateFile>|证书文件路径|
||<-ct CertificateThumbprint>|可以根据需要包含 PIN 证书指纹 (-pin)。|
|-password||显示要求提供密码以解锁卷的提示。 缩写形式： pw|
|-computername||指定将使用 bde.exe 来修改在其他计算机上的 BitLocker 保护。 缩写形式： cn|
||\<名称 >|表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?||显示在命令提示符下简短帮助。|
|-help 或-h||显示在命令提示符下完成的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-解锁**命令与另一个驱动器保存到备份文件夹的恢复密钥文件解锁驱动器 E。
```
manage-bde –unlock E: -recoverykey "F:\Backupkeys\recoverykey.bek"
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)