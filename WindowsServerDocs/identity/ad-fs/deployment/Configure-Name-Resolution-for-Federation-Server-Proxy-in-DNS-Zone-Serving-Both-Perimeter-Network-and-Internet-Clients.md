---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: 在为外围网络和 Internet 客户端提供服务的 DNS 区域中为联合服务器代理配置名称解析
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cef87e725db7068ac4ed93524e09a25de95ec276
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828511"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>在为外围网络和 Internet 客户端提供服务的 DNS 区域中为联合服务器代理配置名称解析


以便名称解析可成功用于在 Active Directory 联合身份验证服务中的联合身份验证服务器代理\(AD FS\)的一个或多个域名系统中的方案\(DNS\)区域为同时提供服务外围网络和 Internet 客户端，必须完成以下任务：  
  
-   必须配置 DNS 您控制的 Internet 区域中，若要解决的 AD FS 的所有 Internet 客户端请求都主机名到联合服务器代理。 若要实现此目的，在添加主机\(A\)到联合服务器代理在 Internet DNS 区域资源记录。  
  
-   必须配置外围网络中的 DNS 解析所有传入的客户端请求适用于 AD FS 托管到联合身份验证服务器的名称。 若要实现此目的，在添加主机\(A\)到联合服务器代理在外围 DNS 区域资源记录。  
  
> [!NOTE]  
> 假定主机\(A\)资源记录的联合身份验证服务器已创建在公司网络 DNS。 如果此记录尚不存在，创建此记录，然后执行这些过程。 有关如何创建主机的详细信息\(A\)资源记录，用于联合身份验证服务器，请参阅[将主机添加&#40;A&#41;公司 dns 中为联合身份验证服务器的资源记录](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>添加主机\(A\)到联合服务器代理在 Internet DNS 区域资源记录  
这样，在 Internet 上的客户端计算机成功可以通过新部署的联合服务器代理访问的联合身份验证服务器，必须首先创建主机\(A\)您控制的 Internet DNS 区域中的资源记录。 此资源记录的帐户联合身份验证服务器的主机名解析\(例如，fs.fabrikam.com\)帐户联合服务器代理的 IP 地址\(等 131.107.27.68\)中外围网络。  
  
> [!NOTE]  
> 假定您使用 DNS 服务器服务与运行 Windows 2000 Server、 Windows Server 2003 或 Windows Server 2008 的 DNS 服务器来控制 Internet DNS 区域。  
  
中的成员身份**管理员**，或等效身份是完成此过程所需的最小。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>若要将主机添加\(A\)到联合服务器代理在 Internet DNS 区域资源记录  
  
1.  在 Internet DNS 区域的 DNS 服务器，打开 DNS 管理单元\-中。  
  
2.  在控制台树中，右键\-单击适用的正向查找区域，然后单击**新的主机\(A 或 AAAA\)** 。  
  
3.  在中**名称**，键入联合身份验证服务器的计算机名称。 例如，对于完全限定的域名\(FQDN\) fs.fabrikam.com，键入**fs**。  
  
4.  在中**IP 地址**，为新联合服务器代理，例如，131.107.27.68 键入 IP 地址。  
  
5.  单击“添加主机”  。  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>添加主机\(A\)到联合服务器代理在外围 DNS 区域资源记录  
以便 Internet 客户端请求可以成功处理的联合服务器代理并连接到联合身份验证服务器，它们解决由 Internet DNS 区域后，必须创建主机\(A\)外围网络中的资源记录DNS 区域。 此资源记录的帐户联合身份验证服务器的主机名解析\(例如，fs。 fabrikam.com\)到帐户联合身份验证服务器的 IP 地址\(，例如 192.168.1.4\)企业网络中。  
  
> [!NOTE]  
> 假设使用运行 Windows 2000 Server、 Windows Server 2003、 Windows Server 2008 或 Windows Server® 2012年与 DNS 服务器服务的 DNS 服务器的目的控制外围 DNS 区域。  
  
中的成员身份**管理员**，或等效身份是完成此过程所需的最小。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>若要将主机添加\(A\)到联合服务器代理在外围 DNS 区域资源记录  
  
1.  在外围网络的 DNS 服务器，打开**DNS 管理单元\-中**。  
  
2.  在控制台树中，右键\-单击适用的正向查找区域，然后单击**新的主机\(A 或 AAAA\)** 。  
  
3.  在中**名称**，键入联合身份验证服务器的计算机名称。 例如，对于 FQDN fs.fabrikam.com，键入 **fs**。  
  
4.  在中**IP 地址**文本框中，键入 IP 地址的企业网络中联合身份验证服务器，如 192.168.1.4。  
  
5.  单击“添加主机”  。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[联合服务器代理的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

