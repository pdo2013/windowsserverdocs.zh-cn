---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: "何时创建联盟服务器电场的日落"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-farm"></a>何时创建联盟服务器电场的日落

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

请考虑在 Active Directory 联合身份验证服务 \(AD FS\) 创建联盟服务器电场的日落，当您的较大的广告 FS 部署，并且你想要提供故障、load\ 平衡或可扩展性到你的组织的联合身份验证服务。 在同一网络创建两个或多个联合身份验证的服务器的操作，配置每个使用相同的联合身份验证服务，并将公钥每个服务器 token\ 签名证书的添加到广告 FS 管理 snap\ 中创建联合身份验证的服务器场。  
  
你可以创建联盟服务器电场的日落，或使用广告 FS 联盟服务器配置向导到现有场安装其他联合身份验证的服务器。 有关详细信息，请参阅[何时创建联盟服务器](When-to-Create-a-Federation-Server.md)。  
  
> [!NOTE]  
> 当你选择的选项来创建**新联合身份验证的服务器场**使用广告 FS 联盟服务器配置向导，该向导将尝试自行创建容器对象 \(for sharing certificates\) Active Directory 中。 因此，很重要，你首次登录到计算机上，有设置联盟服务器角色，使用帐户，以创建该容器对象 Active Directory 中有足够的权限。  
  
联合身份验证的服务器可以组合为一场之前，他们必须首先要群集以便完全到达单个的请求合格的 \(FQDN\) 送交服务器电场的日落中的各种联盟服务器的域名。 你可以通过部署在公司的网络中的网络负载平衡 \(NLB\) 创建服务器群集。 本指南假定确认 NLB 已配置相应地群集每一场中联合身份验证的服务器。  
  
有关如何配置群集 FQDN 使用 Microsoft NLB 技术，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkID=74651)。  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>部署联合身份验证的服务器农场里的最佳做法  
我们建议下面的最佳实践部署联盟服务器生产环境中：  
  
-   如果你将在同一时间部署多个联合身份验证的服务器，或者你知道，你将会添加更多服务器到场随着时间的推移，请考虑场中创建的现有联合身份验证的服务器服务器图像，然后在你需要快速创建其他联合身份验证的服务器时安装该映像。  
  
    > [!NOTE]  
    > 如果你决定用于部署其他联合身份验证的服务器服务器图像方法，你无需完成的任务[清单：设置联合服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)每当你想要向场添加新的服务器。  
  
-   使用 NLB 或某些其他形式的群集为许多联合身份验证的服务器计算机分配一个 IP 地址。  
  
-   保留场中每个联盟服务器的静态 IP 地址，并根据你的域名系统 \(DNS\) 配置中动态主机配置协议 \(DHCP\) 插入排除项每个 IP 地址。 MicrosoftNLB 技术要求参与 NLB 群集每个服务器即可分配的静态 IP 地址。  
  
-   如果广告 FS 配置数据库将存储在数据库中，可以避免同时编辑 SQL 数据库从多个联合身份验证的服务器。  
  
## <a name="configuring-federation-servers-for-a-farm"></a>配置场联盟服务器  
下表介绍了，以便每个联合身份验证的服务器可以参与场环境，必须先完成的任务。  
  
|任务|描述|  
|--------|---------------|  
|如果你使用 SQL Server 存储广告 FS 配置数据库|包含两个或多个共享相同的广告 FS 配置的联合服务器联合身份验证的服务器场数据库和 token\ 签名证书。 在这两个 Windows 内部数据库或 SQL Server 数据库中，可以存储配置数据库。 如果你计划存储配置数据库 SQL 数据库中，请确保配置数据库是访问，以便它可以访问所有参与农场里的新联盟服务器。 **注意：**场方案，是非常重要的配置数据库会位于不还参与作为联盟服务器该电场的日落中的计算机。 MicrosoftNLB 不允许任何参与场以与另一个通信的计算机。 **注意：**确保在参与场每联盟服务器上的 Internet 信息服务 \(IIS\)\) 广告 FS 应用程序池身份具有阅读访问配置数据库。|  
|获取和共享证书|你可以获取单个服务器身份验证证书公共证书颁发机构从 \ (CA\)-例如，VeriSign。 然后可以配置证书，以便所有联合身份验证的服务器共享相同专用证书的主要部分。 有关如何共享同一个证书的详细信息，请参阅[清单：设置联合服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。 **注意：**广告 FS 管理 snap\ 中指联盟服务器服务通信证书为服务器身份验证证书。<br /><br />有关详细信息，请参阅[服务器联合身份验证的证书要求](Certificate-Requirements-for-Federation-Servers.md)。|  
|指向相同的 SQL Server 实例|如果广告 FS 配置数据库将存储在数据库中，新的联合服务器必须指向场中其他联合身份验证的服务器，以便新服务器可以参与电场的日落使用相同的 SQL Server 实例。|  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
