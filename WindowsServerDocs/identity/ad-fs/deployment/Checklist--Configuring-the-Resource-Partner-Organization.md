---
ms.assetid: 80d50a9f-428e-40fe-b6b3-9837fd9a3efc
title: 清单-配置资源合作伙伴公司
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4c73d84a6a32a90f5a5a60f0a0f6e6405ea3b729
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="checklist-configuring-the-resource-partner-organization"></a>清单： 配置资源合作伙伴公司

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

资源合作伙伴公司包含承载将被用户帐户伙伴中访问 Web\ 基于应用程序的 Web 服务器。 在此组织的管理员必须使用广告 FS 管理 snap\ 中创建索赔提供商信任代表与帐户合作伙伴组织它们信任的关系。 反过来，帐户合作伙伴管理员必须创建信赖的方信任为他们想要信任每个帐户合作伙伴组织。  
  
本文包含所必需的部署 Active Directory 联合身份验证服务 \(AD FS\) 资源合作伙伴组织中的任务。 它还包括配置需要建立 one\ 半联盟合作组件任务。  
  
如果你正在部署[Web SSO 设计](https://technet.microsoft.com/library/dd807033.aspx)，不需要按照该清单。 但是，你必须完成此检查成功部署表中的任务[联盟 Web SSO 设计](https://technet.microsoft.com/library/dd807050.aspx)。  
  
> [!IMPORTANT]  
> 请确保帐户的合作伙伴公司的管理员遵循的指南中[清单：配置的帐户的合作伙伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)以确保所有必要的部署的任务将完成若要一半联盟合作成功创建第二个  
  
> [!NOTE]  
> 完成此清单订单中的任务。 当参考链接将你带到过程时，返回到本主题之后你完成该过程中的步骤，以便你可以继续进行此清单中的其余任务。  
  
![配置资源合作伙伴组织](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：配置资源合作伙伴公司**  
  
||任务|参考|  
|-|--------|-------------|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|今天有生产环境中的现有广告 FS 1.0 或 1.1 部署，如果看到向右有关如何将设置从你的当前联合身份验证服务迁移到新的广告 FS 联合身份验证服务的信息的链接。 如果你首次在你的组织部署广告 FS 使用广告 FS，你可以跳过此步骤，然后继续此清单，以便了解如何进行设置新的资源合作伙伴公司中的下一个任务。|![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[计划迁移至广告 FS](https://technet.microsoft.com/library/ff678044.aspx)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|根据你的部署目标，查看关于组件联盟应用程序访问为用户提供所需的信息。|![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[你 Active Directory 用户提供访问您的索赔识别应用程序和服务](https://technet.microsoft.com/library/dd807071.aspx)<br /><br />![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[向应用程序和服务的其他公司提供您 Active Directory 用户的访问权限](https://technet.microsoft.com/library/dd807123.aspx)<br /><br />![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[提供另一台组织访问你的索赔识别应用程序和服务中的用户](https://technet.microsoft.com/library/dd807099.aspx)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|确定哪些广告 FS 设计此资源的合作伙伴公司应与相关联。|![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SSO 设计的 Web](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联盟 Web SSO 设计](https://technet.microsoft.com/library/dd807050.aspx)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|查看不同的应用程序类型，并确定哪个应用程序部署。|![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定你联盟应用程序的策略资源合作伙伴](https://technet.microsoft.com/library/dd807077.aspx)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|部署广告 FS 服务器之前，请查看;1。\) 的 Windows 内部数据库 \(WID\) 或 SQL Server 存储广告 FS 配置选择优缺点数据库 2。\) 广告 FS 部署拓扑类型和关联的服务器放置和网络布局建议。|![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定您的广告 FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[广告 FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|查看广告 FS 容量规划指南以确定正确数联合身份验证的服务器和 server 联合身份验证的代理服务器应生产环境中使用。|![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划广告 FS 服务器容量](https://technet.microsoft.com/library/gg749899.aspx)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|实际上规划和实现帐户合作伙伴部署的物理拓扑，确定广告 FS 设计是否需要一个或多个联合身份验证的服务器或联合身份验证的服务器代理。|![配置资源合作伙伴组织](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：设置联盟服务器](Checklist--Setting-Up-a-Federation-Server.md)<br /><br />![配置资源合作伙伴组织](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：向上联合服务器代理的设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|确定你想要添加到广告 FS 的特性官方商城的类型。 然后，添加使用 snap\ 中广告 FS 管理特性应用商店。|![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[角色的特性官方商城](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<br /><br />![配置资源合作伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加特性官方商城](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|如果你将需要发送索赔，或使用从帐户合作伙伴已使用者的声明广告 FS 1.0 或 1.1 联合身份验证服务，请参阅与以前版本的广告 FS 互操作以了解如何配置广告 FS 右侧的链接。 如果帐户合作伙伴公司也使用广告 FS 发送或消耗到你的组织的索赔，你可以跳过此步骤，并继续在本文中的下一个任务。|![配置资源合作伙伴组织](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划与广告 FS 互操作性 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|部署资源合作伙伴组织中的第一个联盟服务器后，请使用广告 FS 管理 snap\ 中创建索赔提供商信任关系。 通过输入关于手动帐户合作伙伴的数据或使用向你提供管理员帐户合作伙伴公司的联合元数据 URL，则可以创建索赔提供商信任。 你可以使用联盟元数据自动检索资源合作伙伴的数据。 **注意：**如果帐户合作伙伴发布了其联盟元数据或可以提供它为你使用的文件副本，我们建议因为它可以节省时间自动检索数据。|![配置资源合作伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[依赖方信任手动创建](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually)<br /><br />![配置资源合作伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建依赖方信任使用联盟元数据](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|具体取决于你的组织的需求，创建一个或多个声明规则集指定 snap\ 中广告 FS 管理，因此将经过传入的索赔，每个索赔提供商信任转换，或在资源合作伙伴相应的索赔均相应地映射。|![配置资源合作伙伴组织](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：创建的索赔提供商信任的索赔规则](Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)|  
|![配置资源合作伙伴组织](media/icon_checkboxo.gif)|\(Optional\) 索赔描述可能需要创建一个尚不存在，如果将履行你的组织的需求。 广告 FS 包含公开的索赔说明的一组默认 snap\ 中广告 FS 管理。|![配置资源合作伙伴组织](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加索赔说明](../../ad-fs/operations/Add-a-Claim-Description.md)|  
