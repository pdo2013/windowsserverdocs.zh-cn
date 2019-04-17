---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: "安装联合身份验证服务代理角色服务"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: aea1ef335604aa18f0b1a1c22ef13f6fee1601b3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-proxy-role-service"></a>安装联合身份验证服务代理角色服务

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

将计算机配置的先决条件应用程序和证书后，你将准备好进行安装的 Active Directory 联合身份验证服务 \(AD FS\) 联合身份验证服务代理角色服务。 你可以使用下面的过程中安装联合身份验证服务代理角色服务。 当你在计算机上安装了联合身份验证服务代理角色服务时，该计算机变得联合身份验证的服务器代理。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-install-the-federation-service-proxy-role-service"></a>若要安装联合身份验证服务代理角色服务  
  
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
  
9. 在**选择角色服务**页上，选择**联合身份验证服务代理**复选框，然后依次单击**下一步**。  
  
10. 在**Web 服务器角色 \(IIS\)**页上，单击**下一步**。  
  
11. 在**选择角色服务**页上，单击**下一步**。  
  
12. 请验证信息，请在后**确认安装选择**页上，选择**必要时自动重新启动目标服务器**复选框，然后依次单击**安装**。  
  
13. 在**安装进度**页面，验证一切正常，安装，然后单击**关闭**。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  
[清单：联合身份验证的服务器代理服务器设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

