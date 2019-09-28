---
title: mstsc
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bf813c75c83154c76d4aeb53a259495d4ad1369e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373353"
---
# <a name="mstsc"></a>mstsc

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

创建与远程桌面会话主机（rd 会话主机）服务器或其他远程计算机的连接，编辑现有远程桌面连接（.rdp）配置文件，并迁移使用客户端连接管理器创建的旧连接文件到新的 .rdp 连接文件。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>语法
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

## <a name="parameters"></a>Parameters

|        参数        |                                                         描述                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   指定用于连接的 .rdp 文件的名称。                                    |
|   /v： < Server [： @no__t]   |                指定远程计算机，还可以选择要连接到的端口号。                 |
|         /admin          |                                   将你连接到用于管理服务器的会话。                                   |
|           /f            |                                    以全屏模式启动远程桌面连接。                                    |
|       /w： <Width>        |                                      指定远程桌面窗口的宽度。                                      |
|       /h： <Height>       |                                     指定远程桌面窗口的高度。                                      |
|         /public         |                  在公用模式下运行远程桌面。 在公用模式下，不会缓存密码和位图。                  |
|          /span          | 使远程桌面的宽度和高度与本地虚拟桌面相匹配，如有必要，跨越多个监视器。 |
| /edit <Connection File> |                                         打开指定的 .rdp 文件以进行编辑。                                          |
|        /migrate         |       将通过客户端连接管理器创建的旧连接文件迁移到新的 .rdp 连接文件。       |
|           /?            |                                            在命令提示符下显示帮助。                                             |

## <a name="remarks"></a>备注
-   默认情况下，将为每个用户在用户的 Documents 文件夹中存储为隐藏文件。 用户创建的 .rdp 文件默认保存在用户的 Documents 文件夹中，但可以保存在任何位置。
-   若要跨越多台监视器，监视器必须使用相同的分辨率，并且必须水平对齐（即并排）。 当前不支持在客户端系统上垂直跨多个监视器。

## <a name="BKMK_examples"></a>示例
-   若要以全屏模式连接到会话，请键入：
    ```
    mstsc /f
    ```
-   若要打开一个名为 .rdp 的文件进行编辑，请键入：
    ```
    mstsc /edit filename.rdp
    ```

#### <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
-   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
