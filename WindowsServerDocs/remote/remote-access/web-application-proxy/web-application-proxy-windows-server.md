---
ms.assetid: d8adcb68-18e0-41bf-a817-d57344bf2e7d
title: Windows Server 2016 中的 Web 应用程序代理
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: 4f2827f02ec13d187cdf360637882c6c9d4b2441
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387968"
---
# <a name="web-application-proxy-in-windows-server-2016"></a>Windows Server 2016 中的 Web 应用程序代理

>适用于：Windows Server 2016

@no__t 0This 内容与 Web 应用程序代理的本地版本相关。若要启用对云中的本地应用程序的安全访问，请参阅[Azure AD 应用程序代理内容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。 **  
  
本部分中的内容介绍了 Windows Server 2016 的 Web 应用程序代理中的新增功能和更改功能。 此处列出的新功能和更改是在使用预览版时最有可能遇到的最大影响。  
  
## <a name="web-application-proxy-new-features-in-windows-server-2016"></a>Windows Server 2016 中的 Web 应用程序代理新增功能
  
- HTTP 基本应用程序发布的预身份验证  
  
  HTTP Basic 是由许多协议（包括 ActiveSync）使用的授权协议，用于将丰富的客户端（包括智能手机）与 Exchange 邮箱连接起来。 通常，Web 应用程序代理使用不受 ActiveSync 客户端支持的重定向与 AD FS 进行交互。 此新版本的 Web 应用程序代理通过启用 HTTP 应用程序将应用程序的非声明信赖方信任接收到联合身份验证服务来支持发布应用程序。  
  
  有关 HTTP 基本发布的详细信息，请参阅[使用 AD FS 预身份验证发布应用程序](Publishing-Applications-using-AD-FS-Preauthentication.md#publish-an-application-that-uses-http-basic)  
  
- 应用程序的通配符域发布  
  
  为了支持 SharePoint 2013 等方案，应用程序的外部 URL 现在可以包含通配符，使你能够从特定域（例如，https：//* .com）中发布多个应用程序。 这将简化 SharePoint 应用程序的发布。  
  
- HTTP 到 HTTPS 的重定向  
  
  为了确保用户可以访问你的应用程序，即使他们忽视在 URL 中键入 HTTPS，Web 应用程序代理现在支持 HTTP 到 HTTPS 的重定向。  
  
- HTTP 发布  
  
  现在可以使用传递预身份验证发布 HTTP 应用程序了  
  
- 发布远程桌面网关应用  
  
  有关 Web 应用程序代理中的 RDG 的详细信息，请参阅[通过 SharePoint、Exchange 和 RDG 发布应用程序](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)  
  
- 新的调试日志，以便更好地进行故障排除并改进服务日志，以完成审核记录和改进的错误处理  
  
  有关故障排除的详细信息，请参阅[Web 应用程序代理疑难解答](https://technet.microsoft.com/library/dn770156.aspx)  
  
- 管理员控制台 UI 改进  
  
- 将客户端 IP 地址传播到后端应用程序  
  
## <a name="see-also"></a>请参阅  
  
-   [Windows Server 2016 中的新增功能](https://technet.microsoft.com/library/dn765472.aspx)  
  
-   [使用 AD FS 预身份验证发布应用程序](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
-   [Web 应用程序代理疑难解答](https://technet.microsoft.com/library/dn770156.aspx)  
  


