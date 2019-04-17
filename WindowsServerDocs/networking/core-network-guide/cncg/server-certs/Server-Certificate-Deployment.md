---
title: 服务器证书部署
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a10a9bafa6a8c9fddecac799ec8e837bf339d0e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment"></a>服务器证书部署

>适用于：Windows Server（半年通道），Windows Server 2016

按照以下步骤来安装企业根证书颁发机构 （加拿大） 和部署服务器证书用于 PEAP 和 EAP。  
  
> [!IMPORTANT]  
> 安装 Active Directory 证书服务之前，必须命名计算机，配置计算机的静态 IP 地址，并加入域的计算机。 安装广告客户服务后，你无法更改计算机名称或的计算机，域会员，但如果需要你可以更改的 IP 地址。 有关如何来完成这些任务的详细信息，请参阅 Windows Server&reg; 2016年[Core 网络指南](../../Core-Network-Guide.md)。  

  
-   [安装 Web 服务器 WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [在 DNS 为 WEB1 创建别名 (CNAME) 记录](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [配置 WEB1 分配证书吊销列出 (Crl)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [准备 CAPolicy inf 文件](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [安装证书颁发机构](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [在 CA1 配置 CDP 和 AIA 扩展](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [将 CA 证书和 CRL 复制到虚拟目录](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [配置服务器证书模板](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [配置服务器证书自动注册](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [刷新组策略](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [验证服务器证书服务器的注册](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> 本指南中的步骤不包括在其中的情况下的说明进行操作**用户帐户控制**对话框中打开请求您授予权限，才能继续。 如果此对话框打开时，你将在指南中，执行过程，并且如果对话框中已打开，以响应你的操作，请单击**继续**。  
  


