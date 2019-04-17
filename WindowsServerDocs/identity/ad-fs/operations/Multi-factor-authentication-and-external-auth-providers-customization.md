---
title: "多重身份验证和外部身份验证的提供商自定义"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>多重身份验证和外部身份验证的提供商自定义 

>适用于：Windows Server 2016，Windows Server 2012 R2

在广告 FS out\ of\ the\ 框提供对多重身份验证的支持。 例如，你可以配置广告 FS built\ 中证书身份验证用作第二个的因素身份验证。 你还可以使用外部身份验证的提供商。 这种方法可以启用广告 FS 与其他服务、Azure 多重身份验证，如集成，或者可以开发你自己的提供商。 请参阅[解决方案指南：管理风险 Multi\ 因素访问控制](https://technet.microsoft.com/library/dn280937.aspx)有关如何使用广告 FS 注册外部身份验证提供详细信息。  
  
我们建议提供商的名字外部身份验证使用广告 FS 提供创作身份验证 UI.css 文件中定义的类。 你可以使用下面 cmdlet 导出默认 web 主题，以查看的用户界面类别和.css 文件中定义的元素。 在开发 sign\ 在用户界面外部身份验证的提供商的可用.css 文件。  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
以下是 sign\ 在用户界面，将突出显示为红色，外部身份验证的提供商的示例。 用户界面广告 FS.css 文件中使用的 UI 类。  
  
![广告金融服务和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
在编写新的自定义身份验证方法之前，我们建议你研究了解创作要求内容的广告 FS 主题和风格定义。  
  
-   自定义身份验证方法仅作者 HTML 段广告 FS sign\ 中页面和不完整的页面。 应使用广告 FS 样式定义获得一致的外观和的行为。  
  
![广告金融服务和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   请注意，广告 FS 管理员可以自定义广告 FS 样式。 . 我们不建议硬编码自己的样式。 不过，我们建议使用尽可能广告 FS 样式。  
  
-   Out\ of\ 框中，广告 FS 样式都具有一个 left\ to\ 右 \(LTR\) 样式和一个 right\ to\ 左 \(RTL\) 编写。 管理员可以自定义，并且可以提供 language\ 特定通过 web 主题定义的样式。 每种样式表具有各自的注释一样三个部分：  
  
    -   **主题样式**\-这些样式不应并且无法使用。 这些样式旨在跨所有页面定义主题。 它们是按定义元素 ID 有意，以便他们不会重新使用。  
  
    -   **常见的样式**\-这些是用于你的内容的样式。  
  
    -   **窗体因素样式**\-这些是不同外形规格的样式。 你应了解此部分中，以确保你的内容工作使用不同的外形规格，例如，手机和平板电脑。  
  
有关其他信息，请参阅[解决方案指南：管理风险 Multi\ 因素访问控制](https://technet.microsoft.com/library/dn280937.aspx)和[解决方案指南：管理风险的敏感应用程序的其他 Multi\ 强双因素身份](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)。  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
