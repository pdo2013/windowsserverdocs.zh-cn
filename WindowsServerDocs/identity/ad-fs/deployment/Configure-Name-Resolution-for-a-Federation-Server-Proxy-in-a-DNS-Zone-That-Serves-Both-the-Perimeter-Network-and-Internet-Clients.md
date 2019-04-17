---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: "配置名称分辨率的联合在 DNS 服务器代理区域的作为周边这两个网络和 Internet 客户端"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3b154459163b2142ff1d3aba424a86305d093de4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>配置名称分辨率的联合在 DNS 服务器代理区域的作为周边这两个网络和 Internet 客户端

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

以便名称分辨率可以在其中一个或多个域名系统 \(DNS\) 区域提供外围网络和 Internet 客户端 Active Directory 联合身份验证服务 \(AD FS\) 方案联合服务器代理成功工作，必须先完成以下任务：  
  
-   控制你的 Internet 区域中的 DNS 必须配置解决广告 FS 的所有 Internet 射都主机联合身份验证的服务器代理为的名称。 若要完成此操作，你将添加主机 \(A\) 资源记录到 Internet DNS 区域联合身份验证的服务器代理服务器。  
  
-   若要解决有关广告 FS 的所有客户端请求主机联合身份验证的服务器的名称，必须配置外围网络中的 DNS。 若要完成此操作，你添加主机 \(A\) 资源记录到外围 DNS 区域联合身份验证的服务器代理服务器。  
  
> [!NOTE]  
> 假定企业网络 DNS 中已经创建主机 \(A\) 资源记录联合身份验证的服务器。 如果此记录尚不存在，创建此记录并再执行这些过程。 有关如何创建适用于联盟 server 主机 \(A\) 资源记录的详细信息，请参阅[添加主机 & #40;一个 & #41;资源录制到公司 DNS 服务器是否联盟](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>添加到 Internet DNS 区域的主 \(A\) 资源记录联合身份验证的服务器代理服务器  
以便通过新部署联合身份验证的服务器代理服务器，在 Internet 上的客户端计算机成功可以访问联盟服务器，必须首先控制你的 Internet DNS 区域创建主机 \(A\) 资源记录。 此资源记录解决了主机名称帐户联盟服务器 \ (例如，fs.fabrikam.com\) 的帐户联合身份验证的服务器代理的 IP 地址 \ (例如，131.107.27.68\) 外围网络中。  
  
> [!NOTE]  
> 假设你正在使用 DNS 服务器 DNS 服务器服务与运行 Windows 2000 Server、 Windows Server 2003 或 Windows Server 2008 控制 Internet DNS 区域。  
  
在会员**管理员**，或等效的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](http://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>若要添加到 Internet DNS 区域的主 \(A\) 资源记录联合身份验证的服务器代理服务器  
  
1.  在 Internet DNS 区域 DNS 服务器上，打开 snap\ 中 DNS。  
  
2.  在该控制台树，right\ 单击相应的前进查找区域中，，然后单击**新主机 \(A or AAAA\)**。  
  
3.  在**名称**，键入联合服务器的计算机名称。 例如，对于完全限定的域名称 \(FQDN\) fs.fabrikam.com 中，键入**fs**。  
  
4.  在**IP 地址**，新联合服务器代理，例如，131.107.27.68 的键入 IP 地址。  
  
5.  单击**添加主机**。  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>添加到外围 DNS 区域的主 \(A\) 资源记录联合身份验证的服务器代理服务器  
以便 Internet 客户端请求联合身份验证的服务器代理可以成功处理，并且联系联合身份验证的服务器 Internet DNS 区域解析它们后，你必须在外围 DNS 区域创建主机 \(A\) 资源记录。 此资源记录解决了主机名称帐户联盟服务器 \ (例如，fs。 fabrikam.com\) 的帐户联合身份验证的服务器的 IP 地址 \ (例如，192.168.1.4\) 在公司的网络。  
  
> [!NOTE]  
> 假设你使用 DNS 服务器 DNS 服务器服务与运行 Windows 2000 Server、 Windows Server 2003、 Windows Server 2008 或 Windows Server 2012 的® 控制外围 DNS 区域。  
  
在会员**管理员**，或等效的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](http://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>若要添加到外围 DNS 区域的主 \(A\) 资源记录联合身份验证的服务器代理服务器  
  
1.  周边网络 DNS 服务器，在打开**snap\ 中的 DNS**。  
  
2.  在该控制台树，right\ 单击相应的前进查找区域中，，然后单击**新主机 \(A or AAAA\)**。  
  
3.  在**名称**，键入联合服务器的计算机名称。 例如，对于 FQDN fs.fabrikam.com 中，键入**fs**。  
  
4.  在**IP 地址**文本框中，键入 IP 地址的联合服务器在公司的网络，例如，192.168.1.4。  
  
5.  单击**添加主机**。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合身份验证的服务器代理服务器设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[联合身份验证的服务器代理名称分辨率要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

