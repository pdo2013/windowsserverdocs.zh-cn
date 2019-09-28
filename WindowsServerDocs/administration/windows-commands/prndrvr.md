---
title: prndrvr
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 05e03a4b0b5d686c8fbd1646a775c7f0e5c95706
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372116"
---
# <a name="prndrvr"></a>prndrvr

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用**prndrvr**命令可添加、删除和列出打印机驱动程序。

## <a name="syntax"></a>语法
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|-a|安装驱动程序。|
|-d.ddd...e|删除驱动程序。|
|-l|列出由 **-s**参数指定的服务器上安装的所有打印机驱动程序。 如果未指定服务器，Windows 会列出安装在本地计算机上的打印机驱动程序。|
|-x|删除由 **-s**参数指定的服务器上的逻辑打印机未使用的所有打印机驱动程序和其他打印机驱动程序。 如果未指定要从列表中删除的服务器，Windows 将删除本地计算机上所有未使用的打印机驱动程序。|
|-m \<DrivermodelName @ no__t-1|指定要安装的驱动程序（按名称）。 驱动程序通常是以支持的打印机型号命名的。 有关详细信息，请参阅打印机文档。|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|指定要安装的驱动程序的版本。 有关适用于哪个环境的版本的信息，请参阅 **-e**参数的描述。 如果未指定版本，则会安装适用于在其上安装驱动程序的计算机上运行的 Windows 版本的驱动程序版本。<br /><br />-版本**0**支持 windows 95、windows 98 和 windows Millennium edition。<br />-版本**1**支持 Windows NT 3.51。<br />-版本**2**支持 Windows NT 4.0。<br />-版本**3**支持 windows Vista、windows XP、windows 2000 和 windows Server 2003 操作系统。 请注意，这是 Windows Vista 支持的唯一打印机驱动程序版本。|
|-e \<Environment >|指定要安装的驱动程序的环境。 如果未指定环境，则使用要安装驱动程序的计算机的环境。 支持的环境参数包括：<br /><br />-    **"WINDOWS NT x86"**<br />-    **"Windows x64"**<br />-    **"WINDOWS IA64"**|
|-s \<ServerName >|指定承载要管理的打印机的远程计算机的名称。 如果未指定计算机，则使用本地计算机。|
|-u \<UserName >-w \<Password >|指定有权连接到承载要管理的打印机的计算机的帐户。 目标计算机的本地管理员组的所有成员都具有这些权限，但也可以向其他用户授予权限。 如果未指定帐户，则必须使用具有这些权限的帐户登录，才能使命令正常工作。|
|-h \<path >|指定驱动程序文件的路径。 如果未指定路径，则将使用安装 Windows 的位置的路径。|
|-i \<Filename >|指定要安装的驱动程序的完整路径和文件名。 如果未指定文件名，该脚本将使用 Windows 目录的 inf 子目录中的某个收件箱打印机 .inf 文件。<br /><br />如果未指定驱动程序路径，则脚本将在驱动程序 .cab 文件中搜索驱动程序文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
- **Prndrvr**命令是位于%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t 目录中的 Visual Basic 脚本。 若要使用此命令，请在命令提示符下键入**cscript** ，后跟 prndrvr 文件的完整路径，或将目录更改为相应的文件夹。

  例如：
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- 如果提供的信息包含空格，请使用引号将文本括起来（例如 `"computer Name"`）。
- -X 选项会删除所有附加的打印机驱动程序（安装在运行 Windows 备用版本的客户端上使用的驱动程序），即使正在使用主驱动程序也是如此。 如果安装了传真组件，此选项也会删除传真驱动程序。 如果主传真驱动程序未被使用（即，如果没有队列使用它），则会将其删除。 如果删除了主传真驱动程序，则重新启用传真的唯一方法是重新安装传真组件。
- 使用不带参数的**prndrvr**将显示**prndrvr**命令的命令行帮助。

## <a name="BKMK_examples"></a>示例

若要列出 \\ \ printServer1 服务器上的所有驱动程序，请键入：
```
cscript Prndrvr -l -s
```

若要为在 C：\temp 文件夹类型中存储的驱动程序使用 C:\temp\Laserprinter1.inf 驱动程序信息文件为打印机的 "激光打印机型号 1" 模型添加版本 3 Windows x64 打印机驱动程序，请执行以下操作：
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

若要删除 "激光打印机型号 1" 的版本 3 Windows NT x86 打印机驱动程序，请键入：
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>其他参考
[命令行语法关键字](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)
