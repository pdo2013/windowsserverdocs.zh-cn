---
title: manage-bde
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ca23e5f4499672f1e4bfcca6b9ad27f4e84039b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373775"
---
# <a name="manage-bde"></a>manage-bde



用于打开或关闭 BitLocker、指定解锁机制、更新恢复方法以及解锁受 BitLocker 保护的数据驱动器。 此命令行工具可用于代替**BitLocker 驱动器加密**控制面板项。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[Manage-bde: status](manage-bde-status.md)|提供有关计算机上所有驱动器的信息，无论它们是否受 BitLocker 保护。|
|[Manage-bde: on](manage-bde-on.md)|加密驱动器并打开 BitLocker。|
|[Manage-bde: off](manage-bde-off.md)|解密驱动器并关闭 BitLocker。 解密完成后，将删除所有密钥保护程序。|
|[Manage-bde: pause](manage-bde-pause.md)|暂停加密或解密。|
|[Manage-bde: resume](manage-bde-resume.md)|恢复加密或解密。|
|[Manage-bde: lock](manage-bde-lock.md)|阻止对受 BitLocker 保护的数据的访问。|
|[Manage-bde: unlock](manage-bde-unlock.md)|允许使用恢复密码或恢复密钥访问受 BitLocker 保护的数据。|
|[Manage-bde: autounlock](manage-bde-autounlock.md)|管理数据驱动器的自动解锁。|
|[Manage-bde: protectors](manage-bde-protectors.md)|管理加密密钥的保护方法。|
|[Manage-bde: tpm](manage-bde-tpm.md)|配置计算机的受信任的平台模块（TPM）。 运行 Windows 8 或**win8_server_2**的计算机不支持此命令。 若要在这些计算机上管理 TPM，请使用 TPM 管理 MMC 管理单元或适用于 Windows PowerShell 的 TPM 管理 cmdlet。|
|[Manage-bde: setidentifier](manage-bde-setidentifier.md)|将驱动器上的 "驱动器标识符" 字段设置为 "为**你的组织提供唯一标识符**组策略" 设置中指定的值。|
|[Manage-bde:ForceRecovery](manage-bde-forcerecovery.md)|重新启动时，强制 BitLocker 保护的驱动器进入恢复模式。 此命令从驱动器中删除所有与 TPM 相关的密钥保护程序。 计算机重新启动时，只能使用恢复密码或恢复密钥来解锁驱动器。|
|[Manage-bde: changepassword](manage-bde-changepassword.md)|修改数据驱动器的密码。|
|[Manage-bde: changepin](manage-bde-changepin.md)|修改操作系统驱动器的 PIN。|
|[Manage-bde: changekey](manage-bde-changekey.md)|修改操作系统驱动器的启动密钥。|
|[Manage-bde:KeyPackage](manage-bde-keypackage.md)|为驱动器生成密钥包。|
|[Manage-bde: upgrade](manage-bde-upgrade.md)|升级 BitLocker 版本。|
|[Manage-bde:WipeFreeSpace](manage-bde-wipefreespace.md)|擦除驱动器上的可用空间。|
|-? 或 /?|在命令提示符下显示 brief Help。|
|-help 或-h|在命令提示符下显示完整的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例显示了计算机上的驱动器并标识它们是否受 BitLocker 保护以及当前的加密状态。
```
manage-bde -status
```
以下示例说明如何在驱动器 C 上使用恢复密码选项启用 BitLocker。 恢复密码将由 BitLocker 生成并在屏幕上显示，以便你可以录制。
```
manage-bde –on C: -recoverypassword
```
下面的示例演示如何使用恢复密码来解锁受 BitLocker 保护的驱动器。
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [使用命令行启用 BitLocker](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
