---
title: Scwcmd 回滚
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d6cd79c7068d86915141a37b5a4510bddefc94c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852198"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> 适用于：Windows Server 2012 R2, Windows Server 2012

将最新的回退策略可用，应用，然后删除该回退策略。

## <a name="syntax"></a>语法

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/m:\<计算机名 >|指定 NetBIOS 名称、 DNS 名称或 IP 地址可以在其中执行回滚操作的计算机。|
|带 /u:\<用户名 >|指定备用用户帐户以执行远程回滚时使用。 默认值是已登录用户。|
|/pw:\<密码 >|指定一个备用用户凭据执行远程回滚时使用。 默认值是已登录用户。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

Scwcmd.exe 才可在运行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的计算机上。

## <a name="BKMK_Examples"></a>示例

若要回滚安全策略在 IP 地址为 172.16.0.0 的计算机上，请键入：
```
scwcmd rollback /m:172.16.0.0
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)