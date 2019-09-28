---
title: auditpol 获取
description: 适用于 auditpol 的 Windows 命令主题**获取**系统策略、每用户策略、审核选项和审核安全描述符对象。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 296fc5afb540411d76b563faca42fc045b8df3b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382486"
---
# <a name="auditpol-get"></a>auditpol 获取

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

检索系统策略、每个用户的策略、审核选项和审核安全描述符对象。

## <a name="syntax"></a>语法
```
auditpol /get 
[/user[:<username>|<{sid}>]]
[/category:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/subcategory:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/option:<option name>]
[/sd]
[/r]
```
## <a name="parameters"></a>Parameters

|  参数   |                                                                                                                                         描述                                                                                                                                          |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /user     | 显示要为其查询每用户审核策略的安全主体。 必须指定/category 或/subcategory 参数。 用户可以指定为安全标识符（SID）或名称。 如果未指定用户帐户，则查询系统审核策略。 |
|  /category   |                                                          由全局唯一标识符（GUID）或名称指定的一个或多个审核类别。 可以使用星号（\*）指示应查询所有审核类别。                                                          |
| /subcategory |                                                                                                                  GUID 或名称指定的一个或多个审核子类别。                                                                                                                  |
|     /sd      |                                                                                                        检索用于委派审核策略访问权限的安全描述符。                                                                                                        |
|   /option    |                                                                              检索 CrashOnAuditFail、FullprivilegeAuditing、AuditBaseObjects 或 AuditBasedirectories 选项的现有策略。                                                                               |
|      /r      |                                                                                                              以逗号分隔值（CSV）格式显示输出。                                                                                                              |
|      /?      |                                                                                                                             在命令提示符下显示帮助。                                                                                                                             |

## <a name="remarks"></a>备注
所有类别和子类别都可以通过用引号引起来的 GUID 或名称来指定。 可以通过 SID 或名称指定用户。
对于 "每用户策略" 和 "系统策略" 的所有 "获取" 操作，你必须对安全描述符中的该对象集具有 "读取" 权限。 还可以通过拥有 "**管理审核和安全日志**（SeSecurityPrivilege）" 用户权限来执行 get 操作。 但是，此权限允许执行 get 操作所不需要的其他访问权限。
## <a name="BKMK_examples"></a>示例
### <a name="examples-for-the-per-user-audit-policy"></a>每用户审核策略的示例
若要检索来宾帐户的每用户审核策略并显示系统、详细跟踪和对象访问类别的输出，请键入：
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> 此命令在两种情况下很有用。 监视特定用户帐户是否有可疑活动时，可以使用/get 命令，通过使用包含策略启用其他审核来检索特定类别中的结果。 或者，如果某个帐户的审核设置记录了大量但多余的事件，则可以使用/get 命令筛选出该帐户的无关事件，其中包含排除策略。 若要查看所有类别的列表，请使用 auditpol/list/category 命令。
> 若要检索类别和特定子类别的每用户审核策略，以在来宾帐户的系统类别下报告该子类别的非独占和独占设置，请键入：
> ```
> auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> 若要以报表格式显示输出，并包括计算机名称、策略目标、子类别、子类别 GUID、包含设置和排除设置，请键入：
> ```
> auditpol /get /user:guest /category:detailed Tracking" /r
> ```
> ### <a name="examples-for-the-system-audit-policy"></a>系统审核策略示例
> 若要检索用于报告系统审核策略的类别和子类别策略设置的系统类别和子类别的策略，请键入：
> ```
> auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> 若要检索报表格式中详细跟踪类别和子类别的策略，并包括计算机名称、策略目标、子类别、子类别 GUID、包含设置和排除设置，请键入：
> ```
> auditpol /get /category:"detailed Tracking" /r
> ```
> 若要检索具有指定为 Guid 的类别的两个类别的策略（报告以下两个类别的所有子类别的所有审核策略设置），请键入：
> ```
> auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> ### <a name="examples-for-auditing-options"></a>审核选项示例
> 若要检索 AuditBaseObjects 选项的 "已启用" 或 "已禁用" 状态，请键入：
> ```
> auditpol /get /option:AuditBaseObjects
> ```
> [!NOTE]
> 可用选项包括 AuditBaseObjects、AuditBaseOperations 和 FullprivilegeAuditing。
> 若要检索已启用、已禁用或2个 CrashOnAuditFail 选项的状态，请键入：
> ```
> auditpol /get /option:CrashOnAuditFail /r
> ```
> #### <a name="additional-references"></a>其他参考
> [命令行语法项](command-line-syntax-key.md)
