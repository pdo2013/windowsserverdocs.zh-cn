---
title: tftp
description: 在远程计算机之间传输文件。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66f729d090a78b74bc0334cd9b7276219a980e8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370208"
---
# <a name="tftp"></a>tftp

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在运行普通文件传输协议（tftp）服务或后台程序的远程计算机（通常是运行 UNIX 的计算机）之间传输文件。 tftp 通常由在启动过程中从 tftp 服务器检索固件、配置信息或系统映像的嵌入式设备或系统使用。   

## <a name="syntax"></a>语法  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|-i|指定二进制图像传输模式（也称为八进制模式）。 在二进制图像模式下，文件以一字节单位传输。 传输二进制文件时，请使用此模式。 如果省略 **-i** ，则文件将在 ASCII 模式下传输。 这是默认传输模式。 此模式将行尾（EOL）字符转换为指定计算机的相应格式。 在传输文本文件时使用此模式。 如果文件传输成功，将显示数据传输速率。|  
|\<Host @ no__t-1|指定本地或远程计算机。|  
|准备|将本地计算机上的文件*源*传输到远程计算机上的文件*目标*。 由于 tftp 协议不支持用户身份验证，因此用户必须登录到远程计算机上，并且这些文件必须可在远程计算机上写入。|  
|获取|将远程计算机上的文件*目标*传输到本地计算机上的文件*源*。|  
|\<源\>|指定要传输的文件。|  
|\<目标\>|指定文件传输位置。|  

## <a name="remarks"></a>备注  
-   您可以使用 "添加功能向导" 安装 tftp 客户端。  
-   Tftp 协议不支持任何身份验证或加密机制，因此在存在时可能会带来安全风险。 不建议在连接到 Internet 的系统上安装 tftp 客户端。  
-   Tftp 客户端是可选软件，在 Windows Vista 和更高版本的 Windows 操作系统上被标记为已弃用。 出于安全原因，Microsoft 不再提供 tftp 服务器服务。  

## <a name="BKMK_Examples"></a>示例  
从远程计算机**Host1**复制文件 **。**  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
