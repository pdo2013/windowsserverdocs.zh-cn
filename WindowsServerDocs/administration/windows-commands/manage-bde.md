---
title: manage-bde
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8923177b03f378f8252c532ec386f1808e516e1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874498"
---
# <a name="manage-bde"></a>manage-bde



用于打开或关闭 BitLocker，指定解除锁定机制，更新恢复方法，并解锁受 BitLocker 保护的数据驱动器。 此命令行工具可用来代替**BitLocker 驱动器加密**控制面板项。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[管理 bde： 状态](manage-bde-status.md)|无论它们受 BitLocker 保护的计算机上，提供有关所有驱动器的信息。|
|[管理 bde： 上](manage-bde-on.md)|将加密驱动器并打开 BitLocker 时。|
|[管理 bde： 关闭](manage-bde-off.md)|解密驱动器，并将关闭 BitLocker。 解密完成后，将删除所有密钥保护程序。|
|[管理 bde： 暂停](manage-bde-pause.md)|暂停加密或解密。|
|[管理 bde： 恢复](manage-bde-resume.md)|恢复加密或解密。|
|[管理 bde： 锁](manage-bde-lock.md)|阻止访问受 BitLocker 保护的数据。|
|[管理 bde： 解锁](manage-bde-unlock.md)|允许访问受 BitLocker 保护数据的恢复密码或恢复密钥。|
|[管理 bde： 自动解除锁定](manage-bde-autounlock.md)|管理的数据驱动器自动解锁。|
|[管理 bde： 保护程序](manage-bde-protectors.md)|管理加密密钥的保护方法。|
|[管理 bde: tpm](manage-bde-tpm.md)|配置计算机的受信任的平台模块 (TPM)。 此命令不支持在运行 Windows 8 的计算机上或**win8_server_2**。 若要管理这些计算机上的 TPM，适用于 Windows PowerShell 使用 TPM 管理 MMC 管理单元或 TPM 管理 cmdlet。|
|[管理 bde: setidentifier](manage-bde-setidentifier.md)|设置中指定的值的驱动器上的驱动器标识符字段**为你的组织提供的唯一标识符**组策略设置。|
|[管理-bde:ForceRecovery](manage-bde-forcerecovery.md)|强制重新启动受 BitLocker 保护驱动器进入恢复模式。 此命令从驱动器删除所有与 TPM 相关的密钥保护程序。 在计算机重新启动时，可以使用恢复密码或恢复密钥，以解锁驱动器。|
|[管理 bde： 密码更改](manage-bde-changepassword.md)|修改数据驱动器的密码。|
|[管理 bde: changepin](manage-bde-changepin.md)|修改操作系统驱动器的 PIN。|
|[管理 bde: changekey](manage-bde-changekey.md)|修改操作系统驱动器的启动密钥。|
|[管理-bde:KeyPackage](manage-bde-keypackage.md)|生成一个驱动器的密钥包。|
|[管理 bde： 升级](manage-bde-upgrade.md)|BitLocker 版本升级。|
|[管理-bde:WipeFreeSpace](manage-bde-wipefreespace.md)|擦除在驱动器上的可用空间。|
|-? 或 /?|显示在命令提示符下简短帮助。|
|-help 或-h|显示在命令提示符下完成的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例显示在计算机上的驱动器，并标识它们是否是受 BitLocker 保护和的当前加密状态。
```
manage-bde -status
```
下面的示例演示使用恢复密码选项 C 驱动器上的启用 BitLocker。 将生成的 BitLocker 和，以便可以将其记录在屏幕上显示恢复密码。
```
manage-bde –on C: -recoverypassword
```
下面的示例说明了通过使用恢复密码解锁受 BitLocker 保护驱动器。
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [使用命令行启用 BitLocker](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
