---
ms.assetid: ''
title: AD FS 中的客户端访问声明类型
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a689e68ae60268880fd28158820c1803ab35bc33
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358620"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>AD FS 中的客户端访问策略声明类型

为了提供其他请求上下文信息，客户端访问策略使用以下声明类型，AD FS 从请求标头信息生成以进行处理。  有关详细信息，请参阅[声明引擎的角色](../technical-reference/the-role-of-the-claims-engine.md)。

## <a name="x-ms-forwarded-client-ip"></a>X 毫秒-转发的客户端-IP

声明类型：`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

此 AD FS 声明在认定上表示一个 "最佳尝试"，该地址为发出请求的用户（例如，Outlook 客户端）的 IP 地址。 此声明可包含多个 IP 地址，包括转发请求的每个代理的地址。  此声明是从当前仅由 Exchange Online 设置的 HTTP 标头中填充的，它会在将身份验证请求传递到 AD FS 时填充标头。 声明的值可以是下列值之一：


- 单个 IP 地址-直接连接到 Exchange Online 的客户端的 IP 地址

    >!纪录企业网络上的客户端的 IP 地址将显示为组织的出站代理或网关的外部接口 IP 地址。

- 一个或多个 IP 地址
  - 如果 Exchange Online 无法确定正在连接的客户端的 IP 地址，它将基于 x 转发的标头的值设置值，该标头可以包含在基于 HTTP 的请求中，并且受多个客户端、负载均衡器和市场上的代理。
  - 指示客户端 IP 地址的多个 IP 地址和传递请求的每个代理的地址将用逗号分隔。

    >!纪录与 Exchange Online 基础结构相关的 IP 地址将不会出现在列表中。


>!出现Exchange Online 目前只支持 IPV4 地址;它不支持 IPV6 地址。 


## <a name="x-ms-client-application"></a>X-客户端-应用程序

声明类型：`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

此 AD FS 声明表示最终客户端使用的协议，该协议与所使用的应用程序松耦合。  此声明是从当前仅由 Exchange Online 设置的 HTTP 标头中填充的，它会在将身份验证请求传递到 AD FS 时填充标头。 根据应用程序的不同，此声明的值将是下列值之一：



- 如果使用 Exchange Active Sync 的设备，则值为 ""。 
- 使用 Microsoft Outlook 客户端可能会导致以下任何值：
    - Microsoft. 自动发现
    - OfflineAddressBook
    - Microsoft. RPC
    - WebServices
    - Microsoft. Mapi
- 此标头的其他可能的值包括：
    - Microsoft. Powershell
    - Microsoft. SMTP
    - PopImap
    - "
    - Microsoft。

## <a name="x-ms-client-user-agent"></a>X-MS-客户端-用户代理

声明类型：`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

此 AD FS 声明提供了一个字符串，表示客户端用于访问服务的设备类型。 当客户想要阻止某些设备（如智能手机）的访问时，可以使用此方法。  此声明是从当前仅由 Exchange Online 设置的 HTTP 标头中填充的，它会在将身份验证请求传递到 AD FS 时填充标头。 此声明的示例值包括（但不限于）以下值。
>!纪录下面是一个示例，说明 x ms 用户代理值可能包含的客户端（其 x ms-客户端应用程序为 "

- Vortex/1。0
- Apple-iPad1C1/812。1
- Apple-iPhone3C1/811。2
- Apple-iPhone/704.11
- Moto-DROID2/4.5。1
- SAMSUNGSPHD700/100.202
- Android/0。3

>!纪录此值也可能为空。


## <a name="x-ms-proxy"></a>X-MS-Proxy

声明类型：`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

此 AD FS 声明指示请求已通过联合服务器代理。  此声明由联合服务器代理填充，后者在将身份验证请求传递到后端联合身份验证服务时填充标头。 然后 AD FS 将其转换为声明。 

声明的值是传递请求的联合服务器代理的 DNS 名称。

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-终结点绝对路径（活动 vs 被动）

声明类型：`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

此声明类型可用于确定源自 "active" （胖）客户端的请求与 "被动" （基于 web 浏览器）客户端。 这样，就会阻止来自基于浏览器的应用程序（例如 Outlook Web 访问、SharePoint Online 或 Office 365 门户）的外部请求，同时会阻止来自丰富客户端（如 Microsoft Outlook）的请求。

声明的值是接收请求的 AD FS 服务的名称。
