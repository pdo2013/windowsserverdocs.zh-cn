---
title: 使用发现映像命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ede9fbbb-0bba-4309-8c21-3cc13e1dc3cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b17777eb4d8541ce5669ee6becbd51e2b9da236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362989"
---
# <a name="using-the-new-discoverimage-command"></a>使用发现映像命令



从现有启动映像创建新的发现映像。 发现映像是强制 setup.exe 程序在 Windows 部署服务模式下启动，然后发现 Windows 部署服务服务器的启动映像。 通常，这些映像可用于将映像部署到不能启动到 PXE 的计算机。 有关详细信息，请参阅创建映像（[https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)）。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /New-DiscoverImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/WDSServer:<Server name>]
         [/Overwrite:{Yes | No | Append}]
```

## <a name="parameters"></a>Parameters

|        参数         |                                                                                                                                                                                                                                                                                                                                                                                                                       描述                                                                                                                                                                                                                                                                                                                                                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： @no__t 名称 >] |                                                                                                                                                                                                                                                                                                                                     指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。                                                                                                                                                                                                                                                                                                                                     |
|   /Image： @no__t 名称 >   |                                                                                                                                                                                                                                                                                                                                                                                                      指定源启动映像的名称。                                                                                                                                                                                                                                                                                                                                                                                                       |
|    /Architecture： {x86    |                                                                                                                                                                                                                                                                                                                                                                                                                          ia64                                                                                                                                                                                                                                                                                                                                                                                                                           |
| [/Filename： \<File name >] |                                                                                                                                                                                                                                                                                                                                                                         如果无法按名称唯一地标识映像，则必须使用此选项指定文件名。                                                                                                                                                                                                                                                                                                                                                                          |
|    /DestinationImage     | 指定目标映像的设置。 可以使用以下选项指定设置：</br>-/FilePath： < 文件路径和名称 >-设置新映像的完整文件路径。</br>-[/Name： \<Name >]-设置图像的显示名称。 如果未指定显示名称，将使用源映像的显示名称。</br>-[/Description：\<Description >]-设置映像的描述。</br>-[/WDSServer：\<Server name >]-指定从指定映像启动的所有客户端应联系以下载安装映像的服务器的名称。 默认情况下，启动此映像的所有客户端都将发现有效的 Windows 部署服务服务器。 使用此选项将绕过发现功能，并强制启动的客户端联系指定的服务器。</br>-[/Overwrite： {Yes |

## <a name="BKMK_examples"></a>示例

若要从启动映像中创建发现映像并将其命名为 WinPEDiscover，请键入：
```
WDSUTIL /New-DiscoverImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPEDiscover.wim"
```
若要创建启动映像的发现映像，并使用指定的设置将其命名为 WinPEDiscover，请键入：
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:"\\Server\Share\WinPEDiscover.wim" 
/Name:"New WinPE image" /Description:"WinPE image for WDS Client discovery" /Overwrite:No
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)