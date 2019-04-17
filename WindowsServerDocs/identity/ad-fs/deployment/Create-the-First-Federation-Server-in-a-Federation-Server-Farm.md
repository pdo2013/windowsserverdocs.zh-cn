---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: "联合身份验证的服务器场中创建的第一个联盟服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: af0aa61f0d16d4ca567b140c95d74445d09f1cf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>联合身份验证的服务器场中创建的第一个联盟服务器

 >适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

安装联合身份验证服务角色服务，并配置了计算机上的所需的证书后，你就可以配置计算机变得联合身份验证的服务器。 可以使用下面的过程中设置变得新联盟服务器场使用广告 FS 联盟服务器配置向导中的第一个联盟服务器的计算机。  
  
在一场中创建的第一个联盟服务器的操作还创建新的联合身份验证服务，并使主要联合身份验证的服务器的此计算机。 这意味着，将使用的广告 FS 配置数据库 read\/写入副本配置此计算机。 此电场的日落中的所有其他联盟服务器必须复制到它们存储本地广告 FS 配置数据库他们仅 read\ 份主要联盟服务器的任何更改。 有关此复制过程的详细信息，请参阅[广告 FS 配置数据库中的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
> [!NOTE]  
> 联合 Web Single\-Sign\-On \(SSO\) 设计，你必须至少一个联盟服务器帐户合作伙伴组织中的和资源合作伙伴组织中的至少一个联盟服务器。 有关详细信息，请参阅[放置联合服务器](https://technet.microsoft.com/library/dd807127.aspx)。  
  
域管理员或写入访问在 Active Directory 程序数据容器授予委派的域帐户中的会员是的最低要求才能完成此过程。  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>联合身份验证的服务器场中创建的第一个联盟服务器  
  
1.  有两种方法可以启动广告 FS 联合身份验证的服务器配置向导。 若要启动该向导，请执行下列情况之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开广告 FS 管理 snap\ 中，单击**广告 FS 联盟服务器配置向导**链接上**概述**页面或**操作**窗格。  
  
    -   随时后设置向导完整、 打开 Windows 资源管理器，导航到**C:\\Windows\\ADFS**文件夹，然后 double\ 单击**FsConfigWizard.exe**。  
  
2.  在**欢迎**页上，检查**创建新的联合身份验证服务**选中，则，然后单击**下一步**。  
  
3.  在**选择 Stand\-孤单或电场的日落部署**页上，单击**新联合身份验证的服务器场**，然后单击**下一步**。  
  
4.  在**指定联合身份验证服务名称**页上，检查**SSL 证书**，所显示正确无误。 如果这不是正确的证书，选择相应的证书从**SSL 证书**列表。  
  
    在安全套接字层 \(SSL\) 设置默认网站中生成该证书。 如果只有一个 SSL 证书配置默认网站，该证书呈现，并使用自动处于选中状态。 如果多个 SSL 证书配置默认网站，下面列出了所有这些证书，并且必须从它们进行选择。 如果没有配置默认网站 SSL 设置，从在本地计算机上的个人证书应用商店中提供的证书生成列表。  
  
    > [!NOTE]  
    > 该向导将不会允许你如果 SSL 证书配置 iis 覆盖证书。 这将确保任何适用保留 SSL 证书以前 IIS 配置。 若要解决这一限制，你可以删除证书或将其重新配置手动与 IIS 管理控制台。  
  
5.  如果您已经选择广告 FS 数据库存在，**现有广告 FS 配置检测到数据库**页面显示时。 如果该页面显示时，请单击**删除数据库**，然后单击**下一步**。  
  
    > [!CAUTION]  
    > 仅当你确信此广告 FS 数据库中的数据并不重要或，不使用生产联盟服务器场中选择此选项。  
  
6.  在**指定服务帐户**页上，单击**浏览**。 在**浏览**对话框中，找到域帐户，将用作此新联盟服务器电场的日落，在服务帐户，然后单击**确定**。 键入此帐户的密码，确认它，，然后单击**下一步**。  
  
    > [!NOTE]  
    > 请参阅[手动联合身份验证的服务器场配置服务帐户](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)有关指定的联合身份验证的服务器场服务帐户详细信息。 联合身份验证的服务器场在每个联合服务器必须指定相同的服务帐户场可以运行了。 例如，contoso\\ADFS2SVC 创建服务帐户时，您将配置为联合身份验证的服务器角色并将参与相同场每台计算机必须在联合身份验证的服务器配置向导场可以运行了指定 contoso\\ADFS2SVC 在此步骤。  
  
7.  在**应用设置为随时**页上，检查的详细信息。 如果似乎正确设置，请单击**下一步**若要开始使用以下设置配置广告 FS。  
  
8.  在**配置结果**页上，检查结果。 完成所有配置步骤后，请单击**关闭**退出向导。  
  
    > [!IMPORTANT]  
    > 出于安全部署，当你使用的广告 FS 联盟服务器配置向导联合身份验证的服务器场配置禁用项目分辨率和回复检测。 该向导将自动配置 Windows 内部存储服务配置数据数据库。 你可能会但是，误撤销此更改通过启用使用的项目分辨率端点**端点**中 snap\ 中广告 FS 管理或在 Windows PowerShell Enable\ ADFSEndpoint cmdlet 节点。 请注意，以便此端点保持禁用一起使用联合身份验证的服务器场和 Windows 内部数据库时重新配置默认设置。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  

