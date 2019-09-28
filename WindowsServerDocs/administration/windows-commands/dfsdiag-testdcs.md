---
title: dfsdiag TestDCs
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a193e68b6f015b1535a98e20b52deb2a4a14034c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378436"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

通过在指定域中的每个域控制器上执行以下测试来检查域控制器的配置：  
  
-   验证分布式文件系统 @no__t 0DFS @ no__t 命名空间服务是否正在运行，并且其启动类型是否已设置为 "自动"。  
  
-   检查是否支持 NETLOGON 和 SYSvol 的 site @ no__t-0costed 引用。  
  
-   验证站点关联的一致性（按主机名和 IP 地址）。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|\/Domain： <Domain name>|要检查的域。|  
  
## <a name="remarks"></a>备注  
@no__t 0Domain 是一个可选参数。 默认值为本地主机联接到的本地域。  
  
## <a name="BKMK_Examples"></a>示例  
若要验证 Contoso.com 域中的域控制器的配置，请键入：  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

