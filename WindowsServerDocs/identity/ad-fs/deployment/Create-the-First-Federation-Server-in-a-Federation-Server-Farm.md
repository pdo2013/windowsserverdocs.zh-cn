---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: 在联合服务器场中创建第一台联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: af0aa61f0d16d4ca567b140c95d74445d09f1cf3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879298"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>在联合服务器场中创建第一台联合服务器

 >适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在安装联合身份验证服务角色服务并在计算机上配置所需的证书后，你现可配置要成为联合身份验证服务器的计算机。 可以使用以下过程来设置计算机，使之成为新联合服务器场使用 AD FS 联合身份验证服务器配置向导中的第一个联合服务器。  
  
在服务器场中创建第一个联合服务器的操作还将创建一个新的联合身份验证服务，并使此计算机成为主联合服务器。 这意味着此计算机，读取配置\/编写的 AD FS 配置数据库副本。 此服务器场中的所有其他联合身份验证服务器必须将其读取主联合服务器所做任何更改复制\-仅它们在本地存储的 AD FS 配置数据库的副本。 有关这一复制过程的详细信息，请参阅 [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
> [!NOTE]  
> 对于联合 Web 单一\-符号\-上\(SSO\)设计中，您必须在帐户伙伴组织中的至少一台联合服务器和资源伙伴组织中的至少一台联合服务器. 有关详细信息，请参阅 [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx)。  
  
Domain Admins 中的成员身份或委派的域帐户已被授予对 Active Directory 中的程序数据容器的写访问权限，这是完成此过程所需的最低要求。  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>在联合服务器场中创建第一个联合服务器  
  
1.  有两种方法启动 AD FS 联合身份验证服务器配置向导。 若要启动该向导，请执行下列操作之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开 AD FS 管理管理单元\-中，单击**AD FS 联合身份验证服务器配置向导**链接**概述**页中或在**操作**窗格。  
  
    -   任何时间之后安装向导已完成，请打开 Windows 资源管理器，导航至**c:\\Windows\\ADFS**文件夹，并再双击\-单击**FsConfigWizard.exe**.  
  
2.  在“欢迎使用”  页上，验证是否已选中“创建新的联合身份验证服务”  ，然后单击“下一步” 。  
  
3.  上**选择独立\-Alone 或服务器场部署**页上，单击**新联合服务器场**，然后单击**下一步**。  
  
4.  在“指定联合身份验证服务名称”页上，验证“SSL 证书”是否显示正确。 如果这不是正确的证书，请从“SSL 证书”列表中选择相应的证书。  
  
    此证书生成从安全套接字层\(SSL\)设置默认网站。 如果默认网站只具有一个配置的 SSL 证书，则显示该证书并自动选择它以供使用。 如果为默认网站配置了多个 SSL 证书，此处将列出所有这些证书，并且你必须从中进行选择。 如果没有配置默认网站的 SSL 设置，则从本地计算机上的个人证书存储中的可用证书生成列表。  
  
    > [!NOTE]  
    > 如果为 IIS 配置了 SSL 证书，则该向导将不允许覆盖证书。 这可确保之前计划对 SSL 证书所做的任何 IIS 配置将得以保留。 若要解决此限制，可以删除证书，或使用 IIS 管理控制台手动对其重新配置。  
  
5.  如果已选择的 AD FS 数据库存在，则**AD FS 配置数据库检测到现有**页将出现。 如果出现该页面，请单击“删除数据库” ，然后单击“下一步” 。  
  
    > [!CAUTION]  
    > 仅当你确信此 AD FS 数据库中的数据并不重要，或者它未使用生产联合服务器场中时，请选择此选项。  
  
6.  在“指定服务帐户”  页上，单击“浏览” 。 在“浏览”  对话框中，找到将在这个新的联合服务器场中用作服务帐户的域帐户，然后单击“确定” 。 键入此帐户的密码、进行确认，然后单击“下一步” 。  
  
    > [!NOTE]  
    > 请参阅[手动配置联合服务器场服务帐户](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)有关指定联合服务器场的服务帐户的详细信息。 联合服务器场中的每个联合身份验证服务器必须指定可操作的服务器场的相同服务帐户。 例如，如果已创建的服务帐户为 contoso\\ADFS2SVC，为联合服务器角色配置并将参与相同场的每台计算机必须指定 contoso\\ADFS2SVC 在此步骤中的步骤联合身份验证服务器配置向导为场的可操作。  
  
7.  在“已准备好应用设置”页上，查看详细信息。 如果设置正确，请单击**下一步**以开始使用这些设置配置 AD FS。  
  
8.  在“配置结果”  页上，查看结果。 完成所有配置步骤后，单击“关闭”   以退出向导。  
  
    > [!IMPORTANT]  
    > 出于安全部署的目的，在你使用 AD FS 联合服务器配置向导来配置联合服务器场时，将禁用项目分辨率和回复检测。 此向导将自动配置 Windows 内部数据库以供存储服务配置数据。 您可能，但是，错误地撤消此更改，从而使用项目分辨率终结点**终结点**AD FS 管理管理单元中的节点\-中或启用\-ADFSEndpoint cmdletWindows PowerShell。 请注意，不要重新配置默认设置，以便在你同时使用联合服务器场和 Windows 内部数据库时，此终结点仍为禁用状态。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合身份验证服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  

