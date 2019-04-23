---
title: auditpol 还原
description: Windows 命令主题**auditpol 还原**-从语法上符合以逗号分隔的文件将还原系统审核策略设置、 按用户审核策略设置为所有用户和所有审核选项使用 /backup 值 (CSV) 文件格式选项。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1961387083a8a61b27f3e44a2380a6060a02f98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868978"
---
# <a name="auditpol-restore"></a>auditpol 还原

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

从语法上与使用 /backup 的以逗号分隔值 (CSV) 文件格式一致的文件将还原系统审核策略设置，为所有用户和所有审核选项按用户审核策略设置选项。

## <a name="syntax"></a>语法
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/file|指定应从中还原的审核策略的文件。 文件必须由使用 /backup 创建选项，或必须是语法上符合 /backup 所使用的 CSV 文件格式选项。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
对于每个用户策略和系统策略的还原操作，您必须具有写入或完全控制权限，该对象上设置安全描述符中。 此外可以通过拥有执行还原操作**管理审核和安全日志**(SeSecurityPrivilege) 用户权限。 Sesecurityprivilege 权限时，发生意外错误或恶意攻击时还原的安全描述符。
## <a name="BKMK_examples"></a>示例
若要从一个名为已通过使用 /backup auditpolicy.csv 文件还原系统审核策略设置，为所有用户和所有审核选项按用户审核策略设置命令中，键入：
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[auditpol 备份](auditpol-backup.md)
