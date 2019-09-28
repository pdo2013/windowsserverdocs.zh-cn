---
title: manage-bde on
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a95bbc375c0a5b62b96f7c68f7d5ab5e09371d1c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374000"
---
# <a name="manage-bde-on"></a>manage-bde：开启



加密驱动器并打开 BitLocker。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive >|表示驱动器号后跟一个冒号。|
|-ms-fve-recoverypassword|添加数字密码保护程序。 你还可以使用 **-rp**作为此命令的缩写形式。|
|\<NumericalPassword >|表示恢复密码。|
|-recoverykey|添加用于恢复的外部密钥保护程序。 你还可以使用 **-rk**作为此命令的缩写形式。|
|\<PathToExternalDirectory >|表示恢复密钥的目录路径。|
|-启动|添加用于启动的外部密钥保护程序。 你还可以使用 **-sk**作为此命令的缩写形式。|
|\<PathToExternalKeyDirectory >|表示启动密钥的目录路径。|
|-证书|为数据驱动器添加公钥保护程序。 你还可以使用 **-cert**作为此命令的缩写形式。|
|-tpmandpin|为操作系统驱动器添加受信任的平台模块（TPM）和个人标识号（PIN）保护程序。 你还可以使用 **-tp**作为此命令的缩写形式。|
|-tpmandstartupkey|添加操作系统驱动器的 TPM 和启动密钥保护程序。 你还可以使用 **-tsk**作为此命令的缩写形式。|
|-tpmandpinandstartupkey|为操作系统驱动器添加 TPM、PIN 和启动密钥保护程序。 你还可以使用 **-tpsk**作为此命令的缩写形式。|
|-password|添加数据驱动器的密码密钥保护程序。 你还可以使用 **-pw**作为此命令的缩写形式。|
|-ADAccountOrGroup|为卷添加基于 SID 的标识保护程序。 如果用户或计算机具有正确的凭据，则该卷将自动解锁。 指定计算机帐户时，将 **$** 追加到计算机名称，并指定 **– service**以指示应在 BitLocker 服务器的内容中而不是用户的情况下进行解锁。 你还可以使用 **-sid**作为此命令的缩写形式。|
|-UsedSpaceOnly|将加密模式设置为仅加密已用空间。 包含已用空间的卷部分将被加密，但可用空间不会被加密。 如果未指定此选项，则卷上的所有已用空间和可用空间都将加密。 你还可以将**用作**此命令的缩写形式。|
|-encryptionMethod|配置加密算法和密钥大小。 你还可以使用 **-em**作为此命令的缩写形式。|
|-skiphardwaretest|开始加密而不进行硬件测试。 你还可以使用 **-s**作为此命令的缩写形式。|
|-discoveryvolumetype|指定要用于发现数据驱动器的文件系统。 发现数据驱动器是添加到受 BitLocker 保护的 FAT 格式可移动数据驱动器的隐藏驱动器，其中包含 BitLocker To Go 读取器以便 Windows Vista 或 Windows XP 操作系统可用于查看受 BitLocker 保护的驱动器。|
|-ForceEncryptionType|强制 BitLocker 使用软件或硬件加密。 可以将**硬件**或**软件**指定为加密类型。 如果选择了**硬件**参数，但驱动器不支持硬件加密，则 manage-bde 将返回错误。 如果组策略设置禁止指定的加密类型，则 manage-bde 将返回错误。 你还可以使用 **-fet**作为此命令的缩写形式。|
|-RemoveVolumeShadowCopies|强制 deletikon 卷影副本的卷影副本。 运行此命令后，你将无法使用以前的系统还原点还原此卷。 你还可以使用 **-rvsc**作为此命令的缩写形式。|
|\<FileSystemType >|指定可与发现数据驱动器一起使用的文件系统：FAT32、default 或 none。|
|-computername|指定正在使用 manage-bde 修改其他计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。|
|\<名称 >|表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|在命令提示符下显示 brief Help。|
|-help 或-h|在命令提示符下显示完整的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-on**命令为驱动器 C 启用 BitLocker 并将恢复密码添加到驱动器。
```
manage-bde –on C: -recoverypassword
```
下面的示例演示如何使用 **-on**命令打开驱动器 C 的 BitLocker，将恢复密码添加到驱动器，并将恢复密钥保存到驱动器 E。
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
下面**的示例**演示如何使用外部密钥保护程序（例如 USB 密钥）来打开驱动器 C 的 BitLocker，以便解锁操作系统驱动器。 如果使用的是没有 TPM 的计算机，则此方法是必需的。
```
manage-bde -on C: -startupkey E:\
```
下面的示例演示如何使用 **-on**命令来打开数据驱动器 E 的 BitLocker 并添加密码密钥保护程序。 在输入此命令后，manage-bde 会提示你输入密码。
```
manage-bde –on E: -pw
```
下面的示例演示如何使用 **-on**命令为操作系统驱动器 C 启用 BitLocker 并使用基于硬件的加密。
```
manage-bde –on C: -fet Hardware
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)