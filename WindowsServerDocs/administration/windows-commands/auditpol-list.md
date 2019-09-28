---
title: auditpol 列表
description: 用于**auditpol list**的 Windows 命令主题-列出审核策略类别和/或子类别，或者列出为其定义了每用户审核策略的用户。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27a89ae18838989b4f2df27d777c1c35249b8991
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382465"
---
# <a name="auditpol-list"></a>auditpol 列表

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

列出审核策略类别和/或子类别，或者列出为其定义每用户审核策略的用户。

## <a name="syntax"></a>语法
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/user|检索为其定义了每用户审核策略的所有用户。 如果与/v 参数一起使用，则还会显示用户的安全标识符（SID）。|
|/category|显示系统理解的类别名称。 如果与/v 参数一起使用，则还会显示类别全局唯一标识符（GUID）。|
|/subcategory|显示子类别的名称及其关联的 GUID。|
|/v|显示具有类别或子类别的 GUID，或与/user 一起使用时显示每个用户的 SID。|
|/r|以逗号分隔值（CSV）格式将输出显示为报表。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
对于每用户策略的所有列表操作，你必须对安全描述符中的该对象集具有 "读取" 权限。 还可以通过拥有 "**管理审核和安全日志**（SeSecurityPrivilege）" 用户权限来执行列表操作。 但是，此权限允许执行列表操作所不需要的其他访问权限。
## <a name="BKMK_examples"></a>示例
若要列出具有定义的审核策略的所有用户，请键入：
```
auditpol /list /user
```
若要列出具有定义的审核策略及其关联 SID 的所有用户，请键入：
```
auditpol /list /user /v
```
若要以报表格式列出所有类别和子类别，请键入：
```
auditpol /list /subcategory:* /r
```
若要列出详细跟踪和 DS 访问类别的子类别，请键入：
```
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)
