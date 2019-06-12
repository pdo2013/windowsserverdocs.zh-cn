---
title: change user
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bedef9f996554a3b5745b47f646204646fdfa9a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434412"
---
# <a name="change-user"></a>change user

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改远程桌面会话主机 (rd 会话主机) 服务器的安装模式。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。
> ## <a name="syntax"></a>语法
> ```
> change user {/execute | /install | /query}
> ```
> ## <a name="parameters"></a>Parameters
> 
> | 参数 |                                                                                                 描述                                                                                                  |
> |-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | / execute  |                                                                使.ini 文件映射到主目录。 这是默认设置。                                                                 |
> | 操作系统  | 禁用.ini 文件映射到主目录。 所有的.ini 文件读取并写入到系统目录中。 在 rd 会话主机服务器上安装应用程序时，必须禁用.ini 文件映射。 |
> |  /query   |                                                                             显示当前设置的.ini 文件映射。                                                                              |
> |    /?     |                                                                                     在命令提示符下显示帮助。                                                                                     |
> 
> ## <a name="remarks"></a>备注
> - 使用 **更改 user /install** 之前安装的应用程序在系统目录中创建的应用程序的.ini 文件。 创建特定于用户的.ini 文件时，这些文件使用作为源。 安装后该应用程序，使用 **更改用户 / execute** ，将恢复为标准.ini 文件映射。
> - 第一次运行该应用程序，它将搜索其.ini 文件的主目录。 如果对主目录中找不到.ini 文件，但在系统目录中找到，远程桌面服务.ini 文件复制到主目录中，以确保每个用户具有的应用程序.ini 文件的唯一副本。 主目录中创建任何新的.ini 文件。
> - 每个用户应具有应用程序的.ini 文件的唯一副本。 这可以防止不同用户可能有不兼容的应用程序配置 （例如，不同的默认目录或屏幕分辨率）。
> - 当系统处于安装模式 (即， **更改 user /install**)，会出现几个情况。 创建的所有注册表项下灰都显**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentversion\Terminal Server\Install**，在 **\SOFTWARE**的子项或 **\MACHINE**子项。 子项添加到**HKEY_CURrenT_USER**将复制到下方 **\SOFTWARE**子项，并且子项添加到**HKEY_LOCAL_MACHINE**将复制到下方 **\机**子项。 如果应用程序使用的系统调用，如 GetWindowsdirectory，查询 Windows 目录的 rd 会话主机服务器将返回 systemroot 目录。 如果使用的系统调用，如 WritePrivateProfileString，添加了任何.ini 文件条目添加到 systemroot 目录下的.ini 文件。
> - 时系统将恢复为执行模式 (即**更改用户 / execute**)，并且该应用程序尝试读取下的注册表项**HKEY_CURrenT_USER**不存在，远程桌面服务检查以确定下是否存在密钥的副本 **\terminal**子项。 如果是这样，则子项被复制到下的适当位置**HKEY_CURrenT_USER**。 如果应用程序尝试读取.ini 文件不存在，远程桌面服务会搜索该.ini 文件系统根目录下。 如果.ini 文件系统根目录中，该证书复制到用户的主目录的 \Windows 子目录。 如果应用程序查询 Windows 目录，rd 会话主机服务器将返回用户的主目录的 \Windows 子目录。
> - 当您登录时，远程桌面服务检查其系统.ini 文件是否比您的计算机上的.ini 文件。 如果系统版本较新，您的.ini 文件替换或者合并在一起的较新版本。 这取决于 INISYNC 类型为 bit，0x40，是为此.ini 文件设置。 以前版本的.ini 文件会重命名为 Inifile.ctx。 如果系统注册表值下 **\terminal**子项的下您版本比新**HKEY_CURrenT_USER**，删除并替换为新子项的子项的版本从 **\terminal**。
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
>   [命令行语法解答](command-line-syntax-key.md)
>   [更改](change.md)
>   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
