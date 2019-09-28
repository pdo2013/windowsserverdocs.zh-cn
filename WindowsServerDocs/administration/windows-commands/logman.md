---
title: logman
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 574a5203-5b3b-4759-a678-f26d00dde447
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 420c591a8a6c15d563a344d0450be5eb7da46191
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374263"
---
# <a name="logman"></a>logman

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

**logman**创建并管理事件跟踪会话和性能日志，并通过命令行支持性能监视器的许多功能。
## <a name="syntax"></a>语法
```
logman [create | query | start | stop | delete| update | import | export | /?] [options]
```
## <a name="actions"></a>操作
|操作|描述|
|-----|--------|
|[logman create](logman-create.md)|创建计数器、跟踪、配置数据收集器或 API。|
|[logman query](logman-query.md)|查询数据收集器属性。|
|[logman start &#124; stop](logman-start-stop.md)|开始或停止数据收集。|
|[logman delete](logman-delete.md)|删除现有的数据收集器。|
|[logman update](logman-update.md)|更新现有数据收集器的属性。|
|[logman import &#124; export](logman-import-export.md)|从 XML 文件导入数据收集器集，或将数据收集器集导出到 XML 文件。|
