---
title: Web 应用程序代理疑难解答
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.assetid: a2fef55d-747b-4e20-8f21-5f8807e7ef87
ms.technology: web-app-proxy
ms.openlocfilehash: c772b2717ebca02f972027dccb92abd8d69bea6a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388020"
---
# <a name="troubleshooting-web-application-proxy"></a>Web 应用程序代理疑难解答

>适用于：Windows Server 2016

@no__t 0This 内容与 Web 应用程序代理的本地版本相关。若要启用对云中的本地应用程序的安全访问，请参阅[Azure AD 应用程序代理内容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。 **  
  
本部分提供了有关 Web 应用程序代理的疑难解答过程，包括事件解释和解决方案。 有三个位置显示错误：  
  
-   在 Web 应用程序代理管理员控制台中  
  
    管理员控制台中列出的每个事件 ID 都可以在 Windows 事件查看器中查看，并在下面找到相应的说明和解决方案。  
  
    打开事件查看器并在应用程序**和服务日志**下查找与 Web 应用程序代理相关的事件， > **Microsoft** > **Windows** >  **Web 应用程序代理**@no__t  
  
    ![](media/Troubleshooting-Web-Application-Proxy/WebApplicationProxyTroubleshooting.png)  
  
    如果需要，可以通过打开分析和调试日志并打开 Web 应用程序代理会话日志来获取详细日志，该日志位于 "\ Microsoft \ Windows \ Web 应用程序代理 \ 管理员" 下的 Windows 事件查看器中。  
  
-   在 PowerShell 错误中  
  
    在配置期间遇到的问题的事件显示在 PowerShell 中。  
  
    使用标准 PowerShell 错误提示向 PowerShell 用户显示所有错误。 所有 PowerShell 命令都记录为事件。 在 PowerShell 中发生的所有事件都列在 ID 号为12016的 Windows 事件查看器中，并在 PowerShell 部分中定义。  
  
-   在最佳做法分析器  
  
    [Web 应用程序代理的最佳做法分析器](assetId:///995f5cea-e334-4ce6-a14c-6a2d03c0c79a)中介绍了这些事件  
  
## <a name="powershell-messages"></a>PowerShell 消息  
  
|事件或症状|可能的原因|分辨率|  
|--------------------|------------------|--------------|  
|信任证书（"ADFS ProxyTrust-<WAP machine name>"）无效|这种情况可能由以下任何原因引起：<br /><br />-应用程序代理计算机已关闭太长时间。<br />-Web 应用程序代理与 AD FS 之间的断开<br />-证书基础结构问题<br />-AD FS 机上的更改，或者 Web 应用程序代理与 AD FS 之间的续订过程在每8小时未按计划运行，则他们需要续订信任<br />-不同步 Web 应用程序代理计算机和 AD FS 的时钟。|请确保已同步时钟。 运行 Install-webapplicationproxy cmdlet。|  
|在 AD FS 中找不到配置数据|这可能是因为 Web 应用程序代理尚未完全安装，或者是因为 AD FS 数据库中的更改或数据库损坏。|运行 Install-webapplicationproxy Cmdlet|  
|Web 应用程序代理尝试读取 AD FS 中的配置时出错。|这可能表示 AD FS 不可访问，或者 AD FS 在尝试从 AD FS 数据库中读取配置时遇到了内部问题。|验证 AD FS 是否可访问并且工作正常。|  
|AD FS 中存储的配置数据已损坏，或者 Web 应用程序代理无法对其进行分析。<br /><br />或者<br /><br />Web 应用程序 Proxywas 无法从 AD FS 检索信赖方的列表。|如果在 AD FS 中修改了配置数据，则可能会发生这种情况。|重新启动 Web 应用程序 Proxyservice。 如果问题仍然存在，请运行 Install-webapplicationproxy Cmdlet。|  
  
## <a name="administrator-console-events"></a>管理员控制台事件  
以下管理员控制台事件通常表明身份验证错误、无效的 tokes 或已过期的 cookie。  
  
|事件或症状|可能的原因|分辨率|  
|--------------------|------------------|--------------|  
|11005<br /><br />Web 应用程序代理无法使用配置中的机密创建 cookie 加密密钥。|PowerShell 命令更改了全局配置 "AccessCookiesEncryptionKey" 参数：Set-webapplicationproxyconfiguration-RegenerateAccessCookiesEncryptionKey|不需要执行任何操作。 问题 cookie 已被删除，用户已重定向到 STS 进行身份验证。|  
|12000<br /><br />Web 应用程序代理至少无法检查60分钟的配置更改|Web 应用程序代理无法使用命令 Set-webapplicationproxyconfiguration/应用程序访问 Web 应用程序代理配置。 这通常是由于缺少与 AD FS 的连接或需要使用 AD FS 续订信任而造成的。|检查与 AD FS 的连接。 可以使用 link https：//< FQDN_AD_FS_Proxy >/FederationMetadata/2007-06/FederationMetadata.xmlMake 来执行此操作。请确保 AD FS 和 Web 应用程序代理之间建立了信任关系。 如果这些解决方案不起作用，请运行 Install-webapplicationproxy Cmdlet。|  
|12003<br /><br />Web 应用程序代理无法分析访问 cookie。|这可能表示 Web 应用程序代理和 AD FS 未连接，或者它们不接收相同的配置。|检查与 AD FS 的连接。 可以使用 link https：//< FQDN_AD_FS_Proxy >/FederationMetadata/2007-06/FederationMetadata.xmlMake 来执行此操作。请确保 AD FS 和 Web 应用程序代理之间建立了信任关系。 如果这些解决方案不起作用，请运行 Install-webapplicationproxy Cmdlet。|  
|12004<br /><br />Web 应用程序代理接收到具有无效访问 cookie 的请求。|此事件可能表示 Web 应用程序代理和 AD FS 未连接，或者它们不接收相同的配置。<br /><br />如果运行了 "AccessCookiesEncryptionKey" 参数 chaged Set-webapplicationproxyconfiguration-RegenerateAccessCookiesEncryptionKey PowerShell 命令，则此事件是正常的，无需解决方法步骤。|检查与 AD FS 的连接。 可以使用 link https：//< FQDN_AD_FS_Proxy >/FederationMetadata/2007-06/FederationMetadata.xmlMake 来执行此操作。请确保 AD FS 和 Web 应用程序代理之间建立了信任关系。 如果这些解决方案不起作用，请运行 Install-webapplicationproxy Cmdlet。|  
|12008<br /><br />Web 应用程序代理超出了对后端服务器允许的最大 Kerberos 身份验证尝试次数。|此事件可能指示 Web 应用程序代理和后端应用程序服务器之间的配置不正确，或者两台计算机上的时间和日期配置存在问题。|后端服务器拒绝了由 Web 应用程序代理创建的 Kerberos 票证。 验证是否正确配置了 Web 应用程序代理和后端应用程序服务器的配置。<br /><br />请确保 Web 应用程序代理和后端应用程序服务器上的时间和日期配置已同步。|  
|12011<br /><br />Web 应用程序代理接收到具有无效访问 cookie 签名的请求。|此事件可能表示 Web 应用程序代理和 AD FS 未连接，或者它们不接收相同的配置。 如果运行了 "AccessCookiesEncryptionKey" 参数 chaged Set-webapplicationproxyconfiguration-RegenerateAccessCookiesEncryptionKey PowerShell 命令，则此事件是正常的，无需解决方法步骤。|检查与 AD FS 的连接。 可以使用 link https：//< FQDN_AD_FS_Proxy >/FederationMetadata/2007-06/FederationMetadata.xmlMake 来执行此操作。请确保 AD FS 和 Web 应用程序代理之间建立了信任关系。 如果这些解决方案不起作用，请运行 Install-webapplicationproxy Cmdlet。|  
|12027<br /><br />代理在处理请求时遇到意外错误。 提供的名称不是格式正确的帐户名称。|此事件可能指示 Web 应用程序代理和域控制器服务器之间的配置不正确，或者两台计算机上的时间和日期配置存在问题。|域控制器拒绝了由 Web 应用程序代理创建的 Kerberos 票证。 验证是否正确配置了 Web 应用程序代理和后端应用程序服务器的配置，特别是 SPN 配置。 请确保 Web 应用程序代理已加入域控制器所在的域，以确保域控制器与 Web 应用程序代理建立信任。请确保 Web 应用程序代理和上的时间和日期配置域控制器已同步。|  
|13012<br /><br />Web 应用程序代理接收到无效边缘令牌签名||请确保更新了[KB 2955164](https://go.microsoft.com/fwlink/?LinkId=400701)的 Web 应用程序代理。|  
|13013<br /><br />Web 应用程序代理收到包含已过期的边缘令牌的请求。|Web 应用程序代理和 AD FS 没有同步时钟。|同步 Web 应用程序代理与 AD FS 之间的时钟。|  
|13014<br /><br />Web 应用程序代理接收到具有无效边缘令牌的请求。 令牌无效，因为无法对其进行分析。|这可能表示 AD FS 配置存在问题。|检查 AD FS 配置，并在必要时还原默认配置。|  
|13015<br /><br />Web 应用程序代理接收到具有过期访问 cookie 的请求。|这可能指示未同步的时钟。|如果使用的是 Web 应用程序代理计算机的群集，请确保同步计算机的时间和日期。|  
|13016<br /><br />Web 应用程序代理无法代表用户检索 Kerberos 票证，因为边缘令牌或访问 cookie 中没有 UPN。|STS 配置存在问题。|修复 STS 中的 UPN 声明配置。|  
|13019<br /><br />Web 应用程序代理无法代表用户检索 Kerberos 票证，因为出现以下常规 API 错误|此事件可能指示 Web 应用程序代理和域控制器服务器之间的配置不正确，或者两台计算机上的时间和日期配置存在问题。|域控制器拒绝了由 Web 应用程序代理创建的 Kerberos 票证。 验证是否正确配置了 Web 应用程序代理和后端应用程序服务器的配置，特别是 SPN 配置。 请确保 Web 应用程序代理已加入域控制器所在的域，以确保域控制器与 Web 应用程序代理建立信任。请确保 Web 应用程序代理和上的时间和日期配置域控制器已同步。|  
|13020<br /><br />Web 应用程序代理无法代表用户检索 Kerberos 票证，因为未定义后端服务器 SPN。|此事件可能指示 Web 应用程序代理和域控制器服务器之间的配置不正确，或者两台计算机上的时间和日期配置存在问题。|域控制器拒绝了由 Web 应用程序代理创建的 Kerberos 票证。 验证是否正确配置了 Web 应用程序代理和后端应用程序服务器的配置，特别是 SPN 配置。 请确保 Web 应用程序代理已加入域控制器所在的域，以确保域控制器与 Web 应用程序代理建立信任。请确保 Web 应用程序代理和上的时间和日期配置域控制器已同步。|  
|13022<br /><br />Web 应用程序代理无法对用户进行身份验证，因为后端服务器使用 HTTP 401 错误响应 Kerberos 身份验证尝试。|此事件可能指示 Web 应用程序代理和后端应用程序服务器之间的配置不正确，或者两台计算机上的时间和日期配置存在问题。|后端服务器拒绝了由 Web 应用程序代理创建的 Kerberos 票证。 验证是否正确配置了 Web 应用程序代理和后端应用程序服务器的配置。请确保 Web 应用程序代理和后端应用程序服务器上的时间和日期配置已同步。|  
|13025<br /><br />客户端未向 Web 应用程序代理提供 SSL 证书。|此事件可能表明时间和日期配置存在问题。|请确保证书基础结构有效，并且 Web 应用程序代理和 AD FS 的时间和日期已同步。 请确保为 Web 应用程序代理配置的指纹是正确的。|  
|13026<br /><br />客户端向 Web 应用程序代理提供 SSL 证书，但证书无效：证书与指纹不匹配。|此事件可能表明时间和日期配置存在问题。|请确保证书基础结构有效，并且 Web 应用程序代理和 AD FS 的时间和日期已同步。 请确保为 Web 应用程序代理配置的指纹是正确的。|  
|13028<br /><br />Web 应用程序代理接收到的请求中包含的边缘令牌尚未生效。|此事件可能表明时间和日期配置存在问题。|请确保证书基础结构有效，并且 Web 应用程序代理和 AD FS 的时间和日期已同步。|  
|13030<br /><br />客户端向 Web 应用程序代理提供 SSL 证书，但信任提供程序不信任颁发客户端证书的证书颁发机构。|此事件可能表明时间和日期配置存在问题。|请确保证书基础结构有效，并且 Web 应用程序代理和 AD FS 的时间和日期已同步。 请确保为 Web 应用程序代理配置的指纹是正确的。|  
|13031<br /><br />客户端向 Web 应用程序代理提供 SSL 证书，但证书链在不受信任提供程序信任的根证书中终止。|此事件可能表明时间和日期配置存在问题。|请确保证书基础结构有效，并且 Web 应用程序代理和 AD FS 的时间和日期已同步。 请确保为 Web 应用程序代理配置的指纹是正确的。|  
|13032<br /><br />客户端向 Web 应用程序代理提供 SSL 证书，但该证书对于请求的用法无效。|此事件可能表明时间和日期配置存在问题。|请确保证书基础结构有效，并且 Web 应用程序代理和 AD FS 的时间和日期已同步。 请确保为 Web 应用程序代理配置的指纹是正确的。|  
|13033<br /><br />客户端向 Web 应用程序代理提供 SSL 证书，但在根据当前系统时钟或签名文件中的时间戳进行验证时，证书不在其有效期内。|此事件可能表明时间和日期配置存在问题。|请确保证书基础结构有效，并且 Web 应用程序代理和 AD FS 的时间和日期已同步。 请确保为 Web 应用程序代理配置的指纹是正确的。|  
|13034<br /><br />客户端向 Web 应用程序代理提供 SSL 证书，但该证书无效。|此事件可能表明时间和日期配置存在问题。|请确保证书基础结构有效，并且 Web 应用程序代理和 AD FS 的时间和日期已同步。 请确保为 Web 应用程序代理配置的指纹是正确的。|  
  
以下管理员控制台事件通常表示与配置相关的问题，例如设置、未成功的请求、无法访问的后端服务器和缓冲区溢出。  
  
|事件或症状|可能的原因|分辨率|  
|--------------------|------------------|--------------|  
|12019<br /><br />Web 应用程序代理无法为以下 URL 创建侦听器。|事件的可能原因是其他服务正在侦听同一 URL。|管理员必须确保没有人侦听或绑定到相同的 Url。 若要进行检查，请运行以下命令： netsh http show urlacl。 如果此 URL 被 Web 应用程序代理计算机上运行的另一个组件使用，请将其删除，或者使用不同的 URL 通过 Web 应用程序代理发布应用程序。|  
|12020<br /><br />Web 应用程序代理无法为以下 URL 创建保留。|事件的一个可能原因是另一个服务在相同的 URL 上有一个保留。|管理员必须确保没有一个绑定到相同的 Url。 若要进行检查，请运行以下命令： netsh http show urlacl。 如果此 URL 被 Web 应用程序代理计算机上运行的另一个组件使用，请将其删除，或者使用不同的 URL 通过 Web 应用程序代理发布应用程序。|  
|12021<br /><br />Web 应用程序代理无法绑定 SSL 服务器证书。 已应用所有其他配置设置。|无法创建和设置 SSL 证书数据的配置记录。|请确保在本地计算机存储中使用私钥的所有 Web 应用程序代理计算机上都安装了为 Web 应用程序 Proxyapplications 配置的证书指纹。|  
|13001<br /><br />后端服务器提供给 Web 应用程序代理的 SSL 服务器证书无效;该证书不受信任。|服务器发送的安全套接字层（SSL）证书中找到一个或多个错误。 这可能表示后端服务器提供的 SSL 无效，或者 Web 应用程序代理和后端服务器之间没有信任关系。|验证后端服务器 SSL 证书。 请确保将 Web 应用程序代理计算机配置为具有正确的根 Ca，以便信任后端服务器证书。|  
|13006|如果错误代码为0x80072ee7，则 failurrre 的原因是无法解析后端服务器 URL。 [@No__t-1](https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85))中介绍了其他错误代码|检查后端服务器 URL 是否正确，以及是否可以从 Web 应用程序代理计算机正确解析其名称。|  
|13007<br /><br />未在预期的间隔内收到来自后端服务器的 HTTP 响应。|后端服务器请求超时或速度缓慢或无响应。|检查后端服务器配置。 如果速度非常慢，请检查与后端服务器的连接，还应考虑更改 InactiveTransactionsTimeoutSec 的 Web 应用程序代理全局配置参数 cmdlet。|  
  
## <a name="see-also"></a>请参阅  
[Windows Server 2016 中 Web 应用程序代理的新增功能](web-application-proxy-windows-server.md)  
[使用 Web 应用程序代理](assetId:///b607b717-2172-4271-98d1-fa8162e0bb2e)  
  


