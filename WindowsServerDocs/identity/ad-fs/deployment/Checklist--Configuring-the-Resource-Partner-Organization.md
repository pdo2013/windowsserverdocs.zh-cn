---
ms.assetid: 80d50a9f-428e-40fe-b6b3-9837fd9a3efc
title: 清单-配置资源伙伴组织
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 41902de1253654eb3eac6176d6e8b7c50dc39663
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192399"
---
# <a name="checklist-configuring-the-resource-partner-organization"></a>清单：配置资源伙伴组织

资源伙伴组织包含承载网站的 Web 服务器\-基于应用程序将帐户伙伴中用户的访问。 此组织中的管理员必须使用 AD FS 管理管理单元\-才能创建声明提供方信任来表示它们帐户伙伴组织之间的信任关系。 反过来，帐户伙伴管理员必须创建每个帐户伙伴组织，他们想要信任的信赖方信任。  
  
此清单包括部署 Active Directory 联合身份验证服务必需的任务\(AD FS\)资源伙伴组织中。 它还包括用于配置的组件所需建立一个多任务\-一半联合合作关系。  
  
如果正在部署 [Web SSO 设计](https://technet.microsoft.com/library/dd807033.aspx), ，无需按照此清单。 但是，您是否需要完成此清单，若要成功部署中的任务 [联合 Web SSO 设计](https://technet.microsoft.com/library/dd807050.aspx)。  
  
> [!IMPORTANT]  
> 请确保帐户伙伴组织的管理员，如下所示中的指导[核对清单：配置帐户伙伴组织](Checklist--Configuring-the-Account-Partner-Organization.md)以确保所有必需的部署任务将会完成若要成功创建第二个半个联合合作关系  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![配置资源伙伴组织](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**核对清单：配置资源伙伴组织**  
  
||任务|参考|  
|-|--------|-------------|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|如果立即在生产环境中有现有的 AD FS 1.0 或 1.1 部署，请参阅有关如何将设置从当前的联合身份验证服务迁移到新的 AD FS 联合身份验证服务的信息权限的链接。 如果要在组织中首次部署 AD FS 使用 AD FS 中，你可以跳过此步骤，并继续此清单中有关如何设置新的资源伙伴组织信息的下一个任务。|![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划迁移到 AD FS](https://technet.microsoft.com/library/ff678044.aspx)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|根据您的部署目标，查看有关用户提供对联合应用程序的访问权限所需的组件的信息。|![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[提供您 Active Directory 用户访问您的声明感知应用程序和服务](https://technet.microsoft.com/library/dd807071.aspx)<br /><br />![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[提供您 Active Directory 用户访问的应用程序和服务的其他组织](https://technet.microsoft.com/library/dd807123.aspx)<br /><br />![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[您声明感知应用程序和服务的另一个组织 Access 中提供的用户](https://technet.microsoft.com/library/dd807099.aspx)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|确定哪些 AD FS 设计此资源伙伴组织将与相关联。|![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Web SSO 设计](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合 Web SSO 设计](https://technet.microsoft.com/library/dd807050.aspx)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|查看不同的应用程序类型，并确定要部署的应用程序。|![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定在联合应用程序策略在资源伙伴](https://technet.microsoft.com/library/dd807077.aspx)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|在开始部署 AD FS 服务器之前，请查看;1.\)选择这两个 Windows 内部数据库的优点和缺点\(WID\)或 SQL Server 存储 AD FS 配置数据库 2。\)AD FS 部署拓扑类型和关联的服务器位置和网络布局建议。|![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定 AD FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|查看 AD FS 容量规划指南以确定联合身份验证服务器和联合服务器代理服务器应在生产环境中使用的正确号码。|![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 服务器容量规划](https://technet.microsoft.com/library/gg749899.aspx)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|为了有效地规划和实施帐户合作伙伴部署的物理拓扑，确定你的 AD FS 设计需要一个或多个联合服务器或联合服务器代理。|![配置资源伙伴组织](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)<br /><br />![配置资源伙伴组织](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|确定想要添加到 AD FS 的属性存储的类型。 然后，将属性存储区中使用 AD FS 管理管理单元添加\-中。|![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[角色的属性存储](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<br /><br />![配置资源伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加属性存储](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|如果你将需要发送声明或使用 AD FS 1.0 或 1.1 联合身份验证服务的用户正在使用其中一个帐户伙伴中的声明，看到的有关如何配置 AD FS 的信息的权限与以前版本的 AD FS 进行互操作的链接。 如果帐户伙伴组织也使用 AD FS 以发送或使用您的组织声明，您可以跳过此步骤，然后继续进行此检查表的下一任务。|![配置资源伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划互操作性与 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|部署资源伙伴组织中的第一个联合服务器后，通过使用 AD FS 管理管理单元创建声明提供程序信任关系\-中。 输入有关帐户伙伴手动数据，或者通过使用帐户伙伴组织的管理员可为您提供的联合身份验证元数据 URL，可以创建声明提供方信任。 可以使用联合元数据来自动检索资源伙伴的数据。 **注意：** 如果帐户伙伴发布其联合元数据，或者可以提供它为您要使用的文件副本，我们建议可以节省时间，因为自动检索数据。|![配置资源伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[信赖方信任手动创建](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually)<br /><br />![配置资源伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建信赖方信任使用联合身份验证元数据](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|根据您的组织的需要，创建一个或多个声明规则设置每个声明提供方信任指定 AD FS 管理管理单元中\-中，以便将通过传递传入声明转换后，或映射到适当地对应中声明资源伙伴。|![配置资源伙伴组织](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：为声明提供方信任创建声明规则](Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)|  
|![配置资源伙伴组织](media/icon_checkboxo.gif)|\(可选\)一个描述可能具有一个尚不存在，如果要创建的声明将满足你的组织的需求。 AD FS 包含一组默认的公开的声明说明在 AD FS 管理管理单元中\-中。|![配置资源伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加声明说明](../../ad-fs/operations/Add-a-Claim-Description.md)|  
