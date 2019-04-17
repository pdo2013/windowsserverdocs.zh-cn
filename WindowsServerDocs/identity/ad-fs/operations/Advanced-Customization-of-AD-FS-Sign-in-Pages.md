---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: "高级自定义的广告 FS 登录页面"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ea01d0ff2a38c4fef2f68091608d777d8412e91b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>高级自定义的广告 FS 登录页面

>适用于：Windows Server 2016，Windows Server 2012 R2
  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>高级自定义的广告 FS Sign\ 中页面  
在 Windows Server 2012 R2 的广告 FS 提供 built\ 中支持自定义 sign\ 中的体验。 对于大多数这些方案中，在 built\ Windows PowerShell cmdlet 是所需的所有。  建议你使用 built\ 中的 Windows PowerShell 命令自定义为广告 FS sign\ 中体验尽可能标准元素。  请参阅[登录自定义的广告 FS 用户](AD-FS-user-sign-in-customization.md)详细信息。  
  
在某些情况下，可能需要提供额外 sign\ 中的体验，无法通过发货 in\ 包装盒广告 FS 现有 PowerShell 命令广告 FS 管理员。 在某些情况下，它是可行 \ （内提供指南 below\) 适用于管理员，要自定义 sign\ 中体验进一步通过添加到的其他逻辑**onload.js**的由广告 FS 并将所有广告 FS 页上执行。  
  
## <a name="things-to-know-before-you-start"></a>在开始之前的注意事项  
  
-   不支持影响重定向流或修改协议参数广告 FS 适用于任何更改。
  
-   原始 onload.js，一个附带的默认 web 主题，包含处理页面呈现的不同外形规格的代码。 建议不应修改原始 onload.js 内容，而只附加处理自定义的逻辑现有 onload.js 你的代码。  
  
-   广告 FS 附带称为默认 built\ 在 web 主题。 你无法修改默认 web 主题的 onload.js。 若要更新 onload.js，您需要创建和自定义 web 主题用于广告 FS sign\ 中的页面。  请参阅[登录自定义的广告 FS 用户](AD-FS-user-sign-in-customization.md)有关如何创建自定义 web 主题的信息。  
  
-   相同 onload.js 将上所有 ADFS 页面 \(ex.执行 基于 form\ 登录页，家庭领域发现页面等。 \)。 你需要确保在你脚本仅获取执行代码为它设计，并且未意外执行。  
  
-   当引用任何 HTML 元素，确保始终检查存在元素上作用之前元素。 这会提供了可靠性，并确保在不包含这元素页上中自定义的逻辑将不会执行。 你只需可以广告 FS sign\ 在页面上查看现有元素查看 HTML 源。  
  
-   强烈建议来验证你的备用环境中的自定义和推出你们到生产广告 FS 服务器之前对其进行测试。 这减少了之前验证这些自定义项打包到公开最终用户的机会。  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>使用 onload.js 自定义的广告 FS sign\ 中体验  
自定义广告 FS 服务 onload.js 时，请使用以下步骤。  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>自定义 onload.js 广告 FS 服务  
  
1.  若要添加到 onload.js 你自定义的逻辑，你需要先创建自定义 web 主题。 已发货的 out\ of\ the\-瓜主题称为默认值。 你可以导出的默认主题和，以便你可以快速开始使用它。 以下 cmdlet 创建重复默认 web 主题一个自定义 web 主题：  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  然后，你可以导出自定义或默认 web 主题，以获取 onload.js 文件。 若要导出 web 主题，请使用以下 cmdlet:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    你将找到 onload.js 下脚本文件夹目录中你指定中导出 cmdlet 上述和自定义逻辑添加到脚本 \ (请参阅中示例部分 below\ 的用例)。  
  
3.  请必要修改以自定义 onload.js 根据你的需求。  
  
4.  使用修改 onload.js 更新主题。 使用以下 cmdlet 应用更新 onload.js 到自定义 web 主题：  
  
    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
  
5.  若要自定义 web 主题广告 FS 应用，使用以下 cmdlet:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>自定义的其他示例。  
以下是代码的添加到不同的 fine\ 曲调供 onload.js 自定义的示例。 添加时自定义的代码，请始终附加自定义代码 onload.js 底部。  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>"使用组织帐户登录"字符串更改的示例 1:  
默认广告 FS form\ 基于 sign\ 中页具有"使用你的组织帐户的登录"的标题上方用户输入的框。  
  
如果你想要这一串替换自己的字符串，你可以到 onload.js 添加以下的代码。  
  
```  
// Sample code to change “Sign in with organizational account” string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>示例 2： 为广告 FS form\ 基于 sign\ 在页面上的登录格式接受 SAM\ 帐户名称  
默认广告 FS form\ 基于 sign\ 中页面支持登录格式用户主要名称 \(UPNs\) \(for example, ** johndoe@contoso.com **\) 或域限定 sam\ 帐户名 \ (**contoso\\johndoe**或**contoso.com\\johndoe**\)。 在您的所有用户来自同一个域，他们只能知道 sam\ 帐户名的情况下您可能想要在使用它们仅 sam\ 帐户名称支持用户可以在登录的方案。 你可以向 onload.js 支持此方案，只需将域"contoso.com"在此示例中的下方替换你想要使用的域中添加以下代码。  
  
```  
if (typeof Login != 'undefined'){  
    Login.submitLoginRequest = function () {   
    var u = new InputUtil();  
    var e = new LoginErrors();  
    var userName = document.getElementById(Login.userNameInput);  
    var password = document.getElementById(Login.passwordInput);  
    if (userName.value && !userName.value.match('[@\\\\]'))   
    {  
        var userNameValue = 'contoso.com\\' + userName.value;  
        document.forms['loginForm'].UserName.value = userNameValue;  
    }  
  
    if (!userName.value) {  
       u.setError(userName, e.userNameFormatError);  
       return false;  
    }  
  
    if (!password.value)   
    {  
        u.setError(password, e.passwordEmpty);  
        return false;  
    }  
    document.forms['loginForm'].submit();  
    return false;  
};  
}  
  
```  
  
## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  

