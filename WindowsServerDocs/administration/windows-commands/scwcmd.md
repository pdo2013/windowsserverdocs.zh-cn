---
title: Scwcmd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 19d631f97c194a78819491f32955e391d3be5a70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883878"
---
# <a name="scwcmd"></a>Scwcmd

> 适用于：Windows Server 2012 R2, Windows Server 2012

包含与安全配置向导 (SCW) 的 Scwcmd.exe 命令行工具可用于执行以下任务：
-   使用 SCW 生成策略配置一个或多个服务器。
-   分析与 SCW 生成策略的一个或多个服务器。
-   以 HTML 格式查看分析结果。
-   回滚 SCW 策略。
-   将一个 SCW 生成策略转换为本机支持的组策略的文件。
-   使用 SCW 中注册的安全配置数据库扩展插件。

当你使用**scwcmd**若要配置、 分析或回滚在远程服务器上，SCW 策略必须安装在远程服务器上。

## <a name="syntax"></a>语法

```
scwcmd <command> [<subcommand>]
```

## <a name="parameters"></a>Parameters

|子命令|描述|
|----------|-----------|
|/ 分析|确定计算机是否符合策略。</br>请参阅[Scwcmd： 分析](scwcmd-analyze.md)有关语法和选项。|
|/configure|将 SCW 生成安全策略应用到计算机。</br>请参阅[Scwcmd： 配置](scwcmd-configure.md)有关语法和选项。|
|/register|扩展或通过注册包含角色、 任务、 服务或端口定义的安全配置数据库文件来自定义 SCW 安全配置数据库。</br>请参阅[Scwcmd： 注册](scwcmd-register.md)有关语法和选项。|
|/rollback|将最新的回退策略可用，应用，然后删除该回退策略。</br>请参阅[Scwcmd： 回滚](scwcmd-rollback.md)有关语法和选项。|
|/transform|转换到 Active Directory 域服务中的新组策略对象 (GPO) 中使用 SCW 生成的安全策略文件。</br>请参阅[Scwcmd： 转换](scwcmd-transform.md)语法和选项。|
|/view|使用指定的.xsl 转换来呈现一个.xml 文件。</br>请参阅[Scwcmd： 视图](scwcmd-view.md)有关语法和选项。|
|/?|在命令提示符下显示帮助。|

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
