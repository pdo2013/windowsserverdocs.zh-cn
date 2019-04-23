---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: 使用 SharePoint、Exchange 和 RDG 发布应用程序
description: ''
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: 21ea6ecf717392027c1d00f818e230e2fe56a11d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826818"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>使用 SharePoint、Exchange 和 RDG 发布应用程序

>适用于：Windows Server 2016

**此内容是相关的本地版本的 Web 应用程序代理。若要支持通过云对本地应用程序的安全访问，请参阅[Azure AD 应用程序代理内容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**  
  
本主题介绍将发布 SharePoint Server、 Exchange Server 或通过 Web 应用程序代理的远程桌面网关 (RDP) 所需的任务。  

>[!NOTE]
>为提供此信息的是。  远程桌面服务支持，建议使用[Azure 应用代理提供对安全远程访问本地应用程序](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)。
  
## <a name="BKMK_6.1"></a>发布 SharePoint Server  
SharePoint 站点配置为基于声明的身份验证或集成 Windows 身份验证时，你可以发布 Web 应用程序代理通过 SharePoint 站点。 如果你想要使用预身份验证的 Active Directory 联合身份验证服务 (AD FS)，必须配置信赖方使用的向导之一。  
  
-   如果 SharePoint 站点使用基于声明的身份验证，则必须使用添加信赖方信任向导来配置用于身份验证的信赖方信任。  
  
-   如果 SharePoint 站点使用集成 Windows 身份验证，则必须使用添加非基于声明的信赖方信任向导来配置用于身份验证的信赖方信任。 假如配置 KDC，可以将 IWA 与基于声明的 Web 应用程序一起使用。  
  
    若要允许用户使用集成 Windows 身份验证进行身份验证，Web 应用程序代理服务器必须加入到域中。  
  
    必须将应用程序配置为支持 Kerberos 约束委派。 可以在域控制器上针对任何应用程序执行此操作。 如果在 Windows Server 2012 R2 或 Windows Server 2012 上运行，还可以直接在后端服务器上配置应用程序。 有关详细信息，请参阅 [Kerberos 身份验证的新增功能](https://technet.microsoft.com/library/hh831747.aspx)。 您还必须确保，将 Web 应用程序代理服务器配置为委派给服务主体名称的后端服务器。 有关如何配置 Web 应用程序代理发布使用集成 Windows 身份验证的应用程序的演练，请参阅[将站点配置为使用集成 Windows 身份验证](assetId:///b0788958-627f-450f-877c-209b1bd0db52)。  
  
如果使用备用访问映射 (AAM) 或主机命名的站点集来配置 SharePoint 站点，则可以使用不同的外部 URL 和后端服务器 URL 来发布应用程序。 但是，如果未使用 AAM 或主机命名的站点集配置 SharePoint 站点，则必须使用相同的外部 URL 和后端服务器 URL。  
  
## <a name="BKMK_6.2"></a>发布 Exchange Server  
下表描述了可以通过 Web 应用程序代理和这些服务的支持的预身份验证发布的 Exchange 服务：  
  
|Exchange 服务|预身份验证|说明|  
|--------------------|-----------------------|---------|  
|Outlook Web App|的使用非基于声明的身份验证 AD FS<br />-直通<br />AD FS 基于声明的身份验证用于本地 Exchange 2013 Service Pak 1 (SP1)|有关详细信息，请参阅：[结合使用 AD FS 基于声明的身份验证与 Outlook Web App和 EAC](https://go.microsoft.com/fwlink/?LinkId=393723)|  
|Exchange 控制面板|传递||  
|Outlook Anywhere|传递|必须为 Outlook Anywhere 发布三个 URL，以使其正常工作：<br /><br />-自动发现 URL。<br />-Exchange Server; 外部主机名也就是说，为客户端连接到配置的 URL。<br />的 Exchange Server 内部 FQDN。|  
|Exchange ActiveSync|传递<br/> AD FS 使用 HTTP 基本授权协议|| 有关详细信息，请参阅： https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/publishing-applications-using-ad-fs-preauthentication 
  
若要使用集成 Windows 身份验证发布 Outlook Web App，必须使用“添加非基于声明的信赖方信任向导”为应用程序配置信赖方信任。  
  
若要允许用户进行身份验证使用 Kerberos 约束的委派的 Web 应用程序代理服务器必须加入到域。  
  
必须配置要支持 Kerberos 身份验证的应用程序。 此外你需要注册服务主体名称 (SPN) 下运行 web 服务的帐户。 可以在域控制器上或后端服务器上执行此操作。 在负载平衡的 Exchange 环境，这需要使用备用服务帐户，请参阅[的负载平衡客户端访问服务器配置 Kerberos 身份验证](https://technet.microsoft.com/library/ff808312(v=exchg.150).aspx)  
  
如果在 Windows Server 2012 R2 或 Windows Server 2012 上运行，还可以直接在后端服务器上配置应用程序。 有关详细信息，请参阅 [Kerberos 身份验证的新增功能](https://technet.microsoft.com/library/hh831747.aspx)。 您还必须确保，将 Web 应用程序代理服务器配置为委派给服务主体名称的后端服务器。  
  
## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>发布 Web 应用程序代理通过远程桌面网关  
如果你想要限制对远程访问网关的访问并添加用于远程访问的预身份验证，你可以部署，通过 Web 应用程序代理。 这是一个好方法，以确保具有丰富的预身份验证，用于包括 MFA 的 RDG。 不带预身份验证的发布也是一个选项，并提供单一到系统的入口点。  
  
#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>如何在 RDG 使用直通身份验证 Web 应用程序代理发布应用程序  
  
1.  将不同取决于是否安装 RD Web 访问 (/ rdweb) 和 RD 网关 (rpc) 角色是在同一服务器上还是在不同的服务器上。  
  
2.  如果 RD Web 访问和 RD 网关角色相同的 RDG 服务器上托管，则可以只需发布根 FQDN 中 Web 应用程序代理如 https://rdg.contoso.com/。  
  
    您还可以将发布两个虚拟目录单独例如 https://rdg.contoso.com/rdweb/和 https://rdg.contoso.com/rpc/。  
  
3.  如果单独 RDG 服务器上托管 RD Web 访问和 RD 网关，你必须单独发布两个虚拟目录。 您可以使用相同或不同外部 FQDN，例如 https://rdweb.contoso.com/rdweb/和 https://gateway.contoso.com/rpc/。  
  
4.  如果不同的外部和内部 FQDN 不应禁用 RDWeb 发布规则上的请求标头转换。 这可以通过 Web 应用程序代理服务器上运行以下 PowerShell 脚本，但默认情况下应启用它。
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false  
    ```  
  
    > [!NOTE]  
    > 如果你需要支持丰富的客户端，如 RemoteApp 和桌面连接或 iOS 的远程桌面连接，这些不支持预身份验证，因此你必须发布 RDG 使用直通身份验证。  
  
#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>如何在 RDG 使用使用预身份验证的 Web 应用程序代理发布应用程序  
  
1.  Web 应用程序代理预身份验证与 RDG 即可使用传递预身份验证 cookie 被传递到远程桌面连接客户端 (mstsc.exe) 的 Internet explorer 中获取。 这是然后使用远程桌面连接客户端 (mstsc.exe)。 这是然后用作通过远程桌面连接客户端身份验证的凭据。  
  
    以下过程说明要发送到客户端在远程应用 RDP 文件中包括必要的自定义 RDP 属性的集合服务器。 这将告诉客户端的预身份验证是必需的以传递预身份验证 cookie 服务器地址到远程桌面连接客户端 (mstsc.exe)。 在与禁用 Web 应用程序代理应用程序上的 HttpOnly 功能结合使用，它允许远程桌面连接客户端 (mstsc.exe) 来利用通过浏览器获得的 Web 应用程序代理 cookie。  
  
    RD Web 访问服务器身份验证将仍使用 RD Web 访问窗体登录。 这提供了最少数量的用户身份验证提示，如 RD Web 访问登录窗体创建客户端的凭据存储，然后可以使用任何后续的远程应用程序启动的远程桌面连接客户端 (mstsc.exe)。  
  
2.  首先，创建了手动信赖方信任 AD FS 中像您已发布的声明感知应用。 这意味着，您需要创建虚拟信赖方信任是强制执行预身份验证，以便获取对发布服务器而无需通过 Kerberos 约束委派的预身份验证。 一旦用户进行身份验证，通过传递所有其他内容。  
  
    > [!WARNING]  
    > 这可能看起来，使用委派适合，但不能完全解决 mstsc SSO 要求，并且有问题时委派到 /rpc 目录，因为客户端希望处理 RD 网关身份验证本身。  
  
3.  若要创建手动信赖方信任，请在 AD FS 管理控制台中的步骤：  
  
    1.  使用**添加信赖方信任**向导  
  
    2.  选择**手动输入有关信赖方的数据**。  
  
    3.  接受所有默认设置。  
  
    4.  对于信赖方信任标识符，输入将用于 RDG 访问，例如外部 FQDN https://rdg.contoso.com/。  
  
        这是在 Web 应用程序代理发布应用时，将使用的信赖方信任。  
  
4.  发布站点的根目录 (例如， https://rdg.contoso.com/ ) Web 应用程序代理中。 将预身份验证设置为 AD FS 并使用上面创建的信赖方信任。 这将使 /rdweb 和 /rpc 使用相同的 Web 应用程序代理身份验证 cookie。  
  
    就可以将 /rdweb 和 /rpc 发布为单独的应用程序，甚至使用不同的已发布的服务器。 只需确保发布同时使用同一个信赖方信任的 Web 应用程序代理令牌的信赖方信任的颁发和无效，因此在发布使用的同一个信赖方信任的应用程序。  
  
5.  如果不同的外部和内部 FQDN 不应禁用 RDWeb 发布规则上的请求标头转换。 这可以通过 Web 应用程序代理服务器上运行以下 PowerShell 脚本，但默认情况下应启用它：
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false 
    ```  
  
6.  禁用 HttpOnly cookie 属性中 RDG 上的 Web 应用程序代理发布应用程序。 若要允许对 Web 应用程序代理身份验证 cookie 的 RDG ActiveX 控件访问权限，则需要禁用该 Web 应用程序代理 cookie HttpOnly 属性。  
  
    这要求以下命令以安装[Web 应用程序代理修补程序](https://support.microsoft.com/en-gb/kb/3000850)或[ https://support.microsoft.com/en-gb/kb/3000850 ](https://support.microsoft.com/en-gb/kb/3000850)。  
  
    安装此修补程序后指定相关的应用程序名称在 Web 应用程序代理服务器上运行以下 PowerShell 脚本：  
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true  
    ```  
  
    如果禁用 HttpOnly 允许对 Web 应用程序代理身份验证 cookie 的 RDG ActiveX 控件访问权限。  
  
7.  在集合服务器以允许远程桌面连接客户端 (mstsc.exe) 知道预身份验证需要 rdp 文件中配置的相关 RDG 集合。  
  
    -   Windows Server 2012 和 Windows Server 2012 R2 中可以通过 RDG 收集服务器上运行以下 PowerShell cmdlet 完成此：  
  
        ```
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```
  
        请确保删除 < 和 > 括号内时，将替换为你自己的值，例如：
        
        ```
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```
  
    -   在 Windows Server 2008 R2 中：  
  
        1.  登录到终端服务器使用具有管理员权限的帐户。  
  
        2.  转到**启动** >**管理工具** > **终端服务** > **TS RemoteApp 管理器。**  
  
        3.  在中**概述**窗格中的 TS RemoteApp 管理器中，旁边 RDP 设置，单击**更改**。  
  
        4.  上**自定义 RDP 设置**选项卡上，在自定义 RDP 设置中键入以下的 RDP 设置：  
  
            `pre-authentication server address: s: https://externalfqdn/rdweb/`  
  
            `require pre-authentication:i:1`  
  
        5.  完成后，单击**应用**。  
  
            这将告知要发送到客户端在 RDP 文件中包括自定义 RDP 属性的集合服务器。 这将告诉客户端的预身份验证是必需的以传递预身份验证 cookie 服务器地址到远程桌面连接客户端 (mstsc.exe)。 此操作，请结合 Web 应用程序代理应用程序，禁用 HttpOnly 允许远程桌面连接客户端 (mstsc.exe) 来利用通过浏览器获得的 Web 应用程序代理身份验证 cookie。  
  
            有关 RDP 的详细信息，请参阅[配置 TS 网关 OTP 方案](https://technet.microsoft.com/library/cc731249(v=ws.10).aspx)。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [规划使用 Web 应用程序代理发布应用程序](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))  
  
-   [Web 应用程序代理故障排除](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))  
  
-   [Web 应用程序代理操作实例指南](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))  
