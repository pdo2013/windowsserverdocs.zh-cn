---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: 何时创建联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fcfc7d640d3688bf0e23557af9bd56082418ef37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358939"
---
# <a name="when-to-create-a-federation-server-farm"></a>何时创建联合服务器场

如果你有较大的 AD FS 部署，并且想要为组织的联合提供容错、负载 @ no__t-2balancing 或可伸缩性，请考虑在 Active Directory 联合身份验证服务 \(AD FS @ no__t-1 中创建联合服务器场服务. 在同一网络中创建两台或更多联合服务器的操作，将每个联合服务器配置为使用相同的联合身份验证服务，并将每个服务器的 token @ no__t-0signing 证书的公钥添加到 AD FS 管理 snap @ no__t-1in 创建联合服务器场。  
  
您可以使用 AD FS 联合服务器配置向导创建联合服务器场或向现有场安装其他联合服务器。 有关详细信息，请参阅 [When to Create a Federation Server](When-to-Create-a-Federation-Server.md)。  
  
> [!NOTE]  
> 如果选择使用 AD FS 联合服务器配置向导创建**新的联合服务器场**的选项，则向导将尝试在 Active Directory 中创建一个容器对象 @no__t 1for 共享证书 @ no__t。 因此，第一次登录到将要设置联合服务器角色的计算机时，务必使用在 Active Directory 中有创建此容器对象的足够权限的帐户。  
  
在联合服务器可以分组为场之前，必须先对其进行群集化，以便将到达单个完全限定域名 @no__t 的请求路由到服务器场中的各个联合服务器。 可以通过在公司网络内部部署网络负载平衡 \(NLB @ no__t 来创建服务器群集。 本指南假定已将 NLB 正确配置为在场中的每台联合服务器群集。  
  
有关如何使用 Microsoft NLB 技术配置群集 FQDN 的详细信息，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkID=74651)。  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>部署联合服务器场的最佳做法  
建议在生产环境中部署联合服务器时采用以下最佳做法：  
  
-   如果要同时部署多台联合服务器，或者知道将随着时间的推移向场中添加更多服务器，请考虑在服务器场中创建现有联合服务器的服务器映像，然后在需要 cr 时从该映像进行安装快速 e) 其他联合服务器。  
  
    > [!NOTE]  
    > 如果决定使用服务器映像方法部署其他联合服务器，则无需完成 [Checklist 中的任务：每次将新服务器添加到场时，都要设置联合服务器 @ no__t-0。  
  
-   使用 NLB 或其他形式的群集为多台联合服务器计算机分配单个 IP 地址。  
  
-   为场中的每个联合服务器保留一个静态 IP 地址，并根据域名系统 \(DNS @ no__t 配置，在动态主机配置协议中为每个 IP 地址插入一个排除项，\(DHCP @ no__t。 Microsoft NLB 技术要求为加入 NLB 群集的每台服务器分配一个静态 IP 地址。  
  
-   如果 AD FS 配置数据库将存储在 SQL 数据库中，应避免同时从多个联合服务器编辑 SQL 数据库。  
  
## <a name="configuring-federation-servers-for-a-farm"></a>为场配置联合服务器  
下表描述了必须完成的任务，以便每个联合服务器可以参与到场环境中。  
  
|任务|描述|  
|--------|---------------|  
|如果要使用 SQL Server 存储 AD FS 配置数据库|联合服务器场由两台或更多联合服务器组成，它们共享相同的 AD FS 配置数据库和 no__t 0signing 证书。 配置数据库可以存储在 Windows 内部数据库或 SQL Server 数据库中。 如果计划将配置数据库存储在 SQL 数据库中，请确保配置数据库可访问，以便加入场的所有新联合服务器都可以访问该数据库。 **注意：** 对于场方案，将配置数据库放置在未作为服务器场中的联合服务器的计算机上，这一点非常重要。 Microsoft NLB 不允许加入场的任何计算机相互通信。 **注意：** 请确保加入场的每个联合服务器上的 Internet Information Services 中的 AD FS AppPool 的标识 \(IIS @ no__t-1 @ no__t 都具有对配置数据库的读取访问权限。|  
|获取和共享证书|可以从公共证书颁发机构获取单一服务器身份验证证书，\(CA @ no__t-例如 VeriSign。 然后，你可以配置证书，以便所有联合服务器共享证书的相同私钥部分。 有关如何共享相同证书的详细信息，请参阅 [Checklist：设置联合服务器 @ no__t。 **注意：** AD FS 管理 snap @ no__t-0in 是指作为服务通信证书的联合服务器的服务器身份验证证书。<br /><br />有关详细信息，请参阅 [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md)。|  
|指向相同的 SQL Server 实例|如果 AD FS 配置数据库将存储在 SQL 数据库中，则新的联合服务器必须指向场中其他联合服务器使用的同一 SQL Server 实例，以便新服务器可以加入到场中。|  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
