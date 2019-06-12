---
title: auditpol 清除
description: Windows 命令主题**auditpol 清除**-删除按用户审核策略，为所有用户，重置 （禁止） 系统审核策略的所有子类别，并设置为已禁用所有审核选项。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 86b56386ba9bed2486cdf8cdbb4486fcec6c6265
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435148"
---
# <a name="auditpol-clear"></a>auditpol 清除

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除所有用户、 重置 （禁止） 系统审核策略的所有子类别，并设置为已禁用所有审核选项按用户审核策略。

## <a name="syntax"></a>语法
```
auditpol /clear [/y]
```
## <a name="parameters"></a>Parameters

| 参数 |                                   描述                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | 会禁止提示确认是否应该清除所有审核策略设置。 |
|    /?     |                       在命令提示符下显示帮助。                       |

## <a name="remarks"></a>备注
对于每个用户策略和系统策略的清除操作，您必须具有写入或完全控制权限，该对象上设置安全描述符中。 此外可以通过拥有执行清除操作**管理审核和安全日志**(SeSecurityPrivilege) 用户权限。 但是，此权限允许其他不需要执行清除操作的访问。
## <a name="BKMK_examples"></a>示例
若要删除所有用户、 重置 （禁用） 系统审核策略的所有子类别，并将设置所有每用户审核策略审核策略设置为禁用，在确认提示符下键入：
```
auditpol /clear
```
若要删除所有用户的每个用户审核策略，请重置系统审核策略设置的所有子类别，并设置所有审核策略设置为禁用，而无需确认提示时，类型：
```
auditpol /clear /y
```
> [!NOTE]
> 使用脚本来执行此操作时，前面的示例中非常有用。
> #### <a name="additional-references"></a>其他参考
> [命令行语法项](command-line-syntax-key.md)
