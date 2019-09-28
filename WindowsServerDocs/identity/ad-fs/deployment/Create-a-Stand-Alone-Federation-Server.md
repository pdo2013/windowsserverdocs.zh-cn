---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: 创建独立联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3a948fadeb1cfa2ebe259a9bb659e93a85e06910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359656"
---
# <a name="create-a-stand-alone-federation-server"></a>创建独立联合服务器

安装联合身份验证服务角色服务并在计算机上配置所需的证书后，你就可以将计算机配置为联合服务器了。 你可以使用以下过程将计算机设置为备用 @ no__t-0alone 联合服务器。 创建备用 @ no__t-0alone 联合服务器的操作也会创建新联合身份验证服务。 你确实要使用 AD FS 联合服务器配置向导创建联合服务器。  
  
> [!NOTE]  
> 对于联合 Web one @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 设计，在帐户伙伴组织中必须至少有一台联合服务器，在资源伙伴组织中必须至少有一个联合服务器。 有关详细信息，请参阅 [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx)。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  有关使用适当帐户和组成员身份的详细信息，请参阅[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http\/：\/\/go.microsoft.com\/fwlink？LinkId\=83477\)。   
  
### <a name="to-create-a-stand-alone-federation-server"></a>创建备用 @ no__t-0alone 联合服务器  
  
1.  可以通过两种方式启动 AD FS 联合服务器配置向导。 若要启动该向导，请执行下列操作之一：  
  
    -   联合身份验证服务角色服务安装完成后，打开 AD FS 管理 snap @ no__t-0in，并单击 "**概述**" 页上或 "**操作**" 窗格中的 " **AD FS 联合服务器配置向导**" 链接。  
  
    -   安装向导完成后，请打开 Windows 资源管理器，导航到**C @no__t： 1Windows @ no__t-2ADFS**文件夹，然后双击 @ no__t-3click **fsconfigwizard.exe**。  
  
2.  在“欢迎使用” 页上，验证是否已选中“创建新的联合身份验证服务” ，然后单击“下一步”。  
  
3.  在 "**选择 no__t-1Alone 或场部署**" 页上 **，单击 "** "，然后单击 "**下一步**"。  
  
    > [!IMPORTANT]  
    > 当你在 AD FS 联合服务器配置向导中选择 "0alone 联合服务器" 选项时，与此联合身份验证服务关联的服务帐户将自动分配到网络服务帐户。 仅建议在测试实验室环境中评估 AD FS 的情况下使用 NETWORK SERVICE 作为服务帐户。 如果你想要使用 "no__t-0alone 联合服务器" 选项在生产环境中部署联合服务器，则必须将此服务帐户更改为可专用于处理请求的更适合的服务帐户，这一点很重要。这一新联合身份验证服务。 将服务帐户更改为除 NETWORK SERVICE 以外的其他帐户将会缓解可能会导致联合服务器易受到恶意攻击的攻击媒介。  
  
4.  在“指定联合身份验证服务名称”页上，验证“SSL 证书”是否显示正确。 如果没有，请从 " **SSL 证书**" 列表中选择相应的证书。  
  
    此证书从默认网站的安全套接字层 \(SSL @ no__t 设置生成。 如果默认网站只具有一个配置的 SSL 证书，则显示该证书并自动选择它以供使用。 如果为默认网站配置了多个 SSL 证书，此处将列出所有这些证书，并且你必须从中进行选择。 如果没有配置默认网站的 SSL 设置，则从本地计算机上的个人证书存储中的可用证书生成列表。  
  
    > [!NOTE]  
    > 如果为 IIS 配置了 SSL 证书，则该向导将不允许覆盖证书。 这可确保之前计划对 SSL 证书所做的任何 IIS 配置将得以保留。 若要解决此限制，可以删除证书或使用 IIS 管理控制台手动对其重新进行配置。  
  
5.  如果您选择的 AD FS 数据库已经存在，则会出现 "**检测到现有 AD FS 配置数据库**" 页。 如果显示该页，则单击“删除数据库”，然后单击“下一步”。  
  
    > [!CAUTION]  
    > 仅当你确定此 AD FS 数据库中的数据不重要，或者不在生产联合服务器场中使用时，才选择此选项。  
  
6.  在“已准备好应用设置”页上，查看详细信息。 如果设置正确，请单击 "**下一步**" 以开始配置具有这些设置的 AD FS。  
  
7.  在“配置结果” 页上，查看结果。 完成所有配置步骤后，单击“关闭”  以退出向导。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  

