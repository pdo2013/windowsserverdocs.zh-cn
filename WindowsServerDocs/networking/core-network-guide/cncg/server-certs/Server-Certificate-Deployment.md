---
title: 服务器证书部署
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2105e2ccad69fcfdbc13e3201b4362e5c6b932e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406278"
---
# <a name="server-certificate-deployment"></a>服务器证书部署

>适用于：Windows Server（半年频道）、Windows Server 2016

按照以下步骤安装企业根证书颁发机构（CA），并部署服务器证书以便与 PEAP 和 EAP 一起使用。  
  
> [!IMPORTANT]  
> 安装 Active Directory 证书服务之前，必须为计算机命名，为计算机配置静态 IP 地址，并将计算机加入域。 安装 AD CS 后，不能更改计算机的计算机名或域成员身份，但可以根据需要更改 IP 地址。 有关如何完成这些任务的详细信息，请参阅 Windows Server @ no__t-0 2016 [Core 网络指南](../../Core-Network-Guide.md)。  

  
-   [安装 Web 服务器 WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [在 DNS 中为 WEB1 创建别名 (CNAME) 记录](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [配置 WEB1 以分发证书吊销列表（Crl）](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [准备 Capolicy.inf inf 文件](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [安装证书颁发机构](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [在 CA1 上配置 CDP 和 AIA 扩展](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [将 CA 证书和 CRL 复制到虚拟目录](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [配置服务器证书模板](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [配置服务器证书自动注册](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [刷新组策略](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [验证服务器证书的服务器注册](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> 本指南中的过程不包含有关在其中打开 "**用户帐户控制**" 对话框请求你允许继续操作的情况的说明。 如果在执行本指南中的过程时出现了此对话框，并且该对话框是因响应你的操作而出现的，则请单击 **“继续”** 。  
  


