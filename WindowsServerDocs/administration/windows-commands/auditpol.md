---
title: auditpol
description: 适用于**auditpol**的 Windows 命令主题-显示有关的信息和执行函数来操作审核策略。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e249a9e2a07505f052b774208c514b4d16879b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382378"
---
# <a name="auditpol"></a>auditpol



显示有关的信息和执行函数以操作审核策略。

有关如何使用此命令的示例，请参阅每个主题中的 "示例" 部分。

## <a name="syntax"></a>语法

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Parameters

|子命令|描述|
|-----------|-----------|
|/get|显示当前审核策略。</br>请参阅[Auditpol get 获取](auditpol-get.md)语法和选项。|
|/set|设置审核策略。</br>有关语法和选项，请参阅[Auditpol set](auditpol-set.md) 。|
|/list|显示可选的策略元素。</br>请参阅[Auditpol list](auditpol-list.md)了解语法和选项。|
|/backup|将审核策略保存到文件。</br>有关语法和选项，请参阅[Auditpol backup](auditpol-backup.md) 。|
|/restore|从以前使用 auditpol/backup. 创建的文件还原审核策略。</br>有关语法和选项，请参阅[Auditpol restore](auditpol-restore.md) 。|
|/clear|清除审核策略。</br>有关语法和选项，请参阅[Auditpol clear](auditpol-clear.md) 。|
|/remove|删除所有每用户审核策略设置，并禁用所有系统审核策略设置。</br>有关语法和选项，请参阅[Auditpol remove](auditpol-remove.md) 。|
|/resourceSACL|配置全局资源系统访问控制列表（Sacl）。</br>注意:仅适用于 Windows 7 和 Windows Server 2008 R2。</br>请参阅[Auditpol resourceSACL](auditpol-resourcesacl.md)。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

审核策略命令行工具可用于：
-   设置和查询系统审核策略。
-   设置和查询每用户审核策略。
-   设置和查询审核选项。
-   设置和查询用于委托审核策略访问的安全描述符。
-   向逗号分隔值（CSV）文本文件报告或备份审核策略。
-   从 CSV 文本文件加载审核策略。
-   配置全局资源 Sacl。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)