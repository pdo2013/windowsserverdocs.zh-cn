---
title: auditpol 集
description: Windows 命令主题**auditpol 集**-按用户审核策略、 系统审核策略设置或审核选项。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8778401efb272a167aaa3d9abb4ecafc67e5f50d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435113"
---
# <a name="auditpol-set"></a>auditpol 集

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

设置每个用户审核策略、 系统审核策略，或审核选项。

## <a name="syntax"></a>语法
```
auditpol /set
[/user[:<username>|<{sid}>][/include][/exclude]]
[/category:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/subcategory:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/option:<option name> /value: <enable>|<disable>]
```
## <a name="parameters"></a>Parameters

|  参数   |                                                                                                                                          描述                                                                                                                                           |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /user     |                                        设置安全主体为其每个用户审核策略指定的类别或子类别。 必须指定的类别或子类别的选项，为安全标识符 (SID) 或名称。                                         |
|   /include   | 指定 /user;指示用户的每个用户策略，将导致即使未指定系统审核策略生成的审核。 此设置是默认值，并且如果既未将自动应用于 / 包括也 /exclude 参数显式指定。 |
|   /exclude   |                                指定 /user;指示用户的每个用户策略，将导致要取消而不考虑系统审核策略的审核。 对于用户是本地 Administrators 组的成员将忽略此设置。                                |
|  /category   |                                                                            指定由全局唯一标识符 (GUID) 或名称的一个或多个审核类别。 如果未指定用户，系统策略设置。                                                                             |
| /subcategory |                                                                                         一个或多个审核子类别指定的 GUID 或名称。 如果未指定用户，系统策略设置。                                                                                          |
|   /success   |                 指定成功审核。 此设置是默认值，并且如果显式指定 /success 和 /failure 都不参数将自动应用。 必须使用参数，该值指示是否启用或禁用此设置使用此设置。                 |
|   /failure   |                                                                                  指定失败审核。 必须使用参数，该值指示是否启用或禁用此设置使用此设置。                                                                                   |
|   /option    |                                                                                   设置 CrashOnAuditFail、 了、 AuditBaseObjects 或 AuditBasedirectories 选项的审核策略。                                                                                    |
|     /sd      |                 设置用于委派对审核策略访问权限的安全描述符。 必须使用安全描述符定义语言 (SDDL) 指定的安全描述符。 安全描述符必须具有的任意访问控制列表 (DACL)。                 |
|      /?      |                                                                                                                              在命令提示符下显示帮助。                                                                                                                              |

## <a name="remarks"></a>备注
对于每个用户策略和系统策略的所有设置操作，您必须具有写入或完全控制权限，该对象上设置安全描述符中。 此外可以通过拥有执行集运算**管理审核和安全日志**(SeSecurityPrivilege) 用户权限。 但是，此权限允许其他不需要执行 set 操作的访问。
## <a name="BKMK_examples"></a>示例
### <a name="examples-for-the-per-user-audit-policy"></a>每个用户审核策略的示例
若要设置每个用户审核的用户 mikedan 详细跟踪类别下的所有子类别的策略，以便所有用户的成功尝试进行审核，键入：
```
auditpol /set /user:mikedan /category:"detailed Tracking" /include /success:enable
```
若要设置指定的名称和 GUID，以及指定的 GUID，若要禁止显示任何成功或失败的尝试的审核子类别的类别的每个用户审核策略，请键入：
```
auditpol /set /user:mikedan /exclude /category:"Object Access","System",{6997984b-797a-11d9-bed3-505054503030} 
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```
若要设置指定用户的成功的尝试除之外的所有审核的禁止显示功能的所有类别按用户审核策略，请键入：
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```
### <a name="examples-for-the-system-audit-policy"></a>系统审核策略的示例
若要设置的所有子类别下详细的跟踪类别，以包括仅成功的尝试的审核系统审核策略，请键入：
```
auditpol /set /category:"detailed Tracking" /success:enable
```
> [!NOTE]
> 故障设置不会更改。
> 若要设置的对象访问和系统类别 （它隐含的因为列出子类别） 和子类别的失败尝试次数灭火和审核成功的尝试指定的 Guid 的系统审核策略，请键入：
> ```
> auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
> ```
> ### <a name="example-for-auditing-options"></a>审核选项的示例
> 若要将审核选项设置为禁用组选项的启用状态，请键入：
> ```
> auditpol /set /option:CrashOnAuditFail /value:enable
> ```
> #### <a name="additional-references"></a>其他参考
> [命令行语法项](command-line-syntax-key.md)
