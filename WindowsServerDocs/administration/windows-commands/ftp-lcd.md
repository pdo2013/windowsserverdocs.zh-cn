---
title: ftp lcd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eac028c8aa675e680dedefcfe9f0b8da18ce7179
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826888"
---
# <a name="ftp-lcd"></a>ftp: lcd

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改本地计算机上的工作目录。 默认情况下，工作目录是在其中的目录**ftp**已启动。   
## <a name="syntax"></a>语法  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[<directory>]|要更改到在本地计算机上指定的目录。 如果*directory*未指定，则当前工作目录更改为默认目录。|  
## <a name="BKMK_Examples"></a>示例  
更改到本地计算机上的工作目录**C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
