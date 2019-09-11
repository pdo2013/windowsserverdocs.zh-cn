---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: 在 AD FS 中如何使用 URI
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: edd708985b8caac30b8788b12237430c1711f22f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865453"
---
# <a name="how-uris-are-used-in-ad-fs"></a>在 AD FS 中如何使用 URI
统一资源标识符\(URI\)是用作唯一标识符的字符串。  在 AD FS 中，URI 用于标识合作伙伴网络地址和配置对象。  用于标识合作伙伴网络地址时，URI 始终是 URL。  用于标识配置对象时，URI 可以是 URN，也可以是 URL。  有关 URI 的更多常规信息，请参阅 [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) 和 [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453)。  
  
## <a name="uris-as-partner-network-addresses"></a>用作合作伙伴网络地址的 URI  
以下是 AD FS 中的管理员最常处理的网络地址 URL。  
  
-   联合身份验证服务的 url，包括 WS\-联合身份验证、SAML、WS\-信任、联合元数据、WS\-ws-metadataexchange、隐私和组织 url  
  
-   信赖方信任的 url，包括 WS\-联合身份验证、SAML 和联合元数据 url  
  
-   声明提供方信任的 url，包括 WS\-联合身份验证、SAML 和联合元数据 url  
  
## <a name="uris-as-object-identifiers"></a>用作对象标识符的 URI  
下表介绍了 AD FS 中的管理员最常处理的标识符。  
  
|标识符名称|描述|比较|  
|-------------------|---------------|---------------|  
|联合身份验证服务标识符|此标识符用于标识联合身份验证服务。  它由使用来自此联合身份验证服务的声明的信赖方以及向此联合身份验证服务发出声明的声明提供程序使用。|当用户从声明提供程序请求有关此联合身份验证服务的声明时，联合身份验证服务标识符将用于标识声明的目标。<br /><br />当此联合身份验证服务收到来自声明提供程序的声明时，它将通过查找其联合身份验证服务标识符进行检查，以确保声明的目标是它。<br /><br />当信赖方接收来自此联合身份验证服务的声明时，信赖方将检查声明的发出方与联合身份验证服务标识符是否匹配。|  
|信赖方标识符|此标识符用于标识此联合身份验证服务的信赖方。  在向信赖方发出声明时使用。|当用户从该联合身份验证服务为信赖方请求声明时，信赖方标识符将用于标识应作为声明目标的信赖方。  使用前缀匹配\(完成此比较，请参阅\)下文。<br /><br />当信赖方收到声明时，它将检查其在安全令牌中的标识符，以确保声明的目标是它。|  
|声明提供程序标识符|此标识符用于标识此联合身份验证服务的声明提供程序。  在接收来自声明提供程序的声明时使用。|当此联合身份验证服务接收来自声明提供程序的声明时，此联合身份验证服务将检查声明的发出方与声明提供程序标识符是否匹配。|  
|声明类型|此标识符用于定义声明的类型。  它由此联合身份验证服务、声明提供程序和信赖方收发声明时使用。|当联合身份验证服务收到来自声明提供程序的声明时，与相应声明提供程序信任关联的声明规则将允许管理员比较声明类型并处理声明。  与信赖方信任关联的声明规则也允许管理员比较来自声明提供程序信任规则的声明的声明类型，并决定要发出的声明。|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>信赖方标识符的 URI 前缀匹配  
URI 的路径语法按层次结构组织，并由所有 "\/" 字符或所有 "：" 字符分隔。  因此，该路径可以基于分隔字符拆分为多个路径部分。  当前缀匹配时，每个部分必须与这些规则控制匹配\(\)大小写的匹配规则完全匹配。 有关匹配规则的详细信息，请参阅上文所述的 RFC。  
  
在向联合身份验证服务发出的请求中标识信赖方时，AD FS 将使用前缀匹配逻辑来确定 AD FS 配置数据库中是否有匹配的信赖方信任。  
  
例如，如果 AD FS 配置\(数据库 URI1\)中的信赖方标识符是传入请求\(URI2\)中信赖方标识符的前缀，则必须满足以下条件：  
  
-   必须忽略\(结尾分隔符斜杠\)和路径节或颁发机构的冒号  
  
-   URI1 和 URI2 的方案和授权部分必须完全匹配（不区分大小写）  
  
-   URI1 的每个路径部分必须与 URI2 的\(相应路径部分所选\)的区分大小写完全匹配。  
  
-   URI2 的路径部分可以比 URI1 多，但 URI1 的路径部分不能比 URI2 多  
  
-   URI1 的路径部分不能比 URI2 多  
  
-   如果 URI1 具有查询字符串，它必须与 URI2 查询字符串完全匹配  
  
-   如果 URI1 具有片段，它必须与 URI2 片段完全匹配  
  
下表提供了其他示例。  
  
|AD FS 配置数据库中的信赖方标识符|请求消息中的信赖方标识符|请求标识符与配置标识符是否匹配？|Reason|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http：\/\/contoso.com|http：\/\/contoso.com|TRUE|精确匹配|  
|http：\/\/contoso.com\/|http：\/\/contoso.com|TRUE|忽略尾部斜杠|  
|http：\/\/contoso.com|http：\/\/contoso.com\/|TRUE|忽略尾部斜杠|  
|http：\/\/contoso.com|http：\/\/contoso.comhr\/|TRUE|URI1 没有路径，且方案和授权与 URI2 匹配|  
|http：\/\/contoso.comhr\/|http：\/\/contoso.comhr\/web\/|TRUE|第一个路径部分匹配，URI1 没有第二个路径部分|  
|http：\/\/contoso.comhr\/|http：\/\/contoso.comhr\/web\/？m\=t\/|TRUE|如上所述，查询字符串不会更改任何内容|  
|http：\/\/contoso.comhr\/\/|http：\/\/contoso.comhrw\/main\/|FALSE|URI1 路径部分 1 与 URI2 路径部分 1 不匹配|  
|http：\/\/contoso.comhr\/|http：\/\/contoso.com|FALSE|URI1 的路径部分比 URI2 多|  
|http：\/\/contoso.comhr\/|http：\/\/contoso.comhrweb\/|FALSE|第一个路径部分不匹配|  
|http：\/\/contoso.com？\/mt\=|http：\/\/contoso.com？\/mf\=|FALSE|查询字符串部分不匹配|  
|https：\/\/contoso.com|http：\/\/contoso.com|FALSE|方案部分不匹配|  
|http：\/\/sts.contoso.com|http：\/\/contoso.com|FALSE|授权部分不匹配|  
|http：\/\/contoso.com|http：\/\/sts.contoso.com|FALSE|授权部分不匹配|  
  

