---
title: 查询 termserver
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5baab78f6a7222bad121773e686bacb01dc8872a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817228"
---
# <a name="query-termserver"></a>查询 termserver

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示在网络上的所有远程桌面会话主机 (rd 会话主机) 服务器的列表。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。
## <a name="syntax"></a>语法
```
query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<ServerName>|指定标识的 rd 会话主机服务器的名称。|
|/domain:<Domain>|指定查询的终端服务器的域。 不需要指定域，如果要查询在其当前使用的域。|
|/address|显示每个服务器的网络和节点地址。|
|/ 继续|可以防止暂停后显示的信息的每个屏幕。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
-   **查询 termserver**搜索网络以查找所有附加 rd 会话主机服务器，并返回以下信息：
    -   服务器的名称
    -   网络 （和节点地址，如果使用 /address 选项）
## <a name="BKMK_examples"></a>示例
-   若要显示在网络上的所有 rd 会话主机服务器的信息，请键入：
    ```
    query termserver
    ```
-   若要显示有关名为 Server3 的 rd 会话主机服务器的信息，请键入：
    ```
    query termserver Server3
    ```
-   若要显示在 CONTOSO 域中的所有 rd 会话主机服务器的信息，请键入：
    ```
    query termserver /domain:CONTOSO
    ```
-   若要显示名为 Server3 的 rd 会话主机服务器的网络和节点地址，请键入：
    ```
    query termserver Server3 /address
    ```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[查询](query.md)
[远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
