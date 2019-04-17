---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: "将计算机配置为联合身份验证的服务器代理角色"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>将计算机配置为联合身份验证的服务器代理角色

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

使用所需的证书配置计算机，并且安装了 Service 联合身份验证的代理角色服务后，现在可以将计算机变得联合身份验证的服务器代理配置。 你可以使用下面的过程，以便计算机以联合身份验证的服务器代理角色进行操作。  
  
> [!IMPORTANT]  
> 你可以使用此过程配置联合身份验证的服务器代理计算机之前，请确保你已按照中的步骤操作[清单：设置向上联盟服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)按顺序列出了他们。 请确保部署该至少一个联合身份验证的服务器和授权服务器联合身份验证的代理配置的都凭据所有必要实现。 您还必须配置安全套接字层 \(SSL\) 绑定在默认网站上，或将不会启动该向导。 所有这些任务必须此联合身份验证的服务器代理才能完成。  
  
完成设置计算机后，可验证联合身份验证的服务器代理工作正常。 有关详细信息，请参阅[验证联盟服务器代理是运营](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>若要配置为联合身份验证的服务器代理角色计算机  
  
1.  有两种方法可以启动广告 FS 联合身份验证的服务器配置向导。 若要启动该向导，请执行下列情况之一：  
  
    -   在**开始**屏幕上，键入**广告 FS 联盟服务器的代理配置向导**，然后按 ENTER。  
  
    -   Anytime 完成，打开 Windows 资源管理器设置向导后，请导航到**C:\\Windows\\ADFS**文件夹，然后 double\ 单击**FspConfigWizard.exe**。  
  
2.  使用哪种方法，启动该向导中，并在**欢迎**页上，单击**下一步**。  
  
3.  在**指定联合身份验证服务名称**页面上下,**联合身份验证服务名称**，键入表示联合身份验证服务用于代理角色起作用，这台计算机的名称。  
  
4.  根据你的特定网络要求，确定你将需要使用 HTTP 代理服务器请求转发给联合身份验证服务。 如果是这样，请选择**发送到此联合身份验证服务请求时，使用 HTTP 代理服务器**下复选框，**HTTP 代理服务器地址**键入代理服务器地址，请单击**测试连接**以验证连接，然后单击**下一步**。  
  
5.  当系统提示你指定建立信任此联合身份验证的服务器代理之间联合身份验证服务所需的凭据。  
  
    默认情况下，在使用联合身份验证服务或的本地 BUILTIN\\Administrators 组成员的服务帐户可以授权联合身份验证的服务器代理。  
  
6.  在**应用设置为随时**页上，检查的详细信息。 如果似乎正确设置，请单击**下一步**开始配置通过这些代理服务器设置此计算机。  
  
7.  在**配置结果**页上，检查结果。 完成所有配置步骤后，请单击**关闭**退出向导。  
  
    没有任何 Microsoft 管理控制台 \(MMC\) snap\-中管理联合身份验证的服务器 proxys 使用。 若要在你的组织中配置每个联合身份验证的服务器 proxys 设置，请使用 Windows PowerShell cmdlet。  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>代理操作配置备用 TCP\/IP 端口  
默认情况下，联合身份验证的服务器代理服务配置为使用 TCP 端口 443 HTTPS 通信和端口 80 HTTP 与联盟服务器的通信的交通。 配置不同端口，如 TCP 端口 HTTPS 的 444 和端口 81 http，必须先完成以下的任务。  
  
> [!NOTE]  
> 如果你想要最初部署广告 FS 备用 TCP\/IP 端口下运行，你首先应中的联合身份验证的服务器和联合身份验证的服务器代理计算机上 HTTP 和 HTTPS 你 IIS 协议绑定修改端口。 在初始配置广告 FS 配置向导应发生此情况。 如果你首次配置 Internet 信息服务 \(IIS\)，本向导会 \ 基于配置发生内的广告 FS 且下面的过程中没有必要时发现备用 TCP\/IP 端口设置。 如果你想要以后更改端口设置，更新 IIS 协议绑定第一个，然后使用以下步骤来更新端口设置相应地。 编辑 IIS 绑定的详细信息，请参阅[文章 149605](https://go.microsoft.com/fwlink/?LinkId=190275) Microsoft 知识库文章中。  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>配置使用联盟服务器代理备用 TCP\/IP 端口  
  
1.  配置使用而非默认端口联盟服务器。  
  
    若要执行此操作，方法是将其与指定非默认端口号*HttpsPort*和*HttpPort*作为选项**Set\ ADFSProperties** cmdlet。 例如，若要配置这些端口，在联合身份验证的服务器上的 Windows PowerShell 会话中使用以下命令：  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  配置联合身份验证的服务器代理服务器，若要使用默认端口。  
  
    若要执行此操作，方法是将其与指定非默认端口号*HttpsPort*和*HttpPort*作为选项**Set\ ADFSProxyProperties** cmdlet。 例如，若要配置这些端口，在联合身份验证的服务器上的 Windows PowerShell 会话中使用以下命令：  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > 默认情况下联合身份验证的服务器代理服务未启用端点 Url。 如果配置新联合身份验证的服务器安装时，你必须首次启用联合身份验证的服务器代理服务端点。 例如，假设，此过程中的示例是指的所有端点为你启用它们代理服务器 snap\ 中广告 FS 管理中选择它们，然后选择**代理服务器上启用**。  
  
3.  更新 IIS 安装在联合身份验证的服务器代理服务器，以便配置安全肯定标记语言 \(SAML\) 和 WS\ 信任端点以反映更新的端口号。 若要执行此操作，可以使用记事本修改以下位于 systemdrive%\\inetpub\\adfs\\ls\\ 联合身份验证的服务器代理计算机上的 Web.config 文件中。 例如，假设你有一个名为 sts1.contoso.com 的联盟服务器，并且新端口号 444，浏览和联合身份验证的服务器代理计算机上在记事本中打开 Web.config 文件，找到以下部分、修改端口号作为下方，突出显示，然后保存退出记事本。  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  添加联盟服务器代理服务用户帐户访问控制相关的端点 url 列表 \(ACL\)。 例如，如果端口号 1234 年并且用于运行广告 FSfederation 服务器代理服务用户帐户 built\ 在网络服务帐户，请在命令提示符中键入以下命令：  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    上一个命令必须联合身份验证的服务器和联合身份验证的服务器代理计算机上运行。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合身份验证的服务器代理服务器设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

