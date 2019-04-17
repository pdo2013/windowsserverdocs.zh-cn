---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: "安装联合身份验证服务角色服务"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c520cbe22739f2bde263e133c7feb681d824d251
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-role-service"></a>安装联合身份验证服务角色服务

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

现在，你的先决条件应用程序和证书正确配置计算机，你就可以安装的 Active Directory 联合身份验证服务 \(AD FS\) 角色服务的联合身份验证服务。 当你在计算机上安装了联合身份验证服务时，该计算机变得联合身份验证的服务器。  
  
> [!NOTE]  
> 联合 Web Single\-Sign\-On \(SSO\) 设计，你必须至少一个联盟服务器帐户合作伙伴组织中的和资源合作伙伴组织中的至少一个联盟服务器。 有关详细信息，请参阅[放置联合服务器](https://technet.microsoft.com/library/dd807127.aspx)。  
  
你可以使用下面的过程中对计算机进行将成为第一个联合服务器或将变的现有联盟服务器场联合服务器的计算机上安装广告 FS 联合身份验证服务角色服务。  
  
## <a name="prerequisites"></a>先决条件  
验证与专用键 SSL 证书，已安装或导入本地证书官方商城 \(Personal store\)，然后再开始此过程。 如果你使用的 token\ 签名证书颁发的认证颁发机构 \(CA\)，验证与专用键 token\ 签名证书，已安装或导入本地证书官方商城 \(Personal store\)，然后再开始此过程。 或者，你可以创建 self\ 登录、token\ 签名证书使用添加负责向导中，在此过程中所述。 有关 token\ 签名证书的详细信息，请参阅[服务器联合身份验证的证书要求](https://technet.microsoft.com/library/dd807040.aspx)。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
#### <a name="to-install-the-federation-service-role-service"></a>若要安装联合身份验证服务角色服务  
  
1.  在**开始**屏幕上，键入**服务器管理器**，然后按 ENTER。  
  
2.  单击**管理**，然后单击**添加角色和功能**以开始添加角色并功能向导。  
  
3.  在**在开始之前**页上，单击**下一步**。  
  
4.  上**选择安装类型**页上，单击**Role\ 基于或者基于 Feature\ 安装**，然后单击**下一步**。  
  
5.  在**选择目标服务器**页上，单击**从服务器池选择服务器**，确认目标计算机突出显示时，然后单击**下一步**。  
  
6.  上**选择服务器角色**页上，单击**Active Directory 联合身份验证服务**，然后单击旁边。  
  
    > [!NOTE]  
    > 如果提示你安装额外的.NET Framework 或 Windows 的过程激活服务功能，请单击**添加功能**安装它们。  
  
7.  在**选择功能**页上，验证功能设置，然后单击**下一步**。  
  
8.  在**Active Directory 联合身份验证服务 \(AD FS\)**页上，单击**下一步**。  
  
9. 在**选择角色服务**页上，选择**联合身份验证服务**复选框，然后依次单击**下一步**。  
  
10. 在**Web 服务器角色 \(IIS\)**页上，单击**下一步**。  
  
11. 在**选择角色服务**页上，单击**下一步**。  
  
12. 请验证信息，请在后**确认安装选择**页上，选择**必要时自动重新启动目标服务器**复选框，然后依次单击**安装**。  
  
13. 在**安装进度**页面，验证一切正常，安装，然后单击**关闭**。  
  

