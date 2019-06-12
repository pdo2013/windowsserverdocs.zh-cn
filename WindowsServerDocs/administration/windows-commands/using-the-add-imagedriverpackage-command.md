---
title: 使用添加 ImageDriverPackage 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 38d6c032f347f9945701f17b9289f3e3ff474031
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440622"
---
# <a name="using-the-add-imagedriverpackage-command"></a>使用添加 ImageDriverPackage 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将添加到服务器上现有的启动映像驱动程序存储区中的驱动程序包。 映像版本必须是 Windows 7 或 Windows Server 2008 R2 或更高版本。
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
|           [/ 服务器：<Server name>           |                                                                                                                                               指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。                                                                                                                                                |
|             媒体：<Image name>             |                                                                                                                                                                                       指定要添加到驱动程序的图像的名称。                                                                                                                                                                                        |
|               mediatype:Boot               |                                                                                                                                                                指定图像添加到驱动程序的类型。 驱动程序包仅可以添加到启动映像。                                                                                                                                                                 |
| / 体系结构: {x86 &#124; ia64 &#124; x64} |                                                                                                       指定启动映像的体系结构。 因为它是可以在不同的体系结构中包含的启动映像的映像名称，则应指定体系结构，以确保使用正确的映像。                                                                                                        |
|           / Filename:<File name>]           |                                                                                                                                                        指定的文件的名称。 如果名称不能唯一标识该映像，必须指定的文件的名称。                                                                                                                                                        |
|           [/ DriverPackage:<Name>           |                                                                                                                                                                                   指定要添加到映像的驱动程序包的名称。                                                                                                                                                                                    |
|             [/PackageId:<ID>]              | 指定驱动程序包的 Windows 部署服务 ID。 如果不能由名称唯一地标识驱动程序包，必须指定此选项。 若要查找程序包 ID，请单击包中的驱动程序组 (或**的所有包**节点)，右键单击包，然后单击**属性**。 上列出包 ID**常规**选项卡。例如: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}。 |

## <a name="BKMK_examples"></a>示例
若要将驱动程序包添加到启动映像中，键入以下项之一：
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 ImageDriverPackages 命令](using-the-add-imagedriverpackages-command.md)
