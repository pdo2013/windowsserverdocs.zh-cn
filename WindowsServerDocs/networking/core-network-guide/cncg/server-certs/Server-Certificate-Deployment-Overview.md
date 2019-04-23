---
title: 服务器证书部署概述
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0cafa4bdeb80b22d6bac4ad09bcae9436cda97c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877818"
---
# <a name="server-certificate-deployment-overview"></a>服务器证书部署概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题包含以下部分。  
  
-   [服务器证书部署组件](#bkmk_components)
  
-   [服务器证书部署过程概述](#bkmk_process)
  
## <a name="bkmk_components"></a>服务器证书部署组件
若要为企业根证书颁发机构 (CA) 安装 Active Directory 证书服务 (AD CS)，并注册到运行网络策略服务器 (NPS)、 路由和远程访问服务 (RRAS) 服务器的服务器证书，可以使用本指南或同时 NPS 和 RRAS。


如果使用基于证书的身份验证部署 SDN，服务器需要使用服务器证书，以便它们实现安全通信到其他服务器证明其身份。
  
下图显示了将服务器证书部署到 SDN 基础结构中的服务器所需的组件。
  
![服务器证书部署所需基础结构](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> 上图中绘出多个服务器：DC1、 CA1、 WEB1 和许多 SDN 服务器。 本指南提供有关部署和配置 CA1 和 WEB1，以及配置 DC1，本指南假定你已安装在网络上的说明。 如果尚未安装 Active Directory 域，则可以这样通过使用[核心网络指南](https://technet.microsoft.com/library/mt604042.aspx)Windows Server 2016。  
  
上图中所示的每个项的详细信息，请参阅：  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 运行 AD CS 服务器角色  
在此方案中，企业根证书颁发机构 (CA) 也是颁发 CA。 在 CA 颁发到具有正确的安全权限才能注册证书的服务器计算机证书。 CA1 上安装 active Directory 证书服务 (AD CS)。  
  
对于较大的网络或安全问题在其中提供理由，您可以单独的根 CA 和颁发 CA 角色并部署颁发 Ca 的从属 Ca。  
  
在最安全的部署中，离线和物理上安全执行企业根 CA。   
  
#### <a name="capolicyinf"></a>CAPolicy.inf  
安装 AD CS 之前，您配置 CAPolicy.inf 文件使用特定的设置为你的部署。  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>副本**RAS 和 IAS 服务器**证书模板  
在部署服务器证书时，制作一份**RAS 和 IAS 服务器**证书模板，然后配置本指南中的根据你的要求和说明的模板。   
  
您利用模板而不是原始模板的副本，以便为将来可能使用保留的原始模板配置。 配置的副本**RAS 和 IAS 服务器**模板，让 CA 能够创建服务器证书的颁发给 Active Directory 用户和你指定的计算机中的组。  
  
#### <a name="additional-ca1-configuration"></a>其他 CA1 配置  
CA 将发布证书吊销列表 (CRL) 的计算机必须检查以确保作为身份证明提供给他们的证书是有效的证书，并没有被吊销。 必须使用 CRL 的正确位置配置您的 CA，以便计算机知道在哪里查找 CRL 身份验证过程。  
  
### <a name="bkmk_web1"></a>WEB1 运行 Web Services (IIS) 服务器角色  
运行 Web 服务器 (IIS) 服务器角色，WEB1，在计算机上您必须在 CRL 和 AIA 位置用作在 Windows 资源管理器中创建一个文件夹。  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>有关 CRL 和 AIA 的虚拟目录  
在 Windows 资源管理器中创建一个文件夹后，必须将该文件夹配置为在 Internet 信息服务 (IIS) 管理器中，以及配置虚拟目录的访问控制列表以允许计算机访问 AIA 和 CRL 中的虚拟目录在那里发布这些。  
  
### <a name="bkmk_dc1"></a>DC1 运行 AD DS 和 DNS 服务器角色  
DC1 指域控制器和网络上的 DNS 服务器。  
  
#### <a name="group-policy-default-domain-policy"></a>组策略默认域策略  
在 CA 上配置证书模板后，你可以在组策略中配置默认域策略，以便证书会自动注册到 NPS 和 RAS 服务器。 在 DC1 的服务器上的 AD DS 中配置组策略。  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS 别名 (CNAME) 资源记录  
必须创建 Web 服务器，以确保其他计算机可以找到服务器，以及 AIA 和 CRL 的服务器上存储的别名 (CNAME) 资源记录。 此外，使用别名的 CNAME 资源记录提供了灵活性，以便可将 Web 服务器用于其他目的，例如，托管 Web 和 FTP 站点。  
  
### <a name="bkmk_nps1"></a>NPS1 运行网络策略和访问服务服务器角色的网络策略服务器角色服务  
在 Windows Server 2016 核心网络指南中执行任务时安装了 NPS，因此在本指南中执行任务之前，应该已拥有一个或多个 NPSs 安装在网络上。  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>应用组策略和证书注册到服务器  
配置证书模板和自动注册后，你可以在所有目标服务器上刷新组策略。 在此期间，服务器注册来自 CA1 的服务器证书。  
  
### <a name="bkmk_process"></a>服务器证书部署过程概述  
  
> [!NOTE]  
> 节中提供了如何执行这些步骤的详细信息[服务器证书部署](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md)。  
  
在这些阶段中发生的配置服务器证书注册过程：  
  
1.  在 WEB1 上，安装 Web 服务器 (IIS) 角色。  
  
2.  在 DC1 上，创建你的 Web 服务器 WEB1 的别名 (CNAME) 记录。  
  
3.  配置 Web 服务器承载从 CA CRL 然后发布 CRL 并将企业根 CA 证书复制到新的虚拟目录。  
  
4.  打算安装 AD CS 的计算机，将计算机分配静态 IP 地址、 重命名计算机，将计算机加入到域，然后登录到与用户帐户是 Domain Admins 和 Enterprise Admins 组的成员计算机上。  
  
5.  打算安装 AD CS 的计算机，使用特定于你的部署的设置中配置 CAPolicy.inf 文件。  
  
6.  安装 AD CS 服务器角色并执行其他配置的 ca。  
  
7.  将 CRL 和 CA 证书从 CA1 复制到 Web 服务器 WEB1 上的共享。  
  
8.  在 CA 上配置 RAS 和 IAS 服务器证书模板的副本。 在 CA 颁发基于证书模板，因此 CA 可以颁发证书之前，必须配置为服务器证书模板的证书。  
  
9.  在组策略中配置服务器证书自动注册。 在配置自动注册时，自动指定具有 Active Directory 组成员身份的所有服务器都收到服务器证书，每个服务器上的组策略刷新时。 如果以后添加更多的服务器，它们会自动将太收到服务器证书。  
  
10. 在服务器上刷新组策略。 刷新组策略时，服务器将收到服务器证书，基于你在上一步中配置的模板。 身份验证过程中，如果要向客户端计算机证明其身份的服务器和其他服务器使用此证书。  
  
    > [!NOTE]  
    > 所有域成员计算机自动都收到无需进行的自动注册配置企业根 CA 的证书。 此证书与不同配置和使用自动注册的分发服务器证书。 CA 的证书自动安装所有域成员计算机的受信任的根证书颁发机构证书存储，以便他们将信任此 CA 颁发的证书。   
  
10. 验证所有服务器已都注册有效的服务器证书。  
  


