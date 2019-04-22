---
title: auditpol 删除
description: Windows 命令主题**auditpol 删除**-删除指定的帐户或所有帐户的每个用户审核策略。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 566827e93d9f8c9dc0d00f4f704513369fbb44ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818298"
---
# <a name="auditpol-remove"></a>auditpol 删除

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除指定的帐户或所有帐户的每个用户审核策略。

## <a name="syntax"></a>语法
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/user|指定的安全标识符 (SID) 或为其每个用户审核策略的用户的用户名称是要删除。|
|/allusers|删除所有用户的每个用户审核策略。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
对于每个用户策略的删除操作，必须在安全描述符中设置该对象具有写入或完全控制权限。 此外可以通过拥有执行删除操作**管理审核和安全日志**(SeSecurityPrivilege) 用户权限。 但是，此权限允许其他不是执行删除操作所需的访问。
## <a name="BKMK_examples"></a>示例
若要按名称删除用户 mikedan 按用户审核策略，请键入：
```
auditpol /remove /user:mikedan
```
若要删除用户的 SID mikedan 按用户审核策略，请键入：
```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```
若要删除所有用户的每个用户审核策略，请键入：
```
auditpol /remove /allusers
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
