---
title: ftp
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3639626962e295a54c9068c9731f1d30754a9472
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438377"
---
# <a name="ftp"></a>ftp

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将传输到和从运行文件传输协议 (ftp) 服务器服务的计算机的文件。 **ftp**来以交互方式或在批处理模式下处理 ASCII 文本文件。 
## <a name="syntax"></a>语法
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>Parameters

|     参数     |                                                                                                                                                      描述                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    取消显示远程服务器的响应。                                                                                                                                     |
|        -d         |                                                                                                               启用调试，显示所有命令 FTP 客户端与 FTP 服务器之间传递。                                                                                                                |
|        -i         |                                                                                                                            禁用多个文件传输过程的交互式提示。                                                                                                                             |
|        -n         |                                                                                                                                    禁止显示初始连接时自动登录。                                                                                                                                     |
|        -g         |                                         禁用文件名组合。  **Glob**允许使用星号 (\*) 和问号 （？） 作为本地文件和路径名称中的通配符字符。 有关详细信息，请参阅[其他参考](ftp.md#BKMK_additionalRef)。                                          |
|   -s:。<FileName>   | 指定包含一个文本文件**ftp**命令。 这些命令后自动运行**ftp**启动。 此参数允许不含空格。 使用此参数，而不是重定向 ( **<** )。 **注意：** 在 Windows 8 和 Windows Server 2012 或更高版本操作系统中，该文本文件必须采用 utf-8。 |
|        -a         |                                                                                                                 指定绑定的 ftp 数据连接时，可以使用任何本地接口。                                                                                                                  |
|        -A         |                                                                                                                                        作为匿名 ftp 服务器上的日志。                                                                                                                                         |
|  -x:。<SendBuffer>  |                                                                                                                                     重写默认 SO_SNDBUF 大小为 8192。                                                                                                                                     |
|  -:。<RecvBuffer>  |                                                                                                                                     重写默认 SO_RCVBUF 大小为 8192。                                                                                                                                     |
| -b:<AsyncBuffers> |                                                                                                                                    重写默认异步缓冲区计数为 3。                                                                                                                                     |
| -w:。<WindowsSize>  |                                                                                                                   指定传输缓冲区的大小。 默认窗口大小为 4096 个字节。                                                                                                                   |
|        -?         |                                                                                                                                         在命令提示符下显示帮助。                                                                                                                                          |
|      <host>       |                                                                    指定计算机名称、 IP 地址或要连接到 ftp 服务器的 IPv6 地址。 主机名或地址，如果指定，必须在行上的最后一个参数。                                                                    |

## <a name="remarks"></a>备注
- 有关详细信息**ftp**命令在 Windows Server 2003，请参阅[ftp](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx)。
- **ftp**命令行参数不区分大小写。
- 此命令才**Internet 协议 (TCP/IP)** 作为一个组件中的网络连接的网络适配器的属性在安装协议。
- **ftp**可以以交互方式使用。 在启动后， **ftp**创建，你可以在其中使用一个子环境**ftp**命令。 您可以通过键入来返回命令提示符**退出**命令。 当**ftp**子环境正在运行，它将由**ftp >** 命令提示符。 有关详细信息请参阅**ftp**命令。
- **ftp**时安装了 IPv6 协议支持使用 IPv6。 有关详细信息，请参阅[其他参考](ftp.md#BKMK_additionalRef)。
  ## <a name="BKMK_Examples"></a>示例
  若要登录到名为 ftp 服务器上，键入：
  ```
  ftp ftp.example.microsoft.com
  ```
  登录到名为和运行的 ftp 服务器**ftp**名为 resync.txt，类型的文件中包含的命令：
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="BKMK_additionalRef"></a>其他参考
- [IP 版本 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [IPv6 的应用程序](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- [命令行语法项](command-line-syntax-key.md)
