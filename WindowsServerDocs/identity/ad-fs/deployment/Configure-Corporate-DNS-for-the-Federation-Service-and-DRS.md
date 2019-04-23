---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: 为联合身份验证服务和 DRS 配置企业 DNS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876388"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>为联合身份验证服务和 DRS 配置企业 DNS

>适用于：Windows Server 2016, Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>步骤 6：添加主机\(A\)和别名\(CNAME\)公司 dns 中为联合身份验证服务和 DRS 的资源记录  
必须将以下资源记录添加到公司域名系统\(DNS\)联合身份验证服务和你在前面的步骤中配置的设备注册服务。  
  
|条目|在任务栏的搜索框中键入|地址|  
|---------|--------|-----------|  
|federation\_service\_name|主机\(A\)|AD FS 服务器或 AD FS 服务器场的前面配置的负载均衡器的 IP 地址的 IP 地址|  
|enterpriseregistration|别名\(CNAME\)|federation\_server\_name.contoso.com|  
  
可以使用以下过程来添加主机\(A\)和别名\(CNAME\)到公司 DNS 中的为联合身份验证服务器和设备注册服务的资源记录。  
  
中的成员身份**管理员**，或等效身份是完成此过程的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>若要将主机添加\(A\)和别名\(CNAME\)到 DNS 的联合身份验证服务器的资源记录  
  
1.  您在域控制器上，在服务器管理器中，在**工具**菜单上，单击**DNS**以打开 DNS 管理单元\-中。  
  
2.  在控制台树中，展开**域\_控制器\_名称**节点，展开**正向查找区域**，右键\-单击**域\_名称**，然后单击**新的主机\(A 或 AAAA\)**。  
  
3.  在中**名称**框中，键入要用于 AD FS 场的名称。  
  
4.  在 **IP 地址**框中，键入联合身份验证服务器的 IP 地址。 单击“添加主机” 。  
  
5.  右\-单击**域\_名称**节点，，然后单击**新别名\(CNAME\)**。  
  
6.  在“新资源记录”对话框中，在“别名”框内键入 **enterpriseregistration**。  
  
7.  中的完全限定的域名\(FQDN\)的目标主机框中，键入**联合身份验证\_服务\_场\_name.domain\_name.com**，，然后单击**确定**。  
  
    > [!IMPORTANT]  
    > 在现实世界部署中，如果你的公司有多个用户主体名称\(UPN\)后缀，您必须为每个这些 UPN 后缀在 DNS 中创建多个 CNAME 记录。  
  
## <a name="see-also"></a>请参阅 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

