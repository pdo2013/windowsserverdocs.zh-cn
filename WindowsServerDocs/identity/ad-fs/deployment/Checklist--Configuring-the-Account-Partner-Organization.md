---
ms.assetid: 826974ea-3635-40df-aa37-77dd12a363c8
title: 清单-配置帐户伙伴组织
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 73ccbfc5194b36d8612daa7175561539d7a69dff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360015"
---
# <a name="checklist-configuring-the-account-partner-organization"></a>清单：配置帐户伙伴组织

帐户伙伴组织包含将访问 Web 的用户\-基于资源伙伴中的应用程序。 此组织中的管理员必须使用 AD FS 管理管理单元\-才能创建信赖方信任来表示它们资源伙伴组织之间的信任关系。 反过来，资源伙伴管理员必须创建每个帐户伙伴组织，他们想要信任的声明提供方信任。  
  
此清单包含用于在帐户伙伴组织中部署 Active Directory 联合身份验证服务 \(AD FS @ no__t 的任务。 它还包括用于配置的组件所需建立一个多任务\-一半联合合作关系。  
  
如果正在部署 [Web SSO 设计](https://technet.microsoft.com/library/dd807033.aspx), ，无需按照此清单。 但是，您是否需要完成此清单，若要成功部署中的任务 [联合 Web SSO 设计](https://technet.microsoft.com/library/dd807050.aspx)。  
  
> [!IMPORTANT]  
> 请确保资源伙伴组织中的管理员遵循 @no__t 0Checklist：配置资源伙伴组织 @ no__t，以确保所有必需的部署任务都已完成，以成功创建第二一半的联合合作关系。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
@no__t 0configure 帐户伙伴 org @ no__t-1Checklist：配置帐户伙伴组织 @ no__t-0  
  
||任务|参考|  
|-|--------|-------------|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|如果你当前在生产环境中使用现有的 AD FS 1.0 或1.1 部署，请参阅右侧的链接，了解有关如何将设置从当前联合身份验证服务迁移到新 AD FS 联合身份验证服务的信息。 如果你在组织中首次使用 AD FS 部署 AD FS，则可以跳过此步骤，并继续执行此清单中的下一项任务，以了解有关如何设置新的帐户伙伴组织的信息。|@no__t 0configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[计划迁移到 AD FS 2.0](https://technet.microsoft.com/library/ff678044.aspx)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|根据您的部署目标，查看有关用户提供对联合应用程序的访问权限所需的组件的信息。|@no__t 0configure 帐户伙伴组织为你的](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Active Directory 用户提供对声明感知应用程序和服务的访问权限](https://technet.microsoft.com/library/dd807071.aspx)<br /><br />@no__t 0configure 帐户伙伴组织向](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Active Directory 用户提供对其他组织的应用程序和服务的访问权限](https://technet.microsoft.com/library/dd807123.aspx)<br /><br />@no__t 0configure 帐户伙伴组织向](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[其他组织的用户提供对声明感知应用程序和服务的访问权限](https://technet.microsoft.com/library/dd807099.aspx)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|确定哪些 AD FS 设计此帐户伙伴组织将与相关联。|@no__t 0configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[WEB SSO 设计](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />@no__t 0configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合 WEB SSO 设计](https://technet.microsoft.com/library/dd807050.aspx)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|在开始部署 AD FS 服务器之前，请查看;1. @no__t 0 个优点和缺点，可选择 "Windows 内部数据库 \(WID @ no__t" 或 "SQL Server" 来存储 AD FS 配置数据库 2. @no__tAD FS 部署拓扑类型和关联的服务器位置和网络布局建议。|@no__t 0configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定 AD FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />@no__t 0configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|查看 AD FS 容量规划指南以确定联合身份验证服务器和联合服务器代理服务器应在生产环境中使用的正确号码。|![configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 服务器容量规划](https://technet.microsoft.com/library/gg749899.aspx)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|为了有效地规划和实施帐户合作伙伴部署的物理拓扑，确定你的 AD FS 设计需要一个或多个联合服务器或联合服务器代理。|@no__t 0configure 帐户伙伴 org @ no__t-1Checklist：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)<br /><br />@no__t 0configure 帐户伙伴 org @ no__t-1Checklist：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|确定想要添加到 AD FS 的属性存储的类型。 然后，将属性存储区中使用 AD FS 管理管理单元添加\-中。|@no__t 0configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[属性存储的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<br /><br />@no__t 0configure 帐户伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加属性存储](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|如果需要向使用 AD FS 1.0 或1.1 联合身份验证服务的资源伙伴发送声明或使用声明，请参阅右侧的链接，了解有关如何配置 AD FS 以与以前版本的 AD FS 进行互操作的信息。 如果资源伙伴组织也使用 AD FS 以发送或使用您的组织声明，您可以跳过此步骤，然后继续进行此检查表的下一任务。|![configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划与 AD FS 1.x 的互操作性](https://technet.microsoft.com/library/ff678040.aspx)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|部署帐户伙伴组织中的第一个联合服务器后，创建使用 AD FS 管理管理单元的信赖方信任关系\-中。 可以通过手动输入有关资源伙伴的数据，或者使用资源伙伴组织管理员提供给你的联合元数据 URL，来创建信赖方信任。 可以使用联合元数据来自动检索资源伙伴的数据。 **注意：** 如果资源伙伴发布了其联合元数据，或者可以提供此元数据的文件副本让你使用，那么，我们建议你自动检索数据，因为这可以节省时间。|@no__t 0configure 帐户伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手动创建信赖方信任](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)<br /><br />@no__t 0configure 帐户伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[使用联合元数据创建信赖方信任](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|根据您的组织的需要，创建一个或多个声明规则设置为指定每个信赖方信任 AD FS 管理管理单元中\-中，以便将相应地颁发的声明。|@no__t 0configure 帐户伙伴 org @ no__t-1Checklist：为信赖方信任创建声明规则](Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|如果尚不存在，则必须创建索赔说明，将满足您的组织的需求。 AD FS 附带了一组默认的公开的声明说明在 AD FS 管理管理单元中\-中。|@no__t 0configure 帐户伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加声明说明](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|确定您的组织将需要使用标识委托来授权或约束指定的帐户"作为"或模拟其他用户。 这通常是一项要求时前\-结束的 Web 应用程序必须与后交互\-结束的 Web 服务。|![configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用身份委派](https://technet.microsoft.com/library/dd807122.aspx)|  
|![配置帐户伙伴组织](media/icon_checkboxo.gif)|准备联合身份验证的客户端计算机︰<br /><br />-到客户端浏览器的受信任的站点列表中添加帐户伙伴联合身份验证服务器的 URL。<br />-使用组策略将相应的安全套接字层 \(SSL @ no__t 证书推送到客户端计算机。|![configure 帐户伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在帐户伙伴中准备客户端计算机](https://technet.microsoft.com/library/dd807114(v=ws.11).aspx)<br /><br />@no__t 0configure 帐户伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将客户端计算机配置为信任帐户联合服务器](Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)<br /><br />@no__t 0configure 帐户伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[使用组策略将证书分发到客户端计算机](Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)| 
