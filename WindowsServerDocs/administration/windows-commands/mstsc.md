---
title: mstsc
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6f89c1e3b0d36f14dbd55f9e6994c788305b30d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437183"
---
# <a name="mstsc"></a>mstsc

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建与远程桌面会话主机 (rd 会话主机) 服务器或其他远程计算机的连接、 编辑现有的远程桌面连接 (.rdp) 配置文件，然后将迁移与客户端连接管理器中创建的旧连接文件与新的.rdp 连接文件。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

## <a name="parameters"></a>Parameters

|        参数        |                                                         描述                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   指定用于连接的.rdp 文件的名称。                                    |
|   /v:<Server[:<Port>]   |                指定远程计算机和你想要连接的端口号 （可选）。                 |
|         /admin          |                                   将你连接到会话中管理服务器。                                   |
|           /f            |                                    在全屏幕模式下启动远程桌面连接。                                    |
|       /w:<Width>        |                                      指定远程桌面窗口的宽度。                                      |
|       /h:<Height>       |                                     指定远程桌面窗口的高度。                                      |
|         /public         |                  在公用模式下运行远程桌面。 在公用模式下，不缓存密码和位图。                  |
|          /span          | 匹配的远程桌面宽度和高度与本地虚拟桌面，如有必要可以扩展到多个监视器。 |
| /edit <Connection File> |                                         打开指定的.rdp 文件进行编辑。                                          |
|        /migrate         |       将迁移到新的.rdp 连接文件与客户端连接管理器中创建的旧连接文件。       |
|           /?            |                                            在命令提示符下显示帮助。                                             |

## <a name="remarks"></a>备注
-   Default.rdp 存储为每个用户作为用户的 Documents 文件夹中的隐藏文件。 用户创建的.rdp 文件保存在用户的 Documents 文件夹中的默认情况下，但可以保存在任何位置。
-   若要跨监视器，监视器将必须使用相同的分辨率和必须水平对齐 （即，并排显示）。 目前尚不支持跨越在客户端系统上垂直的多个监视器。

## <a name="BKMK_examples"></a>示例
-   若要连接到在全屏幕模式下的会话，请键入：
    ```
    mstsc /f
    ```
-   若要打开 filename.rdp 名以进行编辑的文件，请键入：
    ```
    mstsc /edit filename.rdp
    ```

#### <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
-   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
