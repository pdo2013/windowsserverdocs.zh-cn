---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: "在广告 FS 如何使用 Uri"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 305bf0cece742c961604dacda7e27b8eac8065e5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2

# <a name="how-uris-are-used-in-ad-fs"></a>在广告 FS 如何使用 Uri
统一资源标识符 \(URI\) 是一串的字符用作的唯一标识符。  在广告 FS Uri 用于识别合作伙伴网络地址和配置对象。  当使用于标识合作伙伴网络地址，URI 始终是 URL。  使用于标识配置对象，当 URI 可能 URN 或 URL。  有关 Uri 的详细信息，请参阅[RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289)和[RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453)。  
  
## <a name="uris-as-partner-network-addresses"></a>合作伙伴网络地址作为 Uri  
以下是由广告 FS 中管理员最常处理网络地址 Url。  
  
-   联合身份验证服务，包括 WS\ 联合身份验证、 SAML、 WS\ 信任、 联盟元数据、 WS\ MetadataExchange、 隐私和组织 Url 的 Url  
  
-   信赖的方信任，包括 WS\ 联合身份验证、 SAML，以及联盟元数据 Url 的 Url  
  
-   索赔提供商信任，包括 WS\ 联合身份验证、 SAML，以及联盟元数据 Url Url  
  
## <a name="uris-as-object-identifiers"></a>为对象标识符 Uri  
下表介绍了最常由管理员广告 FS 中的标识符。  
  
|标识符名称|描述|比较|  
|-------------------|---------------|---------------|  
|联合身份验证服务的标识符|此标识符用于识别联合身份验证服务。  使用此联合身份验证服务，以及问题索赔到此联合身份验证服务的索赔提供商来自声明的信赖方使用它。|用户请求来自索赔此联合身份验证服务提供商的索赔时, 联合身份验证服务标识符将用于识别目标的索赔。<br /><br />当此联合身份验证服务收到索赔提供商的索赔时，它将检查以确保索赔时间范围内通过查找有关其联合身份验证服务标识符。<br /><br />当信赖方受到此联合身份验证服务索赔依赖方将检查的索赔发行商匹配联合身份验证服务标识符。|  
|依赖方标识符|此标识符用于识别依赖方到此联合身份验证服务。  使用它时发出到依赖方的索赔。|用户请求的索赔从此联合身份验证服务用于依赖方时, 信赖方标识符将用于识别依赖方应为其定向索赔。  完成此比较使用前缀匹配 \(see below\)。<br /><br />当信赖方收到的索赔时，它将检查其中的安全标记，以确保索赔将针对标识符。|  
|索赔提供商的标识符|此标识符用于标识为此联合身份验证服务的索赔提供程序。  使用它时接收索赔提供商的索赔。|在此联合身份验证服务接收索赔索赔提供商，此联合身份验证服务将检查的索赔发行商匹配的索赔提供商标识符。|  
|声称类型|此标识符用于定义的索赔类型。  通过此联合身份验证服务、 索赔提供商，并信赖方发送和接收索赔时使用。|当联合身份验证服务收到索赔提供商的索赔时，与相应的索赔提供商信任关联的索赔规则允许管理员比较索赔类型，并处理索赔。  与信赖的方信任关联的索赔规则还允许管理员比较从索赔提供商信任规则，利用即将索赔索赔类型，并决定处理的索赔。|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>URI 前缀信赖方标识符匹配  
URI 的路径语法分层组织和任一所有的分隔"\ 月"字符或所有":"字符。  因此路径可能分为根据的分隔字符路径部分。  当前缀匹配、 每个部分必须匹配规则完整匹配 \ （这些规则控制 matches\ 的大小写）。 有关规则匹配的详细信息，请参阅 RFC 上述。  
  
依赖方标识为联合身份验证服务请求中，当广告 FS 使用匹配逻辑前缀确定是否有一个匹配信赖方信任广告 FS 配置数据库中。  
  
例如，如果信赖方标识符广告 FS 配置数据库中的 \(URI1\) 至中传入的信赖方标识符前缀请求 \(URI2\)，必须满足以下然后：  
  
-   尾随分隔符 \(slashes and colons\) 路径的部分或机构必须忽略  
  
-   方案和颁发机构的部分 URI1 和 URI2 必须区分大小写的确切匹配  
  
-   每个路径部分 URI1 必须完全匹配 \ （基于区分大小写 chosen\） 到 URI2 的相应路径节  
  
-   URI2 可能具有多个路径部分比 URI1，但 URI1 必须没有比 URI2 的更多路径节  
  
-   URI1 不能 URI2 比的更多路径节  
  
-   如果 URI1 查询字符串，必须匹配完全到 URI2 查询字符串  
  
-   如果 URI1 具有片段，必须匹配到 URI2 片段准确  
  
下表提供的其他示例。  
  
|依赖方广告 FS 配置数据库中的标识符|依赖方请求邮件中的标识符|请求标识符匹配配置标识符？|原因|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:///\/contoso.com|http:///\/contoso.com|如此|精确匹配|  
|http:///\/contoso.com\/|http:///\/contoso.com|如此|将忽略尾随斜杠|  
|http:///\/contoso.com|http:///\/contoso.com\/|如此|将忽略尾随斜杠|  
|http:///\/contoso.com|http:///\/contoso.com\/hr|如此|URI1 具有没有路径和匹配的方案并到 URI2 颁发机构|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/web|如此|第一个路径区匹配、 URI1 有任何第二个路径部分|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/web\/?m\=t|如此|与上述相同的原因，查询字符串不会更改任何内容|  
|http:///\/contoso.com\/hr\/|http:///\/contoso.com\/hrw\/main|假|第 1 部分中的 URI1 路径与 URI2 路径第 1 部分中不匹配|  
|http:///\/contoso.com\/hr|http:///\/contoso.com|假|URI1 具有多个路径部分 URI2 比|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hrweb|假|第一个路径部分不匹配|  
|http:///\/contoso.com\/?m\=t|http:///\/contoso.com\/?m\=f|假|查询字符串部分不匹配|  
|https:\/\/contoso.com|http:///\/contoso.com|假|方案部分不匹配|  
|http:///\/sts.contoso.com|http:///\/contoso.com|假|颁发机构部分不匹配|  
|http:///\/contoso.com|http:///\/sts.contoso.com|假|颁发机构部分不匹配|  
  

