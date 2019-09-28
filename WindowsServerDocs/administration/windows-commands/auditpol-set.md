---
title: auditpol 集
description: 适用于**auditpol set**的 Windows 命令主题-设置每用户审核策略、系统审核策略或审核选项。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f3c9ec2fab4cad408e0bb845fe157cfdf94f8e09
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382403"
---
# <a name="auditpol-set"></a>auditpol 集

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

设置每用户审核策略、系统审核策略或审核选项。

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
|    /user     |                                        为其设置类别或子类别指定的每用户审核策略的安全主体。 必须指定 "类别" 或 "子类别" 选项，作为安全标识符（SID）或名称。                                         |
|   /include   | 用/user 指定;指示用户的每用户策略将导致生成审核，即使系统审核策略未指定审核也是如此。 此设置是默认设置，如果/include 和/exclude 参数均未显式指定，则会自动应用此设置。 |
|   /exclude   |                                用/user 指定;指示无论系统审核策略如何，用户的每用户策略都将导致抑制审核。 对于作为本地 Administrators 组成员的用户，此设置将被忽略。                                |
|  /category   |                                                                            由全局唯一标识符（GUID）或名称指定的一个或多个审核类别。 如果未指定用户，则设置系统策略。                                                                             |
| /subcategory |                                                                                         GUID 或名称指定的一个或多个审核子类别。 如果未指定用户，则设置系统策略。                                                                                          |
|   /success   |                 指定成功审核。 此设置是默认设置，如果/success 和/failure 参数均未显式指定，则会自动应用此设置。 此设置必须与指示是否启用或禁用该设置的参数一起使用。                 |
|   /failure   |                                                                                  指定失败的审核。 此设置必须与指示是否启用或禁用该设置的参数一起使用。                                                                                   |
|   /option    |                                                                                   为 CrashOnAuditFail、FullprivilegeAuditing、AuditBaseObjects 或 AuditBasedirectories 选项设置审核策略。                                                                                    |
|     /sd      |                 设置用于委托审核策略访问的安全描述符。 安全描述符必须使用安全描述符定义语言（SDDL）来指定。 安全描述符必须具有自由访问控制列表（DACL）。                 |
|      /?      |                                                                                                                              在命令提示符下显示帮助。                                                                                                                              |

## <a name="remarks"></a>备注
对于 "每用户策略" 和 "系统策略" 的所有设置操作，您必须对安全描述符中的该对象设置具有 "写入" 或 "完全控制" 权限。 还可以通过拥有 "**管理审核和安全日志**（SeSecurityPrivilege）" 用户权限来执行设置操作。 但是，此权限允许执行 set 操作所不需要的其他访问权限。
## <a name="BKMK_examples"></a>示例
### <a name="examples-for-the-per-user-audit-policy"></a>每用户审核策略的示例
若要为用户 mikedan 的详细跟踪类别下的所有子类别设置每用户审核策略，以便审核所有用户的成功尝试，请键入：
```
auditpol /set /user:mikedan /category:"detailed Tracking" /include /success:enable
```
若要为由名称和 GUID 指定的类别以及由 GUID 指定的子类别为任何成功或失败的尝试禁用审核，请键入：
```
auditpol /set /user:mikedan /exclude /category:"Object Access","System",{6997984b-797a-11d9-bed3-505054503030} 
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```
若要为指定的用户设置每个用户的审核策略，以使所有类别禁止审核所有失败的尝试，请键入：
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```
### <a name="examples-for-the-system-audit-policy"></a>系统审核策略示例
若要将 "详细跟踪" 类别下的所有子类别的系统审核策略设置为仅包括成功尝试的审核，请键入：
```
auditpol /set /category:"detailed Tracking" /success:enable
```
> [!NOTE]
> 失败设置未更改。
> 若要为对象访问和系统类别设置系统审核策略（隐含，因为列出了子类别）和 Guid 指定的子类别来禁止失败的尝试和审核成功的尝试，请键入：
> ```
> auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
> ```
> ### <a name="example-for-auditing-options"></a>审核选项的示例
> 若要将审核选项设置为 CrashOnAuditFail 选项的 "已启用" 状态，请键入：
> ```
> auditpol /set /option:CrashOnAuditFail /value:enable
> ```
> #### <a name="additional-references"></a>其他参考
> [命令行语法项](command-line-syntax-key.md)
