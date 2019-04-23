---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: 为联合服务器代理角色配置计算机
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861798"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>为联合服务器代理角色配置计算机

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用所需的证书配置计算机并安装联合身份验证服务代理角色服务后，你现可配置要成为联合服务器代理的计算机。 可以使用以下过程，使计算机承担联合服务器代理角色。  
  
> [!IMPORTANT]  
> 使用此过程来配置联合服务器代理计算机之前，请确保遵循中的所有步骤[核对清单：设置了联合身份验证服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)中所列的顺序。 确保至少部署一台联合服务器，并且实施了用于对联合服务器代理配置进行授权的所有必需凭据。 你还必须配置安全套接字层\(SSL\)绑定在默认网站或此向导将不会启动。 必须先完成所有这些任务，然后此联合服务器代理才可正常工作。  
  
完成设置计算机之后，验证联合服务器代理按预期方式工作。 有关详细信息，请参阅[验证联合服务器代理是否正常运行](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>配置计算机以用于联合服务器代理角色  
  
1.  有两种方法启动 AD FS 联合身份验证服务器配置向导。 若要启动该向导，请执行下列操作之一：  
  
    -   上**启动**屏幕上，键入**AD FS 联合服务器代理配置向导**，然后按 ENTER。  
  
    -   安装向导已完成，请打开 Windows 资源管理器后，随时导航到**c:\\Windows\\ADFS**文件夹，并再双击\-单击**FspConfigWizard.exe**.  
  
2.  使用这两种方法中的任意一种，启动该向导，然后在“欢迎使用”页上，单击“下一步”。  
  
3.  在“指定联合身份验证服务名称”页上，在“联合身份验证服务名称”下，键入表示联合身份验证服务的名称，此计算机将为其以代理角色进行操作。  
  
4.  根据你的特定网络要求，确定是否将需要使用 HTTP 代理服务器将请求转发到联合身份验证服务。 如果是这样，则选中“将请求发送到此联合身份验证服务时使用 HTTP 代理服务器”  复选框、在“HTTP 代理服务器地址”  下键入代理服务器的地址、单击“测试连接”  以验证连接性，然后单击“下一步” 。  
  
5.  在收到提示时，指定在此联合服务器代理和联合身份验证服务之间建立信任所需的凭据。  
  
    默认情况下，使用联合身份验证服务或本地内置的成员的服务帐户\\管理员组可以授权联合服务器代理。  
  
6.  在“已准备好应用设置”页上，查看详细信息。 如果设置正确，请单击“下一步”以开始使用这些代理设置配置此计算机。  
  
7.  在“配置结果”  页上，查看结果。 完成所有配置步骤后，单击“关闭”   以退出向导。  
  
    没有 Microsoft 管理控制台\(MMC\)对齐\-中来管理联合身份验证服务器代理。 若要在组织中配置为每个联合服务器代理的设置，请使用 Windows PowerShell cmdlet。  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>配置备用 TCP\/IP 端口用于代理操作  
默认情况下，联合服务器代理服务配置为使用 TCP 端口 443 用于 HTTPS 流量，端口 80 用于 HTTP 流量以与联合身份验证服务器的通信进行。 若要配置不同的端口，例如对应 HTTPS 的 TCP 端口 444 和对应 HTTP 的端口 81，则必须完成以下任务。  
  
> [!NOTE]  
> 如果你想要一开始部署 AD FS 以使用您在备用 TCP 运行\/IP 端口，您应首先修改 IIS 协议绑定中对应 HTTP 和 HTTPS 联合身份验证服务器和联合服务器代理计算机上的端口。 在运行 AD FS 配置向导以进行初始配置之前应发生此情况。 如果配置 Internet Information Services \(IIS\)第一个备用 TCP\/向导时，会发现 IP 端口设置\-基于的配置出现在 AD FS 中，并且不是下面的过程必需。 如果你想要以后更改端口设置，应首先更新 IIS 协议绑定，然后使用以下过程来相应地更新端口设置。 有关编辑 IIS 绑定的详细信息，请参阅[文章 149605](https://go.microsoft.com/fwlink/?LinkId=190275) Microsoft 知识库中。  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>若要配置备用 TCP\/要使用的联合身份验证服务器代理的 IP 端口  
  
1.  配置联合服务器以使用非默认端口。  
  
    若要执行此操作，包括其与通过指定非默认端口号*HttpsPort*并*HttpPort*选项的一部分**设置\-ADFSProperties** cmdlet。 例如，若要配置这些端口，请在联合身份验证服务器计算机的 Windows PowerShell 会话中使用以下命令：  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  配置联合服务器代理为使用非默认端口。  
  
    若要执行此操作，包括其与通过指定非默认端口号*HttpsPort*并*HttpPort*选项的一部分**设置\-ADFSProxyProperties**cmdlet。 例如，若要配置这些端口，请在联合身份验证服务器计算机的 Windows PowerShell 会话中使用以下命令：  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > 对于联合服务器代理服务默认情况下不启用终结点 Url。 如果您在配置新的联合身份验证服务器安装，必须先启用联合身份验证服务器代理服务终结点。 例如，假定，此过程中的示例引用的所有终结点已启用这些代理通过 AD FS 管理管理单元中选择\-中，然后选择**代理上启用**。  
  
3.  更新联合服务器代理上的 IIS 安装，使该安全断言标记语言\(SAML\)和 WS\-信任终结点配置为反映更新的端口号。 若要执行此操作，可以使用记事本来修改 Web.config 文件中，位于系统驱动器 %中的以下\\inetpub\\adfs\\ls\\联合身份验证服务器代理计算机上。 例如，假设有一个名为 sts1.contoso.com 的联合身份验证服务器和新的端口号是 444，浏览到和在联合服务器代理计算机上在记事本中打开 Web.config 文件，找到以下部分，修改作为端口号下面，突出显示然后保存并退出记事本。  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  将联合身份验证服务器代理服务用户帐户添加到访问控制列表\(ACL\)相关终结点 url。 例如，如果端口号是 1234年和用于运行 AD FSfederation 服务器代理的用户帐户下的服务是内置\-在网络服务帐户中，在命令提示符下键入以下命令：  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    必须在联合身份验证服务器和联合服务器代理计算机上运行上述命令。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

