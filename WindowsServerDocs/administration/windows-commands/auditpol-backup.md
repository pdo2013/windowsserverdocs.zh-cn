---
title: auditpol 备份
description: Windows 命令主题**auditpol 备份**-将备份系统审核策略设置、 所有用户和到以逗号分隔值 (CSV) 文本文件的所有审核选项按用户审核策略设置。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78594c0445ae482e49d47b3b67bb867e53866017
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867948"
---
# <a name="auditpol-backup"></a>auditpol 备份

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将备份系统审核策略设置、 所有用户和到以逗号分隔值 (CSV) 文本文件的所有审核选项按用户审核策略设置。

## <a name="syntax"></a>语法
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/file|指定到的审核策略将备份文件的名称。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
为每个用户策略和系统策略的备份操作，您必须具有写入或完全控制权限，该对象上设置安全描述符中。 此外可以通过拥有执行备份操作**管理审核和安全日志**(SeSecurityPrivilege) 用户权限。 但是，此权限允许其他不需要执行列表操作的访问。
## <a name="BKMK_examples"></a>示例
若要备份按用户审核策略设置的所有用户，系统审核策略设置和到名为 auditpolicy.csv，类型的 CSV 格式的文本文件的所有审核选项：
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> 如果未指定驱动器，使用当前目录。
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[auditpol 还原](auditpol-restore.md)
