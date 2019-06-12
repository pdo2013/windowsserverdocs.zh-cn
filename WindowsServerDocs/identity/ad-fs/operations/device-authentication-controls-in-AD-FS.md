---
title: 设备控制 AD FS 中的身份验证
description: 本文档介绍如何启用 Windows Server 2016 和 2012 R2 的 AD FS 中的设备身份验证
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f52d3d237573e4ed0028e228ff80273862a0aaf2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444640"
---
# <a name="device-authentication-controls-in-ad-fs"></a>设备控制 AD FS 中的身份验证
以下文档介绍如何启用 Windows Server 2016 和 2012 R2 中的设备身份验证控件。

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>设备控制 AD FS 2012 R2 中的身份验证
最初在 AD FS 2012 R2 时另一个全局身份验证属性名为`DeviceAuthenticationEnabled`受控的设备身份验证。

若要配置设置， `Set-AdfsGlobalAuthenticationPolicy` cmdlet 已使用，如下所示：


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



若要禁用设备身份验证，已使用同一 cmdlet 将值设置为 $false。

## <a name="device-authentication-controls-in-ad-fs-2016"></a>设备控制 AD FS 2016 中的身份验证
2012 R2 中支持的设备身份验证的唯一类型是进行 clientTLS。  在 AD FS 2016 中，除了进行 clientTLS 有两种新类型的新式设备身份验证的设备身份验证。  这些是：
- PKeyAuth
- PRT

若要控制的新行为`DeviceAuthenticationEnabled`属性结合起来使用一个名为的新属性与`DeviceAuthenticationMethod`。  

设备身份验证方法确定将完成的设备身份验证的类型：PRT、 PKeyAuth、 进行 clientTLS 或某种组合。
它具有以下值：
 - SignedToken:仅 PRT
 - PKeyAuth:PRT + PKeyAuth
 - 进行 ClientTLS:PRT + 进行 clientTLS
 - 所有：上述全部

如您所见，PRT 属于的所有设备身份验证方法，使之成为中效果始终是默认方法启用何时`DeviceAuthenticationEnabled`设置为`$true`。

例如：若要配置的方法，使用 DeviceAuthenticationEnabled cmdlet 为更高版本，以及新的属性：

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> 在 ADFS 2019`DeviceAuthenticationMethod`可与`Set-AdfsRelyingPartyTrust`命令。

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> 启用设备身份验证 (设置`DeviceAuthenticationEnabled`到`$true`) 意味着`DeviceAuthenticationMethod`隐式设置为`SignedToken`，其相当于**PRT**。


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> 默认设备身份验证方法是`SignedToken`。  其他值都**PKeyAuth，** <strong>进行 ClientTLS，</strong>并**所有**。

含义`DeviceAuthenticationMethod`值已略有更改，因为 AD FS 2016 已发布。  请参阅下表中的每个值，具体取决于更新级别的含义：


|AD FS 版本|DeviceAuthenticationMethod 值|方法|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||全部|PRT + PkeyAuth + 进行 clientTLS|
|2016 RTM + 为最新状态与 Windows 更新|SignedToken （即数据已更改）|PRT （仅限）|
||PkeyAuth （新）|PRT + PkeyAuth|
||clientTLS|PRT + 进行 clientTLS|
||全部|PRT + PkeyAuth + 进行 clientTLS|

## <a name="see-also"></a>请参阅
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
