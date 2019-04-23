---
title: ftp open_1
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da7926914e2cbdbb4909093d90c33025ae5cd695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882468"
---
# <a name="ftp-open1"></a>ftp: open_1

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

连接到指定的 ftp 服务器。   
## <a name="syntax"></a>语法  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|<computer>|指定要连接的远程计算机。|  
|[<Port>]|指定要用于连接到 ftp 服务器的 TCP 端口号。 默认情况下使用 TCP 端口 21。|  
## <a name="remarks"></a>备注  
可以使用 （在这种情况下的 DNS 服务器或主机文件必须可用） 的 IP 地址或计算机名称来指定**计算机**。  
## <a name="BKMK_Examples"></a>示例  
连接到 ftp 服务器上**ftp.microsoft.com**。  
```  
Open ftp.microsoft.com  
```  
连接到 ftp 服务器上**ftp.microsoft.com**侦听 TCP 端口 755。  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
