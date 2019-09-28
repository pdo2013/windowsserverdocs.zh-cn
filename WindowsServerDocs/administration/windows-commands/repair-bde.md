---
title: repair-bde
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 817e5fb5cf032376ddfddb3a54f73411ac175def
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384551"
---
# <a name="repair-bde"></a>repair-bde



如果已使用 BitLocker 对驱动器进行加密，请访问严重损坏的硬盘上的加密数据。 只要使用有效的恢复密码或恢复密钥来解密数据，Repair 就可以重建驱动器的关键部分并抢救可恢复数据。 如果驱动器上的 BitLocker 元数据数据已损坏，则除了恢复密码或恢复密钥以外，还必须能够提供备份密钥包。 如果你使用了 AD DS 备份的默认设置，则将在 Active Directory 域服务（AD DS）中备份此密钥包。 使用此密钥包以及恢复密码或恢复密钥，可以在磁盘损坏的情况下解密受 BitLocker 保护的驱动器的部分。 每个密钥包仅适用于具有相应驱动器标识符的驱动器。 你可以使用[Active Directory 的 BitLocker 恢复密码查看器](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx)从 AD DS 中获取此密钥包。

> [!NOTE]
> BitLocker 恢复密码查看器包含为在 Windows Server 2012 上使用服务器管理安装的可选管理功能之一。

对于 Repair 命令行工具存在以下限制：
-   Manage-bde 无法修复在加密或解密过程中失败的驱动器。
-   Manage-bde 假设如果驱动器具有任何加密，则驱动器已完全加密。

有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<InputVolume >|标识要修复的 BitLocker 加密驱动器的驱动器号。 驱动器号必须包含冒号;例如：**C：** 。|
|\<OutputVolumeorImage >|标识要在其上存储已修复驱动器的内容的驱动器。 输出驱动器上的所有信息都将被覆盖。|
|-rk|标识应用于解锁卷的恢复密钥的位置。 此命令还可以指定为 **-recoverykey**。|
|-rp|标识用于解锁卷的数字恢复密码。 此命令还可以指定为 **-ms-fve-recoverypassword**。|
|-pw|标识用于对卷进行解锁的密码。 此命令也可以指定为 **-password**|
|-kp|标识可用于解锁卷的恢复密钥包。 此命令还可以指定为 **-ms-fve-keypackage**。|
|-lf|指定文件的路径，该文件将存储 Repair 错误、警告和信息消息。 此命令也可以指定为 **-logfile**。|
|-f|强制卸除卷，即使它无法锁定也是如此。 此命令也可以指定为 **-force**。|
|-? 或 /?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

如果未指定密钥包的路径，则**repair**将在驱动器上搜索密钥包。 但是，如果硬盘驱动器已损坏，则**manage-bde**可能找不到包，并会提示你提供路径。

## <a name="BKMK_Examples"></a>示例

以下示例尝试修复驱动器 C，并使用存储在驱动器 F 上的恢复密钥文件（RecoveryKey. bek）将该文件的内容写入驱动器 D，并将此尝试的结果写入驱动器 Z 上的日志文件（log .txt）。
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
以下示例尝试修复驱动器 C，并使用指定的48位数恢复密码将驱动器 C 上的内容写入驱动器 D。 应在八个包含六个数字的块中键入恢复密码，并使用连字符分隔每个块。
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
以下示例强制卸除驱动器 C，然后尝试修复驱动器 c，并使用存储在驱动器 F 上的恢复密钥包和恢复密钥文件（RecoveryKey. bek）将驱动器 c 上的内容写入驱动器 D。
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
以下示例尝试修复驱动器 C，并将内容从驱动器 C 写入驱动器 D，你必须键入密码来解锁驱动器 C：出现提示：
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)