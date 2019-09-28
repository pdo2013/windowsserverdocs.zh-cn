---
title: 查询 termserver
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95a2e102a147bd7ebee24995d1e1fcd4147f8174
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371889"
---
# <a name="query-termserver"></a>查询 termserver

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示网络上所有远程桌面会话主机（rd 会话主机）服务器的列表。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>语法
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ## <a name="parameters"></a>Parameters
> 
> |    参数     |                                                                        描述                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               指定标识 rd 会话主机服务器的名称。                                               |
> | /domain： <Domain> | 指定要查询终端服务器的域。 如果要查询当前正在工作的域，则无需指定域。 |
> |     /address     |                                                  显示每个服务器的网络和节点地址。                                                  |
> |    /continue     |                                              阻止显示每个信息屏幕后暂停。                                               |
> |        /?        |                                                            在命令提示符下显示帮助。                                                            |
> 
> ## <a name="remarks"></a>备注
> - **query termserver**在网络中搜索所有附加的 Rd 会话主机服务器，并返回以下信息：
>   - 服务器的名称
>   - 网络（如果使用/address 选项，则使用节点地址）
>     ## <a name="BKMK_examples"></a>示例
> - 若要显示有关网络上所有 rd 会话主机服务器的信息，请键入：
>   ```
>   query termserver
>   ```
> - 若要显示有关名为 Server3 的 rd 会话主机服务器的信息，请键入：
>   ```
>   query termserver Server3
>   ```
> - 若要显示有关域 CONTOSO 中所有 rd 会话主机服务器的信息，请键入：
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - 若要显示名为 Server3 的 rd 会话主机服务器的网络和节点地址，请键入：
>   ```
>   query termserver Server3 /address
>   ```
>   #### <a name="additional-references"></a>其他参考
>   [命令行语法关键字](command-line-syntax-key.md)
>   [query](query.md)
>   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
