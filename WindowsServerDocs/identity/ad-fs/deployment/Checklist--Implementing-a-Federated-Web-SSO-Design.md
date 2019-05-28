---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 清单-实现联合的 Web SSO 设计
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1f7a65687ac07c9f7d9a814b50c4090c70817b4f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192372"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>清单：实现联合 Web SSO 设计

此父清单包含交叉\-引用链接到有关联合 Web 单一的重要概念\-符号\-上\(SSO\)设计 Active Directory 联合身份验证服务\(AD FS\)。 它还包含指向从属清单的链接，该清单将帮助你完成实现此设计所需的任务。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当引用链接将你带到概念主题或从属清单时，在查看概念主题或完成从属清单中的任务后，请返回到本主题，这样你就可以继续完成此清单中的剩余任务。  
  
![联合 web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**核对清单：实现联合 Web SSO 设计**  
  
||任务|参考|  
|-|--------|-------------|  
|![联合的 web sso](media/icon_checkboxo.gif)|查看有关联合 Web SSO 设计的重要概念并确定可用于自定义此设计以满足您的组织的需求的 AD FS 部署目标。|![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合 Web SSO 设计](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[标识 AD FS 部署目标](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划部署](https://technet.microsoft.com/library/dd807083.aspx)|  
|![联合的 web sso](media/icon_checkboxo.gif)|查看硬件、 软件、 证书、 域名系统 \(DNS\), ，属性存储和您的组织中部署 AD FS 的客户端要求。|![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[附录 a:查看 AD FS 要求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![联合的 web sso](media/icon_checkboxo.gif)|查看有关声明的重要概念，在这两个伙伴组织中部署 AD FS 之前声明规则、 属性存储和 AD FS 配置数据库。|![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![联合的 web sso](media/icon_checkboxo.gif)|根据您的设计规划，每个伙伴组织中安装一个或多个联合服务器。 **注意：** 对于联合 Web SSO 设计中，你需要在帐户伙伴组织中的至少一台联合服务器和资源伙伴组织中的至少一台联合服务器。|![联合 web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![联合的 web sso](media/icon_checkboxo.gif)|\(可选\)确定你的组织是否需要联合服务器代理。 如果你的设计规划需要代理，您可以在每个伙伴组织中安装一个或多个联合服务器代理。|![联合 web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![联合的 web sso](media/icon_checkboxo.gif)|根据你的设计规划，请共享证书、配置客户端并在这两个伙伴组织中配置信任关系，以便他们可以通过联合信任进行通信。|![联合 web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：配置帐户伙伴组织](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![联合 web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：配置资源伙伴组织](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![联合的 web sso](media/icon_checkboxo.gif)|如果您是资源伙伴组织中的管理员，声明\-启用 Web 浏览器应用程序、 Web 服务应用程序或使用 WIF 和 WIF SDK 的 Microsoft® Office SharePoint® Server 应用程序。|![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
