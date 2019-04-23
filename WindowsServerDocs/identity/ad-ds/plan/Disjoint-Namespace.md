---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: 不连续的命名空间
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a2e911e889343b05a515c94e615d3648289f2df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883568"
---
# <a name="disjoint-namespace"></a>不连续的命名空间

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

一个或多个域成员计算机具有主域名服务 (DNS) 后缀与计算机的成员的 Active Directory 域的 DNS 名称不匹配时发生非连续命名空间。 例如，使用 corp.fabrikam.com 作为主 DNS 后缀中名为 na.corp.fabrikam.com 的 Active Directory 域的成员计算机使用非连续命名空间。  
  
非连续命名空间是更复杂，无法管理、 维护和比连续的命名空间进行故障排除。 在连续的命名空间中的主 DNS 后缀与 Active Directory 域名称匹配。 编写认为 Active Directory 命名空间是所有域成员计算机的主 DNS 后缀相同的网络应用程序在非连续命名空间中无法正常工作。  
  
## <a name="support-for-disjoint-namespaces"></a>对非连续命名空间的支持  
域成员计算机，包括域控制器上，客户端可在非连续命名空间。 域成员计算机可以注册其主机 (A) 资源记录和 IP 版本 6 (IPv6) 的主机 (AAAA) 资源记录在非连续 DNS 命名空间中的。 域成员计算机以这种方式注册其资源记录，域控制器继续注册等同于 Active Directory 域名的 DNS 区域中的全局和特定于站点的服务 (SRV) 资源记录。  
  
例如，假设名为 na.corp.fabrikam.com 使用 corp.fabrikam.com 作为主 DNS 后缀的 Active Directory 域的域控制器中的 corp.fabrikam.com DNS 区域中，注册主机 (A) 和 IPv6 主机 (AAAA) 资源记录。 域控制器将继续注册全局和特定于站点的服务 (SRV) 资源记录在 _msdcs.na.corp.fabrikam.com 和 na.corp.fabrikam.com DNS 区域，可使服务位置。  
  
> [!IMPORTANT]  
> 虽然 Windows 的操作系统可能支持非连续命名空间，但编写假定主 DNS 后缀与 Active Directory 域后缀相同的应用程序可能无法在此类环境中。 出于此原因，你应测试所有应用程序和其各自的操作系统仔细之前部署非连续命名空间。  
  
非连续命名空间应起作用 （并且支持） 在以下情况下：  
  
-   当具有多个 Active Directory 域的林使用单个 DNS 命名空间，这是也称为 DNS 区域  
  
    此示例是一家公司，如 na.corp.fabrikam.com、 sa.corp.fabrikam.com 和 asia.corp.fabrikam.com 名称使用地区域并使用单个的 DNS 命名空间，如 corp.fabrikam.com。  
  
-   当单个 Active Directory 域拆分为单独的 DNS 命名空间  
  
    此示例是具有 corp.contoso.com 使用如 hr.corp.contoso.com、 production.corp.contoso.com 和 it.corp.contoso.com DNS 区域的 Active Directory 域的公司。  
  
非连续命名空间无法正常工作 （和不支持） 在以下情况下：  
  
-   由域成员的非连续后缀匹配这个或另一个林的 Active Directory 域名。 Kerberos 名称后缀路由会中断。  
  
-   在另一个林中使用相同的非连续后缀。 这可以防止林之间唯一地路由这些后缀。  
  
-   当域成员证书颁发机构 (CA) 服务器更改其完全限定的域名 (FQDN)，以便它不能再使用由 CA 服务器所属域的域控制器使用的相同的主 DNS 后缀。 在这种情况下，可能存在问题验证证书的 CA 服务器颁发的具体取决于在 CRL 分发点中使用哪些 DNS 名称。 但是，如果 CA 服务器置于稳定的非连续命名空间，它可正常工作，并支持。  
  
## <a name="considerations-for-disjoint-namespaces"></a>非连续命名空间的注意事项  
以下注意事项可能有助于确定是否应使用非连续命名空间。  
  
### <a name="application-compatibility"></a>应用程序兼容性  
如前文所述，非连续命名空间可能会导致任何应用程序和编写假定计算机主 DNS 后缀的域名，它是成员的名称完全相同的服务的问题。 部署非连续命名空间之前，必须检查应用程序兼容性问题。 此外，请务必检查您在执行分析时使用的所有应用程序的兼容性。 这包括应用程序从 Microsoft 和其他软件开发人员。  
  
### <a name="advantages-of-disjoint-namespaces"></a>非连续命名空间的优点  
使用非连续命名空间可以具有以下优势：  
  
-   因为一台计算机的主 DNS 后缀可以指示不同的信息，可以从 Active Directory 域名单独管理的 DNS 命名空间。  
  
-   可以将基于业务结构或地理位置上的 DNS 命名空间。 例如，可以单独根据业务单元名称或物理位置，例如属于一个大洲、 国家/地区或生成的命名空间。  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>非连续命名空间的缺点  
使用非连续命名空间可以具有以下缺点：  
  
-   您必须创建和管理不同的 DNS 区域中有使用非连续命名空间的成员计算机的林中每个 Active Directory 域。 （也就是说，它需要更多、 更复杂配置）。  
  
-   必须执行手动步骤来修改和管理 Active Directory 属性，可让域成员，以使用指定的主 DNS 后缀。  
  
-   若要优化的名称解析，必须执行手动步骤来修改和维护组策略要配置与备用的主 DNS 后缀的成员计算机。  
  
    > [!NOTE]  
    > Windows Internet 名称服务 (WINS) 无法用于解析单标签名称偏移量为这个缺点。 有关 WINS 的详细信息，请参阅 WINS 的技术参考 ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303))。  
  
-   当你的环境需要多个主 DNS 后缀时，你必须正确配置的林中的所有 Active Directory 域的 DNS 后缀搜索顺序。  
  
    若要设置的 DNS 后缀搜索顺序，可以使用组策略对象或动态主机配置协议 (DHCP) 服务器服务参数。 此外可以修改注册表。  
  
-   您必须仔细测试兼容性问题的所有应用程序。  
  
详细了解可以采取的步骤来解决这些缺点，请参阅创建非连续 Namespace ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638))。  
  
### <a name="planning-a-namespace-transition"></a>规划的命名空间过渡  
修改命名空间之前，请查看适用于转换已从连续命名空间，以非连续命名空间 （或相反） 的以下注意事项：  
  
-   手动配置服务主体名称 (Spn) 可能不再与匹配的 DNS 名称命名空间更改后。 这可能会导致身份验证失败。  
  
    有关详细信息，请参阅服务登录失败，因为未正确设置 Spn ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304))。  
  
    -   如果通过约束委派使用基于 Windows Server 2003 的计算机，这些计算机可能需要其他配置，以更改 Spn。 有关详细信息，请参阅 Microsoft 知识库中相应的文章 936628 ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306))。  
  
    -   如果你想要委派权限，可以修改 Spn 下级管理员，请参阅修改 spn 的委派权限 ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639))。  
  
-   如果您使用轻型目录访问协议 (LDAP) 通过安全套接字层 (SSL) （称为 LDAPS） 具有非连续命名空间中配置的域控制器的部署中的 CA，则必须使用相应的 Active Directory 域名和主 DNS 后缀时配置 LDAPS 的证书。  
  
    有关域控制器证书要求的详细信息，请参阅 Microsoft 知识库中相应的文章 321051 ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307))。  
  
    > [!NOTE]  
    > 使用 LDAPS 的证书的域控制器可能需要重新部署其证书。 当你执行此操作时，直到重启域控制器可能选择适当的证书。 Windows Server 2003 LDAPS 身份验证和更新相关的详细信息，请参阅 Microsoft 知识库中相应的文章 932834 ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308))。  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>对于非连续命名空间的部署规划  
如果部署中具有非连续命名空间的环境的计算机，请采取以下预防措施：  
  
1.  通知与您开展业务，他们必须测试和支持非连续命名空间的所有软件供应商。 要求他们以验证在使用非连续命名空间的环境中支持他们的应用程序。  
  
2.  在非连续命名空间的实验室环境中测试所有版本的操作系统和应用程序。 执行操作时，请遵循以下建议：  
  
    1.  在您的环境中部署软件之前，请解决所有软件问题。  
  
    2.  如果可能，请参与的操作系统和你计划在非连续命名空间中部署的应用程序的 beta 测试。  
  
3.  确保管理员和帮助台人员意识到的非连续命名空间和其影响。  
  
4.  创建一个计划，从而能够为您转换从一个非连续命名空间以连续的命名空间，如有必要。  
  
5.  宣传非连续命名空间支持与操作系统和应用程序提供程序的重要性。  
  


