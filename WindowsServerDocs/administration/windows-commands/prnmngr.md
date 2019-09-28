---
title: prnmngr
description: 了解如何添加、删除和列出打印机和连接。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 12981519a1d3bfc079a58e5883bc845955b8a8c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372068"
---
# <a name="prnmngr"></a>prnmngr

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

除了设置和显示默认打印机外，还可以添加、删除和列出打印机或打印机连接。

## <a name="syntax"></a>语法
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>Parameters

|           参数           |                                                                                                                                                                                        描述                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             添加本地打印机连接。                                                                                                                                                                              |
|              -d.ddd...e               |                                                                                                                                                                               删除打印机连接。                                                                                                                                                                               |
|              -x               |                                                                                                               从指定了 **-s**参数的服务器中删除所有打印机。 如果未指定服务器，Windows 将删除本地计算机上的所有打印机。                                                                                                               |
|              -g               |                                                                                                                                                                               显示默认打印机。                                                                                                                                                                               |
|              -t               |                                                                                                                                                        将默认打印机设置为 **-p**参数指定的打印机。                                                                                                                                                         |
|              -l               |                                                                                                         列出由 **-s**参数指定的服务器上安装的所有打印机。 如果未指定服务器，Windows 会列出安装在本地计算机上的打印机。                                                                                                         |
|               c               |                                                                                                                                      指定该参数适用于打印机连接。 可以与 **-a**和 **-x**参数一起使用。                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  指定承载要管理的打印机的远程计算机的名称。 如果未指定计算机，则使用本地计算机。                                                                                                                  |
|       -p \<printerName >       |                                                                                                                                                                指定要管理的打印机的名称。                                                                                                                                                                 |
|     -m \<DrivermodelName >     |                                                                                                          指定要安装的驱动程序（按名称）。 驱动程序通常是以支持的打印机型号命名的。 有关详细信息，请参阅打印机文档。                                                                                                           |
|        -r \<PortName >         |                                                                         指定打印机的连接端口。 如果这是并行端口或串行端口，请使用端口的 ID （例如，LPT1：或 COM1：）。 如果这是 TCP/IP 端口，请使用添加端口时指定的端口名称。                                                                          |
| -u \<UserName >-w \<Password > | 指定有权连接到承载要管理的打印机的计算机的帐户。 目标计算机的本地管理员组的所有成员都具有这些权限，但也可以向其他用户授予权限。 如果未指定帐户，则必须使用具有这些权限的帐户登录，才能使命令正常工作。 |
|              /?               |                                                                                                                                                                           在命令提示符下显示帮助。                                                                                                                                                                            |

## <a name="remarks"></a>备注
-   **Prndrvr**命令是位于%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t 目录中的 Visual Basic 脚本。 若要使用此命令，请在命令提示符下键入**cscript** ，后跟**prnmngr**文件的完整路径，或将目录更改为相应的文件夹。 例如：
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   如果提供的信息包含空格，请使用引号将文本括起来（例如 `"computer Name"`）。

## <a name="BKMK_examples"></a>示例
若要添加一个名为 colorprinter_2 的打印机，该打印机连接到本地计算机上的 LPT1 并且需要名为彩色打印机 Driver1 的打印机驱动程序，请键入：
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
若要从名为 HRServer 的远程计算机中删除名为 colorprinter_2 的打印机，请键入：
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>其他参考
[命令行语法关键字](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)
