---
title: change port
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ced5b9f0198179ab8b388f56aaea848b7a966081
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434502"
---
# <a name="change-port"></a>change port

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

列出或更改要与 MS-DOS 应用程序兼容的 COM 端口映射。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。
> ## <a name="syntax"></a>语法
> ```
> change port [<PortX>=<PortY> | /d <PortX> | /query]
> ```
> ## <a name="parameters"></a>Parameters
> 
> |    参数    |              描述               |
> |-----------------|----------------------------------------|
> | <PortX>=<PortY> |    将 COM 映射 <*PortX*> 向 <*PortY*>。    |
> |   /d <PortX>    | com 映射中删除 <*PortX*>。 |
> |     /query      |  显示当前的端口映射。   |
> |       /?        |  在命令提示符下显示帮助。  |
> 
> ## <a name="remarks"></a>备注
> - 大多数 MS-DOS 应用程序支持仅 COM1 到 COM4 串行端口。 **更改端口**命令映射到不同的端口号，从而允许应用程序不支持编号较高的 COM 端口来访问串行端口的串行端口。 重新映射只能用于当前会话，而且不会保留如果从会话注销，然后再次登录。
> - 使用**更改端口**不带任何参数，以显示可用的 COM 端口和及其当前的映射。
>   ## <a name="BKMK_examples"></a>示例
> - 若要将映射 COM12 COM1 到使用基于 MS-DOS 的应用程序，请键入：
>   ```
>   change port com12=com1
>   ```
> - 若要显示的当前端口映射，请键入：
>   ```
>   change port /query
>   ```
>   #### <a name="additional-references"></a>其他参考
>   [命令行语法解答](command-line-syntax-key.md)
>   [更改](change.md)
>   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
