---
title: irftp
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d469270b744a9de881efd9b0cfa6f1105f70519a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848418"
---
# <a name="irftp"></a>irftp

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

红外链接发送文件。    
## <a name="syntax"></a>语法  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|驱动器：\|指定包含你想要通过红外链接发送的文件的驱动器。|  
|[path]FileName|指定的位置和文件的名称或一组你想要通过红外链接发送的文件。 如果指定的一组文件，必须指定每个文件的完整路径。|  
|/h|指定隐藏的模式。 使用隐藏模式下时，将在发送文件而不会显示无线链接对话框。|  
|/s|打开无线链接对话框中，以便可以选择想要发送，而不必使用命令行指定驱动器、 路径和文件名的文件组的文件。|  

## <a name="remarks"></a>备注  
-   然后再使用此命令，确认你想要通过红外链接进行通信的设备已启用红外功能和正常工作，并且，设备之间建立红外链接。  
-   使用不带参数或用于 **/s**， **irftp**打开**无线链接**对话框中，您可以在其中选择想要发送而无需使用命令行的文件。  

## <a name="BKMK_Examples"></a>示例  
通过红外链接发送 Example.txt。  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
