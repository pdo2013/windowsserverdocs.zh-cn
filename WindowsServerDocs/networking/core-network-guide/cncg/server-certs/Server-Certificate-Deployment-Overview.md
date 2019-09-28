---
title: 服务器证书部署概述
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d4b713437f031e4a381d2759bdcbf7f41bd573d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406344"
---
# <a name="server-certificate-deployment-overview"></a>服务器证书部署概述

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题包含以下部分。  
  
-   [服务器证书部署组件](#bkmk_components)
  
-   [服务器证书部署过程概述](#bkmk_process)
  
## <a name="bkmk_components"></a>服务器证书部署组件
你可以使用本指南将 Active Directory 证书服务（AD CS）安装为企业根证书颁发机构（CA），并向运行网络策略服务器（NPS）、路由和远程访问服务（RRAS）的服务器注册服务器证书。NPS 和 RRAS。


如果你使用基于证书的身份验证部署 SDN，则服务器需要使用服务器证书向其他服务器证明其身份，以便它们可以实现安全的通信。
  
下图显示了将服务器证书部署到 SDN 基础结构中的服务器所需的组件。
  
![服务器证书部署所需的基础结构](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> 在上图中，将描述多个服务器：DC1、CA1、WEB1 和多个 SDN 服务器。 本指南提供了有关部署和配置 CA1 和 WEB1 的说明，以及用于配置 DC1 的说明，本指南假定你已在网络上安装了。 如果尚未安装 Active Directory 域，则可以使用 Windows Server 2016 的[核心网络指南](https://technet.microsoft.com/library/mt604042.aspx)来完成此操作。  
  
有关上图所示的每个项的详细信息，请参阅以下内容：  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>运行 AD CS 服务器角色的 CA1  
在此方案中，企业根证书颁发机构（CA）也是颁发 CA。 CA 向具有正确安全权限的服务器计算机颁发证书以注册证书。 Active Directory 证书服务（AD CS）安装在 CA1 上。  
  
对于较大的网络或在安全问题方面提供理由的地方，你可以分离根 CA 和颁发 CA 的角色，并部署颁发 Ca 的从属 Ca。  
  
在最安全的部署中，企业根 CA 处于脱机状态并且受到物理保护。   
  
#### <a name="capolicyinf"></a>Capolicy.inf .inf  
在安装 AD CS 之前，请为你的部署配置具有特定设置的 Capolicy.inf 文件。  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>**RAS 和 IAS 服务器**证书模板的副本  
部署服务器证书时，你将创建一个**RAS 和 IAS 服务器**证书模板的副本，然后根据你的要求以及本指南中的说明配置模板。   
  
使用模板的副本而不是原始模板，以便保留原始模板的配置以供将来使用。 你可以配置 " **RAS 和 IAS 服务器**" 模板的副本，以便 CA 可以创建服务器证书，该证书会向你指定 Active Directory 用户和计算机中的组颁发。  
  
#### <a name="additional-ca1-configuration"></a>其他 CA1 配置  
CA 发布证书吊销列表（CRL），计算机必须进行检查以确保向身份验证提供的证书为有效证书，并且尚未吊销。 你必须配置 CA 的 CRL 正确位置，以便计算机知道在身份验证过程中查找 CRL 的位置。  
  
### <a name="bkmk_web1"></a>运行 Web 服务（IIS）服务器角色的 WEB1  
在运行 Web 服务器（IIS）服务器角色 WEB1 的计算机上，必须在 Windows 资源管理器中创建一个文件夹，将其用作 CRL 和 AIA 的位置。  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>CRL 和 AIA 的虚拟目录  
在 Windows 资源管理器中创建文件夹之后，必须将该文件夹配置为 Internet Information Services （IIS）管理器中的虚拟目录，并配置该虚拟目录的访问控制列表，使计算机能够访问 AIA 和 CRL将其发布到该处。  
  
### <a name="bkmk_dc1"></a>DC1 运行 AD DS 和 DNS 服务器角色  
DC1 是网络上的域控制器和 DNS 服务器。  
  
#### <a name="group-policy-default-domain-policy"></a>组策略默认域策略  
在 CA 上配置证书模板之后，可以在组策略中配置默认域策略，以便将证书自动注册到 NPS 和 RAS 服务器。 在服务器 DC1 上的 AD DS 中配置组策略。  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS 别名（CNAME）资源记录  
您必须为 Web 服务器创建别名（CNAME）资源记录，以确保其他计算机可以找到服务器以及服务器上存储的 AIA 和 CRL。 此外，使用别名 CNAME 资源记录可提供灵活性，以便可以将 Web 服务器用于其他目的，例如托管 Web 和 FTP 站点。  
  
### <a name="bkmk_nps1"></a>NPS1 运行网络策略和访问服务服务器角色的网络策略服务器角色服务  
当你执行 Windows Server 2016 Core 网络指南中的任务时，将安装 NPS，因此，在执行本指南中的任务之前，你应该已在网络上安装了一个或多个 NPSs。  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>应用组策略并向服务器注册证书  
配置证书模板和自动注册后，可以在所有目标服务器上刷新组策略。 此时，服务器将从 CA1 注册服务器证书。  
  
### <a name="bkmk_process"></a>服务器证书部署过程概述  
  
> [!NOTE]  
> [服务器证书部署](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md)部分提供了有关如何执行这些步骤的详细信息。  
  
配置服务器证书注册的过程在以下阶段发生：  
  
1.  在 WEB1 上，安装 Web 服务器（IIS）角色。  
  
2.  在 DC1 上，为 Web 服务器创建一个别名（CNAME）记录，WEB1。  
  
3.  将 Web 服务器配置为托管 CA 颁发的 CRL，然后发布 CRL，并将企业根 CA 证书复制到新的虚拟目录中。  
  
4.  在计划安装 AD CS 的计算机上，为计算机分配静态 IP 地址，重命名计算机，将计算机加入域，然后使用属于 Domain Admins 和 Enterprise Admins 组成员的用户帐户登录计算机。  
  
5.  在计划安装 AD CS 的计算机上，为 Capolicy.inf 文件配置特定于你的部署的设置。  
  
6.  安装 AD CS 服务器角色并执行 CA 的其他配置。  
  
7.  将 CRL 和 CA 证书从 CA1 复制到 Web 服务器 WEB1 上的共享中。  
  
8.  在 CA 上，配置 RAS 和 IAS 服务器证书模板的副本。 CA 基于证书模板颁发证书，因此你必须配置服务器证书的模板，然后 CA 才能颁发证书。  
  
9.  在组策略中配置服务器证书自动注册。 配置自动注册时，在刷新每个服务器上的组策略时，使用 Active Directory 组成员身份指定的所有服务器将自动接收服务器证书。 如果稍后添加更多服务器，它们也会自动接收服务器证书。  
  
10. 刷新服务器上的组策略。 刷新组策略时，服务器将接收服务器证书，该证书基于你在上一步中配置的模板。 此证书由服务器用于在身份验证过程中向客户端计算机和其他服务器证明其身份。  
  
    > [!NOTE]  
    > 所有域成员计算机自动接收企业根 CA 的证书，而无需配置自动注册。 此证书不同于使用自动注册配置和分发的服务器证书。 CA 的证书会自动安装在所有域成员计算机的 "受信任的根证书颁发机构" 证书存储中，以便它们信任此 CA 颁发的证书。   
  
10. 验证所有服务器是否都注册了有效的服务器证书。  
  


