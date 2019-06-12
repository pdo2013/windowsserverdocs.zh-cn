---
title: prndrvr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edf27852c13566ba8c7ca8c16d789d586e749160
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436209"
---
# <a name="prndrvr"></a>prndrvr

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用**prndrvr**命令来添加、 删除和列出打印机的驱动程序。

## <a name="syntax"></a>语法
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|-a|安装的驱动程序。|
|-d|删除驱动程序。|
|-l|列出了安装在指定的服务器上的所有打印机驱动程序 **-s**参数。 如果未指定服务器，Windows 会列出在本地计算机上安装的打印机驱动程序。|
|-x|删除所有打印机驱动程序和其他打印机驱动程序在不使用由指定的服务器上的逻辑打印机 **-s**参数。 如果不指定要从列表中删除的服务器，Windows 将删除本地计算机上的所有未使用的打印机驱动程序。|
|-m \<DrivermodelName\>|（按名称） 指定你想要安装的驱动程序。 驱动程序通常被命名的打印机支持的模型。 请参阅打印机文档了解详细信息。|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|指定你想要安装的驱动程序版本。 请参阅的说明 **-e**版本的信息在其上可用于哪些环境的参数。 如果不指定版本，安装适用于 Windows 的计算机上运行安装该驱动程序的版本的驱动程序的版本。<br /><br />-版本**0**支持 Windows 95、 Windows 98 和 Windows Millennium edition。<br />-版本**1**支持 Windows NT 3.51。<br />-版本**2**支持 Windows NT 4.0。<br />-版本**3**支持 Windows Vista、 Windows XP、 Windows 2000 和 Windows Server 2003 操作系统。 请注意，这是 Windows Vista 支持的唯一打印机驱动程序版本。|
|-e\<环境 >|指定你想要安装的驱动程序的环境。 如果未指定环境，则使用在其中安装该驱动程序的计算机的环境。 受支持的环境参数包括：<br /><br />-    **"Windows NT x86"**<br />-    **"Windows x64"**<br />-    **"Windows IA64"**|
|-s\<服务器名 >|指定托管你想要管理的打印机的远程计算机的名称。 如果不指定计算机，则使用本地计算机。|
|-u \<UserName> -w \<Password>|指定的帐户有权连接到承载你想要管理的打印机的计算机。 所有目标计算机的本地 Administrators 组的成员都具有这些权限，但也可以向其他用户授予权限。 如果不指定一个帐户，你必须登录才能使用该命令这些权限的帐户下。|
|-h \<path>|指定驱动程序文件的路径。 如果不指定路径，则使用 Windows 的安装位置的位置的路径。|
|-i \<Filename.inf >|指定你想要安装的驱动程序的完整路径和文件名称。 如果不指定文件名称，该脚本使用的 Windows 目录的 inf 子目录中的收件箱打印机.inf 文件之一。<br /><br />如果未指定的驱动程序路径，该脚本会搜索 driver.cab 文件中的驱动程序文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
- **Prndrvr**命令是 Visual Basic 脚本位于 %WINdir%\System32\printing_Admin_Scripts\\ <language>目录。 若要使用此命令中，在命令提示符处，键入**cscript**然后到 prndrvr 文件或将目录更改为适当的文件夹的完整路径。

  例如：
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- 如果你提供的信息包含空格，使用文本周围的引号 (例如， `"computer Name"`)。
- -X 选项删除所有附加的打印机驱动程序 （驱动程序使用客户端安装的 Windows 运行备选版本），即使主驱动程序正在使用中。 如果安装传真组件，则此选项也会删除传真驱动程序。 如果不是使用 （也就是说，如果没有使用它的队列） 中，删除主传真驱动程序。 如果删除主传真驱动程序，则重新启用传真的唯一方法是重新安装传真组件。
- 使用不带参数， **prndrvr**将显示命令行帮助**prndrvr**命令。

## <a name="BKMK_examples"></a>示例

若要列出的所有驱动程序上\\\printServer1 服务器中，键入：
```
cscript Prndrvr -l -s
```

若要添加打印机驱动程序存储在 C:\temp 文件夹类型使用 C:\temp\Laserprinter1.inf 驱动程序信息文件的"激光打印机模型 1"模型的版本 3 Windows x64 打印机驱动程序：
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

若要删除"激光打印机模型 1"的第 3 版 Windows NT x86 打印机驱动程序，请键入：
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)
