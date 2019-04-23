---
title: unlodctr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e1a662da10acc65b4ad2fd0d055cf9d46de603be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886648"
---
# <a name="unlodctr"></a>unlodctr

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

从系统注册表中删除性能计数器名称和服务或设备驱动程序的说明文字。   

## <a name="syntax"></a>语法  
```  
Unlodctr <DriverName>   
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|\<DriverName>|删除性能计数器名称设置和说明文字，驱动程序或服务<DriverName>从 Windows Server 2003 注册表。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
> [!WARNING]  
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。  

如果你提供的信息包含空格，使用文本周围的引号 (例如，"<DriverName>")。  

## <a name="BKMK_Examples"></a>示例  
若要删除当前性能的注册表设置和计数器的简单邮件传输协议 (SMTP) 服务的说明文字：  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
  
