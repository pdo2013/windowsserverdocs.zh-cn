---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: "创建独立联盟服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-stand-alone-federation-server"></a>创建独立联盟服务器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

安装联合身份验证服务角色服务，并配置了计算机上的所需的证书后，你就可以配置计算机变得联合身份验证的服务器。 可以使用下面的过程若要设置为计算机成为 stand\ 单独联合身份验证的服务器。 创建 stand\ 单独联盟服务器的操作还会创建新的联合身份验证服务。 您与广告 FS 联盟服务器配置向导创建的联合身份验证的服务器。  
  
> [!NOTE]  
> 联合 Web Single\-Sign\-On \(SSO\) 设计，你必须至少一个联盟服务器帐户合作伙伴组织中的和资源合作伙伴组织中的至少一个联盟服务器。 有关详细信息，请参阅[放置联合服务器](https://technet.microsoft.com/library/dd807127.aspx)。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
### <a name="to-create-a-stand-alone-federation-server"></a>若要创建 stand\ 单独联盟服务器  
  
1.  有两种方法可以启动广告 FS 联合身份验证的服务器配置向导。 若要启动该向导，请执行下列情况之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开广告 FS 管理 snap\ 中，单击**广告 FS 联盟服务器配置向导**链接上**概述**页面或**操作**窗格。  
  
    -   Anytime 完成，打开 Windows 资源管理器设置向导后，请导航到**C:\\Windows\\ADFS**文件夹，然后 double\ 单击**FsConfigWizard.exe**。  
  
2.  在**欢迎**页上，检查**创建新的联合身份验证服务**选中，则，然后单击**下一步**。  
  
3.  上**选择 Stand\-孤单或电场的日落部署**页上，单击**Stand\ 单独联合身份验证的服务器**，然后单击**下一步**。  
  
    > [!IMPORTANT]  
    > 选择时 Stand\ 单独联盟服务器选项广告 FS 联盟服务器配置向导中，通过此联合身份验证服务相关联的服务帐户将自动分配给网络服务帐户。 使用网络服务作为服务帐户仅在某些情况下，你都在此评估的测试实验环境中广告 FS 建议。 如果你想要使用 Stand\ 单独联盟服务器选项联盟服务器生产环境中的部署，它很重要，更改此服务帐户更适合服务帐户，可以专用于请求提供服务的这一新联合身份验证服务。 更改的帐户之外网络服务服务帐户将减少可能攻击方法，否则将使您联合身份验证的服务器受到恶意攻击。  
  
4.  在**指定联合身份验证服务名称**页上，检查**SSL 证书**，所显示正确无误。 如果没有，选择相应的证书从**SSL 证书**列表。  
  
    在安全套接字层 \(SSL\) 设置默认网站中生成该证书。 如果只有一个 SSL 证书配置默认网站，该证书呈现，并使用自动处于选中状态。 如果多个 SSL 证书配置默认网站，下面列出了所有这些证书，并且必须从它们进行选择。 如果没有配置默认网站 SSL 设置，从在本地计算机上的个人证书应用商店中提供的证书生成列表。  
  
    > [!NOTE]  
    > 该向导将不会允许你如果 SSL 证书配置 iis 覆盖证书。 这将确保任何适用保留 SSL 证书以前 IIS 配置。 若要解决此限制时，你可以删除证书或重新配置手动它与 IIS 管理控制台。  
  
5.  如果您已经选择广告 FS 数据库存在，**现有广告 FS 配置检测到数据库**页面显示时。 如果发生这种情况，请单击**删除数据库**，然后单击**下一步**。  
  
    > [!CAUTION]  
    > 仅当你确信此广告 FS 数据库中的数据并不重要或，不使用生产联盟服务器场中选择此选项。  
  
6.  在**应用设置为随时**页上，检查的详细信息。 如果似乎正确设置，请单击**下一步**若要开始使用以下设置配置广告 FS。  
  
7.  在**配置结果**页上，检查结果。 完成所有配置步骤后，请单击**关闭**退出向导。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  

