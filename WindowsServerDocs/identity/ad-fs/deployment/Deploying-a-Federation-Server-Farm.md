---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: "Windows Server 2012R2 广告 FS 部署指导"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f1ea6830237813e6fd2bd6a172f467e8d81065
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-a-federation-server-farm"></a>部署联盟服务器电场的日落

>适用于：Windows Server 2016，Windows Server 2012 R2

部署联盟服务器电场的日落，为了完成此清单订单中的任务。 当参考链接将你转到概念主题，您将检查概念主题，以便你可以继续进行此清单中的其余任务后，返回到该清单。  
  
![部署联盟的服务器场](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：部署联盟服务器电场的日落**  
  
||任务|参考|  
|-|--------|-------------|  
|![部署联盟的服务器电场的日落](media/icon_checkboxo.gif)|当你准备部署 Active Directory 联合身份验证服务 \(AD FS\) 查看重要的概念和事项。 **注意：**|![部署联盟的服务器场](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[广告 FS 设计在 Windows Server 2012 R2 的新指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![部署联盟的服务器场](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[了解关键广告 FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||如果你决定使用你的广告 FS 配置官方商城 Microsoft SQL Server，确保部署正常工作的 SQL Server 实例。|[SQL Server](https://technet.microsoft.com/sqlserver)**警告：**在 Windows Server 2012 R2，如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储您的配置数据，您可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012。|  
|![部署联盟的服务器电场的日落](media/icon_checkboxo.gif)|到 Active Directory 域加入你的计算机。|![部署联盟的服务器场](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[计算机到某个域加入](Join-a-Computer-to-a-Domain.md)|  
|![部署联盟的服务器电场的日落](media/icon_checkboxo.gif)|注册广告 FS 安全套接字层 \(SSL\) 证书。|![部署联盟的服务器场](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[注册广告 FS SSL 证书](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![部署联盟的服务器电场的日落](media/icon_checkboxo.gif)|安装广告 FS 角色服务。|![部署联盟的服务器场](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[安装广告 FS 角色服务](Install-the-AD-FS-Role-Service.md)|  
|![部署联盟的服务器电场的日落](media/icon_checkboxo.gif)|配置的联合身份验证的服务器。|![部署联盟的服务器场](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[配置联盟服务器](Configure-a-Federation-Server.md)|  
|![部署联盟的服务器电场的日落](media/icon_checkboxo.gif)|可选步：与设备注册服务 \(DRS\) 配置的联合身份验证的服务器。|![部署联盟的服务器场](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[配置联合服务器注册设备的服务](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![部署联盟的服务器电场的日落](media/icon_checkboxo.gif)|添加到联合身份验证服务的公司域名系统 \(DNS\) 和 DRS 主机 \(A\) 和别名 \(CNAME\) 资源记录。|![部署联盟的服务器场](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合身份验证服务和 DRS 配置公司 DNS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![部署联盟的服务器电场的日落](media/icon_checkboxo.gif)|验证联盟服务器正常。|![部署联盟的服务器场](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[验证联盟服务器是操作](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>请参阅  
[广告 FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012R2 广告 FS 部署指导](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

