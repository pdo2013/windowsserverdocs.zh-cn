---
title: ftp
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b64e6831fcf8930c62b2a3f04022dc49d5297683
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375860"
---
# <a name="ftp"></a>ftp

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在运行文件传输协议（ftp）服务器服务的计算机之间传输文件。 可以通过处理 ASCII 文本文件，以交互方式或以批处理模式使用**ftp** 。 
## <a name="syntax"></a>语法
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>Parameters

|     参数     |                                                                                                                                                      描述                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    禁止显示远程服务器响应。                                                                                                                                     |
|        -d.ddd...e         |                                                                                                               启用调试，并显示在 FTP 客户端和 FTP 服务器之间传递的所有命令。                                                                                                                |
|        -i         |                                                                                                                            在多个文件传输过程中禁用交互式提示。                                                                                                                             |
|        -n         |                                                                                                                                    在初始连接时禁止自动登录。                                                                                                                                     |
|        -g         |                                         禁用文件名组合。  **Glob**允许将星号（\*）和问号（？）用作本地文件和路径名称中的通配符字符。 有关详细信息，请参阅 "[其他参考](ftp.md#BKMK_additionalRef)"。                                          |
|   -s： <FileName>   | 指定包含**ftp**命令的文本文件。 这些命令将在**ftp**启动后自动运行。 此参数不允许有空格。 使用此参数，而不是重定向（ **<** ）。 **注意：** 在 Windows 8 和 Windows Server 2012 或更高版本的操作系统中，必须用 UTF-8 编写文本文件。 |
|        -a         |                                                                                                                 指定在绑定 ftp 数据连接时可以使用任何本地接口。                                                                                                                  |
|        -A         |                                                                                                                                        以匿名方式登录到 ftp 服务器。                                                                                                                                         |
|  -x： <SendBuffer>  |                                                                                                                                     覆盖默认的 SO_SNDBUF 大小8192。                                                                                                                                     |
|  -r： <RecvBuffer>  |                                                                                                                                     覆盖默认的 SO_RCVBUF 大小8192。                                                                                                                                     |
| -b： <AsyncBuffers> |                                                                                                                                    替代的默认异步缓冲区计数为3。                                                                                                                                     |
| -w： <WindowsSize>  |                                                                                                                   指定传输缓冲区的大小。 默认窗口大小为4096个字节。                                                                                                                   |
|        -?         |                                                                                                                                         在命令提示符下显示帮助。                                                                                                                                          |
|      <host>       |                                                                    指定要连接的 ftp 服务器的计算机名称、IP 地址或 IPv6 地址。 如果指定，主机名或地址必须是行中的最后一个参数。                                                                    |

## <a name="remarks"></a>备注
- 有关 Windows Server 2003 上**ftp**命令的详细信息，请参阅[ftp](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx)。
- **ftp**命令行参数区分大小写。
- 仅当**Internet 协议（tcp/ip）** 协议安装为网络连接中的网络适配器属性中的组件时，此命令才可用。
- **ftp**可以交互方式使用。 启动后， **ftp**将创建一个可在其中使用**ftp**命令的子环境。 可以通过键入**quit**命令返回到命令提示符。 当**ftp**子环境正在运行时， **ftp >** 命令提示符下会指示它。 有关详细信息，请参阅**ftp**命令。
- **ftp**支持在安装 ipv6 协议时使用 ipv6。 有关详细信息，请参阅 "[其他参考](ftp.md#BKMK_additionalRef)"。
  ## <a name="BKMK_Examples"></a>示例
  若要登录到名为 ftp.example.microsoft.com 的 ftp 服务器，请键入：
  ```
  ftp ftp.example.microsoft.com
  ```
  若要登录到名为 ftp.example.microsoft.com 的 ftp 服务器并运行名为 resync .txt 的文件中包含的**ftp**命令，请键入：
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="BKMK_additionalRef"></a>其他参考
- [IP 版本6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [IPv6 应用程序](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- [命令行语法项](command-line-syntax-key.md)
