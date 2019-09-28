---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: 使用 SharePoint、Exchange 和 RDG 发布应用程序
description: ''
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: 0e5c9779fae66e7edec3c1bba471d5cadbe48a72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404235"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>使用 SharePoint、Exchange 和 RDG 发布应用程序

>适用于：Windows Server 2016

@no__t 0This 内容与 Web 应用程序代理的本地版本相关。若要启用对云中的本地应用程序的安全访问，请参阅[Azure AD 应用程序代理内容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。 **  

本主题介绍通过 Web 应用程序代理发布 SharePoint Server、Exchange Server 或远程桌面网关（RDP）所需执行的任务。  

>[!NOTE]
>此信息按原样提供。  远程桌面服务支持并建议使用[Azure 应用代理来提供对本地应用程序的安全远程访问](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)。

## <a name="BKMK_6.1"></a>发布 SharePoint 服务器  
当 SharePoint 站点配置为基于声明的身份验证或集成 Windows 身份验证时，您可以通过 Web 应用程序代理发布 SharePoint 站点。 如果要使用 Active Directory 联合身份验证服务（AD FS）进行预身份验证，则必须使用某个向导配置信赖方。  

-   如果 SharePoint 站点使用基于声明的身份验证，则必须使用添加信赖方信任向导来配置用于身份验证的信赖方信任。  

-   如果 SharePoint 站点使用集成 Windows 身份验证，则必须使用添加非基于声明的信赖方信任向导来配置用于身份验证的信赖方信任。 假如配置 KDC，可以将 IWA 与基于声明的 Web 应用程序一起使用。  

    若要允许用户使用集成 Windows 身份验证进行身份验证，必须将 Web 应用程序代理服务器加入域。  

    必须将应用程序配置为支持 Kerberos 约束委派。 可以在域控制器上针对任何应用程序执行此操作。 如果在后端服务器上运行的是 Windows Server 2012 R2 或 Windows Server 2012，还可以直接在后端服务器上配置应用程序。 有关详细信息，请参阅 [Kerberos 身份验证的新增功能](https://technet.microsoft.com/library/hh831747.aspx)。 你还必须确保将 Web 应用程序代理服务器配置为委派给后端服务器的服务主体名称。 有关如何配置 Web 应用程序代理以发布使用集成 Windows 身份验证的应用程序的演练，请参阅[将站点配置为使用集成 windows 身份验证](assetId:///b0788958-627f-450f-877c-209b1bd0db52)。  

如果使用备用访问映射 (AAM) 或主机命名的站点集来配置 SharePoint 站点，则可以使用不同的外部 URL 和后端服务器 URL 来发布应用程序。 但是，如果未使用 AAM 或主机命名的站点集配置 SharePoint 站点，则必须使用相同的外部 URL 和后端服务器 URL。  

## <a name="BKMK_6.2"></a>发布 Exchange Server  
下表描述了可通过 Web 应用程序代理发布的 Exchange 服务，以及支持这些服务的预身份验证：  


|    Exchange 服务    |                                                                            预身份验证                                                                            |                                                                                                                                       说明                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | -使用非基于声明的身份验证 AD FS<br />-传递<br />-AD FS 对本地 Exchange 2013 Service Pak 1 （SP1）使用基于声明的身份验证 |                                                                  有关详细信息，请参阅：[结合使用 AD FS 基于声明的身份验证与 Outlook Web App和 EAC](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Exchange 控制面板 |                                                                               直通                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook Anywhere    |                                                                               直通                                                                               | 必须为 Outlook Anywhere 发布三个 URL，以使其正常工作：<br /><br />-自动发现 URL。<br />-Exchange 服务器的外部主机名;即，配置为供客户端连接到的 URL。<br />-Exchange Server 的内部 FQDN。 |
|  Exchange ActiveSync   |                                                     直通<br/> 使用 HTTP 基本授权协议的 AD FS                                                      |                                                                                                                                                                                                                                                                                    |

若要使用集成 Windows 身份验证发布 Outlook Web App，必须使用“添加非基于声明的信赖方信任向导”为应用程序配置信赖方信任。  

若要允许用户使用 Kerberos 约束委派进行身份验证，必须将 Web 应用程序代理服务器加入域。  

您必须将应用程序配置为支持 Kerberos 身份验证。 此外，还需要将服务主体名称（SPN）注册到运行 web 服务的帐户。 你可以在域控制器上或在后端服务器上执行此操作。 在负载平衡的 Exchange 环境中，这需要使用备用服务帐户，请参阅[为负载平衡的客户端访问服务器配置 Kerberos 身份验证](https://technet.microsoft.com/library/ff808312(v=exchg.150).aspx)  

如果在后端服务器上运行的是 Windows Server 2012 R2 或 Windows Server 2012，还可以直接在后端服务器上配置应用程序。 有关详细信息，请参阅 [Kerberos 身份验证的新增功能](https://technet.microsoft.com/library/hh831747.aspx)。 你还必须确保将 Web 应用程序代理服务器配置为委派给后端服务器的服务主体名称。  

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>通过 Web 应用程序代理发布远程桌面网关  
如果要限制对远程访问网关的访问并为远程访问添加预身份验证，则可以通过 Web 应用程序代理将其回滚。 这是一种非常好的方法，可确保具有 RDG 的丰富预身份验证，包括 MFA。 也可以选择不使用预先身份验证进行发布，并提供系统中的单一入口点。  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>如何使用 Web 应用程序代理通过身份验证在 RDG 中发布应用程序  

1. 安装将有所不同，具体取决于 RD Web 访问（/rdweb）和 RD 网关（rpc）角色是位于同一服务器上还是不同服务器上。  

2. 如果 RD Web 访问和 RD 网关角色托管在同一 RDG 服务器上，则只需在 Web 应用程序代理中发布根 FQDN，如， https://rdg.contoso.com/ 。  

   你还可以分别发布两个虚拟目录，例如 <https://rdg.contoso.com/rdweb/> 和 https://rdg.contoso.com/rpc/ 。  

3. 如果 RD Web 访问和 RD 网关托管在单独的 RDG 服务器上，则必须单独发布两个虚拟目录。 你可以使用相同或不同的外部 FQDN （例如 https://rdweb.contoso.com/rdweb/ 和 https://gateway.contoso.com/rpc/ ）。  

4. 如果外部和内部 FQDN 不同，则不应在 RDWeb 发布规则上禁用请求标头转换。 可以通过在 Web 应用程序代理服务器上运行以下 PowerShell 脚本来完成此操作，但默认情况下应启用此功能。

   ```  
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false  
   ```  

   > [!NOTE]  
   > 如果需要支持丰富的客户端（例如 RemoteApp 和桌面连接）或 iOS 远程桌面连接，则这些客户端不支持预身份验证，因此你必须使用传递身份验证发布 RDG。  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>如何在 RDG 中使用具有预身份验证的 Web 应用程序代理发布应用程序  

1.  使用 RDG 的 Web 应用程序代理预身份验证通过将 Internet Explorer 获得的预身份验证 cookie 传递到远程桌面连接客户端（mstsc）来实现。 然后，远程桌面连接客户端（mstsc）使用此方法。 然后，远程桌面连接客户端将其用作身份验证的证明。  

    以下过程告知收集服务器在发送到客户端的远程应用 RDP 文件中包含必要的自定义 RDP 属性。 这将告知客户端需要预身份验证，并将预身份验证服务器地址的 cookie 传递给远程桌面连接的客户端（mstsc）。 除了禁用 Web 应用程序代理应用程序上的 HttpOnly 功能，这允许远程桌面连接的客户端（mstsc）利用通过浏览器获取的 Web 应用程序代理 cookie。  

    对 RD Web 访问服务器的身份验证仍将使用 RD Web 访问窗体登录。 这将提供最少数量的用户身份验证提示，因为 RD Web 访问登录窗体将创建客户端凭据存储，然后远程桌面连接客户端（mstsc）将该存储用于任何后续的远程应用程序启动。  

2.  首先，在 AD FS 中创建手动信赖方信任，就像发布声明感知应用一样。 这意味着你必须创建一个虚拟信赖方信任，以便强制执行预身份验证，以便你可以获得预身份验证，而无需对发布的服务器进行 Kerberos 约束委派。 用户经过身份验证后，其他所有内容都将通过。  

    > [!WARNING]  
    > 这似乎是使用委托的方法，但它并不能完全解决 mstsc SSO 要求，并在委派到/rpc 目录时出现问题，因为客户端希望处理 RD 网关身份验证本身。  

3.  若要创建手动信赖方信任，请按照 AD FS 管理控制台中的步骤操作：  

    1.  使用 "**添加信赖方信任**向导"  

    2.  选择 **"手动输入信赖方数据"** 。  

    3.  接受所有默认设置。  

    4.  对于信赖方信任标识符，输入将用于 RDG 访问的外部 FQDN，例如 https://rdg.contoso.com/ 。  

        这是你在 Web 应用程序代理中发布应用时将使用的信赖方信任。  

4.  在 Web 应用程序代理中发布站点的根目录（例如 https://rdg.contoso.com/ ）。 将 "预身份验证" 设置为 "AD FS"，并使用前面创建的信赖方信任。 这会使/rdweb 和/rpc 使用相同的 Web 应用程序代理身份验证 cookie。  

    可以将/rdweb 和/rpc 发布为单独的应用程序，甚至可以使用不同的已发布服务器。 你只需确保使用相同的信赖方信任进行发布，因为为信赖方信任颁发了 Web 应用程序代理令牌，因此，在使用相同的信赖方信任发布的应用程序之间有效。  

5.  如果外部和内部 FQDN 不同，则不应在 RDWeb 发布规则上禁用请求标头转换。 可以通过在 Web 应用程序代理服务器上运行以下 PowerShell 脚本来完成此操作，但默认情况下应启用此脚本：

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false 
    ```  

6.  在 RDG 发布的应用程序上禁用 Web 应用程序代理中的 HttpOnly cookie 属性。 若要允许 RDG ActiveX 控件访问 Web 应用程序代理的身份验证 cookie，必须禁用 Web 应用程序代理 cookie 上的 HttpOnly 属性。  

    这需要安装[Web 应用程序代理修补程序](https://support.microsoft.com/en-gb/kb/3000850)或[https://support.microsoft.com/en-gb/kb/3000850](https://support.microsoft.com/en-gb/kb/3000850)。  

    安装修补程序后，请在 Web 应用程序代理服务器上运行以下 PowerShell 脚本，并指定相关的应用程序名称：  

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true  
    ```  

    禁用 HttpOnly 允许 RDG ActiveX 控件对 Web 应用程序代理身份验证 cookie 的访问。  

7.  在集合服务器上配置相关的 RDG 集合，使远程桌面连接的客户端（mstsc）知道 rdp 文件中需要预身份验证。  

    -   在 Windows Server 2012 和 Windows Server 2012 R2 中，可以通过在 RDG 收集服务器上运行以下 PowerShell cmdlet 来完成此操作：  

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        请确保在将替换为自己的值时删除 < 和 > 方括号，例如：

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   在 Windows Server 2008 R2 中：  

        1.  使用具有管理员权限的帐户登录到终端服务器。  

        2.  请参阅**开始** >**管理工具** > **终端服务** > **TS RemoteApp Manager。**  

        3.  在 TS RemoteApp Manager 的 "**概述**" 窗格中，单击 "RDP 设置" 旁边的 "**更改**"。  

        4.  在 "**自定义 Rdp 设置**" 选项卡上，在 "自定义 rdp 设置" 框中键入以下 RDP 设置：  

            `pre-authentication server address: s: https://externalfqdn/rdweb/`  

            `require pre-authentication:i:1`  

        5.  完成后，请单击 "**应用**"。  

            这会告知收集服务器在发送到客户端的 RDP 文件中包含自定义 RDP 属性。 这将告知客户端需要预身份验证，并将预身份验证服务器地址的 cookie 传递到远程桌面连接客户端（mstsc）。 这与在 Web 应用程序代理应用程序上禁用 HttpOnly 的结合允许远程桌面连接客户端（mstsc）利用通过浏览器获取的 Web 应用程序代理身份验证 cookie。  

            有关 RDP 的详细信息，请参阅[配置 TS 网关 OTP 方案](https://technet.microsoft.com/library/cc731249(v=ws.10).aspx)。  

## <a name="BKMK_Links"></a>另请参阅  

-   [规划使用 Web 应用程序代理发布应用程序](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))  

-   [Web 应用程序代理疑难解答](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))  

-   [Web 应用程序代理操作实例指南](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))  
