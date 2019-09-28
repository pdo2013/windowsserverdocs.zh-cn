---
title: auditpol 还原
description: 适用于**auditpol restore**的 Windows 命令主题-还原系统审核策略设置、所有用户的按用户审核策略设置，以及从语法上与逗号分隔值（CSV）文件格式一致的文件中的所有审核选项由/backup 选项使用。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b91f3745354c695c4ab0c71b429718bff05d8098
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382412"
---
# <a name="auditpol-restore"></a>auditpol 还原

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

为所有用户还原系统审核策略设置、每用户审核策略设置，以及从语法上与/backup 选项使用的逗号分隔值（CSV）文件格式一致的文件中的所有审核选项。

## <a name="syntax"></a>语法
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/file|指定应将审核策略还原到的文件。 该文件必须使用/backup 选项创建，或者必须在语法上与/backup 选项使用的 CSV 文件格式一致。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
对于每用户策略和系统策略的还原操作，你必须对安全描述符中的该对象集具有 "写入" 或 "完全控制" 权限。 还可以通过拥有 "**管理审核和安全日志**（SeSecurityPrivilege）" 用户权限来执行还原操作。 如果在发生意外错误或恶意攻击时还原安全描述符，SeSecurityPrivilege 将非常有用。
## <a name="BKMK_examples"></a>示例
若要从使用/backup 命令创建的名为 auditpolicy 的文件中还原系统审核策略设置、所有用户的每用户审核策略设置以及所有审核选项，请键入：
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>其他参考
[命令行语法关键字](command-line-syntax-key.md)
[auditpol 备份](auditpol-backup.md)
