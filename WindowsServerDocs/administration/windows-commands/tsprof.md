---
title: tsprof
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e17d60126125fcd4b10373133dd61ca0db030290
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836068"
---
# <a name="tsprof"></a>tsprof

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将远程桌面服务用户配置信息从一个用户复制到另一个。
远程桌面服务用户配置信息显示在远程桌面服务扩展到本地用户和组以及 active directory 用户和计算机。

**tsprof**还可以为用户设置的配置文件路径。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/update|更新配置文件路径信息的 <*用户名*> 域中 <*DomainName*> 向 <*配置文件路径*>。|
|/domain:\<DomainName >|指定在其中应用了操作的域的名称。|
|/ 本地|该操作仅适用于本地用户帐户。|
|/ 配置文件：\<路径 >|指定的配置文件路径的远程桌面服务扩展中本地用户和组以及 active directory 用户和计算机中所示。|
|\<UserName>|指定要更新或查询服务器配置文件路径的用户的名称。|
|/copy|中的用户配置信息复制\< *SourceUser*> 到\< *DestinationUser*> 并更新的配置文件路径信息\< *DestinationUser*> 到\<*配置文件路径*>。 这两\< *SourceUser*> 和\< *DestinationUser*> 必须是本地或域中必须是\< *DomainName*>.|
|\<Src_usr>|指定要复制的用户配置信息的用户的名称。|
|\<Dest_usr>|指定要向其复制的用户配置信息的用户的名称。|
|/q|显示要为其查询服务器配置文件路径的用户的当前配置文件路径。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **Tsprof**命令已运行 Windows Server 2008 R2 的计算机上运行 Windows Server 2008 或 RD 会话主机角色服务的计算机上安装终端服务器角色服务时才可用。

## <a name="BKMK_examples"></a>示例
-   若要将用户的配置信息从 LocalUser1 复制到 LocalUser2，键入：
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   若要设置到名为"c:\profiles"的目录 LocalUser1 远程桌面服务配置文件路径，请键入：
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[远程桌面服务 & #40;终端服务和 #41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
