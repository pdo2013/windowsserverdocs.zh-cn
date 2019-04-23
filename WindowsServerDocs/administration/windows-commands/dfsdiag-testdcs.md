---
title: Dfsdiag TestDCs
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62956ae65d2311939ac0db6a4b86950f21dba407
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836598"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag TestDCs

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

通过指定域中每个域控制器上执行以下测试将检查域控制器的配置：  
  
-   验证分布式文件系统\(DFS\) Namespace 服务正在运行，并且其启动类型设置为自动。  
  
-   检查站点的支持\-成本 NETLOGON 和 SYSvol 的引用。  
  
-   通过主机名和 IP 地址来验证站点关联的一致性。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|\/域：<Domain name>|你想要检查的域。|  
  
## <a name="remarks"></a>备注  
\/域是一个可选参数。 默认值为本地主机加入到本地域。  
  
## <a name="BKMK_Examples"></a>示例  
若要验证 Contoso.com 域中域控制器的配置，请键入：  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法解答](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

