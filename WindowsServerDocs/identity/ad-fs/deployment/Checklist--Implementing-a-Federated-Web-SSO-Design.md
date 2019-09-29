---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 清单-实现联合 Web SSO 设计
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 165471fd06031a68343a54d019357afee782d082
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359948"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>清单：实现联合 Web SSO 设计

此父清单包含跨 @ no__t-0reference 链接，这些链接指向有关联合 Web Single @ no__t-1Sign @ no__t-@no__t 2On 的重要概念，这些概念适用于 Active Directory 联合身份验证服务 \(SSO FS @ no__t。 它还包含指向从属清单的链接，该清单将帮助你完成实现此设计所需的任务。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当引用链接将你带到概念主题或从属清单时，在查看概念主题或完成从属清单中的任务后，请返回到本主题，这样你就可以继续完成此清单中的剩余任务。  
  
@no__t 0federated web sso @ no__t-1Checklist：实现联合 Web SSO 设计**  
  
||任务|参考|  
|-|--------|-------------|  
|![联合的 web sso](media/icon_checkboxo.gif)|查看有关联合 Web SSO 设计的重要概念并确定可用于自定义此设计以满足您的组织的需求的 AD FS 部署目标。|@no__t 0federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合 WEB Sso 设计](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[标识 AD FS 部署目标](https://technet.microsoft.com/library/dd807053.aspx)的 0federated web sso @no__t<br /><br />@no__t 0federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划你的部署](https://technet.microsoft.com/library/dd807083.aspx)|  
|![联合的 web sso](media/icon_checkboxo.gif)|查看硬件、 软件、 证书、 域名系统 \(DNS\), ，属性存储和您的组织中部署 AD FS 的客户端要求。|@no__t 0federated web sso @ no__t-1Appendix A：查看 AD FS 要求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![联合的 web sso](media/icon_checkboxo.gif)|查看有关声明的重要概念，在这两个伙伴组织中部署 AD FS 之前声明规则、 属性存储和 AD FS 配置数据库。|@no__t 0federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[了解关键 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![联合的 web sso](media/icon_checkboxo.gif)|根据您的设计规划，每个伙伴组织中安装一个或多个联合服务器。 **注意：** 对于联合 Web SSO 设计，帐户伙伴组织和资源伙伴组织中必须至少有一个联合服务器。|@no__t 0federated web sso @ no__t-1Checklist：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![联合的 web sso](media/icon_checkboxo.gif)|\(Optional @ no__t-1 确定你的组织是否需要联合服务器代理。 如果你的设计规划需要代理，您可以在每个伙伴组织中安装一个或多个联合服务器代理。|@no__t 0federated web sso @ no__t-1Checklist：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![联合的 web sso](media/icon_checkboxo.gif)|根据你的设计规划，请共享证书、配置客户端并在这两个伙伴组织中配置信任关系，以便他们可以通过联合信任进行通信。|@no__t 0federated web sso @ no__t-1Checklist：配置帐户伙伴组织 @ no__t-0<br /><br />@no__t 0federated web sso @ no__t-1Checklist：配置资源伙伴组织 @ no__t-0|  
|![联合的 web sso](media/icon_checkboxo.gif)|如果你是资源伙伴组织中的管理员，请使用 WIF 和 WIF SDK 声明 @ no__t-0enable 你的 Web 浏览器应用程序、Web 服务应用程序或 Microsoft® Office SharePoint® Server 应用程序。|@no__t 0federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />@no__t 0federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity FOUNDATION SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
