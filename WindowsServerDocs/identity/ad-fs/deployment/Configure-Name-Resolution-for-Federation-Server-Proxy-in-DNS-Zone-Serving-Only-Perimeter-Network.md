---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: 在仅为外围网络提供服务的 DNS 区域中为联合服务器代理配置名称解析
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de4627f2e03e6432f4e678cd9ca932819cb483d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408430"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>在仅为外围网络提供服务的 DNS 区域中为联合服务器代理配置名称解析


因此，名称解析可以在 Active Directory 联合身份验证服务 \(AD FS @ no__t 方案中成功地运行联合服务器，其中一个或多个域名系统 \(DNS @ no__t 区域仅为外围网络提供服务，以下必须完成任务：  
  
-   必须更新联合服务器代理上的 hosts 文件，才能添加联合服务器的 IP 地址。  
  
-   外围网络中的 DNS 必须配置为将 AD FS 主机名的所有客户端请求解析为联合服务器代理。 为此，请为联合服务器代理向外围 DNS 添加主机 @no__t 0A @ no__t 资源记录。  
  
> [!NOTE]  
> 这些过程假定已在公司网络 DNS 中创建联合服务器的主机 @no__t 0A @ no__t 资源记录。 如果此记录尚不存在，请创建此记录，然后执行这些过程。 有关如何为联合服务器创建主机 \(A @ no__t 资源记录的详细信息，请参阅[向联合服务器的企业 DNS 添加&#40;主机&#41; a 资源记录](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>将联合服务器的 IP 地址添加到 hosts 文件  
这样，联合服务器代理在帐户伙伴的外围网络中可以按预期方式工作。必须在该联合服务器代理上的 hosts 文件中添加一个条目，该服务器指向联合服务器的 DNS 主机名 \(for 示例，no__t-2for 示例，在帐户伙伴的企业网络中，192.168.1.4 @ no__t。 @no__t 如果将此条目添加到 hosts 文件中，联合服务器代理将无法与自身联系以解析对帐户伙伴中的联合服务器的客户端 @ no__t-0initiated 调用。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>将联合服务器的 IP 地址添加到 hosts 文件  
  
1.  导航到% systemroot% \\Winnt @ no__t-1System32 @ no__t-2Drivers directory 文件夹，并找到**hosts**文件。  
  
2.  启动记事本，然后打开“主机”文件。  
  
3.  将帐户伙伴中的联合服务器的 IP 地址和主机名添加到**hosts**文件中，如以下示例中所示：  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  保存并关闭该文件。  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>为联合服务器代理向外围 DNS 添加主机 @no__t 0A @ no__t 资源记录  
为了使 Internet 上的客户端可以通过新部署的联合服务器代理成功访问联合服务器，你必须首先在外围 DNS 中创建主机 @no__t 0A @ no__t 资源记录。 此资源记录将帐户联合服务器 @no__t 的主机名（0for） no__t-1 解析为帐户联合服务器 @no__t 代理的 IP 地址（在外围网络中 131.107.27.68 @ no__t-3）。  
  
> [!NOTE]  
> 假设你正在使用运行 Windows 2000 Server、Windows Server 2003 或 Windows Server 2008 和 DNS 服务器服务的 DNS 服务器来控制外围 DNS 区域。  
  
**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>为联合服务器代理向外围 DNS 添加主机 \(A @ no__t 资源记录  
  
1.  在外围网络的 DNS 服务器上，打开 DNS snap @ no__t-0in。 单击 "**开始**"，指向 "**管理工具**"，然后单击 " **DNS**"。  
  
2.  在控制台树中，右键 @ no__t-0click 适用的正向查找区域，然后单击 "**新建主机 \(a 或 AAAA @ no__t**"。  
  
3.  在 "**名称**" 中，仅键入联合服务器的计算机名称。 例如，对于完全限定的域名 \(FQDN @ no__t-1 fs.fabrikam.com，请键入**fs**。  
  
4.  在 " **IP 地址**" 中，键入新联合服务器代理的 IP 地址，例如**131.107.27.68**。  
  
5.  单击“添加主机”。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[联合服务器代理的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

