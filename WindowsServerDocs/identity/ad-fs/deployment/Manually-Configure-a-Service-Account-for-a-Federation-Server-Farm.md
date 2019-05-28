---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: 为联合服务器场手动配置服务帐户
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b027bff4645203c44e228f11c651b767fa4502e0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192057"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>为联合服务器场手动配置服务帐户

如果您想要在 Active Directory 联合身份验证服务配置联合服务器场环境\(AD FS\)，必须创建并配置专用的服务帐户在 Active Directory 域服务中\(AD DS\)场将驻留的位置。 然后配置服务器场中的每个联合服务器以使用此帐户。 当你想要允许对任何使用 Windows 集成身份验证的 AD FS 场中的联合身份验证服务器进行身份验证的企业网络上的客户端计算机时，必须在组织中完成以下任务。  

> [!IMPORTANT]
> AD FS 支持使用 AD FS 3.0 (Windows Server 2012 R2) 中，截至[组托管服务帐户](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA\)作为服务帐户。  这是推荐的选项，因为它消除了对管理服务帐户密码，随着时间的推移的需要。  本文档介绍如何使用的传统服务帐户，如在仍在运行 Windows Server 2008 R2 或更早的域功能级别的域中的备用的用例\(DFL\)。

> [!NOTE]  
> 你只能针对整个联合服务器场执行一次此过程中的任务。 更高版本，当使用 AD FS 联合身份验证服务器配置向导创建联合身份验证服务器，你必须指定此相同帐户上**服务帐户**每台联合服务器场中的向导页。  
  
#### <a name="create-a-dedicated-service-account"></a>创建一个专用服务帐户  
  
1.  创建专用的用户\/服务位于标识提供程序组织中的 Active Directory 林中的帐户。 此帐户是在服务器场方案中工作并允许传递的 Kerberos 身份验证协议所必需\-通过每个联合身份验证服务器上的身份验证。 使用此帐户仅用于联合身份验证服务器场的目的。  
  
2.  编辑用户帐户属性，并选中“密码永不过期”  复选框。 此操作可确保此服务帐户的功能因域密码更改要求而不会中断。  
  
    > [!NOTE]  
    > 为此专用帐户使用网络服务帐户将会在通过 Windows 集成身份验证尝试访问时导致随机失败，因为 Kerberos 票证不能从一台服务器到另一台服务器上进行验证。  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>设置服务帐户的 SPN  
  
1.  因为 AD FS 应用程序池的应用程序池标识运行的域用户\/服务帐户，必须配置服务主体名称\(SPN\)使用 Setspn.exe 命令域中的该帐户\-行工具。 运行 Windows Server 2008 的计算机上的默认情况下，Setspn.exe 安装。 加入到同一个域的计算机上运行以下命令，用户\/服务帐户所在：  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    例如，在服务器聚集在域名系统中的所有联合身份验证方案\(DNS\)主机名 fs.fabrikam.com 解析为分配给 AD FS 应用程序池的服务帐户名称名为 adfs2farm，请键入命令为遵循，，然后按 ENTER:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    有必要为此帐户只完成一次该任务。  
  
2.  AD FS 应用程序池标识更改为服务帐户后，设置的访问控制列表\(Acl\)上要允许此新帐户的读取访问权限，以使 AD FS 应用程序池可以读取策略数据的 SQL Server 数据库。  
  

