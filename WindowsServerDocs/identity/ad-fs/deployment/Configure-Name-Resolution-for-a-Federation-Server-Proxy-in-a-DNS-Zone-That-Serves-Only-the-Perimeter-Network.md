---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: 在仅为外围网络提供服务的 DNS 区域中为联合服务器代理配置名称解析
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7d046c720c5c6250b6efa03e068aa66e2a6bbe3d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192305"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>在仅为外围网络提供服务的 DNS 区域中为联合服务器代理配置名称解析


这样的名称解析成功适用于 Active Directory 联合身份验证服务中的联合身份验证服务器\(AD FS\)的一个或多个域名系统中的方案\(DNS\)区域提供仅外围网络，以下必须完成任务：  
  
-   若要添加的联合身份验证服务器的 IP 地址，必须更新联合服务器代理上的 hosts 文件。  
  
-   若要解决的 AD FS 的所有客户端请求主机名到联合服务器代理，必须配置外围网络中的 DNS。 若要执行此操作，添加主机\(A\)到联合服务器代理的外围网络 DNS 资源记录。  
  
> [!NOTE]  
> 这些过程假设主机\(A\)资源记录的联合身份验证服务器已创建在公司网络 DNS。 如果此记录尚不存在，创建此记录，然后执行这些过程。 有关如何创建主机的详细信息\(A\)资源记录，用于联合身份验证服务器，请参阅[将主机添加&#40;A&#41;公司 dns 中为联合身份验证服务器的资源记录](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>将联合身份验证服务器的 IP 地址添加到主机文件  
以便联合服务器代理能够正常地在帐户伙伴的外围网络中，必须将条目添加到联合身份验证服务器的 DNS 主机名指向该联合服务器代理上的 hosts 文件\(例如 fs.fabrikam.com\)和 IP 地址\(，例如 192.168.1.4\)在帐户伙伴的企业网络中。 将此项添加到主机文件可防止联合服务器代理本身来解决客户端联系\-启动到在帐户伙伴中联合身份验证服务器的调用。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>若要将联合身份验证服务器的 IP 地址添加到主机文件  
  
1.  导航到 %systemroot%\\Winnt\\System32\\驱动程序目录文件夹并找到**主机**文件。  
  
2.  启动记事本，然后打开“主机”  文件。  
  
3.  添加到帐户伙伴中的 IP 地址和联合身份验证服务器的主机名**主机**文件，在下面的示例所示：  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  保存并关闭该文件。  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>添加主机\(A\)到联合服务器代理的外围网络 DNS 资源记录  
这样，在 Internet 上的客户端成功可以通过新部署的联合服务器代理访问的联合身份验证服务器，必须先创建一个主机\(A\)外围 DNS 中的资源记录。 此资源记录的帐户联合身份验证服务器的主机名解析\(例如，fs.fabrikam.com\)帐户联合服务器代理的 IP 地址\(等 131.107.27.68\)中外围网络。  
  
> [!NOTE]  
> 假定，您使用的 DNS 服务器，使用 DNS 服务器服务时，运行 Windows 2000 Server、 Windows Server 2003 或 Windows Server 2008 来控制外围 DNS 区域。  
  
中的成员身份**管理员**，或等效身份是完成此过程所需的最小。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>若要将主机添加\(A\)到联合服务器代理的外围网络 DNS 资源记录  
  
1.  在外围网络的 DNS 服务器，打开 DNS 管理单元\-中。 单击**启动**，依次指向**管理工具**，然后单击**DNS**。  
  
2.  在控制台树中，右键\-单击适用的正向查找区域，然后单击**新的主机\(A 或 AAAA\)** 。  
  
3.  在中**名称**，键入联合身份验证服务器的计算机名称。 例如，对于完全限定的域名\(FQDN\) fs.fabrikam.com，键入**fs**。  
  
4.  在中**IP 地址**，为新的联合服务器代理，例如，键入的 IP 地址**131.107.27.68**。  
  
5.  单击“添加主机”  。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[联合服务器代理的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

