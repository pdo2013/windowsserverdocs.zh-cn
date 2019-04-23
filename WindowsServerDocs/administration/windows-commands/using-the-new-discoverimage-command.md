---
title: 使用新发现映像命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1212571fc07b912ab9fa3344b973ce5a25085d86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835768"
---
# <a name="using-the-new-discoverimage-command"></a>使用新发现映像命令



从现有的启动映像创建新的发现映像。 发现映像是强制 Setup.exe 程序在 Windows 部署服务模式下启动并发现 Windows 部署服务服务器的启动映像。 通常这些映像用于将映像部署到不支持的启动到 PXE 的计算机。 有关详细信息，请参阅创建映像 ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311))。

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

|参数|描述|
|---------|-----------|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/ Image:\<映像名称 >|指定源启动映像的名称。|
|/ 体系结构: {x86 | ia64 | x64}|指定要返回的映像的体系结构。 因为它是可以在不同的体系结构中包含不同的启动映像的同一映像名称，指定体系结构值可确保 WDSUTIL 返回正确的映像。|
|[/ Filename:\<文件名称 >]|如果名称不能唯一标识该映像，必须使用此选项以指定的文件的名称。|
|/DestinationImage|指定目标图像的设置。 可以指定设置，请使用以下选项：</br>-/FilePath: < 文件路径和名称 >-为新映像集完整文件路径。</br>-[/Name:\<名称 >]-设置图像的显示名称。 如果未不指定任何显示名称，将使用源映像的显示名称。</br>-[/ 说明：\<说明 >]-设置映像的说明。</br>-[/ WDSServer:\<服务器名称 >]-指定从指定的映像启动的所有客户端应联系，若要下载的安装映像的服务器的名称。 默认情况下，启动此映像的所有客户端将发现有效的 Windows 部署服务服务器。 使用此选项会绕过的发现功能，并强制启动客户端联系指定的服务器。</br>-[/Overwrite: {是 | 否 | Append}]-确定是否在指定的文件 **/DestinationImage** /FilePath 已存在具有该名称的另一个文件时应覆盖。 **是**将覆盖现有文件。 **不**（默认值） 后，如果已存在具有相同名称的另一个文件发生错误。 **追加**作为现有的.wim 文件中的新映像附加所生成的图像。|

## <a name="BKMK_examples"></a>示例

若要创建发现映像从启动映像，并将其命名 WinPEDiscover.wim，键入：
```
WDSUTIL /New-DiscoverImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPEDiscover.wim"
```
若要创建发现映像从启动映像，并将其命名 WinPEDiscover.wim 使用指定的设置，请键入：
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:"\\Server\Share\WinPEDiscover.wim" 
/Name:"New WinPE image" /Description:"WinPE image for WDS Client discovery" /Overwrite:No
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)