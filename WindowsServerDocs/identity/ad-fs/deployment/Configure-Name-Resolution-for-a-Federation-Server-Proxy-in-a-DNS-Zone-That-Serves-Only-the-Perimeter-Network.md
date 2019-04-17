---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: "为提供仅外围网络 DNS 区域中联合服务器代理配置名称分辨率"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b8e3cc133ce95872881115608bb8cfb17b2427
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>为提供仅外围网络 DNS 区域中联合服务器代理配置名称分辨率

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

以便名称分辨率可以成功适用于在其中一个或多个域名系统 \(DNS\) 区域提供仅外围网络 Active Directory 联合身份验证服务 \(AD FS\) 方案中安装了联盟服务器，必须先完成以下任务：  
  
-   在联合身份验证的服务器代理主机文件必须先更新，添加联盟服务器的 IP 地址。  
  
-   必须配置外围网络中的 DNS 解决射广告 fs 主机联合身份验证的服务器代理为的名称。 若要执行此操作，你将添加主机 \(A\) 资源记录到周边 DNS 联合身份验证的服务器代理服务器。  
  
> [!NOTE]  
> 这些步骤假定企业网络 DNS 中已经创建主机 \(A\) 资源记录联合身份验证的服务器。 如果此记录尚不存在，创建此记录，并再执行这些过程。 有关如何创建适用于联盟 server 主机 \(A\) 资源记录的详细信息，请参阅[添加主机 & #40;一个 & #41;资源录制到公司 DNS 服务器是否联盟](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>添加到主机文件联合服务器的 IP 地址  
以便联合身份验证的服务器代理可按预期在外围帐户合作伙伴的网络，必须将一项添加到主机文件在指向联合服务器 DNS 主机名该联盟服务器代理 \ (例如，fs.fabrikam.com\) 和 IP 地址 \ (例如，192.168.1.4\) 中的帐户的合作伙伴公司的网络。 将此项目添加到主机文件阻止联合身份验证的服务器代理联系自身解决 client\ 启动到中帐户合作伙伴联合身份验证的服务器通话。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>若要添加到主机文件联合服务器的 IP 地址  
  
1.  导航到 %systemroot%\\Winnt\\System32\\Drivers 目录文件夹并找到**主机**文件。  
  
2.  启动记事本，然后打开**主机**文件。  
  
3.  在帐户合作伙伴中添加的 IP 地址和联盟服务器的主机名**主机**文件，如以下示例所示：  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  保存并关闭该文件。  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>添加周边 DNS 主机 \(A\) 资源记录联合身份验证的服务器代理服务器  
以便通过新部署联合身份验证的服务器代理服务器，在 Internet 上的客户端成功可以访问联盟服务器，首先必须在外围 DNS 创建主机 \(A\) 资源记录。 此资源记录解决了主机名称帐户联盟服务器 \ (例如，fs.fabrikam.com\) 的帐户联合身份验证的服务器代理的 IP 地址 \ (例如，131.107.27.68\) 外围网络中。  
  
> [!NOTE]  
> 假设你正在使用 DNS 服务器，DNS 服务器服务中，运行 Windows 2000 Server、 Windows Server 2003 或 Windows Server 2008 控制外围 DNS 区域。  
  
在会员**管理员**，或等效的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>可以为联合身份验证的服务器代理外围 DNS 添加主机 \(A\) 资源记录  
  
1.  周边网络 DNS 服务器，在打开 snap\ 中 DNS。 单击**开始**，指向**管理工具**，然后单击**DNS**。  
  
2.  在该控制台树，right\ 单击相应的前进查找区域中，，然后单击**新主机 \(A or AAAA\)**。  
  
3.  在**名称**，键入联合服务器的计算机名称。 例如，对于完全限定的域名称 \(FQDN\) fs.fabrikam.com 中，键入**fs**。  
  
4.  在**IP 地址**，新联合身份验证的服务器代理，例如，键入 IP 地址**131.107.27.68**。  
  
5.  单击**添加主机**。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合身份验证的服务器代理服务器设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[联合身份验证的服务器代理名称分辨率要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

