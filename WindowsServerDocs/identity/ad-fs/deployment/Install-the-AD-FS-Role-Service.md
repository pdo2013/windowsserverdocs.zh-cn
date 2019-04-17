---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: "安装广告 FS 角色服务"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-ad-fs-role-service"></a>安装广告 FS 角色服务

>适用于：Windows Server 2016，Windows Server 2012 R2

你可以使用下面的过程中运行的 Windows Server 2012 R2 变得联盟服务器电场的日落中的第一个联盟服务器或联盟服务器现有联盟服务器场中的计算机上安装广告 FS 角色服务。  
  
在会员**管理员**，或等效，在本地计算机上的已完成此过程的最低要求。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>若要安装广告 FS 服务器角色，通过添加角色和功能向导  
  
1.  打开服务器管理器。 若要打开服务器管理器，请单击**服务器管理器**上**开始**屏幕上，或**服务器管理器**桌面任务栏中。 在**快速启动**的选项卡**欢迎**磁贴上**仪表板**页上，单击**添加角色和功能**。 或者，你可以单击**添加角色和功能**上**管理**菜单。  
  
2.  在**在开始之前**页上，单击**下一步**。  
  
3.  在**选择安装类型**页上，单击**Role\ 基于或者基于 Feature\ 安装**，然后单击**下一步**。  
  
4.  在**选择目标服务器**页上，单击**从服务器池选择服务器**，确认目标计算机选择，然后单击**下一步**。  
  
5.  在**选择服务器角色**页上，单击**Active Directory 联合身份验证服务**，然后单击**下一步**。  
  
6.  在**选择功能**页上，单击**下一步**。 所需的先决条件未预先为你选择。 不需要选择任何其他功能。  
  
7.  在**Active Directory 联合身份验证服务 \(AD FS\)**页上，单击**下一步**。  
  
8.  请验证信息，请在后**确认安装选择**页上，单击**安装**。  
  
9. 在**安装进度**页面，验证一切正常，安装，然后单击**关闭**。  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>若要安装 Windows PowerShell 通过广告 FS server 角色  
  
1.  你想要配置为服务器联合身份验证的计算机，打开 Windows PowerShell 命令窗口中，并运行以下命令：`Install-windowsfeature adfs-federation –IncludeManagementTools`。  
  
## <a name="see-also"></a>请参阅 

[广告 FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012R2 广告 FS 部署指导](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联盟服务器电场的日落](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

