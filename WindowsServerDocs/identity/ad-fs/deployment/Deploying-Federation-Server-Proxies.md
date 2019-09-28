---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: 部署联合服务器代理
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c6af319283de72963691ae3e91c3db5992bdec72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408406"
---
# <a name="deploying-federation-server-proxies"></a>部署联合服务器代理

若要在 Active Directory 联合身份验证服务 @no__t 中部署联合服务器代理，请完成 [Checklist 中的每个任务：设置联合服务器代理 @ no__t。  
  
> [!NOTE]  
> 使用此清单时，建议您先阅读[Windows server 2012 的 "AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)" 中的联合服务器代理规划指南的参考，然后再开始配置服务器的步骤。 按照清单，可以更好地了解联合服务器代理的设计和部署过程。  
  
## <a name="about-federation-server-proxies"></a>关于联合服务器代理  
联合服务器代理是运行 Windows Server®2012和已手动配置为在代理角色中操作 AD FS 软件的计算机。 你可以在组织中使用联合服务器来提供企业网络上防火墙后的 Internet 客户端和联合服务器之间的中介服务。  
  
> [!NOTE]  
> 尽管不能在同一台计算机上安装联合服务器和联合服务器代理角色，但联合服务器可以执行联合服务器代理功能。 有关详细信息，请参阅 [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx)。  
  
在 Windows Server®2012计算机上安装 AD FS 软件并将其配置为在代理角色中服务的操作使该计算机成为联合服务器代理。  
  

