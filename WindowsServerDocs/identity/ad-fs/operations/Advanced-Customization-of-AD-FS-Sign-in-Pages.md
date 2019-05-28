---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: AD FS 登录页的高级自定义
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73ff3fc6df872edd29735ee96c0918144250d5f1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190039"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>AD FS 登录页的高级自定义

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>AD FS 登录的高级自定义\-页中  
Windows Server 2012 R2 中的 AD FS 提供了构建\-中支持自定义登录\-体验中。 对于这些情况下，内置的大多数\-在 Windows PowerShell cmdlet 所需要的所有。  建议你使用内置\-在 Windows PowerShell 命令来为 AD FS 自定义的标准元素中登录\-尽可能的经验。  请参阅[登录自定义 AD FS 用户](AD-FS-user-sign-in-customization.md)有关详细信息。  
  
在某些情况下，AD FS 管理员可能想要提供其他登录\-体验的不是可以通过现有的 PowerShell 命令中发售的\-与 AD FS 的框。 在某些情况下，它是可行\(在下面提供的指导原则\)的管理员可以自定义登录\-体验中进一步通过将添加到其他逻辑**onload.js** ，由 AD FS 提供，将所有 AD FS 页上执行。  
  
## <a name="things-to-know-before-you-start"></a>在开始前需知事项  
  
-   不支持任何更改会影响重定向流或修改 AD FS 配合的协议参数。
  
-   原始 onload.js，一个附带的默认 web 主题，包含用于处理针对不同外形因素的页面呈现代码。 建议不应修改原始 onload.js 内容而仅将你的代码追加到现有 onload.js 处理自定义逻辑。  
  
-   AD FS 附带的内置\-中称为默认 web 主题。 不能修改默认 web 主题的 onload.js。 若要更新 onload.js，您必须创建并使用自定义 web 主题进行 AD FS 登录\-页中。  请参阅[登录自定义 AD FS 用户](AD-FS-user-sign-in-customization.md)有关如何创建自定义 web 主题的信息。  
  
-   将在所有 ADFS 页上执行相同 onload.js \(ex。 窗体\-基于登录页上，主领域发现页面和等\)。 您需要确保您的脚本中的代码只能获取执行设计并不会执行意外。  
  
-   当引用的任何 HTML 元素，请确保始终检查之前对该元素的元素存在。 这提供了可靠性，并可确保在自定义逻辑将不执行不包含此元素的页上。 只可以查看 HTML 源上的 AD FS 登录\-页可以查看现有元素中。  
  
-   强烈建议验证你的备用环境中的自定义和滚动一下到生产 AD FS 服务器之前对其进行测试。 这将减少暴露给之前验证这些自定义的最终用户的可能性。  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>自定义 AD FS 登录\-使用 onload.js 体验中  
自定义 AD FS 服务 onload.js 时，请使用以下步骤。  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>自定义 AD FS 服务 onload.js  
  
1.  若要将自定义逻辑添加到 onload.js，需要先创建自定义 web 主题。 寄出的主题\-的\-\-框称为默认值。 可以导出并使用该默认主题，以便可以快速启动。 以下 cmdlet 创建自定义 web 主题，复制默认 web 主题：  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  然后可以导出自定义或默认 web 主题获取 onload.js 文件。 若要导出 web 主题，请使用以下 cmdlet:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    您会发现 onload.js 脚本文件夹下的目录中指定导出 cmdlet 更高版本中，然后在脚本中添加自定义逻辑\(请参阅下面的示例部分中的用例\)。  
  
3.  进行必要的修改，以自定义 onload.js 根据你的需求。  
  
4.  使用修改后的 onload.js 更新主题。 使用以下 cmdlet 将更新 onload.js 应用于自定义 web 主题：  

     对于 Windows Server 2012 R2 上的 AD FS:  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
    对于 Windows Server 2016 上的 AD FS:

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\ADFStheme\script\onload.js"   
  
    ```  
  
5.  若要将自定义 web 主题应用到的 AD FS，使用以下 cmdlet:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>其他自定义示例  
下面的示例的自定义代码添加到不同的细的 onload.js\-优化目的。 在添加自定义代码时，请始终追加在自定义代码到 onload.js 底部。  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>示例 1： 更改"使用组织帐户登录"的字符串  
默认 AD FS 窗体\-基于登录\-页中具有用户输入框上方的"使用你的组织帐户登录"标题。  
  
如果你想要此字符串替换为你自己的字符串，您可以将以下代码添加到 onload.js。  
  
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
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>示例 2： 接受 SAM\-AD FS 窗体上的登录名格式作为帐户名\-基于登录\-页中  
默认 AD FS 窗体\-基于登录\-页中支持的用户主体名称的登录名格式\(Upn\) \(等**johndoe@contoso.com** \)或域限定 sam\-帐户名\( **contoso\\johndoe**或**contoso.com\\johndoe**\)。 所有用户均来自同一个域，并且它们只知道 sam\-帐户名称，可能想要支持用户可以登录，其中的方案中使用它们 sam\-帐户仅名称。 您可以将以下代码添加到 onload.js 以支持这种情况下，只需替换你想要使用的域的域"contoso.com"中的示例如下。  
  
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
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
  

