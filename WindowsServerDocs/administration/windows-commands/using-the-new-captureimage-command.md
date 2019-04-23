---
title: 使用新捕获映像命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d9f402acb9904624bdb4193a4306d57b104eda8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888608"
---
# <a name="using-the-new-captureimage-command"></a>使用新捕获映像命令



从现有的启动映像创建新的捕获映像。 捕获映像是启动 Windows 部署服务的启动映像捕获实用程序而不是启动安装程序。 当引用计算机 （即已使用 Sysprep 准备） 启动到捕获映像后时，向导创建引用计算机的安装映像，并将其保存为 Windows 映像 (.wim) 文件。 可以还将映像添加到媒体 （如 CD、 DVD 或 USB 驱动器），并从该媒体将计算机启动。 创建安装映像后，您可以将图像添加到 PXE 启动部署服务器。 有关详细信息，请参阅创建映像 ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311))。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /New-CaptureImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
        /FilePath:<File path and name>
        [/Name:<Name>]
        [/Description:<Description>]
        [/Overwrite:{Yes | No | Append}]
        [/UnattendFilePath:<File path>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/ Image:\<映像名称 >|指定源启动映像的名称。|
|/ 体系结构: {x 86 | ia64 | x64}|指定要使用的映像的体系结构。 由于你可以在不同的体系结构中有不同的启动映像的同一映像名称，使用指定这可确保正确的映像。|
|[/ 文件名：\<Filename>]|如果名称不能唯一标识该映像，必须使用此选项以指定的文件的名称。|
|/DestinationImage|指定目标图像的设置。 指定设置，请使用以下选项：</br>-   /FilePath:\<文件路径和名称 > 设置新的捕获映像的完整文件路径。</br>-[/Name:\<名称 >]-设置图像的显示名称。 如果未不指定任何显示名称，将使用源映像的显示名称。</br>-[/ 说明：\<说明 >]-设置映像的说明。</br>-[/Overwrite: {是 | 否 | Append}]-确定是否在指定的文件 **/DestinationImage** /FilePath 已存在具有该名称的另一个文件时应覆盖。 **是**将覆盖现有文件。 **不**（默认值） 后，如果已存在具有相同名称的另一个文件发生错误。 **追加**作为现有的.wim 文件中的新映像附加所生成的图像。</br>-   [/UnattendFilePath:\<文件路径 >]-设置的完整路径和无人参与的映像捕获文件的名称。|

## <a name="BKMK_examples"></a>示例

若要创建一个捕获映像并将其命名 WinPECapture.wim，键入：
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"

```
若要创建捕获映像并应用指定的设置，请键入：
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)