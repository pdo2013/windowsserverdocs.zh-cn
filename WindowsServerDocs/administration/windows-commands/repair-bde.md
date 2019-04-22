---
title: repair-bde
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e2bce0c0a0f12a3a171c161a669c903044e8b4b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813538"
---
# <a name="repair-bde"></a>repair-bde



如果使用 BitLocker 加密驱动器，访问加密数据会严重损坏的硬盘上。 Repair-bde 可以重新构造驱动器的关键部分，以及补救可恢复数据，前提是有效的恢复密码或恢复密钥用于解密数据。 如果已损坏驱动器上的 BitLocker 元数据数据，您必须能够提供除了恢复密码或恢复密钥的备份密钥包。 此密钥数据包被备份 Active Directory 域服务 (AD DS) 中的默认设置使用的 AD DS 备份。 与此密钥数据包以及恢复密码或恢复密钥，可以解密受 BitLocker 保护驱动器的部分，如果磁盘已损坏。 每个密钥数据包将仅适用于有相应的驱动器标识符。 可以使用[Active Directory 的 BitLocker 恢复密码查看器](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx)从 AD DS 中获取此密钥数据包。

> [!NOTE]
> 作为一个可安装 Windows Server 2012 上使用服务器管理的可选的管理功能中包含 BitLocker 恢复密码查看器。

修复 bde 命令行工具存在以下限制：
-   修复 bde 无法修复加密或解密过程中失败的驱动器。
-   修复 bde 假定，如果驱动器的任何加密，然后对驱动器已完全进行加密。

有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<InputVolume>|标识你想要修复的 BitLocker 加密驱动器的驱动器号。 驱动器号必须包含一个冒号;例如：**C:**。|
|\<OutputVolumeorImage>|标识在其中存储已修复驱动器的内容的驱动器。 输出驱动器上的所有信息将被都覆盖。|
|-rk|标识应该用于解锁该卷的恢复密钥的位置。 此命令还可以指定作为 **-recoverykey**。|
|-rp|标识应该用于解锁该卷的数字恢复密码。 此命令还可以指定作为 **-recoverypassword**。|
|-pw|标识应该用于解锁该卷的密码。 此命令还可能指定为 **-密码**|
|-kp|标识可用于解锁该卷的恢复密钥程序包。 此命令还可以指定作为 **-keypackage**。|
|-lf|指定将存储修复 bde 错误、 警告和信息性消息的文件的路径。 此命令还可以指定作为 **-logfile**。|
|-f|强制即使不能锁定要卸除卷。 此命令还可以指定作为 **-强制**。|
|-? 或 /?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

如果未指定密钥包的路径，**修复 bde**将搜索密钥数据包的驱动器。 但是，如果硬盘已损坏，则**修复 bde**可能无法查找包并将提示你提供的路径。

## <a name="BKMK_Examples"></a>示例

以下示例将尝试修复驱动器 C，并将内容写入从驱动器 C D 驱动器使用 F 驱动器上存储的恢复密钥文件 (RecoveryKey.bek)，并将此尝试的结果写入到驱动器 Z 上的日志文件 (log.txt)。
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
下面的示例尝试修复驱动器 C，并将内容驱动器 C 上写入到驱动器 D，通过使用指定的 48 位恢复密码。 以连字符分隔每个块，应在六位数字的八个区块中键入恢复密码。
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
下面的示例强制驱动器 C 要卸除，然后尝试修复驱动器 C，然后将内容写入驱动器 C 上驱动器 D 使用恢复密钥程序包和恢复密钥文件，(RecoveryKey.bek) 存储在驱动器 f。
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
下面的示例尝试修复驱动器 C，并将内容从驱动器 C 写入到驱动器 D 且必须键入密码以解锁驱动器 c： 出现提示时：
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)