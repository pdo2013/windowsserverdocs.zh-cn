---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: "添加到联合身份验证的服务器场联合服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d67f4c252ad25a05f11b88771f12fd01d13137d4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>添加到联合身份验证的服务器场联合服务器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

安装联合身份验证服务角色服务，并配置了计算机上的所需的证书后，你就可以配置计算机变得联合身份验证的服务器。 可以使用下面的过程中加入新联合身份验证的服务器场到一台计算机。  
  
一台计算机加入场与广告 FS 联盟服务器配置向导。 当你使用该向导将计算机加入现有电场的日落时，计算机配置的广告 FS 配置数据库仅 read\ 副本，并且它必须从主联盟服务器接收更新。  
  
> [!NOTE]  
> 联合 Web Single\-Sign\-On \(SSO\) 设计，你必须至少一个联盟服务器帐户合作伙伴组织中的和资源合作伙伴组织中的至少一个联盟服务器。 有关详细信息，请参阅[放置联合服务器](https://technet.microsoft.com/library/dd807127.aspx)。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>若要添加到联盟服务器电场的日落联合服务器  
  
1.  有两种方法可以启动广告 FS 联合身份验证的服务器配置向导。 若要启动该向导，请执行下列情况之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开广告 FS 管理 snap\ 中，单击**广告 FS 联盟服务器配置向导**链接上**概述**页面或**操作**窗格。  
  
    -   Anytime 完成，打开 Windows 资源管理器设置向导后，请导航到**C:\\Windows\\ADFS**文件夹，然后 double\ 单击**FsConfigWizard.exe**。  
  
2.  在**欢迎**页上，检查**联盟服务器添加到现有的联合身份验证服务**选中，则，然后单击**下一步**。  
  
3.  如果您已经选择广告 FS 数据库存在，**现有广告 FS 配置检测到数据库**页面显示时。 如果发生这种情况，请单击**删除数据库**，然后单击**下一步**。  
  
    > [!CAUTION]  
    > 仅当你确信此广告 FS 数据库中的数据并不重要或，不使用生产联盟服务器场中选择此选项。  
  
4.  在**指定的主联合身份验证的服务器和服务帐户**页面上下,**主要联合身份验证的服务器名称**，在电场的日落，键入主联合身份验证的服务器的计算机名称，然后单击**浏览**。 在**浏览**对话框中，找到用作 service 帐户的现有联合身份验证的服务器场中, 所有其他联盟服务器域帐户，然后单击**确定**。 键入密码和确认它，然后单击**下一步**:  
  
    > [!NOTE]  
    > 有关指定的联合身份验证的服务器场服务帐户的详细信息，请参阅[手动联合身份验证的服务器场配置服务帐户](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。 联合身份验证的服务器场在每个联合服务器必须指定相同的服务帐户场可以运行了。 例如，contoso\\ADFS2SVC 创建服务帐户时，您将配置为联合身份验证的服务器角色并将参与相同电场的日落每台计算机必须在联合身份验证的服务器配置向导场可以运行了指定 contoso\\ADFS2SVC 在此步骤。  
  
5.  在**应用设置为随时**页上，检查的详细信息。 如果似乎正确设置，请单击**下一步**若要开始使用以下设置配置广告 FS。  
  
6.  在**配置结果**页上，检查结果。 完成所有配置步骤后，请单击**关闭**退出向导。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  

