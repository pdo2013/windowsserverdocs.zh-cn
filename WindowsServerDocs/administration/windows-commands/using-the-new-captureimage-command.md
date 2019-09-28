---
title: 使用捕获映像命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb48cb76ef99eac51b862a5e1a3d1999a1cfc89d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363046"
---
# <a name="using-the-new-captureimage-command"></a>使用捕获映像命令



从现有的启动映像创建新的捕获映像。 捕获映像是启动 Windows 部署服务捕获实用程序（而不是启动安装程序）的启动映像。 将引用计算机（已使用 Sysprep 进行准备）引导到捕获映像时，向导将创建引用计算机的安装映像，并将其保存为 Windows 映像（.wim）文件。 还可以将图像添加到媒体（例如 CD、DVD 或 USB 驱动器），然后从该媒体启动计算机。 创建安装映像后，你可以将映像添加到服务器以进行 PXE 启动部署。 有关详细信息，请参阅创建映像（[https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)）。

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

|        参数         |                                                                                                                                                                                                                         描述                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： @no__t 名称 >] |                                                                                                                                       指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。                                                                                                                                        |
|   /Image： @no__t 名称 >   |                                                                                                                                                                                                         指定源启动映像的名称。                                                                                                                                                                                                         |
|   /Architecture： {x86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| [/Filename:\<Filename >] |                                                                                                                                                                            如果无法按名称唯一地标识映像，则必须使用此选项指定文件名。                                                                                                                                                                            |
|    /DestinationImage     | 指定目标映像的设置。 使用以下选项指定设置：</br>/FilePath@no__t 路径和名称 > 设置新捕获映像的完整文件路径。</br>-[/Name：\<Name >]-设置图像的显示名称。 如果未指定显示名称，将使用源映像的显示名称。</br>-[/Description：\<Description >]-设置映像的描述。</br>-[/Overwrite： {Yes |

## <a name="BKMK_examples"></a>示例

若要创建捕获映像并将其命名为 WinPECapture，请键入：
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"
```
若要创建捕获映像并应用指定的设置，请键入：
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)