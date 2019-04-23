---
title: 多重身份验证和外部身份验证提供程序自定义
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864798"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>多重身份验证和外部身份验证提供程序自定义 

>适用于：Windows Server 2016, Windows Server 2012 R2

提供多重身份验证支持是在 AD FS\-的\-\-框。 例如，可以配置 AD FS 使用构建\-中用作第二重身份验证的证书身份验证。 此外还可以使用外部身份验证提供程序。 这种方法可以使 AD FS 以与其他服务集成，如 Azure 多重身份验证，也可以开发自己的提供程序。 请参阅[解决方案指南：使用多管理风险\-因素访问控制](https://technet.microsoft.com/library/dn280937.aspx)详细了解如何通过使用 AD FS 注册外部身份验证提供程序。  
  
我们建议外部身份验证提供程序使用 AD FS 提供的用于创建身份验证的用户界面的.css 文件中定义的类。 可以使用以下 cmdlet 导出默认 Web 主题，并检查用户界面类和 .css 文件中定义的元素。 可以登录的开发中使用.css 文件\-外部身份验证提供程序的用户界面中。  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
以下是一种符号\-在用户界面中，突出显示的红色，由外部身份验证提供程序。 用户界面将使用 AD FS.css 文件中的 UI 类。  
  
![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
在编写新的自定义身份验证方法之前，我们建议你先研究 AD FS 主题和样式定义，以了解创建要求的内容。  
  
-   自定义身份验证方法仅创建 HTML 片段在 AD FS 登录\-页和不完整的页面中。 应使用 AD FS 的样式定义以获得一致的外观和行为。  
  
![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   请注意，AD FS 管理员可以自定义 AD FS 样式。 . 我们不建议对你自己的样式进行硬编码。 相反，我们建议使用 AD FS 样式只要有可能。  
  
-   Out\-的\-框中，AD FS 样式创建为一个左\-到\-右\(LTR\)样式和一个右\-到\-左\(RTL\). 管理员可以自定义两者，并可以提供语言\-通过 web 主题定义的特定样式。 每个样式表都具有三个部分及其各自的备注：  
  
    -   **主题样式**\-不应而且不能使用这些样式。 这些样式是为了在所有页面上定义主题。 它们由元素 ID 特意定义，以便不会重新使用这些样式。  
  
    -   **常见样式**\-这些是应该用于你的内容的样式。  
  
    -   **外形因素样式**\-这些是针对不同外形因素的样式。 你应该了解此部分以确保你的内容能够适应不同的外形因素，例如手机和平板电脑。  
  
有关其他信息，请参阅[解决方案指南：使用多管理风险\-因素访问控制](https://technet.microsoft.com/library/dn280937.aspx)和[解决方案指南：使用其他多管理风险\-重于敏感应用程序的身份验证](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md) 
