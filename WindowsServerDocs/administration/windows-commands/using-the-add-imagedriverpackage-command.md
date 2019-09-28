---
title: 使用 ImageDriverPackage 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1069b7c63e8b3bbc28fd900e8c869afc8abc03bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363735"
---
# <a name="using-the-add-imagedriverpackage-command"></a>使用 ImageDriverPackage 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将驱动程序存储区中的驱动程序包添加到服务器上的现有启动映像。 映像版本必须是 Windows 7 或 Windows Server 2008 R2 或更高版本。
## <a name="syntax"></a>语法
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parameters

|                 参数                  |                                                                                                                                                                                                            描述                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           [/Server： <Server name>           |                                                                                                                                               指定服务器的名称。 此名称可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。                                                                                                                                                |
|             媒体： <Image name>             |                                                                                                                                                                                       指定要将驱动程序添加到的映像的名称。                                                                                                                                                                                        |
|               媒体：启动               |                                                                                                                                                                指定要向其添加驱动程序的图像的类型。 只能将驱动程序包添加到启动映像。                                                                                                                                                                 |
| /Architecture： {x86 &#124; ia64 &#124; x64} |                                                                                                       指定启动映像的体系结构。 由于不同体系结构中的启动映像可能具有相同的映像名称，因此应指定体系结构以确保使用正确的映像。                                                                                                        |
|           /Filename： <File name>]           |                                                                                                                                                        指定文件的名称。 如果无法按名称唯一地标识图像，则必须指定文件名。                                                                                                                                                        |
|           [/DriverPackage： <Name>           |                                                                                                                                                                                   指定要添加到映像的驱动程序包的名称。                                                                                                                                                                                    |
|             [/PackageId： @no__t]              | 指定驱动程序包的 Windows 部署服务 ID。 如果无法按名称唯一地标识驱动程序包，则必须指定此选项。 若要查找包 ID，请单击包所在的驱动程序组（或 "**所有包**" 节点），右键单击该包，然后单击 "**属性**"。 包 ID 在 "**常规**" 选项卡上列出。例如： {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}。 |

## <a name="BKMK_examples"></a>示例
若要将驱动程序包添加到启动映像，请键入下列内容之一：
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>其他参考
[使用 ImageDriverPackages 命令](using-the-add-imagedriverpackages-command.md)的[命令行语法键](command-line-syntax-key.md)

