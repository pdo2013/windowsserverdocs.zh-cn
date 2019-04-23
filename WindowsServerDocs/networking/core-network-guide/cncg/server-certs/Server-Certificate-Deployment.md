---
title: 服务器证书部署
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 751c5c5958b3d06ae0f4b701e4d6e10a7fef19dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858488"
---
# <a name="server-certificate-deployment"></a>服务器证书部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

按照以下步骤来安装企业根证书颁发机构 (CA) 和部署服务器证书用于 PEAP 和 EAP。  
  
> [!IMPORTANT]  
> 在安装 Active Directory 证书服务之前，必须命名计算机，将计算机配置具有静态 IP 地址，并将计算机加入到域。 安装 AD CS 后，您不能更改计算机名称或域成员身份的计算机，但您可以根据需要更改 IP 地址。 有关如何完成这些任务的详细信息，请参阅 Windows Server&reg; 2016年[核心网络指南](../../Core-Network-Guide.md)。  

  
-   [安装 Web Server WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [为 WEB1 在 DNS 中创建一个别名 (CNAME) 记录](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [配置 WEB1 分发证书吊销列表 (Crl)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [准备 CAPolicy inf 文件](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [安装证书颁发机构](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [CA1 上配置的 CDP 和 AIA 扩展](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [将 CA 证书和 CRL 复制到虚拟目录](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [配置服务器证书模板](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [配置服务器证书自动注册](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [刷新组策略](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [验证服务器证书的服务器注册](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> 本指南中的过程中不包括用例的说明**用户帐户控制**对话框将打开以继续的权限。 如果在执行本指南中的过程时出现了此对话框，并且该对话框是因响应你的操作而出现的，则请单击 **“继续”**。  
  


