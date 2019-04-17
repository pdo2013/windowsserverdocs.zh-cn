---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: "清单-配置广告 FS 消耗广告 FS 来自声明 1.x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>清单：配置广告 FS 消耗广告 FS 来自声明 1.x

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-ad-fs-1x"></a>清单：配置广告 FS 消耗广告 FS 来自声明 1.x  
本文包含所需的在 Windows Server 2012 消耗由广告 FS 1 发送的索赔配置你的 Active Directory 联合身份验证服务 \(AD FS\) 联合身份验证服务的任务。*x*联合身份验证服务。  
  
> [!NOTE]  
> 完成此清单订单中的任务。 当参考链接将你带到过程时，返回到本主题之后你完成该过程中的步骤，以便你可以继续进行此清单中的其余任务。  
  
![消耗广告 FS 来自声明](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：配置广告 FS 消耗广告 FS 来自声明 1.x**  
  
||任务|参考|  
|-|--------|-------------|  
|![使用来自广告 FS 声明](media/icon_checkboxo.gif)|在 Windows Server 2012 的广告金融服务和以前版本的广告 FS 之间互操作性计划，并了解更多有关名称 ID 声明类型。|![消耗广告 FS 来自声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划与广告 FS 互操作性 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![使用来自广告 FS 声明](media/icon_checkboxo.gif)|你可以与以前版本的广告 FS 互操作之前，你必须先创建索赔的提供商信任广告 FS 联合身份验证服务。 **注意：**无法与广告 FS 1 中创建信任。*x*联合身份验证服务使用联盟元数据。<br /><br />当您设置信任使用向右链接中的过程时，你必须执行以下操作添加索赔提供商信任向导中设置此信任与广告 FS 1 互操作。*x*联合身份验证服务：<br /><br />1.打开**选择数据源**页上，选择**有关依赖 Enter 数据方信任手动**。<br />2.打开**选择配置文件**页上，选择**广告 FS 1.0 和 1.1 个人资料**。<br />3.打开**配置 URL**页面上下, **WS\ 联盟被动 URL**，类型**联合身份验证服务端点 URL**定义广告 FS 1 中。*x*合作伙伴的联合身份验证服务。<br />4.打开**配置标识符**页面上下,**索赔提供商信任标识符**，类型**联合身份验证服务 URI**定义广告 FS 1 中。*x*合作伙伴的联合身份验证服务。|![消耗广告 FS 来自声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[索赔提供商信任手动创建](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![使用来自广告 FS 声明](media/icon_checkboxo.gif)|在你之前创建的索赔提供商信任，你必须创建一个索赔规则，需要从广告 FS 传入的索赔 1.x 联合身份验证服务和直通、筛选器，或将其转换为名称 ID 声明类型。<br /><br />当名称 ID 声明类型传递，通过筛选，或转换时，它可以用作输入到另一台规则或规则，以便可以理解并由 Windows Server 2012 中广告 FS 联合身份验证服务。|![消耗广告 FS 来自声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[创建发送广告 FS 规则 1.x 兼容的声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![使用来自广告 FS 声明](media/icon_checkboxo.gif)|广告 FS 1 管理员联系。*x*联合身份验证服务和广告 FS 1 与系统管理员。*x*联合身份验证服务设置新的资源合作伙伴信任。 此外，使用联合身份验证服务 URI 提供该管理员 \（在联合身份验证服务 properties\) 联合身份验证服务端点 URL，然后导出 token\-签名证书文件 \（使用公用关键 only\)。 管理员将需要设置信任这些商品。|N\/A|  
  

