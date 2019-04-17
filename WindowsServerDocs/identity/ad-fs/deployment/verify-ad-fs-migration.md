---
title: "迁移广告 FS 2.0 联盟服务器"
description: "在迁移到 Windows Server 2012 R2 的广告 FS 服务器上提供的信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2017
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>验证该广告 FS 2.0 迁移到 Windows Server 2012 R2

当完成相同 server 迁移的你对 Windows Server 2012 R2 的 Active Directory 联合身份验证服务 (广告 FS) 电场的日落时，你可以使用下面的过程验证场中的联盟服务器操作;也就是说，相同网络的任何客户可以联系 federtation 服务器。  
  
在会员**用户**，**备份运营商**，**电源用户**，**管理员**或等效，在本地计算机上的最低要求完成此过程。
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>若要验证联盟服务器在运行  
  
1.  打开浏览器窗口和在地址栏中，键入联合身份验证的服务器名称，然后将其与附加`federationmetadata/2007-06/federationmetadata.xml`浏览的联合身份验证服务元数据端点。 例如， `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` 。  
  
在你的浏览器窗口中，您可以看到没有任何 SSL 错误或警告联合服务器元数据，您联合身份验证的服务器操作。  
  
2.  你还可以浏览到登录页面广告 FS (你联合身份验证服务名称附加`adfs/ls/idpinitiatedsignon.htm`，例如`https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`)。  可以登录域管理员凭据登录页面广告 FS 显示此信息。  
  
> [!IMPORTANT]
>  确保要配置你的浏览器设置以信任联合身份验证的服务器角色通过添加你的联合身份验证服务名称 (例如， `https://fs.contoso.com`) 到浏览器的本地 intranet 区域。  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [准备迁移广告 FS 联盟服务器](prepare-migrate-ad-fs-server-r2.md)  
 [迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)   
 [迁移广告 FS 联合身份验证的服务器代理](migrate-fed-server-proxy-r2.md)   
