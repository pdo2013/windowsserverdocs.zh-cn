---
title: showmount
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26acf24922e6ac53a5c902d65eb0f23bff0af93b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852348"
---
# <a name="showmount"></a>showmount

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可以使用**showmount**以显示已装载的目录。  
  
## <a name="syntax"></a>语法  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>描述  
**Showmount**命令\-行实用程序会显示有关导出 nfs 服务器上指定的计算机的已装载的文件系统的信息*Server*。 如果*服务器*未提供，则**showmount**有关计算机的信息显示在其上**showmount**运行命令。  
  
必须提供以下选项之一：  
  
- **\-e** -显示所有文件系统在服务器上导出。  
- **\-** -显示所有网络文件系统\(NFS\)客户端和每个已装载的服务器上的目录。  
- **\-d** -显示当前装载 NFS 客户端的服务器上的所有目录。  
  
## <a name="see-also"></a>请参阅  
[有关网络文件系统命令参考服务](services-for-network-file-system-command-reference.md)  