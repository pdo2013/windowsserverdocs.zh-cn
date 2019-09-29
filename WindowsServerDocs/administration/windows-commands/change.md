---
title: change
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90012116-0fb3-4f34-a819-cf4d4b4f8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eee52bbb24824ea01f9c55a4bfe6e3e60ad2ab58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379555"
---
# <a name="change"></a>change

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更改登录、COM 端口映射和安装模式远程桌面会话主机（rd 会话主机）服务器设置。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>语法
> ```
> change logon
> change port
> change user
> ```
> ## <a name="parameters"></a>Parameters
> 
> |            参数            |                                                   描述                                                   |
> |---------------------------------|-----------------------------------------------------------------------------------------------------------------|
> | [change logon](change-logon.md) | 启用或禁用从 rd 会话主机服务器上的客户端会话登录，或显示当前登录状态。 |
> |  [change port](change-port.md)  |                列出或更改 COM 端口映射，使其与 MS-DOS 应用程序兼容。                |
> |  [change user](change-user.md)  |                            更改 rd 会话主机服务器的安装模式。                             |
> 
> #### <a name="additional-references"></a>其他参考
> [命令行语法解答](command-line-syntax-key.md)
> [远程桌面服务&#40;终端服务和&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
