---
title: AD FS 中的设备身份验证控件
description: 本文档介绍如何在 Windows Server 2016 和 2012 R2 AD FS 中启用设备身份验证
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 87c011b18ad4a1d464072c1ea90b09a44e831378
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407365"
---
# <a name="device-authentication-controls-in-ad-fs"></a>AD FS 中的设备身份验证控件
以下文档演示如何在 Windows Server 2016 和 2012 R2 中启用设备身份验证控件。

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>AD FS 2012 R2 中的设备身份验证控制
最初在 AD FS 2012 R2 中，有一个名为 @no__t 的全局身份验证属性，该属性可控制设备身份验证。

若要配置该设置，请使用 @no__t cmdlet，如下所示：


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



若要禁用设备身份验证，则使用同一 cmdlet 将值设置为 $false。

## <a name="device-authentication-controls-in-ad-fs-2016"></a>AD FS 2016 中的设备身份验证控制
2012 R2 中受支持的唯一设备身份验证类型为 clientTLS。  在 AD FS 2016 中，除 clientTLS 外，还提供了两种新类型的设备身份验证用于新式设备身份验证。  这些是：
- PKeyAuth
- PRT

若要控制新行为，请将 `DeviceAuthenticationEnabled` 属性与名为 @no__t 的新属性结合使用。  

设备身份验证方法决定将完成的设备身份验证类型：PRT、PKeyAuth、clientTLS 或某种组合。
它具有以下值：
 - SignedToken:仅 PRT
 - PKeyAuth:PRT + PKeyAuth
 - ClientTLSPRT + clientTLS
 - 所有：上述全部

如您所见，PRT 是所有设备身份验证方法的一部分，使其生效默认方法，当 `DeviceAuthenticationEnabled` 设置为 `$true` 时，始终启用。

例如：若要配置方法，请使用上面所示的 DeviceAuthenticationEnabled cmdlet，同时使用新属性：

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> 在 ADFS 2019 中，`DeviceAuthenticationMethod` 可以与 @no__t 命令一起使用。

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> 启用设备身份验证（将 @no__t 设置为 `$true`）意味着 `DeviceAuthenticationMethod` 隐式设置为 `SignedToken`，这相当于**PRT**。


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> 默认设备身份验证方法为 `SignedToken`。  其他值包括**PKeyAuth、** <strong>ClientTLS</strong>和**All**。

自 AD FS 2016 发布后，@no__t 值的含义略有变化。  请参阅下表以了解每个值的含义，具体取决于更新级别：


|AD FS 版本|DeviceAuthenticationMethod 值|也就是说|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||全部|PRT + PkeyAuth + clientTLS|
|2016 RTM + 与 Windows 更新最新|SignedToken （已更改）|PRT （仅限）|
||PkeyAuth （新）|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||全部|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>请参阅
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
