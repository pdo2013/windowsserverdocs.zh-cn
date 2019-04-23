---
title: Dfsdiag TestReferral
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd1b87befa8a9cfda5ea27a4ce5a5105ea1a1009
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848018"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag TestReferral

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检查分布式文件系统\(DFS\)引用通过执行以下测试：  
  
-   当使用不带参数的 DFSpath 参数时，此命令将验证引用列表，包括所有受信任的域。  
  
-   该命令时指定的域，执行域控制器的运行状况检查\(dfsdiag \/testdcs\)并测试的站点关联以及本地主机的域缓存。  
  
-   当指定的域和\\SYSvol 或\\NETLOGON，除了执行相同的运行状况检查时指定的域，该命令将检查的生存时间\(TTL\) SYSvol 或 NETLOGON 引用的匹配 900 秒的默认值。  
  
-   当指定命名空间根，除了执行相同的运行状况检查，因为该命令时指定的域，执行 DFS 配置检查\(dfsdiag \/TestDFSConfig\)和命名空间的完整性检查\(dfsdiag \/TestDFSIntegrity\)。  
  
-   当指定的 DFS 文件夹\(链接\)，除了执行相同的运行状况检查，因为当您指定命名空间根路径，该命令将验证文件夹目标的站点配置\(dfsdiag \/testsites\)并验证站点关联的本地主机。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|\/DFSpath:<path for getting referrals>|此 DFS 路径可以是以下值之一：<br /><br />-   \(空白\):测试受信任的域。<br />-   \\\\域：域控制器的引用。<br />-   \\\\域\\SYSvol:SYSvol 引用。<br />-   \\\\域\\NETLOGON:NETLOGON 引用。<br />-   \\\\<Domain or server>\\<Namespace Root>:Namespace 根路径引用。<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>:DFS 文件夹\(链接\)引用。|  
|\/完整|仅适用于域和根的引用。 用于验证的注册表和 active directory 域服务之间的站点关联信息一致性\(AD DS\)。|  
  
## <a name="BKMK_Examples"></a>示例  
若要 TBD，键入：  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
若要 TBD，键入：  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法解答](command-line-syntax-key.md)  
  

