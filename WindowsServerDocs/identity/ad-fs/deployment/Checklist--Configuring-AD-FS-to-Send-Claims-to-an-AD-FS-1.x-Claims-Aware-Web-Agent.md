---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: "部署联盟服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32675aa7fb8c7b928bcf80a4d1072fe5eab7cd61
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>清单：配置广告 FS 发送到广告 FS 1.x 声明感知 Web 代理索赔

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>清单：配置广告 FS 发送到广告 FS 1.x claims\ 意识到 Web 代理索赔  
本文包含所需的在 Windows Server 2012 发送可以运行广告 FS 1 的 Web 服务器托管的应用程序中理解的索赔配置你的 Active Directory 联合身份验证服务 \(AD FS\) 联合身份验证服务的任务。*x* claims\ 意识到 Web 代理。  
  
> [!NOTE]  
> 完成此清单订单中的任务。 当参考链接将你带到过程时，返回到本主题之后你完成该过程中的步骤，以便你可以继续进行此清单中的其余任务。  
  
![配置广告 FS 发送索赔](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：配置广告 FS 发送到广告 FS 1.x claims\ 意识到 Web 代理索赔**  
  
||任务|参考|  
|-|--------|-------------|  
|![配置广告 FS 发送声明](media/icon_checkboxo.gif)|在 Windows Server 2012 的广告金融服务和以前版本的广告 FS 之间互操作性计划，并了解更多有关名称 ID 声明类型。|![配置广告 FS 发送索赔](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划与广告 FS 互操作性 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![配置广告 FS 发送声明](media/icon_checkboxo.gif)|如果你未使用该链接右侧创建信赖方信任之间广告 FS 联合身份验证服务，在 Windows Server 2012 和广告 FS 1 的第一次。*x*联合身份验证服务。|[清单：配置广告 FS 发送到广告 FS 1.x 联合身份验证服务索赔](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![配置广告 FS 发送声明](media/icon_checkboxo.gif)|之前，就可以获得与由广告 FS 1 托管的应用程序的互操作。*x* claims\ 意识到 Web 代理，必须首先信赖的方信任广告 FS 联合身份验证服务中创建广告 FS 1 Windows Server 2012。 *x* claims\ 意识到 Web 代理。 **注意：**创建广告 FS 联合身份验证服务中的此信任相当于添加了新**应用程序**到广告 FS 1.x 联合身份验证服务 \ (**联盟 Service\\Trust Policy\\My Organization\\Application**\)。 必须此信赖的方信任，因为广告 FS 没有等效**应用程序**中自行 snap\ 节点。 但是，它仍必须具有安全通道给应用程序。<br /><br />当您设置信任使用向右链接中的过程时，你必须执行以下操作添加依赖方信任向导中设置此信任与广告 FS 1 互操作。*x* claims\ 意识到 Web 代理：<br /><br />1.打开**选择数据源**页上，选择**有关依赖 Enter 数据方信任手动**。<br />2.打开**选择配置文件**页上，选择**广告 FS 1.0 和 1.1 个人资料**。<br />3.打开**配置 URL**页面上下, **WS\ 联盟被动 URL**，类型**应用程序 URL**定义广告 FS 1 中。*x*合作伙伴的联合身份验证服务。<br />4.打开**配置标识符**页面上下,**信赖部分信任标识符**，类型**应用程序 URL**定义广告 FS 1 中。*x* claims\ 意识到 Web 代理|![配置广告 FS 发送索赔](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[依赖方信任手动创建](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![配置广告 FS 发送声明](media/icon_checkboxo.gif)|运行广告 FS 1 的 Web 服务器管理员联系。*x* claims\ 意识到 Web 代理和已编辑与 claims\ 识别的应用程序关联的 web.config 文件该管理员 \（下默认网站中 Internet 信息服务 \(IIS\)\) 来 Web 代理对准广告 FS 联合身份验证服务。<br /><br />例如，替换*myresourcefederationserver*标记中`<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>`具有有效的广告 FS 联盟服务器名称 web.config 文件。<br /><br />这是必需的应用程序和能够使用在 Windows Server 2012 广告 FS 联合身份验证服务从发送给它声明广告 FS 1.x claims\ 意识到 Web 代理。|N\/A|  
|![配置广告 FS 发送声明](media/icon_checkboxo.gif)|在你之前创建了信赖方信任，您需要创建索赔将采取传入特性官方商城已解压缩的索赔和通过、筛选，或将其插入名称 ID 转换规则声称类型可一目了然和使用该广告 FS 1。*x* claims\ 意识到 Web 代理。 **注意：**创建该规则之前，请确保创建此规则索赔规则集具有之前它先从属性应用商店中提取轻型目录访问协议 \(LDAP\) 特性索赔规则。 此声明将为你创建发送广告 FS 1 规则输入使用。*x*\-compatible 索赔。 有关如何创建规则解压缩 LDAP 特性的详细信息，请参阅[创建规则应用于作为索赔发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![配置广告 FS 发送索赔](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[创建发送广告 FS 规则 1.x 兼容的声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

