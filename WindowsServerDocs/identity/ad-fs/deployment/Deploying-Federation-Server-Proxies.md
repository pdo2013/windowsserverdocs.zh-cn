---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: 部署联合服务器代理
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816258"
---
# <a name="deploying-federation-server-proxies"></a>部署联合服务器代理

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

若要部署 Active Directory 联合身份验证服务中的联合服务器代理\(AD FS\)，完成每个中的任务[核对清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
> [!NOTE]  
> 当使用此清单时，我们建议您先阅读规划指南中的联合服务器代理的引用[Windows Server 2012 中 AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)开始配置服务器的过程之前。 以下清单提供了联合身份验证的设计和部署过程的更好地了解服务器代理。  
  
## <a name="about-federation-server-proxies"></a>有关联合服务器代理  
联合服务器代理是运行 Windows Server® 2012年和 AD FS 软件已被手动配置为充当代理角色的计算机。 你可以在组织中使用联合服务器来提供企业网络上防火墙后的 Internet 客户端和联合服务器之间的中介服务。  
  
> [!NOTE]  
> 尽管不能在同一台计算机上安装联合身份验证服务器和联合服务器代理角色，联合身份验证服务器可以执行联合身份验证服务器代理功能。 有关详细信息，请参阅 [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx)。  
  
在 Windows Server® 2012年计算机上安装 AD FS 软件并将其用于代理角色配置的操作将使该计算机的联合身份验证服务器代理。  
  

