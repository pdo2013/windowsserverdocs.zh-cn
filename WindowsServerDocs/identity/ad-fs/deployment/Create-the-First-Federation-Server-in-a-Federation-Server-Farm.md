---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: 在联合服务器场中创建第一台联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 09b577ddcf722c6eac17ea145f29f9583d1cdb00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408426"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>在联合服务器场中创建第一台联合服务器

安装联合身份验证服务角色服务并在计算机上配置所需的证书后，你就可以将计算机配置为联合服务器了。 您可以使用以下过程将计算机设置为使用 AD FS 联合服务器配置向导的新联合服务器场中的第一个联合服务器。  
  
在服务器场中创建第一个联合服务器的操作还将创建一个新的联合身份验证服务，并使此计算机成为主联合服务器。 这意味着，将为此计算机配置 AD FS 配置数据库的 read @ no__t-0write 副本。 此服务器场中的所有其他联合服务器必须将主联合服务器上所做的任何更改复制到它们在本地存储的 AD FS 配置数据库的 read @ no__t 副本。 有关这一复制过程的详细信息，请参阅 [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
> [!NOTE]  
> 对于联合 Web one @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 设计，在帐户伙伴组织中必须至少有一台联合服务器，在资源伙伴组织中必须至少有一个联合服务器。 有关详细信息，请参阅 [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx)。  
  
Domain Admins 中的成员身份或委派的域帐户已被授予对 Active Directory 中的程序数据容器的写访问权限，这是完成此过程所需的最低要求。  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>在联合服务器场中创建第一个联合服务器  
  
1.  可以通过两种方式启动 AD FS 联合服务器配置向导。 若要启动该向导，请执行下列操作之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开 AD FS 管理 snap @ no__t-0in，并单击 "**概述**" 页上或 "**操作**" 窗格中的 " **AD FS 联合服务器配置向导**" 链接。  
  
    -   安装向导完成后的任意时间，打开 Windows 资源管理器，导航到**C @no__t： 1Windows @ no__t-2ADFS**文件夹，然后双击 @ no__t-3click **fsconfigwizard.exe**。  
  
2.  在“欢迎使用” 页上，验证是否已选中“创建新的联合身份验证服务” ，然后单击“下一步”。  
  
3.  在 "**选择 no__t-1Alone 或场部署**" 页上，单击 "**新建联合服务器场**"，然后单击 "**下一步**"。  
  
4.  在“指定联合身份验证服务名称”页上，验证“SSL 证书”是否显示正确。 如果这不是正确的证书，请从“SSL 证书”列表中选择相应的证书。  
  
    此证书从默认网站的安全套接字层 \(SSL @ no__t 设置生成。 如果默认网站只具有一个配置的 SSL 证书，则显示该证书并自动选择它以供使用。 如果为默认网站配置了多个 SSL 证书，此处将列出所有这些证书，并且你必须从中进行选择。 如果没有配置默认网站的 SSL 设置，则从本地计算机上的个人证书存储中的可用证书生成列表。  
  
    > [!NOTE]  
    > 如果为 IIS 配置了 SSL 证书，则该向导将不允许覆盖证书。 这可确保之前计划对 SSL 证书所做的任何 IIS 配置将得以保留。 若要解决此限制，可以删除证书，或使用 IIS 管理控制台手动对其重新配置。  
  
5.  如果您选择的 AD FS 数据库已经存在，则会出现 "**检测到现有 AD FS 配置数据库**" 页。 如果出现该页面，请单击“删除数据库”，然后单击“下一步”。  
  
    > [!CAUTION]  
    > 仅当你确定此 AD FS 数据库中的数据不重要，或者不在生产联合服务器场中使用时，才选择此选项。  
  
6.  在“指定服务帐户” 页上，单击“浏览”。 在“浏览” 对话框中，找到将在这个新的联合服务器场中用作服务帐户的域帐户，然后单击“确定”。 键入此帐户的密码、进行确认，然后单击“下一步”。  
  
    > [!NOTE]  
    > 有关指定联合服务器场的服务帐户的详细信息，请参阅[手动配置联合服务器场的服务帐户](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。 联合服务器场中的每个联合服务器都必须为要操作的场指定相同的服务帐户。 例如，如果创建的服务帐户是 contoso @ no__t-0ADFS2SVC，则为联合服务器角色配置的每台计算机以及将加入同一场的计算机都必须在联合服务器的此步骤中指定 contoso @ no__t-1ADFS2SVC要使场正常工作的配置向导。  
  
7.  在“已准备好应用设置”页上，查看详细信息。 如果设置正确，请单击 "**下一步**" 以开始配置具有这些设置的 AD FS。  
  
8.  在“配置结果” 页上，查看结果。 完成所有配置步骤后，单击“关闭”  以退出向导。  
  
    > [!IMPORTANT]  
    > 出于安全部署的目的，在你使用 AD FS 联合服务器配置向导来配置联合服务器场时，将禁用项目分辨率和回复检测。 此向导将自动配置 Windows 内部数据库以供存储服务配置数据。 但是，你可能会错误地撤消此更改，方法是使用 AD FS 管理 snap @ no__t-1in 中的 "**终结点**" 节点或 Windows PowerShell 中的 Enable @ No__t-2ADFSEndpoint Cmdlet 启用项目解析终结点。 请注意，不要重新配置默认设置，以便在你同时使用联合服务器场和 Windows 内部数据库时，此终结点仍为禁用状态。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  

