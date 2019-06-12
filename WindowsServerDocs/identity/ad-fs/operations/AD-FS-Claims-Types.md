---
ms.assetid: ''
title: 客户端访问的声明类型中的 AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0ffa4273a2c776a16f3ea0ce77d1b3a528481468
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445159"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>客户端访问策略的声明类型中的 AD FS

若要提供额外的请求上下文信息，客户端访问策略，请使用 AD FS 生成从处理的请求标头信息的以下声明类型。  有关详细信息请参阅[声明引擎的角色](../technical-reference/the-role-of-the-claims-engine.md)。

## <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

此 AD FS 声明所表示的次认定发出请求的 IP 地址的用户 （例如，Outlook 客户端） 的"最佳尝试"。 此声明可以包含多个 IP 地址，包括每个代理将请求转发地址。  此声明填充从当前的 HTTP 标头只能通过 Exchange Online，填充标头将身份验证请求传递给 AD FS 时设置。 声明的值可以是以下值之一：


- 单个 IP 地址-直接连接到 Exchange Online 的客户端的 IP 地址

    >![注意]企业网络上的客户端的 IP 地址将显示为组织的出站代理服务器或网关的外部接口 IP 地址。

- 一个或多个 IP 地址
  - 如果无法确定 Exchange Online 的连接的客户端的 IP 地址，它会设置值的基础 x 转发的标头，可以包含在基于 HTTP 的非标准标头请求和支持的多个客户端，负载均衡器的值和市场上的代理。
  - 将由逗号分隔多个 IP 地址，该值指示客户端 IP 地址和每个传递请求的代理服务器的地址。

    >![注意]Exchange Online 基础结构相关的 IP 地址不会出现在列表中。


>![警告]Exchange Online 目前支持仅的 IPV4 地址。它不支持 IPV6 地址。 


## <a name="x-ms-client-application"></a>X-MS-Client-Application

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

此 AD FS 声明表示对应于正在使用的应用程序的松散的最终客户端使用的协议。  此声明填充从当前的 HTTP 标头只能通过 Exchange Online，填充标头将身份验证请求传递给 AD FS 时设置。 根据应用程序，此声明的值将是以下之一：



- 对于使用 Exchange Active Sync 的设备，则值为 Microsoft.Exchange.ActiveSync。 
- 使用 Microsoft Outlook 客户端可能会导致任何以下值：
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- 此标头的其他可能值包括：
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

此 AD FS 声明提供表示客户端正在使用以访问服务的设备类型的字符串。 这可用来在客户想要阻止某些设备 （例如，特定类型的智能手机） 的访问权限时。  此声明填充从当前的 HTTP 标头只能通过 Exchange Online，填充标头将身份验证请求传递给 AD FS 时设置。 此声明的示例值包括 （但不限于） 以下值。
>![注意]下面是示例的 x ms 用户代理值可能包含其 x-ms-客户端应用程序是"Microsoft.Exchange.ActiveSync"为客户端

- 顶点/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>![注意]还有可能此值为空。


## <a name="x-ms-proxy"></a>X-MS-Proxy

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

此 AD FS 声明指示请求了通过联合身份验证服务器代理。  此声明由联合服务器代理，填充标头将身份验证请求传递到后端联合身份验证服务时填充。 AD FS 然后将其转换为声明。 

声明的值是传递请求的联合身份验证服务器代理的 DNS 名称。

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-终结点的绝对路径 （活动与被动）

声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

此声明类型可以用于确定从与"被动"（web 浏览器基于的） 客户端的"活动"（丰富） 客户端发出的请求。 这使从基于浏览器的应用程序如 Outlook Web Access、 SharePoint Online，或 Office 365 门户会阻止来自如 Microsoft Outlook 的丰富客户端的请求而允许的外部请求。

声明的值是接收请求的 AD FS 服务的名称。
