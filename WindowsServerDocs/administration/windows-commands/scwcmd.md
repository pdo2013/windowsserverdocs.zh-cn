---
title: Scwcmd
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fae9476f94af5faa6e942239e7d91cf589bb1776
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384265"
---
# <a name="scwcmd"></a>Scwcmd

> 适用于：Windows Server 2012 R2、Windows Server 2012

安全配置向导（SCW）附带的 Scwcmd 命令行工具可用于执行以下任务：
-   使用 SCW 生成的策略配置一台或多台服务器。
-   使用 SCW 生成的策略分析一个或多个服务器。
-   以 HTML 格式查看分析结果。
-   回滚 SCW 策略。
-   将 SCW 生成的策略转换为组策略支持的本机文件。
-   使用 SCW 注册安全配置数据库扩展。

使用**scwcmd**配置、分析或回滚远程服务器上的策略时，必须在远程服务器上安装 SCW。

## <a name="syntax"></a>语法

```
scwcmd <command> [<subcommand>]
```

## <a name="parameters"></a>Parameters

|命令|描述|
|----------|-----------|
|/analyze|确定计算机是否符合策略。</br>请参阅[Scwcmd：分析](scwcmd-analyze.md)语法和选项。|
|/configure|将 SCW 生成的安全策略应用到计算机。</br>请参阅[Scwcmd： configure](scwcmd-configure.md) for 句法 and options。|
|/register|通过注册包含角色、任务、服务或端口定义的安全配置数据库文件来扩展或自定义 SCW 安全配置数据库。</br>有关语法和选项，请参阅[Scwcmd： register](scwcmd-register.md) 。|
|/rollback|应用最新的可用回滚策略，然后删除该回滚策略。</br>有关语法和选项，请参阅[Scwcmd： rollback](scwcmd-rollback.md) 。|
|/transform|将使用 SCW 生成的安全策略文件转换为 Active Directory 域服务中的新组策略对象（GPO）。</br>请参阅[Scwcmd：转换](scwcmd-transform.md)语法和选项。|
|/view|使用指定的 .xsl 转换呈现 .xml 文件。</br>有关语法和选项，请参阅[Scwcmd： view](scwcmd-view.md) 。|
|/?|在命令提示符下显示帮助。|

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
