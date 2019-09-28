---
title: auditpol clear
description: 适用于 auditpol 的 Windows 命令主题**清除**-删除所有用户的每用户审核策略，重置（禁用）所有子类别的系统审核策略，并将所有审核选项设置为 "已禁用"。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4fd2cce2b860ee41725b698dcd36ca38b2c4c6a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382424"
---
# <a name="auditpol-clear"></a>auditpol clear

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

删除所有用户的每用户审核策略，重置（禁用）所有子类别的系统审核策略，并将所有审核选项设置为 "已禁用"。

## <a name="syntax"></a>语法
```
auditpol /clear [/y]
```
## <a name="parameters"></a>Parameters

| 参数 |                                   描述                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | 禁止提示确认是否应清除所有审核策略设置。 |
|    /?     |                       在命令提示符下显示帮助。                       |

## <a name="remarks"></a>备注
对于 "每用户策略" 和 "系统策略" 的清除操作，你必须对安全描述符中的该对象集具有 "写入" 或 "完全控制" 权限。 还可以通过拥有 "**管理审核和安全日志**（SeSecurityPrivilege）" 用户权限来执行清除操作。 但是，此权限允许执行清除操作所不需要的其他访问权限。
## <a name="BKMK_examples"></a>示例
若要为所有用户删除每用户审核策略，请重置（禁用）所有子类别的系统审核策略，并将所有审核策略设置设置为 "已禁用"，并在确认提示符下键入：
```
auditpol /clear
```
若要为所有用户删除每用户审核策略，请重置所有子类别的系统审核策略设置，并将所有审核策略设置设置为 "已禁用"，而无需确认提示，请键入：
```
auditpol /clear /y
```
> [!NOTE]
> 使用脚本执行此操作时，上述示例非常有用。
> #### <a name="additional-references"></a>其他参考
> [命令行语法项](command-line-syntax-key.md)
