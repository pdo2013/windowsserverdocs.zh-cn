---
title: 服务器证书部署概述
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4650c99325def63fd0df25989c661230fada3dac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-overview"></a>服务器证书部署概述

>适用于：Windows Server（半年通道），Windows Server 2016

本主题包含以下部分。  
  
-   [服务器证书部署组件](#bkmk_components)
  
-   [服务器证书部署过程概述](#bkmk_process)
  
## <a name="bkmk_components"></a>服务器证书部署组件
你可以使用本指南，作为企业根证书颁发机构 （加拿大） 安装 Active Directory 证书 （广告客户服务） 服务，并注册 server 证书到网络策略 Server (NPS)、 路由和远程访问服务 (RRAS)，或 NPS 和 RRAS 正在运行的服务器。


如果你使用证书基于身份验证部署 SDN，使用服务器证书，以便他们实现安全通信到其他服务器证明其身份需要服务器。
  
下图显示所需部署到你 SDN 基础结构的服务器服务器证书组件。
  
![服务器证书部署所需的基础结构](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> 在上图中，多个服务器所示： DC1、 CA1、 WEB1 和许多 SDN 服务器。 本指南提供如何部署和配置 CA1 和 WEB1，以及如何配置 DC1，本指南假设你已安装在你的网络上的说明进行操作。 如果你尚未安装 Active Directory 域，你可以通过使用执行操作[Core 网络指南](https://technet.microsoft.com/library/mt604042.aspx)为 Windows Server 2016。  
  
在上图中显示每个项目的详细信息，请参阅：  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 运行广告客户服务服务器角色  
在此情况下，企业根证书颁发机构） 也是颁发。 CA 问题到服务器的计算机有权注册证书正确的安全证书。 上 CA1 安装 active Directory 证书服务 （广告客户服务）。  
  
对于更大的网络或安全问题，提供对齐，可以单独根 CA 和颁发的角色和部署颁发的 Ca 的附属 Ca。  
  
在最安全的部署，离线状态并且物理上安全拍摄企业根 CA。   
  
#### <a name="capolicyinf"></a>CAPolicy.inf  
安装广告客户服务之前，你配置 CAPolicy.inf 文件的特定设置为你部署。  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>将复制的**RAS 和 IAS 服务器**证书模板  
服务器证书部署时，你将进行一份**RAS 和 IAS 服务器**证书模板，然后将根据你的要求和相关说明模板配置本指南中。   
  
你利用模板，而不是原始模板一份以便原始模板配置保存供以后使用。 配置的副本**RAS 和 IAS 服务器**模板它，以便 CA 可以创建 server 证书的问题 Active Directory 用户以及指定你的计算机中的组。  
  
#### <a name="additional-ca1-configuration"></a>其他 CA1 配置  
CA 发布计算机必须检查以确保的有效的证书作为证明身份提供给他们的证书，并且未被吊销证书吊销列表 (CRL)。 你必须与 CRL 正确位置配置你 CA，以便计算机知道在哪里可以查找 CRL 身份验证过程。  
  
### <a name="bkmk_web1"></a>WEB1 运行 Web 服务 (IIS) 服务器角色  
在计算机运行的 Web 服务器 (IIS) 服务器角色 WEB1，你必须在用作 CRL 和 AIA 的位置 Windows 资源管理器中中创建一个文件夹。  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>虚拟目录 CRL 和 AIA  
在 Windows 资源管理器中创建一个文件夹后，你必须为中，Internet 信息服务 (IIS) 管理器，以及配置虚拟目录访问控制列表允许计算机那里发布后，请访问 AIA 和 CRL 虚拟目录配置文件夹。  
  
### <a name="bkmk_dc1"></a>DC1 运行广告 DS 和 DNS 服务器角色  
DC1 是域控制器、 DNS 服务器，在你的网络。  
  
#### <a name="group-policy-default-domain-policy"></a>组策略默认域策略  
配置证书模板 CA 上后，你可以在组策略配置默认域策略，以便证书的注册 NPS 和 RAS 的服务器。 组策略中 DC1 服务器上的广告 DS 配置。  
  
#### <a name="dns-alias-cname-resource-record"></a>别名是 (CNAME) 资源记录 DNS  
你必须创建别名 (CNAME) 资源记录的 Web 服务器，以确保服务器，以及 AIA 和存储在服务器 CRL 可以找到的其他计算机。 此外，使用某一别名 CNAME 资源记录提供大的灵活性，以便你可以使用的 Web 服务器用于其他用途，例如举办网站和 FTP 站点。  
  
### <a name="bkmk_nps1"></a>NPS1 运行网络策略和服务的访问权限的服务器角色网络策略服务器角色服务  
当你在 Windows Server 2016 Core 网络指南执行任务，因此你执行任务，本指南中之前，你应该已经拥有一个或多个 NPS 服务器安装在你的网络上安装了 NPS 服务器。  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>组策略应用和到服务器注册证书  
配置证书模板和自动注册后，你可以恢复组策略，在所有目标的服务器上。 在此期间，服务器注册从 CA1 服务器证书。  
  
### <a name="bkmk_process"></a>服务器证书部署过程概述  
  
> [!NOTE]  
> 如何执行以下步骤操作的详细信息部分提供[服务器证书部署](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md)。  
  
在这些阶段发生配置服务器证书的注册的过程：  
  
1.  在 WEB1 上, 安装的 Web 服务器 (IIS) 角色。  
  
2.  DC1 上, 创建别名 (CNAME) 记录的你的 Web 服务器，WEB1。  
  
3.  将配置你的 Web 服务器主机 CRL ca，然后发布 CRL 并复制到新的虚拟目录企业根证书。  
  
4.  打算安装广告客户服务计算机上指定的静态 IP 地址的计算机将计算机重命名、 加入域，计算机，然后登录到计算机的域管理员和企业管理员的组成员的用户帐户。  
  
5.  在计算机上打算安装广告客户服务，为配置 CAPolicy.inf 文件特定于您的部署的设置。  
  
6.  安装广告客户服务服务器角色并执行其他配置 CA。  
  
7.  将 CRL，CA 证书从 CA1 复制到 Web 服务器 WEB1 上共享。  
  
8.  在加拿大配置一份 RAS 和 IAS 服务器证书模板。 CA 问题基于证书模板，因此你必须之前来颁发证书配置为服务器证书模板证书。  
  
9.  将自动服务器证书的注册配置为在组策略中。 自动注册配置时，你已自动指定 Active Directory 组成员的所有服务器将刷新组策略，在每个服务器时都收到服务器证书。 如果你稍后添加详细服务器，他们将自动接收服务器证书，也。  
  
10. 刷新组策略，在服务器上。 刷新时组策略，服务器接收服务器证书，根据你在上一步中配置模板。 该证书可供向客户端计算机证明其身份的服务器和其他服务器的身份验证过程。  
  
    > [!NOTE]  
    > 所有计算机都域成员自动都接收企业根加拿大的证书，而无需注册的配置。 该证书没有不同于服务器证书你配置并使用注册的分配。 CA 证书自动安装所有域成员计算机的受信任的根证书颁发机构证书应用商店，以便它们将信任证书颁发的此 CA。   
  
10. 验证所有服务器都登记了服务器有效证书。  
  


