---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: "联合身份验证服务和 DRS 配置公司 DNS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>联合身份验证服务和 DRS 配置公司 DNS

>适用于：Windows Server 2016，Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>第 6 步： 添加主机 \(A\) 和别名 \(CNAME\) 资源录制到公司 DNS 联合身份验证服务和 DRS  
你必须联合身份验证服务的公司域名系统 \(DNS\) 和设备注册服务配置前面的步骤中添加以下资源记录。  
  
|输入|键入|地址|  
|---------|--------|-----------|  
|federation\_service\_name|主机 \(A\)|广告 FS 服务器或负载平衡放广告 FS 服务器电场的日落配置的 IP 地址的 IP 地址|  
|enterpriseregistration|别名 \(CNAME\)|federation\_server\_name.contoso.com|  
  
可以使用下面的过程中添加到公司 DNS 联合身份验证的服务器和设备注册服务主机 \(A\) 和别名 \(CNAME\) 资源记录。  
  
在会员**管理员**，或等效，才能完成此过程的最低要求。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>可以为您联合身份验证的服务器 DNS 添加主机 \(A\) 和别名 \(CNAME\) 资源记录  
  
1.  你域控制器上，在服务器管理器中，在**工具**菜单上，单击**DNS**打开 snap\ 中 DNS。  
  
2.  控制台树中，展开**domain\_controller\_name**节点、 展开**前进查找区域**，right\ 单击**domain\_name**，然后单击**新主机 \(A or AAAA\)**。  
  
3.  在**名称**框中，键入要使用你的广告 FS 电场的日落的名称。  
  
4.  在**IP 地址**框中，键入 IP 地址的联合身份验证的服务器。 单击**添加主机**。  
  
5.  Right\ 单击**domain\_name**节点，然后依次单击**新别名 \(CNAME\)**。  
  
6.  在**新资源记录**对话框中，键入**enterpriseregistration**中**别名**框。  
  
7.  完整域中命名 \(FQDN\) 的目标主机框中键入**federation\_service\_farm\_name.domain\_name.com**，然后单击**确定**。  
  
    > [!IMPORTANT]  
    > 在实际情况中部署中，如果你的公司所拥有多个用户主要名称 \(UPN\) 后缀，你必须创建多个 CNAME 记录的这些 UPN 后缀 DNS 在每个。  
  
## <a name="see-also"></a>请参阅 

[广告 FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012R2 广告 FS 部署指导](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联盟服务器电场的日落](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

