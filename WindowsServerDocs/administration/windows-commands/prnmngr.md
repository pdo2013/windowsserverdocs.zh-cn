---
title: prnmngr
description: 了解如何添加、 删除和列出打印机和连接。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2c0aa44cc6f27e553bf8c1b57356b884bc0cd632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887198"
---
# <a name="prnmngr"></a>prnmngr

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

添加、 删除和列出打印机或打印机连接，以及设置和显示默认打印机。

## <a name="syntax"></a>语法
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|-a|添加本地打印机连接。|
|-d|删除打印机连接。|
|-x|从与指定的服务器中删除所有打印机 **-s**参数。 如果未指定服务器，Windows 会删除本地计算机上的所有打印机。|
|-g|显示默认打印机。|
|-t|将默认打印机设置为指定的打印机 **-p**参数。|
|-l|列出了安装在指定的服务器上的所有打印机 **-s**参数。 如果未指定服务器，Windows 会列出在本地计算机上安装的打印机。|
|c|指定该参数，适用于打印机连接。 可用于 **-a**并 **-x**参数。|
|-s <ServerName>|指定托管你想要管理的打印机的远程计算机的名称。 如果不指定计算机，则使用本地计算机。|
|-p \<printerName>|指定你想要管理的打印机的名称。|
|-m \<DrivermodelName>|（按名称） 指定你想要安装的驱动程序。 驱动程序通常被命名的打印机支持的模型。 请参阅打印机文档了解详细信息。|
|-r\<端口名 >|指定打印机已连接的端口。 如果这是一种并行或串行端口，使用该端口的 ID (例如，LPT1： 或 COM1:)。 如果这是一个 TCP/IP 端口，使用已添加端口时指定的端口名称。|
|-u \<UserName> -w \<Password>|指定的帐户有权连接到承载你想要管理的打印机的计算机。 所有目标计算机的本地 Administrators 组的成员都具有这些权限，但也可以向其他用户授予权限。 如果不指定一个帐户，你必须登录才能使用该命令这些权限的帐户下。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **Prndrvr**命令是 Visual Basic 脚本位于 %WINdir%\System32\printing_Admin_Scripts\\ <language>目录。 若要使用此命令中，在命令提示符处，键入**cscript**跟完整路径**prnmngr**文件，或将目录更改为适当的文件夹。 例如：
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   如果你提供的信息包含空格，使用文本周围的引号 (例如， `"computer Name"`)。

## <a name="BKMK_examples"></a>示例
若要添加名为 colorprinter_2 连接 LPT1 到本地计算机上，需要调用彩色打印机 Driver1 的打印机驱动程序的打印机，请键入：
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
若要删除名为 colorprinter_2 从名为 HRServer 的远程计算机的打印机，请键入：
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)
