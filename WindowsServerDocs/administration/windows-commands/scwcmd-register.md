---
title: Scwcmd 注册
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc8b4a06af519b0da01dfcab8de0139b12cc68f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834968"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> 适用于：Windows Server 2012 R2, Windows Server 2012

扩展或自定义通过注册包含角色、 任务、 服务或端口定义的安全配置数据库文件的安全配置向导 (SCW) 安全配置数据库。

## <a name="syntax"></a>语法

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/kbname:\<MyApp >|指定将在其下注册的安全配置数据库扩展的名称。 必须指定此参数。|
|/kbfile:\<Kb.xml>|指定将用于扩展或自定义基本的安全配置数据库的安全配置数据库文件的路径和文件。 若要验证的安全配置数据库文件符合 SCW 架构，请使用 %windir%\security\KBRegistrationInfo.xsd 架构定义文件。 必须提供此选项，除非 **/d**指定参数。|
|/kb:\<路径 >|指定包含要更新的 SCW 安全配置数据库文件的目录的路径。 如果未指定此选项，则使用 %windir%\security\msscw\kbs。|
|/d|从安全配置数据库的安全配置数据库扩展中取消注册。 由 /kbname 参数指定要取消注册的扩展。 ( **/Kbfile**不应指定参数。)指定要取消注册中的扩展安全配置数据库 **/kb**参数。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

Scwcmd.exe 才可在运行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的计算机上。

## <a name="BKMK_Examples"></a>示例

若要注册安全配置数据库文件的名称的位置的 MyApp 下名为 SCWKBForMyApp.xml \\ \\kbserver\kb，类型：
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
若要撤消注册位于安全配置数据库 MyApp \\ \\kbserver\kb，类型：
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)