---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: 何时创建联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816228"
---
# <a name="when-to-create-a-federation-server-farm"></a>何时创建联合服务器场

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

请考虑在 Active Directory 联合身份验证服务中创建联合服务器场\(AD FS\)当有更大的 AD FS 部署，并且你想要提供容错、 负载\-平衡或可伸缩性再到你组织的联合身份验证服务。 在同一网络中创建两个或多个联合身份验证服务器、 配置每个使用相同的联合身份验证服务，并且添加每个服务器的公钥的令牌的行为\-签名证书为 AD FS 管理管理单元\-中创建联合服务器场。  
  
可以创建联合服务器场，也可以使用 AD FS 联合身份验证服务器配置向导，向现有场安装其他联合服务器。 有关详细信息，请参阅 [When to Create a Federation Server](When-to-Create-a-Federation-Server.md)。  
  
> [!NOTE]  
> 当您选择的选项来创建**新联合服务器场**使用 AD FS 联合身份验证服务器配置向导，该向导将尝试创建一个容器对象\(用于共享证书\)在 Active Directory。 因此，第一次登录到将要设置联合服务器角色的计算机时，务必使用在 Active Directory 中有创建此容器对象的足够权限的帐户。  
  
联合身份验证服务器可以分组为场之前，他们必须首先群集化，以便请求到达单个完全限定的域名\(FQDN\)路由到服务器场中的各种联合身份验证服务器。 可以通过部署网络负载平衡创建服务器群集\(NLB\)企业网络内部。 本指南假定，NLB 经过适当配置为每个联合服务器场中创建群集。  
  
有关如何配置群集 FQDN 的详细信息使用 Microsoft NLB 技术，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkID=74651)。  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>部署联合服务器场的最佳做法  
我们建议部署在生产环境中的联合身份验证服务器的以下最佳实践：  
  
-   如果您将多个联合身份验证服务器部署在同一时间，或者您知道，你将添加更多的服务器到服务器场随着时间的推移，请考虑在场中创建的现有的联合身份验证服务器的服务器映像，然后需要 cr 时从该映像安装eate 其他联合服务器快速。  
  
    > [!NOTE]  
    > 如果您决定使用服务器映像方法部署其他联合服务器，不需要完成的任务[核对清单：Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)每次想要向场中添加新的服务器。  
  
-   使用 NLB 或某种其他形式的聚类分析来对多个联合服务器计算机分配单个 IP 地址。  
  
-   保留每台联合服务器场中，具体取决于在域名系统的静态 IP 地址\(DNS\)配置、 排除的每个 IP 地址在动态主机配置协议中的插入\(DHCP\). Microsoft NLB 技术要求为加入 NLB 群集的每台服务器分配一个静态 IP 地址。  
  
-   如果将 SQL 数据库中存储的 AD FS 配置数据库，避免同时编辑多个联合身份验证服务器中的 SQL 数据库。  
  
## <a name="configuring-federation-servers-for-a-farm"></a>为场配置联合服务器  
下表介绍，以便每台联合服务器可以加入场环境必须完成的任务。  
  
|任务|描述|  
|--------|---------------|  
|如果要使用 SQL Server 存储 AD FS 配置数据库|联合服务器场包含两个或多个共享相同的 AD FS 配置数据库和令牌的联合身份验证服务器\-签名证书。 配置数据库可以存储在 Windows 内部数据库或 SQL Server 数据库中。 如果你打算将配置数据库存储在 SQL 数据库，请确保配置数据库是可访问，以便加入该场的所有新的联合身份验证服务器可以访问它。 **注意：** 对于场方案中，很重要的配置数据库位于不在该场中的联合身份验证服务器作为还参与的计算机上。 Microsoft NLB 不允许加入场的任何计算机相互通信。 **注意：** 确保在 Internet 信息服务中 AD FS 应用程序池标识\(IIS\) \)上每个联合服务器场中参与的服务器具有读取访问权限配置数据库。|  
|获取和共享证书|你可以获取一台服务器身份验证证书从公共证书颁发机构\(CA\)— 例如 VeriSign。 然后可以配置该证书，以便联合身份验证的所有服务器都共享的相同私钥部分的证书。 有关如何共享相同的证书的详细信息，请参阅[核对清单：设置联合身份验证服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。 **注意：** 在 AD FS 管理管理单元\-中是指用作服务通信证书的联合身份验证服务器的服务器身份验证证书。<br /><br />有关详细信息，请参阅 [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md)。|  
|指向相同的 SQL Server 实例|如果将 SQL 数据库中存储的 AD FS 配置数据库，新的联合身份验证服务器必须指向场中的其他联合身份验证服务器，以便新服务器可以加入场中使用的同一 SQL Server 实例。|  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
