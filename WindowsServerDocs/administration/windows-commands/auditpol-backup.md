---
title: auditpol 备份
description: 适用于**auditpol 备份**的 Windows 命令主题-备份系统审核策略设置、所有用户的按用户审核策略设置，以及所有审核选项到逗号分隔值（CSV）文本文件。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96b98a05740d3ce1bfe14eda4c5d97ba6c09ff32
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382448"
---
# <a name="auditpol-backup"></a>auditpol 备份

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将所有用户的系统审核策略设置、每用户审核策略设置以及逗号分隔值（CSV）文本文件备份到所有审核选项。

## <a name="syntax"></a>语法
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parameters

| 参数 |                                 描述                                 |
|-----------|-----------------------------------------------------------------------------|
|   /file   | 指定审核策略将备份到的文件的名称。 |
|    /?     |                    在命令提示符下显示帮助。                     |

## <a name="remarks"></a>备注
对于每用户策略和系统策略的备份操作，您必须对安全描述符中的该对象集具有 "写入" 或 "完全控制" 权限。 还可以通过拥有 "**管理审核和安全日志**（SeSecurityPrivilege）" 用户权限来执行备份操作。 但是，此权限允许执行列表操作所不需要的其他访问权限。
## <a name="BKMK_examples"></a>示例
若要将所有用户、系统审核策略设置和所有审核选项的每用户审核策略设置备份到名为 auditpolicy 的 CSV 格式的文本文件中，请键入：
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> 如果未指定驱动器，则使用当前目录。
> #### <a name="additional-references"></a>其他参考
> [命令行语法关键字](command-line-syntax-key.md)
> [auditpol restore](auditpol-restore.md)
