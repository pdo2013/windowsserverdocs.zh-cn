---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: "部署联合身份验证的服务器代理服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-server-proxies"></a>部署联合身份验证的服务器代理服务器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

部署中 Active Directory 联合身份验证服务 \(AD FS\) 联合 server 代理，完成每个在任务[清单：设置向上联盟服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
> [!NOTE]  
> 当你使用此清单时，我们建议你先阅读联合身份验证的服务器代理计划中的指南参考[广告 FS 设计指南 Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx)之前开始配置服务器的过程。 按照清单服务器代理提供联合身份验证的设计和部署过程更好地理解。  
  
## <a name="about-federation-server-proxies"></a>有关联合身份验证的服务器代理服务器  
联合身份验证的服务器代理是运行 Windows Server 的计算机® 2012 年广告 FS 可和软件已手动配置代理角色中采取措施。 可以使用在你的组织的服务器联合身份验证的代理提供之间 Internet 客户和企业网络上的防火墙是联合服务器中间服务。  
  
> [!NOTE]  
> 虽然无法同一台计算机上安装的联合身份验证的服务器和联合身份验证的服务器代理角色，联合服务器可以执行联盟服务器代理功能。 有关详细信息，请参阅[何时创建联盟服务器](https://technet.microsoft.com/library/dd807101.aspx)。  
  
安装在 Windows Server 广告 FS 软件法案® 2012 年计算机并将其配置为在代理角色提供使该计算机联合身份验证的服务器代理。  
  

