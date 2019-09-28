---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: 不连续的命名空间
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5abe67c89ce4c2f4b5056f6197242b5db8db340e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408850"
---
# <a name="disjoint-namespace"></a>不连续的命名空间

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

当一台或多台域成员计算机的主域名服务（DNS）后缀与计算机所属的 Active Directory 域的 DNS 名称不匹配时，将发生非连续命名空间。 例如，在名为 na.corp.fabrikam.com 的 Active Directory 域中使用主 DNS 后缀 corp.fabrikam.com 的成员计算机使用的是非连续命名空间。  
  
与连续命名空间相比，非连续命名空间的管理、维护和故障排除更复杂。 在一个连续的命名空间中，主 DNS 后缀与 Active Directory 域名匹配。 写入的网络应用程序假定 Active Directory 命名空间与所有域成员计算机的主 DNS 后缀完全相同，因此在非连续命名空间中不能正常工作。  
  
## <a name="support-for-disjoint-namespaces"></a>支持非连续命名空间  
域成员计算机（包括域控制器）可以在非连续命名空间中运行。 域成员计算机可以在不连续的 DNS 命名空间中注册其主机（A）资源记录和 IP 版本6（IPv6）主机（AAAA）资源记录。 当域成员计算机以这种方式注册其资源记录时，域控制器将继续在 DNS 区域中注册与 Active Directory 域名相同的全局和特定于站点的服务（SRV）资源记录。  
  
例如，假设名为 na.corp.fabrikam.com 的域 Active Directory 控制器使用主 DNS 后缀 corp.fabrikam.com 在 corp.fabrikam.com DNS 区域中注册主机（A）和 IPv6 主机（AAAA）资源记录。 域控制器继续注册 _msdcs 和 na.corp.fabrikam.com DNS 区域中的全局和特定于站点的服务（SRV）资源记录，这使得服务位置成为可能。  
  
> [!IMPORTANT]  
> 尽管 Windows 操作系统可能支持非连续命名空间，但编写的应用程序假定主 DNS 后缀与 Active Directory 域后缀在此类环境中不起作用。 出于此原因，在部署非连续命名空间之前，应仔细测试所有应用程序及其相应的操作系统。  
  
在以下情况下，非连续命名空间应正常工作（支持）：  
  
-   当具有多个 Active Directory 域的林使用单个 DNS 命名空间（也称为 DNS 区域）时  
  
    例如，使用 na.corp.fabrikam.com、sa.corp.fabrikam.com 和 asia.corp.fabrikam.com 等名称的区域的公司使用单个 DNS 命名空间（如 corp.fabrikam.com）。  
  
-   单个 Active Directory 域拆分为单独的 DNS 命名空间时  
  
    例如，Active Directory 具有 corp.contoso.com 域的公司使用 hr.corp.contoso.com、production.corp.contoso.com 和 it.corp.contoso.com 等 DNS 区域。  
  
在以下情况下，非连续命名空间不能正常运行（并且不受支持）：  
  
-   域成员使用的非连续后缀与此或另一个林中的 Active Directory 域名匹配。 这会中断 Kerberos 名称后缀路由。  
  
-   其他林中使用了相同的非连续后缀。 这会阻止在林之间唯一路由这些后缀。  
  
-   当域成员证书颁发机构（CA）服务器更改其完全限定的域名（FQDN）时，使其不再使用 CA 服务器所属域的域控制器所使用的主 DNS 后缀。 在这种情况下，你可能会在验证 CA 服务器颁发的证书时出现问题，具体取决于 CRL 分发点中使用的 DNS 名称。 但是，如果将 CA 服务器置于稳定的非连续命名空间中，它将正常工作并受支持。  
  
## <a name="considerations-for-disjoint-namespaces"></a>非连续命名空间的注意事项  
以下注意事项可能会帮助你决定是否应使用不连续的命名空间。  
  
### <a name="application-compatibility"></a>应用程序兼容性  
如前所述，无交集命名空间可能导致任何编写的应用程序和服务出现问题，假定计算机主 DNS 后缀与它所属的域名相同。 在部署非连续命名空间之前，必须检查应用程序的兼容性问题。 此外，请务必检查执行分析时使用的所有应用程序的兼容性。 这包括来自 Microsoft 和其他软件开发人员的应用程序。  
  
### <a name="advantages-of-disjoint-namespaces"></a>非连续命名空间的优点  
使用不连续的命名空间具有以下优势：  
  
-   由于计算机的主 DNS 后缀可以指示不同的信息，因此你可以单独管理 DNS 命名空间，Active Directory 域名。  
  
-   你可以根据业务结构或地理位置分离 DNS 命名空间。 例如，可以根据业务部门名称或物理位置（如洲、国家/地区或建筑）来分隔命名空间。  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>非连续命名空间的缺点  
使用不连续的命名空间可能具有以下缺点：  
  
-   对于包含使用非连续命名空间的成员计算机的林中的每个 Active Directory 域，都必须创建和管理单独的 DNS 区域。 （也就是说，它需要更复杂的配置。）  
  
-   您必须执行手动步骤来修改和管理允许域成员使用指定的主 DNS 后缀的 Active Directory 属性。  
  
-   若要优化名称解析，必须执行手动步骤来修改和维护组策略以配置具有备用主 DNS 后缀的成员计算机。  
  
    > [!NOTE]  
    > Windows Internet 名称服务（WINS）可用于通过解析单标签名称来抵消此缺点。 有关 WINS 的详细信息，请参阅 WINS 技术参考（[https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)）。  
  
-   如果你的环境需要多个主 DNS 后缀，则必须相应地为林中的所有 Active Directory 域配置 DNS 后缀搜索顺序。  
  
    若要设置 DNS 后缀搜索顺序，可以使用组策略对象或动态主机配置协议（DHCP）服务器服务参数。 你还可以修改注册表。  
  
-   您必须仔细测试所有应用程序的兼容性问题。  
  
有关可用于解决这些缺点的步骤的详细信息，请参阅创建非连续命名空间（[https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)）。  
  
### <a name="planning-a-namespace-transition"></a>规划命名空间转换  
在修改命名空间之前，请查看以下注意事项，这些注意事项适用于从连续命名空间转换为非连续命名空间（或相反）：  
  
-   命名空间更改后，手动配置的服务主体名称（Spn）可能不再与 DNS 名称匹配。 这可能会导致身份验证失败。  
  
    有关详细信息，请参阅由于 Spn 设置不当（[https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)），服务登录失败。  
  
    -   如果你将基于 Windows Server 2003 的计算机用于约束委派，则这些计算机可能需要其他配置来更改 Spn。 有关详细信息，请参阅 Microsoft 知识库中的文章936628（[https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)）。  
  
    -   如果要委派权限来修改从属管理员的 Spn，请参阅委派权限以修改 Spn （[https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)）。  
  
-   如果在具有非连续命名空间中配置的域控制器的部署中将轻型目录访问协议（LDAP）与安全套接字层（SSL）（称为 LDAPS）一起使用，则必须使用适当的 Active Directory 域名和配置 LDAPS 证书时的主 DNS 后缀。  
  
    有关域控制器证书要求的详细信息，请参阅 Microsoft 知识库中的文章321051（[https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)）。  
  
    > [!NOTE]  
    > 使用用于 LDAPS 的证书的域控制器可能要求你重新部署其证书。 如果这样做，则在重新启动域控制器之前，可能不会选择适当的证书。 有关 LDAPS 身份验证和 Windows Server 2003 的相关更新的详细信息，请参阅 Microsoft 知识库中的文章932834（[https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)）。  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>规划非连续命名空间部署  
如果在具有非连续命名空间的环境中部署计算机，请采取以下预防措施：  
  
1.  通知您业务的所有软件供应商必须测试并支持非连续命名空间。 要求他们在使用不连续命名空间的环境中验证其应用程序是否支持这些应用程序。  
  
2.  在非连续命名空间实验室环境中测试所有版本的操作系统和应用程序。 当你执行此操作时，请遵循以下建议：  
  
    1.  在将软件部署到环境之前，请解决所有软件问题。  
  
    2.  如果可能，请参与你计划在非连续命名空间中部署的操作系统和应用程序的 beta 测试。  
  
3.  确保管理员和支持人员知道不相交的命名空间及其影响。  
  
4.  如果需要，请创建一个计划，使你能够从非连续命名空间转换为连续命名空间。  
  
5.  宣传与操作系统和应用程序提供程序的非连续命名空间支持的重要性。  
  


