---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: 为联合身份验证服务和 DRS 配置企业 DNS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9f0b04f9dc050117fdefc630759c86d2b1bb1ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408440"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>为联合身份验证服务和 DRS 配置企业 DNS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>步骤 6：向联合身份验证服务和 DRS 的企业 DNS 添加主机 \(A @ no__t，并将别名 \(CNAME @ no__t  
对于你在前面的步骤中配置的联合身份验证服务和设备注册服务，你必须将以下资源记录添加到公司域名系统 \(DNS @ no__t-1。  
  
|条目|type|地址|  
|---------|--------|-----------|  
|federation @ no__t-0service @ no__t-1name|Host \(A @ no__t-1|AD FS 服务器场前面配置的 AD FS 服务器的 IP 地址或负载均衡器的 IP 地址|  
|enterpriseregistration|Alias \(CNAME @ no__t-1|federation @ no__t-0server\_name.contoso.com|  
  
你可以使用以下过程向联合服务器和设备注册服务的企业 DNS 添加 \(A @ no__t 和 alias \(CNAME @ no__t 资源记录。  
  
**Administrators**中的成员身份或等效身份是完成此过程的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>向联合服务器的 DNS 添加主机 \(A @ no__t，并将别名 \(CNAME @ no__t 资源记录添加到 DNS  
  
1.  在域控制器上的 "服务器管理器中的"**工具**"菜单上，单击" **DNS** "打开 dns snap @ no__t-2in。  
  
2.  在控制台树中，展开**域 @ no__t-1controller @ no__t-2name**节点，展开 "**正向查找区域**"，右键 @ no__t-4click **domain @ no__t-6Name**，然后单击 "**新建主机 \(A" 或 "AAAA @ no__t-9**"。  
  
3.  在 "**名称**" 框中，键入要用于 AD FS 场的名称。  
  
4.  在 **IP 地址**框中，键入联合身份验证服务器的 IP 地址。 单击“添加主机”。  
  
5.  右键 @ no__t-0click **domain @ no__t-2name**节点，然后单击 "**新建别名 \(CNAME @ no__t-5**"。  
  
6.  在“新资源记录”对话框中，在“别名”框内键入 **enterpriseregistration**。  
  
7.  在 "目标主机" 框的完全限定的域名 \(FQDN @ no__t-1 中，键入**federation @ no__t-3service @ no__t-4farm @ no__t-5name @ no__t-** 6name，然后单击 **"确定"** 。  
  
    > [!IMPORTANT]  
    > 在实际部署中，如果你的公司有多个用户主体名称 \(UPN @ no__t 后缀，则必须在 DNS 中为每个 UPN 后缀创建多个 CNAME 记录。  
  
## <a name="see-also"></a>请参阅 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

