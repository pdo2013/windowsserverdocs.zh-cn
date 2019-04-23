---
title: Web 应用程序代理疑难解答
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a2fef55d-747b-4e20-8f21-5f8807e7ef87
ms.technology: web-app-proxy
ms.openlocfilehash: c63dbc415765cbe60bfbde7bf180f3dd967d6ab8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841578"
---
# <a name="troubleshooting-web-application-proxy"></a>Web 应用程序代理疑难解答

>适用于：Windows Server 2016

**此内容是相关的本地版本的 Web 应用程序代理。若要支持通过云对本地应用程序的安全访问，请参阅[Azure AD 应用程序代理内容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**  
  
本部分提供有关包括事件说明和解决方案的 Web 应用程序代理故障排除过程。 有以下三个位置显示错误的位置：  
  
-   在管理员控制台的 Web 应用程序代理  
  
    可以在 Windows 事件查看器和相应说明中查看管理员控制台中列出每个事件 ID 和下方找到解决方案。  
  
    打开事件查看器并查找与下的 Web 应用程序代理相关的事件**应用程序和服务日志** > **Microsoft** > **Windows**  >  **Web 应用程序代理** > **管理员**  
  
    ![](media/Troubleshooting-Web-Application-Proxy/WebApplicationProxyTroubleshooting.png)  
  
    如果需要详细的日志均可通过打开分析和调试日志，并在下的 Windows 事件查看器中打开 Web 应用程序代理会话日志中，找到 \ Microsoft \ Windows \ Web 应用程序代理 \ 管理员。  
  
-   中的 PowerShell 错误  
  
    在 PowerShell 中会显示在配置期间遇到问题时的事件。  
  
    向 PowerShell 用户使用标准 PowerShell 错误提示显示所有错误。 作为事件记录所有 PowerShell 命令。 在 PowerShell 中发生的所有事件的 ID 号 12016，Windows 事件查看器中列出和 PowerShell 部分中定义如下。  
  
-   在最佳实践分析工具  
  
    这些事件中所述[Web 应用程序代理的最佳实践分析工具](assetId:///995f5cea-e334-4ce6-a14c-6a2d03c0c79a)  
  
## <a name="powershell-messages"></a>PowerShell 消息  
  
|事件或症状|可能的原因|分辨率|  
|--------------------|------------------|--------------|  
|信任证书 ("ADFS ProxyTrust- <WAP machine name>") 无效|这种情况可能由以下任何原因引起：<br /><br />应用程序代理计算机已关闭的时间太长。<br />-Web 应用程序代理与 AD FS 之间断开连接<br />-证书基础结构问题<br />-在 AD FS 计算机或 Web 应用程序代理与 AD FS 之间的续订流程上的更改未运行按计划每隔 8 小时，则它们需要续订信任<br />-Web 应用程序代理计算机和 AD FS 的时钟不同步。|请确保时钟同步。 运行 Install-webapplicationproxy cmdlet。|  
|在 AD FS 中找不到配置数据|这可能是因为 Web 应用程序代理尚未或由于中的 AD FS 数据库或数据库的损坏的更改未完全安装。|运行 Install-webapplicationproxy Cmdlet|  
|Web 应用程序代理在尝试读取从 AD FS 配置时出错。|这可能表示 AD FS 不可访问，或 AD FS 遇到尝试从 AD FS 数据库中读取配置出现内部问题。|验证 AD FS 可访问并且工作正常。|  
|存储在 AD FS 配置数据已损坏或 Web 应用程序代理无法对其进行分析。<br /><br />或者<br /><br />Web 应用程序 Proxywas 无法检索 AD FS 的信赖方列表。|如果已在 AD FS 中修改的配置数据，这可能会出现。|重新启动 Web 应用程序 Proxyservice。 如果问题仍然存在，请运行 Install-webapplicationproxy Cmdlet。|  
  
## <a name="administrator-console-events"></a>管理员控制台事件  
以下管理员控制台事件通常表示身份验证错误，无效 tokes 或过期的 cookie。  
  
|事件或症状|可能的原因|分辨率|  
|--------------------|------------------|--------------|  
|11005<br /><br />Web 应用程序代理无法创建使用从配置的机密的 cookie 加密密钥。|全局配置"AccessCookiesEncryptionKey"参数进行了更改的 PowerShell 命令：Set-WebApplicationProxyConfiguration -RegenerateAccessCookiesEncryptionKey|无操作是必需的。 已删除有问题的 cookie，用户已被重定向到 STS 进行身份验证。|  
|12000<br /><br />Web 应用程序代理无法检查配置更改至少 60 分钟|Web 应用程序代理无法访问使用命令获取 WebApplicationProxyConfiguration/应用程序的 Web 应用程序代理配置。 这通常被引起需续订与 AD FS 信任关系或没有使用 AD FS 的连接。|检查与 AD FS 的连接。 您可以执行此操作使用链接 https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake 确保 AD FS 和 Web 应用程序代理之间建立信任关系。 如果这些解决方案不起作用，运行 Install-webapplicationproxy Cmdlet。|  
|12003<br /><br />Web 应用程序代理无法分析访问 cookie。|这可能表示 Web 应用程序代理和 AD FS 未连接或用户未收到相同的配置。|检查与 AD FS 的连接。 您可以执行此操作使用链接 https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake 确保 AD FS 和 Web 应用程序代理之间建立信任关系。 如果这些解决方案不起作用，运行 Install-webapplicationproxy Cmdlet。|  
|12004<br /><br />Web 应用程序代理接收到无效访问 cookie 的请求。|此事件可能表示 Web 应用程序代理和 AD FS 未连接或用户未收到相同的配置。<br /><br />如果运行"AccessCookiesEncryptionKey"参数是 chaged Set-webapplicationproxyconfiguration RegenerateAccessCookiesEncryptionKey PowerShell 命令，通过此事件是正常情况，要求没有解决方法步骤。|检查与 AD FS 的连接。 您可以执行此操作使用链接 https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake 确保 AD FS 和 Web 应用程序代理之间建立信任关系。 如果这些解决方案不起作用，运行 Install-webapplicationproxy Cmdlet。|  
|12008<br /><br />Web 应用程序代理超过了允许访问后端服务器的 Kerberos 身份验证尝试最大数目。|此事件可能表示 Web 应用程序代理和后端应用程序服务器或两台计算机上的时间和日期配置存在问题之间配置不正确。|后端服务器拒绝了创建的 Web 应用程序代理的 Kerberos 票证。 验证正确配置的 Web 应用程序代理和后端应用程序服务器配置。<br /><br />请确保 Web 应用程序代理和后端应用程序服务器上的时间和日期配置已同步。|  
|12011<br /><br />Web 应用程序代理接收到的无效访问 cookie 签名的请求。|此事件可能表示 Web 应用程序代理和 AD FS 未连接或用户未收到相同的配置。 如果运行"AccessCookiesEncryptionKey"参数是 chaged Set-webapplicationproxyconfiguration RegenerateAccessCookiesEncryptionKey PowerShell 命令，通过此事件是正常情况，要求没有解决方法步骤。|检查与 AD FS 的连接。 您可以执行此操作使用链接 https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake 确保 AD FS 和 Web 应用程序代理之间建立信任关系。 如果这些解决方案不起作用，运行 Install-webapplicationproxy Cmdlet。|  
|12027<br /><br />代理服务器处理请求时遇到意外的错误。 提供的名称不正确的帐户名称。|此事件可能表示 Web 应用程序代理与域控制器服务器或两台计算机上的时间和日期配置存在问题之间配置不正确。|域控制器拒绝了创建的 Web 应用程序代理的 Kerberos 票证。 验证 Web 应用程序代理和后端应用程序服务器的配置是否配置正确，尤其是 SPN 配置。 请确保 Web 应用程序代理是域已加入到域控制器，以确保域控制器会确保建立与 Web 应用程序 Proxy.Make 信任同一个域的 Web 应用程序代理服务器上的时间和日期配置和域控制器进行同步。|  
|13012<br /><br />Web 应用程序代理接收到无效边缘令牌签名||请确保更新与 Web 应用程序代理[KB 2955164](https://go.microsoft.com/fwlink/?LinkId=400701)。|  
|13013<br /><br />Web 应用程序代理收到包含已过期的边缘令牌的请求。|Web 应用程序代理和 AD FS 没有同步的时钟。|同步 Web 应用程序代理与 AD FS 之间的时钟。|  
|13014<br /><br />Web 应用程序代理接收到无效边缘令牌的请求。 该令牌无效，因为无法分析。|这可能表示 AD FS 配置出现问题。|检查 AD FS 配置，并如有必要，还原默认配置。|  
|13015<br /><br />Web 应用程序代理收到使用过期的访问 cookie 的请求。|这可能表明未同步的时钟。|如果您正在使用一组 Web 应用程序代理计算机，请确保计算机的日期和时间同步。|  
|13016<br /><br />Web 应用程序代理无法检索代表用户的 Kerberos 票证，因为边缘令牌或访问 cookie 中没有 upn。|没有 STS 配置出现问题。|在 STS 中修复 UPN 声明配置。|  
|13019<br /><br />由于以下常规 API 错误，web 应用程序代理无法检索代表用户的 Kerberos 票证|此事件可能表示 Web 应用程序代理与域控制器服务器或两台计算机上的时间和日期配置存在问题之间配置不正确。|域控制器拒绝了创建的 Web 应用程序代理的 Kerberos 票证。 验证 Web 应用程序代理和后端应用程序服务器的配置是否配置正确，尤其是 SPN 配置。 请确保 Web 应用程序代理是域已加入到域控制器，以确保域控制器会确保建立与 Web 应用程序 Proxy.Make 信任同一个域的 Web 应用程序代理服务器上的时间和日期配置和域控制器进行同步。|  
|13020<br /><br />Web 应用程序代理无法检索代表用户的 Kerberos 票证，因为后端服务器 SPN 未定义。|此事件可能表示 Web 应用程序代理与域控制器服务器或两台计算机上的时间和日期配置存在问题之间配置不正确。|域控制器拒绝了创建的 Web 应用程序代理的 Kerberos 票证。 验证 Web 应用程序代理和后端应用程序服务器的配置是否配置正确，尤其是 SPN 配置。 请确保 Web 应用程序代理是域已加入到域控制器，以确保域控制器会确保建立与 Web 应用程序 Proxy.Make 信任同一个域的 Web 应用程序代理服务器上的时间和日期配置和域控制器进行同步。|  
|13022<br /><br />Web 应用程序代理无法验证用户身份，因为后端服务器响应 Kerberos 身份验证尝试以 HTTP 401 错误。|此事件可能表示 Web 应用程序代理和后端应用程序服务器或两台计算机上的时间和日期配置存在问题之间配置不正确。|后端服务器拒绝了创建的 Web 应用程序代理的 Kerberos 票证。 验证正确配置的 Web 应用程序代理和后端应用程序服务器配置。请确保 Web 应用程序代理和后端应用程序服务器上的时间和日期配置已同步。|  
|13025<br /><br />客户端未向 Web 应用程序代理中显示的 SSL 证书。|此事件可能表示时间和日期配置存在问题。|请确保证书基础结构有效，并且同步的 Web 应用程序代理和 AD FS 的日期和时间。 请确保为 Web 应用程序代理配置的指纹是正确的订阅。|  
|13026<br /><br />客户端提供给 Web 应用程序代理的 SSL 证书，但证书不是有效： 证书与指纹不匹配。|此事件可能表示时间和日期配置存在问题。|请确保证书基础结构有效，并且同步的 Web 应用程序代理和 AD FS 的日期和时间。 请确保为 Web 应用程序代理配置的指纹是正确的订阅。|  
|13028<br /><br />Web 应用程序代理收到包含尚不是有效的边缘令牌的请求。|此事件可能表示时间和日期配置存在问题。|请确保证书基础结构有效，并且同步的 Web 应用程序代理和 AD FS 的日期和时间。|  
|13030<br /><br />客户端提供给 Web 应用程序代理的 SSL 证书，但信任提供程序不信任证书颁发机构颁发的客户端证书。|此事件可能表示时间和日期配置存在问题。|请确保证书基础结构有效，并且同步的 Web 应用程序代理和 AD FS 的日期和时间。 请确保为 Web 应用程序代理配置的指纹是正确的订阅。|  
|13031<br /><br />客户端提供给 Web 应用程序代理的 SSL 证书，但在不受信任提供程序的根证书中的证书链终止。|此事件可能表示时间和日期配置存在问题。|请确保证书基础结构有效，并且同步的 Web 应用程序代理和 AD FS 的日期和时间。 请确保为 Web 应用程序代理配置的指纹是正确的订阅。|  
|13032<br /><br />客户端提供给 Web 应用程序代理的 SSL 证书，但证书对于请求的用法无效。|此事件可能表示时间和日期配置存在问题。|请确保证书基础结构有效，并且同步的 Web 应用程序代理和 AD FS 的日期和时间。 请确保为 Web 应用程序代理配置的指纹是正确的订阅。|  
|13033<br /><br />客户端提供给 Web 应用程序代理的 SSL 证书，但该证书不在其有效期内根据当前系统时钟或签名文件中的时间戳验证时。|此事件可能表示时间和日期配置存在问题。|请确保证书基础结构有效，并且同步的 Web 应用程序代理和 AD FS 的日期和时间。 请确保为 Web 应用程序代理配置的指纹是正确的订阅。|  
|13034<br /><br />客户端提供给 Web 应用程序代理的 SSL 证书，但该证书无效。|此事件可能表示时间和日期配置存在问题。|请确保证书基础结构有效，并且同步的 Web 应用程序代理和 AD FS 的日期和时间。 请确保为 Web 应用程序代理配置的指纹是正确的订阅。|  
  
通常表明的问题，与配置设置、 不成功的请求、 后端服务器无法访问，如以下管理员控制台事件，并且缓冲区溢出。  
  
|事件或症状|可能的原因|分辨率|  
|--------------------|------------------|--------------|  
|12019<br /><br />Web 应用程序代理无法创建侦听器的以下 URL。|该事件的可能原因是另一个服务正在侦听同一 URL。|管理员必须确保没有人在侦听或绑定到相同的 Url。 若要验证这点，运行命令： netsh http show urlacl。 如果 Web 应用程序代理计算机上运行的另一个组件使用此 URL，请删除它，或使用不同的 URL 发布通过 Web 应用程序代理应用程序。|  
|12020<br /><br />Web 应用程序代理无法创建以下 url 保留项。|该事件的可能原因是另一个服务必须保留在相同的 URL。|管理员必须确保没有人将绑定到相同的 Url。 若要验证这点，运行命令： netsh http show urlacl。 如果 Web 应用程序代理计算机上运行的另一个组件使用此 URL，请删除它，或使用不同的 URL 发布通过 Web 应用程序代理应用程序。|  
|12021<br /><br />Web 应用程序代理无法绑定的 SSL 服务器证书。 已应用所有其他配置设置。|无法创建并设置 SSL 证书数据的配置记录。|请确保为 Web 应用程序 Proxyapplications 配置的证书指纹安装在所有 Web 应用程序代理计算机上的本地计算机存储中的专用密钥。|  
|13001<br /><br />由后端服务器提供给 Web 应用程序代理的 SSL 服务器证书不是有效，则为该证书不受信任。|由服务器发送的安全套接字层 (SSL) 证书中找到了一个或多个错误。 这可能表示后端服务器提供了无效的 SSL 或者没有 Web 应用程序代理和后端服务器之间没有信任关系。|验证后端服务器 SSL 证书。 请确保 Web 应用程序代理计算机配置具有正确的根 Ca 信任后端服务器证书。|  
|13006|错误代码时 0x80072ee7，failurrre 被导致的无法解析后端服务器 URL。 中介绍了其他错误代码 [https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85)](https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85))|检查后端服务器 URL 正确以及能够正确地从 Web 应用程序代理计算机解析其名称。|  
|13007<br /><br />预期的间隔内未收到来自后端服务器的 HTTP 响应。|后端服务器请求已超时或缓慢或无响应。|检查后端服务器配置。 如果速度很慢，检查与后端服务器的连接，并考虑 InactiveTransactionsTimeoutSec 更改 Web 应用程序代理的全局配置参数 cmdlet。|  
  
## <a name="see-also"></a>请参阅  
[什么是 Windows Server 2016 中的 Web 应用程序代理中的新增功能](web-application-proxy-windows-server.md)  
[使用 Web 应用程序代理](assetId:///b607b717-2172-4271-98d1-fa8162e0bb2e)  
  


