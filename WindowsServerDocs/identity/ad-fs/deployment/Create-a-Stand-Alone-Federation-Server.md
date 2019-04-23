---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: 创建独立联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888288"
---
# <a name="create-a-stand-alone-federation-server"></a>创建独立联合服务器

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在安装联合身份验证服务角色服务并在计算机上配置所需的证书后，你现可配置要成为联合身份验证服务器的计算机。 可以使用以下过程来设置计算机，使之成为独立\-单独的联合身份验证服务器。 创建独立的操作\-单独的联合身份验证服务器还会创建新的联合身份验证服务。 实现使用 AD FS 联合身份验证服务器配置向导创建联合身份验证服务器。  
  
> [!NOTE]  
> 对于联合 Web 单一\-符号\-上\(SSO\)设计中，您必须在帐户伙伴组织中的至少一台联合服务器和资源伙伴组织中的至少一台联合服务器. 有关详细信息，请参阅 [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx)。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-create-a-stand-alone-federation-server"></a>若要创建独立\-单独的联合身份验证服务器  
  
1.  有两种方法启动 AD FS 联合身份验证服务器配置向导。 若要启动该向导，请执行下列操作之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开 AD FS 管理管理单元\-中，单击**AD FS 联合身份验证服务器配置向导**链接**概述**页中或在**操作**窗格。  
  
    -   安装向导已完成，请打开 Windows 资源管理器后，随时导航到**c:\\Windows\\ADFS**文件夹，并再双击\-单击**FsConfigWizard.exe**.  
  
2.  在“欢迎使用”  页上，验证是否已选中“创建新的联合身份验证服务”  ，然后单击“下一步” 。  
  
3.  上**选择独立\-Alone 或服务器场部署**页上，单击**突出\-单独的联合身份验证服务器**，然后单击**下一步**。  
  
    > [!IMPORTANT]  
    > 如果选择独立\-AD FS 联合身份验证服务器配置向导中的单独的联合身份验证服务器选项，与此联合身份验证服务关联的服务帐户将自动分配给网络服务帐户。 在其中您正在评估测试实验室环境中的 AD FS 的情况下，建议仅使用网络服务作为服务帐户。 如果你想要使用独立\-单独的联合身份验证服务器选项部署在生产环境中，联合身份验证服务器务必将此服务帐户更改为更合适的服务帐户的可专用于服务此新的联合身份验证服务的请求数。 将服务帐户更改为以外的其他网络服务帐户将会减少可能的攻击载体，否则会使你的联合身份验证服务器容易受到恶意攻击。  
  
4.  在“指定联合身份验证服务名称”页上，验证“SSL 证书”是否显示正确。 如果没有，请选择从相应的证书**SSL 证书**列表。  
  
    此证书生成从安全套接字层\(SSL\)设置默认网站。 如果默认网站只具有一个配置的 SSL 证书，则显示该证书并自动选择它以供使用。 如果为默认网站配置了多个 SSL 证书，此处将列出所有这些证书，并且你必须从中进行选择。 如果没有配置默认网站的 SSL 设置，则从本地计算机上的个人证书存储中的可用证书生成列表。  
  
    > [!NOTE]  
    > 如果为 IIS 配置了 SSL 证书，则该向导将不允许覆盖证书。 这可确保之前计划对 SSL 证书所做的任何 IIS 配置将得以保留。 若要解决此限制，可以删除该证书或手动重新配置它与 IIS 管理控制台。  
  
5.  如果已选择的 AD FS 数据库存在，则**AD FS 配置数据库检测到现有**页将出现。 如果显示该页，则单击“删除数据库”，然后单击“下一步”。  
  
    > [!CAUTION]  
    > 仅当你确信此 AD FS 数据库中的数据并不重要，或者它未使用生产联合服务器场中时，请选择此选项。  
  
6.  在“已准备好应用设置”页上，查看详细信息。 如果设置正确，请单击**下一步**以开始使用这些设置配置 AD FS。  
  
7.  在“配置结果”  页上，查看结果。 完成所有配置步骤后，单击“关闭”   以退出向导。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合身份验证服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  

