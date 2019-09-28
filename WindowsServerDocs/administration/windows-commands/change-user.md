---
title: change user
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 771e39182c17b9a6710e49eff2f5302e539bdbb5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379572"
---
# <a name="change-user"></a>change user

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更改远程桌面会话主机（rd 会话主机）服务器的安装模式。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>语法
> ```
> change user {/execute | /install | /query}
> ```
> ## <a name="parameters"></a>Parameters
> 
> | 参数 |                                                                                                 描述                                                                                                  |
> |-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | / execute  |                                                                使.ini 文件映射到主目录。 此为默认设置。                                                                 |
> | 操作系统  | 禁用.ini 文件映射到主目录。 所有的.ini 文件读取并写入到系统目录中。 在 rd 会话主机服务器上安装应用程序时，必须禁用 .ini 文件映射。 |
> |  /query   |                                                                             显示当前设置的.ini 文件映射。                                                                              |
> |    /?     |                                                                                     在命令提示符下显示帮助。                                                                                     |
> 
> ## <a name="remarks"></a>备注
> - 使用 **更改 user /install** 之前安装的应用程序在系统目录中创建的应用程序的.ini 文件。 创建特定于用户的.ini 文件时，这些文件使用作为源。 安装后该应用程序，使用 **更改用户 / execute** ，将恢复为标准.ini 文件映射。
> - 第一次运行该应用程序，它将搜索其.ini 文件的主目录。 如果对主目录中找不到.ini 文件，但在系统目录中找到，远程桌面服务.ini 文件复制到主目录中，以确保每个用户具有的应用程序.ini 文件的唯一副本。 主目录中创建任何新的.ini 文件。
> - 每个用户应具有应用程序的.ini 文件的唯一副本。 这可以防止不同用户可能有不兼容的应用程序配置 （例如，不同的默认目录或屏幕分辨率）。
> - 当系统处于安装模式 (即， **更改 user /install**)，会出现几个情况。 所有创建的注册表项都将在 **\SOFTWARE**子项或 **\MACHINE**子项下的 " **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentversion\Terminal Server\Install**" 下隐藏。 添加到**HKEY_CURrenT_USER**的子项在 **\SOFTWARE**子项下复制，添加到**HKEY_LOCAL_MACHINE**的子项会复制到 **\MACHINE**子项下。 如果应用程序使用系统调用（如 GetWindowsdirectory）查询 Windows 目录，则 rd 会话主机服务器会返回 systemroot 目录。 如果使用的系统调用，如 WritePrivateProfileString，添加了任何.ini 文件条目添加到 systemroot 目录下的.ini 文件。
> - 当系统返回到执行模式（即，**更改用户/execute**），并且应用程序尝试读取不存在的**HKEY_CURrenT_USER**下的注册表项时，远程桌面服务将检查是否存在此项的副本 **\Terminal Server\Install**子项。 如果是这样，则将子项复制到**HKEY_CURrenT_USER**下的相应位置。 如果应用程序尝试读取.ini 文件不存在，远程桌面服务会搜索该.ini 文件系统根目录下。 如果.ini 文件系统根目录中，该证书复制到用户的主目录的 \Windows 子目录。 如果应用程序查询 Windows 目录，则 rd 会话主机服务器将返回用户的主目录的 \Windows 子目录。
> - 当您登录时，远程桌面服务检查其系统.ini 文件是否比您的计算机上的.ini 文件。 如果系统版本较新，您的.ini 文件替换或者合并在一起的较新版本。 这取决于 INISYNC 类型为 bit，0x40，是为此.ini 文件设置。 以前版本的.ini 文件会重命名为 Inifile.ctx。 如果 **\Terminal Server\Install**子项下的系统注册表值比**HKEY_CURrenT_USER**下的版本新，则会删除这些子项的版本，并将其替换为 **\Terminal Server\Install**中的新子项。
>   ## <a name="BKMK_examples"></a>示例
> - 若要禁用对主目录中的.ini 文件映射，请键入︰
>   ```
>   change user /install
>   ```
> - 若要启用对主目录中的.ini 文件映射，请键入︰
>   ```
>   change user /execute
>   ```
> - 若要显示.ini 文件映射的当前设置，请键入︰
>   ```
>   change user /query
>   ```
>   #### <a name="additional-references"></a>其他参考
>   [命令行语法关键字](command-line-syntax-key.md)
>   [更改](change.md)
>   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
