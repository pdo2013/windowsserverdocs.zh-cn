---
title: unlodctr
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85a66b521f404358705962078f33af4bec1ebae5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363898"
---
# <a name="unlodctr"></a>unlodctr

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

从系统注册表中删除服务或设备驱动程序的性能计数器名称和说明文本。   

## <a name="syntax"></a>语法  
```  
Unlodctr <DriverName>   
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|\<DriverName >|从 Windows Server 2003 注册表删除驱动程序或服务 @no__t 的性能计数器名称设置和说明文本。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
> [!WARNING]  
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。  

如果提供的信息包含空格，请使用引号将文本括起来（例如，"<DriverName>"）。  

## <a name="BKMK_Examples"></a>示例  
若要为简单邮件传输协议（SMTP）服务删除当前性能注册表设置和计数器说明文本，请执行以下操作：  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
  
