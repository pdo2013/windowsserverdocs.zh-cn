---
title: tftp
description: 传输到和从远程计算机的文件。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ad195409076840fda0e8d6bf5cd0c295a62cdede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845898"
---
# <a name="tftp"></a>tftp

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将文件传输到和从远程计算机通常运行 UNIX，普通文件传输协议 (tftp) 服务或后台程序正在运行的计算机。 tftp 通常使用嵌入式的设备或在启动过程中从 tftp 服务器检索固件、 配置信息或系统映像的系统。   

## <a name="syntax"></a>语法  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|-i|指定二进制图像传输模式 （也称为八位字节模式）。 在二进制映像模式下，文件传输单字节为单位。 传输的二进制文件时，请使用此模式。 如果 **-i**是省略，该文件以 ASCII 模式传输。 这是默认的传输模式。 此模式将结束的行 （行尾） 字符转换为适当的格式指定的计算机。 传输文本文件时，请使用此模式。 如果文件传输成功，将显示的数据传输速率。|  
|\<主机\>|指定的本地或远程计算机。|  
|put|将文件传输*源*文件在本地计算机上*目标*远程计算机上。 Tftp 协议不支持用户身份验证，因为用户必须登录到远程计算机，并且文件必须是远程计算机上可写入。|  
|获取|将文件传输*目标*文件在远程计算机上*源*本地计算机上。|  
|\<源\>|指定要传输的文件。|  
|\<目标\>|指定传输文件的位置。|  

## <a name="remarks"></a>备注  
-   你可以安装 tftp 客户端使用添加功能向导。  
-   Tftp 协议不支持任何身份验证或加密机制，并且这种情况下可以带来安全风险时存在。 对于连接到 Internet 的系统，建议不要安装 tftp 客户端。  
-   Tftp 客户端是可选软件，并标记为已弃用在 Windows Vista 和更高版本的 Windows 操作系统上。 Tftp 服务器服务不能再由 Microsoft 提供出于安全原因。  

## <a name="BKMK_Examples"></a>示例  
将文件复制**boot.img**从远程计算机**Host1**。  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
