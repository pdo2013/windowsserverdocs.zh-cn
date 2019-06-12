---
title: auditpol get
description: Windows 命令主题**auditpol 获取**-检索系统策略，每个用户策略，审核选项和审核安全描述符对象。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 03ba59b19af42ab2d3fdd1dd52d976d381779640
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435144"
---
# <a name="auditpol-get"></a>auditpol get

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检索系统策略，每个用户策略，审核选项和审核安全描述符对象。

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
|    /user     | 显示为其查询按用户审核策略的安全主体。 必须指定 /category 或 /subcategory 参数。 用户可能会指定为安全标识符 (SID) 或名称。 如果指定没有用户帐户，则查询系统审核策略。 |
|  /category   |                                                          指定由全局唯一标识符 (GUID) 或名称的一个或多个审核类别。 一个星号 (\*) 可用于指示应查询所有审核类别。                                                          |
| /subcategory |                                                                                                                  一个或多个审核子类别指定的 GUID 或名称。                                                                                                                  |
|     /sd      |                                                                                                        检索用于委派对审核策略访问权限的安全描述符。                                                                                                        |
|   /option    |                                                                              检索 CrashOnAuditFail、 了、 AuditBaseObjects 或 AuditBasedirectories 选项的现有策略。                                                                               |
|      /r      |                                                                                                              在报表格式，以逗号分隔值 (CSV) 中显示输出。                                                                                                              |
|      /?      |                                                                                                                             在命令提示符下显示帮助。                                                                                                                             |

## <a name="remarks"></a>备注
按 GUID 或名称用引号引起来，可以指定所有类别和子类别。 用户可以通过 SID 或名称指定。
对于每个用户策略和系统策略的所有 get 操作，您必须具有读取安全描述符中设置该对象的权限。 此外可以通过拥有执行 get 操作**管理审核和安全日志**(SeSecurityPrivilege) 用户权限。 但是，此权限允许其他不需要执行的 get 操作的访问。
## <a name="BKMK_examples"></a>示例
### <a name="examples-for-the-per-user-audit-policy"></a>每个用户审核策略的示例
若要检索按用户审核策略为来宾帐户和系统，显示的输出详细跟踪，和对象访问类别中，键入：
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> 此命令将两个方案中非常有用。 监视可疑活动的特定用户帐户，可以使用 /get 命令来使用包含策略以启用其他审核检索特定类别中的结果。 或者，如果登录的帐户上的审核设置大量但多余的事件，可以使用 /get 命令来筛选出排除策略使用该帐户无关的事件。 对于所有类别的列表，使用 auditpol /list /category 命令。
> 若要检索按用户审核策略类别和特定子类别，报告该来宾帐户的系统类别下的子类别的非独占和独占设置，请键入：
> ```
> auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> 若要在报表格式中显示的输出并包括计算机名称、 策略目标、 子类别、 子类别 GUID，包含设置和排除设置，请键入：
> ```
> auditpol /get /user:guest /category:detailed Tracking" /r
> ```
> ### <a name="examples-for-the-system-audit-policy"></a>系统审核策略的示例
> 若要检索的策略的系统类别和子类别，将报告系统审核策略的类别和子类别策略设置，请键入：
> ```
> auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> 若要检索的策略的详细的跟踪类别和子类别中的报表格式，包括计算机名、 策略目标、 子类别、 子类别的 GUID、 包含设置和排除设置类型：
> ```
> auditpol /get /category:"detailed Tracking" /r
> ```
> 若要检索包含类别的两个类别的策略指定为 Guid，它报告的所有子类别的所有审核策略设置在以下两种类别，类型：
> ```
> auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> ### <a name="examples-for-auditing-options"></a>审核选项的示例
> 若要检索的状态，启用或禁用的 AuditBaseObjects 选项中，键入：
> ```
> auditpol /get /option:AuditBaseObjects
> ```
> [!NOTE]
> 可用选项包括 AuditBaseObjects、 AuditBaseOperations 和了。
> 若要检索已启用的状态，已禁用或 2 的 CrashOnAuditFail 选项中，键入：
> ```
> auditpol /get /option:CrashOnAuditFail /r
> ```
> #### <a name="additional-references"></a>其他参考
> [命令行语法项](command-line-syntax-key.md)
