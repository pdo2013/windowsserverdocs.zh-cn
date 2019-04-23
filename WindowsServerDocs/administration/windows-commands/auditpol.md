---
title: auditpol
description: Windows 命令主题**auditpol** -显示有关的信息并执行用于操作审核策略的功能。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7e8364be977e868ac161704e67c37ec5c400e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849218"
---
# <a name="auditpol"></a>auditpol



显示有关的信息并执行用于操作审核策略的功能。

有关如何使用此命令的示例，请参阅每个主题中的示例部分。

## <a name="syntax"></a>语法

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Parameters

|子命令|描述|
|-----------|-----------|
|/get|显示当前审核策略。</br>请参阅[Auditpol get](auditpol-get.md)有关语法和选项。|
|/set|设置审核策略。</br>请参阅[Auditpol 设置](auditpol-set.md)有关语法和选项。|
|/list|显示可选择策略元素。</br>请参阅[Auditpol 列表](auditpol-list.md)有关语法和选项。|
|/backup|保存到文件中的审核策略。</br>请参阅[Auditpol 备份](auditpol-backup.md)有关语法和选项。|
|/restore|从以前已通过使用 auditpol /backup 文件还原审核策略。</br>请参阅[Auditpol 还原](auditpol-restore.md)有关语法和选项。|
|/ 清除|清除审核策略。</br>请参阅[Auditpol 清除](auditpol-clear.md)有关语法和选项。|
|/remove|将删除所有每用户审核策略设置并禁用所有系统审核策略设置。</br>请参阅[Auditpol 删除](auditpol-remove.md)有关语法和选项。|
|/resourceSACL|配置全局资源系统访问控制列表 (Sacl)。</br>注意：仅适用于 Windows 7 和 Windows Server 2008 R2。</br>请参阅[Auditpol resourceSACL](auditpol-resourcesacl.md)。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

审核策略命令行工具可用于：
-   设置和查询系统审核策略。
-   设置和查询按用户审核策略。
-   设置和查询审核选项。
-   设置和查询用于委派对审核策略访问权限的安全描述符。
-   报告或备份到以逗号分隔值 (CSV) 文本文件的审核策略。
-   从 CSV 文本文件中加载的审核策略。
-   配置全局资源 Sacl。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)