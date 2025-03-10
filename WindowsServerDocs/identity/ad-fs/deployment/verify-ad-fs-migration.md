---
title: 迁移 AD FS 2.0 联合服务器
description: 提供有关将 AD FS 服务器迁移到 Windows Server 2012 R2 的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 024f7a586c58dcf5f0d6f9c3aa291e6bef8d838b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408187"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>验证 AD FS 2.0 迁移到 Windows Server 2012 R2

完成将 Active Directory 联合身份验证服务（AD FS）场迁移到 Windows Server 2012 R2 的相同服务器迁移后，你可以使用以下过程来验证场中的联合服务器是否正常运行;也就是说，同一网络中的任何客户端都可以访问 federtation 服务器。  
  
若要完成此过程，必须至少拥有本地计算机上的**用户**、**备份操作员**、**高级用户**、**管理员**成员身份或等效身份。
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>验证联合服务器是否正常运行的步骤  
  
1.  打开浏览器窗口，然后在地址栏中键入联合服务器名称，然后将其`federationmetadata/2007-06/federationmetadata.xml`追加到以浏览到联合身份验证服务元数据终结点。 例如， `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` 。  
  
如果可以在浏览器窗口中看到联合服务器元数据且未看到任何 SSL 错误或警告，则表示联合服务器在正常运行。  
  
2. 也可以浏览到 AD FS 登录页（联合身份验证服务名称后接 `adfs/ls/idpinitiatedsignon.htm`，例如 `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`）。  这样便会显示 AD FS 登录页，你可以在其中使用域管理员凭据登录。  
  
> [!IMPORTANT]
>  请确保将浏览器设置配置为信任联合服务器角色，方法是将联合身份验证服务名称（例如`https://fs.contoso.com`）添加到浏览器的本地 intranet 区域。  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [正在准备迁移 AD FS 联合服务器](prepare-migrate-ad-fs-server-r2.md)  
 [迁移 AD FS 联合服务器](migrate-ad-fs-fed-server-r2.md)   
 [迁移 AD FS 联合服务器代理](migrate-fed-server-proxy-r2.md)   
