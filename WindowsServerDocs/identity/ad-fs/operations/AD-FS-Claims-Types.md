---
ms.assetid: 
title: "客户端访问声称广告 FS 中类型"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>客户端访问策略声称广告 FS 中类型

若要提供其他请求上下文信息，请客户端访问策略，请使用以下的索赔类型广告 FS 生成从处理请求标头信息。  有关详细信息，请参阅[的索赔引擎角色](../technical-reference/the-role-of-the-claims-engine.md)。

##<a name="x-ms-forwarded-client-ip"></a>X MS-转发的客户端-IP

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

此广告 FS 索赔表示"最佳企图"有助于确定发出请求用户（例如，Outlook 客户端）的 IP 地址。 此声明可能包含多个 IP 地址，其中包括每个请求转发的代理服务器的地址。  此声明填充当前 HTTP 标题只能通过 Exchange Online，它对广告 FS 传递验证请求时填充标题设置。 声明值可以是以下情况之一：


- 一个 IP 地址-直接连接到联机更换费用的客户端路由器的 IP 地址

    >![注意]公司的网络上的客户端的 IP 地址将显示为组织的站代理或网关的外部界面 IP 地址。

- 一个或多个 IP 地址
    - 如果无法确定 Exchange Online 连接的客户端的 IP 地址，它将设置基于 x-转发-有关标头的值纳入 HTTP 基于非标准标题请求和支持的许多客户端、负载平衡和代理服务器市场上的值。
    - 多个 IP 地址，表明客户 IP 地址和每个传递请求的代理服务器的地址会用逗号分隔。

    >![注意]不会出现在列表中与相关的 Exchange Online 基础结构的 IP 地址。


>![警告]当前支持的只 IPV4 地址; Exchange Online 它不支持 IPV6 地址。 


## <a name="x-ms-client-application"></a>X MS 的客户端应用程序

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

此广告 FS 索赔表示协议终止的客户端，它松对应正在使用的应用程序使用。  此声明填充当前 HTTP 标题只能通过 Exchange Online，它对广告 FS 传递验证请求时填充标题设置。 具体取决于该应用程序，将为此声明的值下列情况之一：



- 对于使用 Exchange 活动同步的设备，值为 Microsoft.Exchange.ActiveSync。 
- Microsoft Outlook 客户端的使用可能会导致的任何以下值：
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- 其他可能值的此标头如下：
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS 的客户端的用户代理

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

此广告 FS 索赔提供字符串代表客户端使用访问该服务的设备类型。 当客户想要阻止访问的某些设备（如特定类型的智能手机一样），可以使用此。  此声明填充当前 HTTP 标题只能通过 Exchange Online，它对广告 FS 传递验证请求时填充标题设置。 此声明的示例值包括（但不是限于）以下值。
>![注意]下面是示例 x ms 用户代理值可能包含的客户端其 x-ms 的客户端应用程序是"Microsoft.Exchange.ActiveSync"

- 涡流/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android / 0.3

>![注意]还有可能此值为空。


## <a name="x-ms-proxy"></a>X MS 代理

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

此广告 FS 索赔表示请求已通过联合身份验证的服务器代理。  此声明填充由后端联合身份验证服务传递验证请求时填充标题联盟服务器代理服务器。 广告 FS 然后将其转换为索赔。 

声明值是传递请求联合服务器代理 DNS 名称。

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-端点-绝对路径 (活动 vs 被动)

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

此声明类型可用于确定源自而不是"无源"（web 浏览器基于）客户端的"活动"（丰富）客户端的请求。 这使外部请求从如 Outlook Web Access、SharePoint Online 或 Office 365 门户时，将阻止来自丰富的客户端，如 Microsoft Outlook 请求允许浏览器基于应用程序。

此声明的值为收到此请求广告 FS 服务的名称。
