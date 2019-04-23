---
ms.assetid: d8adcb68-18e0-41bf-a817-d57344bf2e7d
title: Windows Server 2016 中的 Web 应用程序代理
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: 2f24e1b8605503d338b15f385017bbeacce682fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872208"
---
# <a name="web-application-proxy-in-windows-server-2016"></a>Windows Server 2016 中的 Web 应用程序代理

>适用于：Windows Server 2016

**此内容是相关的本地版本的 Web 应用程序代理。若要支持通过云对本地应用程序的安全访问，请参阅[Azure AD 应用程序代理内容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**  
  
在本部分内容介绍新增功能和 Web 应用程序代理的 Windows Server 2016 中已更改。 新功能和更改此处列出的那些最有可能产生重大影响，以使用预览版。  
  
## <a name="web-application-proxy-new-features-in-windows-server-2016"></a>Windows Server 2016 中的 web 应用程序代理新功能
  
-   用于 HTTP 基本应用程序发布预身份验证  
  
    HTTP 基本身份验证是许多协议，包括 ActiveSync，用来连接丰富客户端，包括智能手机，与您的 Exchange 邮箱的授权协议。 传统上与 AD FS 使用重定向 ActiveSync 客户端上不受支持的交互，web 应用程序代理。 此新版本的 Web 应用程序代理提供支持将通过启用 HTTP 应用程序以接收非声明使用 HTTP 基本应用程序发布到联合身份验证服务应用程序的信赖方信任。  
  
    HTTP 基本发布的详细信息，请参阅[使用 AD FS 预身份验证发布应用程序](Publishing-Applications-using-AD-FS-Preauthentication.md#publish-an-application-that-uses-http-basic)  
  
-   通配符域发布的应用程序  
  
    若要支持方案，例如 SharePoint 2013，应用程序的外部 URL 现在可以包含通配符，使您能够发布特定域，例如， https://*.sp-apps.contoso.com 从多个应用程序。 这将简化发布 SharePoint 应用。  
  
-   HTTP 到 HTTPS 重定向  
  
    若要确保你的用户可以访问你的应用，即使他们忘了在 URL 中键入 HTTPS，Web 应用程序代理现在支持 HTTP 到 HTTPS 重定向。  
  
-   HTTP 发布  
  
    现可发布 HTTP 应用程序使用传递预身份验证  
  
-   发布远程桌面网关应用  
  
    RDG 中 Web 应用程序代理的详细信息，请参阅[与 SharePoint、 Exchange 和 RDG 发布应用程序](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)  
  
-   新的调试日志，以更好地进行故障排除和更高的服务日志中查看完整的审核线索和改进的错误处理  
  
    有关疑难解答的详细信息，请参阅[故障排除 Web 应用程序代理](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   管理员控制台 UI 改进  
  
-   对后端应用程序的客户端 IP 地址的传播  
  
## <a name="see-also"></a>请参阅  
  
-   [什么是 Windows Server 2016 中的新增功能](https://technet.microsoft.com/library/dn765472.aspx)  
  
-   [使用 AD FS 预身份验证发布应用程序](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
-   [Web 应用程序代理故障排除](https://technet.microsoft.com/library/dn770156.aspx)  
  


