---
title: auditpol 列表
description: Windows 命令主题**auditpol 列表**-列表审核策略类别和/或子类别，或定义列表用户为其每个用户审核策略。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 08f524ef0aacd731f709ce7a2e17b3d831da1e5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858578"
---
# <a name="auditpol-list"></a>auditpol 列表

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

列表的审核策略类别和/或子类别或为其定义的每个用户审核策略的列表用户。

## <a name="syntax"></a>语法
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/user|检索已为其定义了每个用户的审核策略的所有用户。 如果使用 /v 参数，还会显示用户的安全标识符 (SID)。|
|/category|显示系统理解的类别的名称。 如果使用 /v 参数，还会显示类别的全局唯一标识符 (GUID)。|
|/subcategory|显示子类别和关联的 GUID 的名称。|
|/v|显示类别或子类别，GUID 或当用于 /user，显示每个用户的 SID。|
|/r|作为以逗号分隔值 (CSV) 格式的报表中显示输出。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
对于每个用户策略的所有列表操作，您必须具有读取权限的安全描述符中设置该对象。 此外可以通过拥有执行列表操作**管理审核和安全日志**(SeSecurityPrivilege) 用户权限。 但是，此权限允许其他不需要执行列表操作的访问。
## <a name="BKMK_examples"></a>示例
若要列出所有用户都有一个定义的审核策略，请键入：
```
auditpol /list /user
```
若要列出所有用户都有一个定义的审核策略及其关联的 SID，请键入：
```
auditpol /list /user /v
```
若要列出所有类别和子类别中的报表格式，请键入：
```
auditpol /list /subcategory:* /r
```
若要列出子类别的详细跟踪和 DS 访问类别，请键入：
```
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
