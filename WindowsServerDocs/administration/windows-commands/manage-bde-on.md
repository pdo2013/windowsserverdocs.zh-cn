---
title: 管理 bde 上
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b50cad64025e85824a8f0a27d773ffb614491fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841178"
---
# <a name="manage-bde-on"></a>管理 bde： 上



将加密驱动器并打开 BitLocker 时。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

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
|\<Drive>|表示驱动器号后, 接一个冒号。|
|-recoverypassword|添加数字密码保护程序。 此外可以使用 **-rp**作为此命令的简化版本。|
|\<NumericalPassword>|表示恢复密码。|
|-recoverykey|添加外部密钥保护程序进行恢复。 此外可以使用 **-rk**作为此命令的简化版本。|
|\<PathToExternalDirectory>|表示恢复密钥的目录路径。|
|-startupkey|添加启动外部密钥保护程序。 此外可以使用 **-sk**作为此命令的简化版本。|
|\<PathToExternalKeyDirectory>|表示启动密钥的目录路径。|
|-certificate|添加对数据驱动器的密钥保护程序。 此外可以使用 **-证书**作为此命令的简化版本。|
|-tpmandpin|添加受信任的平台模块 (TPM) 和个人识别号 (PIN) 保护程序的操作系统驱动器。 此外可以使用 **-tp**作为此命令的简化版本。|
|-tpmandstartupkey|将添加为操作系统驱动器的 TPM 和启动密钥保护程序。 此外可以使用 **-tsk**作为此命令的简化版本。|
|-tpmandpinandstartupkey|添加 TPM、 PIN 和启动操作系统驱动器的密钥保护程序。 此外可以使用 **-tpsk**作为此命令的简化版本。|
|-password|添加密码密钥保护程序对数据驱动器。 此外可以使用 **-pw**作为此命令的简化版本。|
|-ADAccountOrGroup|添加卷的基于 SID 的标识保护程序。 如果用户或计算机具有正确的凭据，将自动解锁该卷。 当指定了计算机帐户，将**$** 到的计算机名称，并指定 **– 服务**以指示应 BitLocker 服务器而不是内容中解锁用户。 此外可以使用 **-sid**作为此命令的简化版本。|
|-UsedSpaceOnly|将加密模式设置为仅已用空间加密。 包含已用的空间的卷的部分将进行加密，但可用空间将不会。 如果未指定此选项，所有已用空间和将加密的卷上的可用空间... 此外可以使用 **-使用**作为此命令的简化版本。|
|-encryptionMethod|配置的加密算法和密钥大小。 此外可以使用 **-em**作为此命令的简化版本。|
|-skiphardwaretest|开始硬件测试情况下的加密。 此外可以使用 **-s**作为此命令的简化版本。|
|-discoveryvolumetype|指定要用于发现数据驱动器的文件系统。 发现数据驱动器是一个隐藏的驱动器添加到在 FAT 格式，受 BitLocker 保护的可移动数据驱动器，包括 BitLocker To Go 阅读器，使 Windows Vista 或 Windows XP 操作系统可用于查看受 BitLocker 保护的驱动器。|
|-ForceEncryptionType|强制使用软件或硬件加密的 BitLocker。 您可以指定**硬件**或**软件**作为加密类型。 如果**硬件**选择参数，但驱动器不支持硬件加密，管理 bde 将返回错误。 如果组策略设置禁止指定的加密类型，管理 bde 将返回错误。 此外可以使用 **-fet**作为此命令的简化版本。|
|-RemoveVolumeShadowCopies|强制 deletikon 的卷影副本卷。 你将无法再还原此卷运行此命令后使用以前的系统还原点。 此外可以使用 **-rvsc**作为此命令的简化版本。|
|\<FileSystemType>|指定哪些文件系统可以与发现数据驱动器一起使用：FAT32、 默认值，或 none。|
|-computername|指定管理 bde 使用修改的其他计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。|
|\<名称 >|表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|显示在命令提示符下简短帮助。|
|-help 或-h|显示在命令提示符下完成的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-在**命令以启用 BitLocker 的驱动器 C，并将恢复密码添加到驱动器。
```
manage-bde –on C: -recoverypassword
```
下面的示例演示如何使用 **-在**命令以启用 BitLocker 的驱动器 C，将恢复密码添加到该驱动器，然后将恢复密钥保存到驱动器 E。
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
下面的示例演示如何使用 **-在**命令，以启用 bitlocker 的驱动器 C 通过使用外部密钥保护程序 （如 USB 密钥） 来解锁操作系统驱动器。 如果没有安装 TPM 的计算机上使用 BitLocker，则需要使用此方法。
```
manage-bde -on C: -startupkey E:\
```
下面的示例演示如何使用 **-在**命令以启用 BitLocker 数据驱动器 E，并添加密码密钥保护程序。 管理 bde 将提示你输入此命令后输入的密码。
```
manage-bde –on E: -pw
```
下面的示例演示如何使用 **-在**命令以启用 BitLocker 操作系统驱动器 C 的并使用基于硬件的加密。
```
manage-bde –on C: -fet Hardware
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)