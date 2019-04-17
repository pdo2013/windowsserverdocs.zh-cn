---
title: "在广告 FS 设备身份验证控件"
description: "本文介绍了如何启用 Windows Server 2016 和 2012 R2 的设备中广告 FS 的身份验证"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="device-authentication-controls-in-ad-fs"></a>在广告 FS 设备身份验证控件
下面的文档显示如何启用 Windows Server 2016 和 2012 R2 的设备身份验证控件。

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>设备广告 FS 2012 R2 的身份验证控件
最初在广告 FS 2012 R2 时一个名为全球的身份验证属性`DeviceAuthenticationEnabled`控制的设备身份验证。

若要配置设置，请`Set-AdfsGlobalAuthenticationPolicy`cmdlet 使用下方所示：


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



若要禁用的设备身份验证，相同 cmdlet 用于将设置为 $false。

## <a name="device-authentication-controls-in-ad-fs-2016"></a>广告 FS 2016 中的设备身份验证控件
仅在 2012 R2 中受支持的设备身份验证的类型是 clientTLS。  在广告 FS 2016 中，除了 clientTLS 有两种新的设备用于现代设备身份验证的身份验证。  如下所示：
- PKeyAuth
- PRT 的参照

若要控制新行为，`DeviceAuthenticationEnabled`结合使用与新属性称为属性`DeviceAuthenticationMethod`。  

设备身份验证方法确定设备将完成的身份验证类型：prt 的参照、PKeyAuth，clientTLS，或某些组合。
它具有以下值：
 - 仅 prt 的 SignedToken：参照
 - PKeyAuth: prt 的参照 + PKeyAuth
 - ClientTLS: prt 的参照 + clientTLS 
 - 所有：上述所有

如你所见，prt 的参照是所有设备身份验证方法、的一部分使其生效始终默认方法时启用`DeviceAuthenticationEnabled`设置为`$true`。

示例：若要配置方法，请使用 DeviceAuthenticationEnabled cmdlet 作为上方，以及新属性：

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> 启用设备身份验证 (设置`DeviceAuthenticationEnabled`到`$true`) 意味着`DeviceAuthenticationMethod`隐式设置为`SignedToken`，这相当于**prt 的参照**。


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>默认设备身份验证方法是`SignedToken`。  其他值都是**PKeyAuth，*** ClientTLS，**和**所有**。

含义`DeviceAuthenticationMethod`值已更改略有自发布广告 FS 2016。  请参阅下表中每个值，具体取决于更新级别的含义：


|广告 FS 版本|DeviceAuthenticationMethod 值|意味着|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|Prt 的参照 + PkeyAuth|
||clientTLS|clientTLS|
||所有|Prt 的参照 + PkeyAuth + clientTLS|
|2016 RTM + 保持最新的 Windows 更新|SignedToken（更改含义）|Prt 的参照（仅提供英文版）|
||PkeyAuth（新）|Prt 的参照 + PkeyAuth|
||clientTLS|Prt 的参照 + clientTLS|
||所有|Prt 的参照 + PkeyAuth + clientTLS|

## <a name="see-also"></a>请参阅
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
