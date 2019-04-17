---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: "断开连接 Namespace"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac231dbfbaaeafa39199e29a1744d84d5cec23e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="disjoint-namespace"></a>断开连接 Namespace

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

一个或多个域成员计算机已与这两台计算机已经成员 Active Directory 域 DNS 名称不匹配的主要域名服务 (DNS) 职务时发生命名空间断开连接。 例如，在名为 na.corp.fabrikam.com Active Directory 的域中使用的 corp.fabrikam.com 主 DNS 职务成员计算机使用命名空间断开连接。  
  
命名空间断开连接很复杂管理、维护和比连续命名空间疑难解答。 连续命名空间，在主 DNS 职务匹配的 Active Directory 的域名。 编写假设 Active Directory 命名空间为相同的所有域成员计算机主 DNS 职务到的网络应用中命名空间断开连接无法正常工作。  
  
## <a name="support-for-disjoint-namespaces"></a>断开连接命名空间的支持  
域成员计算机，包括域控制器，可以在命名空间断开连接正常工作。 域成员计算机可以注册他们主机 () 资源录制和 IP 版本 6 (IPv6) 主机 (AAAA) 资源记录中 DNS 命名空间断开连接。 当域成员计算机以这种方式注册它们资源的记录时，域控制器继续等同于 Active Directory 域名 DNS 区域中注册全球的和特定于站点服务 (SRV) 资源记录。  
  
例如，假设，域控制器 Active Directory 域名为使用的 corp.fabrikam.com 主 DNS 职务的 na.corp.fabrikam.com 主机 (A) 和注册 IPv6 主机 (AAAA) 资源记录 corp.fabrikam.com DNS 区域中。 域控制器继续注册全球的和特定于站点服务 (SRV) 资源记录 _msdcs。na.corp.fabrikam.com 和 na.corp.fabrikam.com DNS 区域中，这使位置服务。  
  
> [!IMPORTANT]  
> 虽然 Windows 操作系统可能支持命名空间断开连接，假设主 DNS 职务为相同的 Active Directory 域职务编写的应用程序可能无法正常此类环境中。 出于此原因，你应该测试的所有应用程序和他们各自的操作系统仔细部署命名空间断开连接之前。  
  
命名空间断开连接应使用（也支持）在以下情况下：  
  
-   具有多个 Active Directory 域森林时使用的单个 DNS 命名空间，即所谓 DNS 区域  
  
    它的一个示例是与名称，如 na.corp.fabrikam.com、sa.corp.fabrikam.com 和 asia.corp.fabrikam.com 使用区域域，则使用单个 DNS 命名空间，如 corp.fabrikam.com 的公司。  
  
-   当单个域的 Active Directory 拆分为独立 DNS 命名空间  
  
    它的一个示例是 Active Directory corp.contoso.com 使用 hr.corp.contoso.com、production.corp.contoso.com 和 it.corp.contoso.com 等 DNS 区域的域的公司。  
  
命名空间断开连接无法正常工作（和不受支持）在以下情况下：  
  
-   断开连接职务域成员使用匹配 Active Directory 的域名这个或另一个森林中。 这将中断 Kerberos 名称职务路由。  
  
-   在另一台森林使用相同的断开连接职务。 这将阻止森林之间的唯一路由这些后缀。  
  
-   当域成员证书颁发机构时）服务器更改完全被合格域名 (FQDN)，以便它不会再使用相同的主要 DNS 职务使用的域控制器 CA 服务器的域。 在此情况下，可能颁发，具体取决于 CRL 分布点中采用了哪些 DNS 名称 CA 服务器具有验证证书的问题。 但是，如果 CA 服务器置于稳定已命名空间断开连接，它正常工作，并且受支持。  
  
## <a name="considerations-for-disjoint-namespaces"></a>断开连接命名空间的事项  
以下事项可帮助你决定是否你应使用命名空间断开连接。  
  
### <a name="application-compatibility"></a>应用程序兼容性  
如上所述，命名空间断开连接可能会导致的任何应用程序和服务编写假设计算机的主 DNS 职务为相同的成员它的域名称的问题。 部署命名空间断开连接之前，必须选中应用程序的兼容性问题。 此外，请确保查看所有时进行分析你使用的应用程序的兼容性。 这包括 Microsoft 和其他软件开发人员的应用。  
  
### <a name="advantages-of-disjoint-namespaces"></a>断开连接命名空间的优势  
使用命名空间断开连接可能有以下优势：  
  
-   由于计算机的主 DNS 职务可以指示不同的信息，可以通过 Active Directory 域名单独管理 DNS 空间。  
  
-   你可以单独 DNS 空间根据企业结构或地理位置。 例如，你可以单独根据企业设备名称或物理位置，例如大陆、国家/地区或建筑命名空间。  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>断开连接命名空间的缺点  
使用命名空间断开连接可能会以下缺点：  
  
-   你必须创建和管理已使用命名空间断开连接的成员计算机森林中的每个 Active Directory 域单独 DNS 区域。 （即，它需要其他和更复杂的配置。）  
  
-   你必须执行手动步骤修改和管理允许使用指定的主 DNS 后缀成员域的 Active Directory 属性。  
  
-   若要优化名称分辨率，必须执行修改和维护组策略，用来配置成员计算机备用主 DNS 后缀手动步骤。  
  
    > [!NOTE]  
    > Windows Internet 名称服务 (WINS) 可用于减去此缺点解决单标签名称。 有关 WINS 的详细信息，请参阅 WINS 技术参考 ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303))。  
  
-   当您的环境需要多个主要 DNS 后缀时，你必须相应地配置树林中的 DNS 职务搜索顺序所有 Active Directory 域。  
  
    若要设置 DNS 职务搜索订单，你可以使用组策略对象或动态主机配置协议 (DHCP) 服务器服务参数。 你还可以修改注册表。  
  
-   仔细必须测试用于兼容性问题的所有应用。  
  
有关详细信息可以采取的步骤来解决这些缺点，请参阅创建不连贯 Namespace ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638))。  
  
### <a name="planning-a-namespace-transition"></a>计划命名空间转换  
修改命名空间之前，请查看以下事项，从连续命名空间来不连续命名空间（或反向操作）适用于过渡：  
  
-   手动配置服务主要名称 (Spn) 可能不会再匹配 DNS 名称命名空间更换后。 这可能导致身份验证失败情况。  
  
    有关详细信息，请参阅 Service 登录失败截止到正确设置 Spn ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304))。  
  
    -   如果你使用 Windows Server 2003 基于计算机约束委派与时，这些计算机可能需要额外的配置更改 Spn。 有关详细信息，请参阅在 Microsoft 知识库文章 936628 ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306))。  
  
    -   如果你想要委派修改为了附属管理员权限，请参阅到修改 Spn 委派颁发机构 ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639))。  
  
-   如果你使用通过安全套接字层 (SSL)（也称为 LDAPS）中的部署具有域控制器在命名空间断开连接配置 ca 轻型目录访问协议 (LDAP)，必须使用相应的 Active Directory 域名和主 DNS 职务，当您将配置 LDAPS 证书。  
  
    域控制器证书要求的详细信息，请参阅 Microsoft 知识库文章 321051 ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307))。  
  
    > [!NOTE]  
    > 用于 LDAPS 证书的域控制器可能需要重新部署它们的证书。 执行此操作时，域控制器不可能选择相应的证书，直到重新启动它们。 Windows Server 2003 LDAPS 身份验证和相关的更新的详细信息，请参阅 Microsoft 知识库文章 932834 ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308))。  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>计划命名空间断开连接部署  
如果计算机已经命名空间断开连接的环境中部署，采取以下步骤：  
  
1.  通知您与之执行业务他们必须测试，并支持命名空间断开连接的所有软件供应商。 请求他们检查他们的环境中使用不连贯命名空间支持他们的应用程序。  
  
2.  测试命名空间断开连接实验环境中的所有版本的操作系统和应用程序。 当你执行此操作时，请按照这些建议：  
  
    1.  到您的环境中部署该软件之前，将解决所有软件问题。  
  
    2.  如果可能，请参与 beta 版测试的操作系统和计划部署在不连贯命名空间的应用程序。  
  
3.  确保管理员和技术支持人员了解命名空间断开连接，它的影响。  
  
4.  创建就可以为你转换从命名空间断开连接到连续命名空间，如有必要的套餐。  
  
5.  进行宣传命名空间断开连接支持操作系统和应用程序的提供商的重要性。  
  


