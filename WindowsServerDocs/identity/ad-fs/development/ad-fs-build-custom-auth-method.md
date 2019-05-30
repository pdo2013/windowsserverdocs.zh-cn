---
title: 适用于 Windows Server 中的 AD FS 构建自定义身份验证方法
description: 此方案介绍如何为 Windows Server 中的 AD FS 构建自定义身份验证方法。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a5cff8ea1a7792906e5fd74981772e4760a2808c
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66314709"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>适用于 Windows Server 中的 AD FS 构建自定义身份验证方法

本演练提供了用于 Windows Server 2012 R2 中的 AD FS 实现自定义身份验证方法的说明。 有关详细信息，请参阅[其他身份验证方法](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\))。


> [!WARNING]
> 您可以在此处生成的示例是&nbsp;仅为了。 &nbsp;这些说明适用于可能会公开模型的所需的元素的最简单、 最小实现。&nbsp;没有任何身份验证后端、 错误处理或配置数据。 
> <P></P>



## <a name="setting-up-the-development-box"></a>设置开发环境中

本演练使用 Visual Studio 2012。  可以使用任何开发环境，可以为 Windows 创建一个.NET 类生成项目。 项目必须面向.NET 4.5，因为**BeginAuthentication**并**TryEndAuthentication**方法使用的类型**System.Security.Claims.Claim**，它属于.NETFramework 版本 4.5.There 是一个引用所需的项目：


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>引用 dll</strong></p></td>
<td><p><strong>在哪里可以找到它</strong></p></td>
<td><p><strong>所需的</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft.IdentityServer.Web.dll</p></td>
<td><p>Dll 位于 %windir%\ADFS 在其安装 AD FS 的 Windows Server 2012 R2 服务器上。</p>
<p></p>
<p>必须将此 dll 复制到开发计算机并在项目中创建的显式引用。</p></td>
<td><p>接口类型包括 IAuthenticationContext，IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>创建提供程序

1.  在 Visual Studio 2012:选择文件-\>新-\>项目...

2.  选择类库并确保面向.NET 4.5。
    
    ![创建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "创建提供程序")

3.  制作一份**Microsoft.IdentityServer.Web.dll** %windir%从\\AD FS 已安装和将其粘贴在你的开发计算机上的项目文件夹中的 Windows Server 2012 R2 server 上的 ADFS。

4.  在中**解决方案资源管理器**，右键单击**引用**和**添加引用...**

5.  浏览到的本地副本**Microsoft.IdentityServer.Web.dll**和**添加...**

6.  单击**确定**以确认新的引用：
    
    ![创建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "创建提供程序")
    
    您应立即设置来解决所有提供程序所需的类型。 

7.  将新类添加到你的项目 (右键单击项目，**添加。...类...** ) 并为其提供一个名称，如**MyAdapter**，如下所示：
    
    ![创建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "创建提供程序")

8.  在新文件 MyAdapter.cs 中，使用以下替换现有代码：
    
        using System;
         using System.Collections.Generic;
         using System.Linq;
         using System.Text;
         using System.Threading.Tasks;
         using System.Globalization;
         using System.IO;
         using System.Net;
         using System.Xml.Serialization;
         using Microsoft.IdentityServer.Web.Authentication.External;
         using Claim = System.Security.Claims.Claim;
         
         namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {
         
         }
         }
    
    现在，应当能够按 F12 （右键单击 – 转到定义） 上 IAuthenticationAdapter 可查看所需的接口成员的集。 
    
    接下来，你可以执行这些简单实现。

9.  您的类的全部内容替换为以下：
    
        namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {
         public IAuthenticationAdapterMetadata Metadata
         {
         //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
         }
         
         public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
         {
         //return new instance of IAdapterPresentationForm derived class
         
         }
         
         public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
         {
         return true; //its all available for now
         
         }
         
         public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
         {
         //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter
         
         }
         
         public void OnAuthenticationPipelineUnload()
         {
         
         }
         
         public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
         {
         //return new instance of IAdapterPresentationForm derived class
         
         }
         
         public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
         {
         //return new instance of IAdapterPresentationForm derived class
         
         }
         
         }
         }

10. 我们未准备好尚未生成...有两个更多的接口转。
    
    向项目中添加另外两个类： 一个是元数据和另一个用于演示文稿窗体。  您可以添加这些相同的文件中为上面的类。
    
        class MyMetadata : IAuthenticationAdapterMetadata
         {
         
         }
         
         class MyPresentationForm : IAdapterPresentationForm
         {
         
         }

11. 接下来，您可以添加每个所需的成员。第一个 （包含有用的内联注释） 的元数据
    
        class MyMetadata : IAuthenticationAdapterMetadata
         {
         //Returns the name of the provider that will be shown in the AD FS management UI (not visible to end users)
         public string AdminName
         {
         get { return "My Example MFA Adapter"; }
         }
         
         //Returns an array of strings containing URIs indicating the set of authentication methods implemented by the adapter 
         /// AD FS requires that, if authentication is successful, the method actually employed will be returned by the
         /// final call to TryEndAuthentication(). If no authentication method is returned, or the method returned is not
         /// one of the methods listed in this property, the authentication attempt will fail.
         public virtual string[] AuthenticationMethods 
         {
         get { return new[] { "http://example.com/myauthenticationmethod1", "http://example.com/myauthenticationmethod2" }; }
         }
         
         /// Returns an array indicating which languages are supported by the provider. AD FS uses this information
         /// to determine the best language\locale to display to the user.
         public int[] AvailableLcids
         {
         get
         {
         return new[] { new CultureInfo("en-us").LCID, new CultureInfo("fr").LCID};
         }
         }
         
         /// Returns a Dictionary containing the set of localized friendly names of the provider, indexed by lcid. 
         /// These Friendly Names are displayed in the "choice page" offered to the user when there is more than 
         /// one secondary authentication provider available.
         public Dictionary<int, string> FriendlyNames
         {
         get
         {
         Dictionary<int, string> _friendlyNames = new Dictionary<int, string>();
         _friendlyNames.Add(new CultureInfo("en-us").LCID, "Friendly name of My Example MFA Adapter for end users (en)");
         _friendlyNames.Add(new CultureInfo("fr").LCID, "Friendly name translated to fr locale");
         return _friendlyNames;
         }
         }
         
         /// Returns a Dictionary containing the set of localized descriptions (hover over help) of the provider, indexed by lcid. 
         /// These descriptions are displayed in the "choice page" offered to the user when there is more than one 
         /// secondary authentication provider available.
         public Dictionary<int, string> Descriptions
         {
         get 
         {
         Dictionary<int, string> _descriptions = new Dictionary<int, string>();
         _descriptions.Add(new CultureInfo("en-us").LCID, "Description of My Example MFA Adapter for end users (en)");
         _descriptions.Add(new CultureInfo("fr").LCID, "Description translated to fr locale");
         return _descriptions; 
         }
         }
         
         /// Returns an array indicating the type of claim that that the adapter uses to identify the user being authenticated.
         /// Note that although the property is an array, only the first element is currently used.
         /// MUST BE ONE OF THE FOLLOWING
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"
         public string[] IdentityClaims
         {
         get { return new[] { "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" }; }
         }
         
         //All external providers must return a value of "true" for this property.
         public bool RequiresIdentity
         {
         get { return true; }
         }
        }
    
    下一步，表示窗体：
    
        class MyPresentationForm : IAdapterPresentationForm
         {
         /// Returns the HTML Form fragment that contains the adapter user interface. This data will be included in the web page that is presented
         /// to the cient.
         public string GetFormHtml(int lcid)
         {
         string htmlTemplate = Resources.FormPageHtml; //todo we will implement this
         return htmlTemplate;
         }
         
         /// Return any external resources, ie references to libraries etc., that should be included in 
         /// the HEAD section of the presentation form html. 
         public string GetFormPreRenderHtml(int lcid)
         {
         return null;
         }
         
         //returns the title string for the web page which presents the HTML form content to the end user
         public string GetPageTitle(int lcid)
         {
         return "MFA Adapter";
         }
         
         
         }

12. 请注意 todo，对于**Resources.FormPageHtml**上述元素。 
    
    您可以修复此错误分钟，但首先让我们添加最后一个所需返回语句，基于到初始 MyAdapter 类的新实现类型。  若要执行此操作，将添加中的项*斜体*下面到现有 IAuthenticationAdapter 实现：
    
        class MyAdapter : IAuthenticationAdapter
         {
         public IAuthenticationAdapterMetadata Metadata
         {
         //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
         get { return new MyMetadata(); }
         }
         
         public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
         {
         //return new instance of IAdapterPresentationForm derived class
         return new MyPresentationForm();
         }
         
         public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
         {
         return true; //its all available for now
         }
         
         public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
         {
         //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter
         
         }
         
         public void OnAuthenticationPipelineUnload()
         {
         
         }
         
         public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
         {
         //return new instance of IAdapterPresentationForm derived class
         return new MyPresentationForm();
         }
         
         public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
         {
         //return new instance of IAdapterPresentationForm derived class
         outgoingClaims = new Claim[0];
         return new MyPresentationForm();
         }
         
         }

13. 现在的资源文件包含 html 片段。 使用以下内容在项目文件夹中创建新的文本文件：
    
        <div id="loginArea">
         <form method="post" id="loginForm" >
         <!-- These inputs are required by the presentation framework. Do not modify or remove -->
         <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
         <input id="context" type="hidden" name="Context" value="%Context%"/>
         <!-- End inputs are required by the presentation framework. -->
         <p id="pageIntroductionText">This content is provided by the MFA sample adapter. Challenge inputs should be presented below.</p>
         <label for="challengeQuestionInput" class="block">Question text</label>
         <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
         <div id="submissionArea" class="submitMargin">
         <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
         </div>
         </form>
         <div id="intro" class="groupMargin">
         <p id="supportEmail">Support information</p>
         </div>
         <script type="text/javascript" language="JavaScript">
         //<![CDATA[
         function AuthPage() { }
         AuthPage.submitAnswer = function () { return true; };
         //]]>
         </script></div>

14. 然后，选择**项目-\>添加组件...资源**文件并将文件命名**资源**，然后单击**添加：**
    
    ![创建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "创建提供程序")

15. 然后，在**Resources.resx**文件中，选择**添加资源...添加现有文件**。  导航到文本文件 （包含 html 片段） 您保存上面。
    
    请确保 GetFormHtml 代码的新资源名称是否正确解析由跟资源本身的名称的资源文件 （.resx 文件） 名称前缀：
    
        public string GetFormHtml(int lcid)
        {
         string htmlTemplate = Resources.MfaFormHtml; //Resxfilename.resourcename
         return htmlTemplate;
        }
    
    现在应能够生成。

## <a name="build-the-adapter"></a>生成的适配器

适配器应内置的可安装到 GAC 中在 Windows 中的强名称的.NET 程序集。  若要在 Visual Studio 项目中实现此目的，完成以下步骤：

1.  右键单击你在解决方案资源管理器中的项目名称，然后单击**属性**。

2.  上**签名**选项卡上，选中**程序集签名**，然后选择 **\<新建...\>** 下**选择强名称密钥文件：**  输入的密钥文件名称和密码，然后单击**确定**。  然后，确保**为程序集签名**已选中并**仅延迟签名**处于未选中状态。  属性**签名**页应如下所示：
    
    ![生成提供程序](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "生成提供程序")

3.  然后生成解决方案。

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>部署到测试计算机以 AD FS 适配器

可以通过 AD FS 来调用外部提供程序之前，必须在系统中注册它。  适配器提供程序必须提供安装程序，它将执行必要的安装操作包括安装在 gac 中，并且安装程序必须在 AD FS 支持注册。  如果这样做，管理员必须执行以下 Windows PowerShell 步骤。  可以在实验室中使用以下步骤，若要启用测试和调试。

### <a name="prepare-the-test-ad-fs-machine"></a>准备测试 AD FS 中的计算机

复制文件并添加到 gac 中。

1.  确保您有 Windows Server 2012 R2 计算机或虚拟机。

2.  安装 AD FS 角色服务和配置包含至少一个节点的场。
    
    有关设置实验室环境中的联合身份验证服务器的详细步骤，请参阅[Windows Server 2012 R2 AD FS 部署指南](https://msdn.microsoft.com/en-us/library/dn486820\(v=msdn.10\))。

3.  将 Gacutil.exe 工具复制到服务器。
    
    可在 Gacutil.exe **%homedrive%\\Program Files (x86)\\Microsoft SDKs\\Windows\\v8.0A\\bin\\NETFX 4.0 工具\\** Windows 8 计算机上。  你将需要**gacutil.exe**文件本身，以及**1033年**， **EN-US**，和其他本地化的资源文件夹下面**NETFX 4.0 工具**位置。

4.  复制提供程序文件 （一个或多个强名称签名的.dll 文件） 到与相同的文件夹位置**gacutil.exe** （location 是仅为方便起见）

5.  将.dll 文件添加到服务器场中每个 AD FS 联合身份验证服务器上的 GAC 中：
    
    示例： 使用命令行工具 GACutil.exe 将 dll 添加到 gac 中： `C:\>.\gacutil.exe /if .\<yourdllname>.dll`
    
    若要查看在 GAC 中生成条目：`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>在 AD FS 中注册您的提供程序

一旦满足上述先决条件，打开在联合服务器上的 Windows PowerShell 命令窗口并输入以下命令 (请注意，是否使用的使用 Windows 内部数据库的联合服务器场，则必须在执行这些命令主联合服务器场的）：

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`
    
    其中 YourTypeName 是.NET 强类型名称："YourDefaultNamespace.YourIAuthenticationAdapterImplementationClassName，程序集名称，版本 = YourAssemblyVersion，区域性 = 中性，PublicKeyToken = YourPublicKeyTokenValue，processorArchitecture = MSIL"
    
    此操作将在 AD FS 中，作为 AnyNameYouWish 上面提供的名称与注册外部提供程序。

2.  重新启动 AD FS 服务 （Windows 服务管理单元中，例如使用）。

3.  运行以下命令： `Get-AdfsAuthenticationProvider`。
    
    这显示为一个提供程序在系统中您的提供程序。
    
    例如：
    
        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv
    
    如果必须在 AD FS 环境中启用设备注册服务，还执行以下脚本：  `PS C:\>net start drs`
    
    若要验证是否已注册的提供程序，请使用以下命令：`PS C:\>Get-AdfsAuthenticationProvider`。
    
    这显示为一个提供程序在系统中您的提供程序。

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>创建调用您的适配器的 AD FS 身份验证策略

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>创建使用 AD FS 管理管理单元中的身份验证策略

1.  打开 AD FS 管理管理单元中 (从服务器管理器**工具**菜单)。

2.  单击**身份验证策略**。

3.  在中心窗格中下,**多重身份验证**，单击**编辑**右侧的链接**全局设置**。

4.  下**选择附加身份验证方法**在页面的底部，选中复选框提供商的 AdminName。 单击 **“应用”** 。

5.  提供的"触发器"来调用下使用您的适配器，MFA**位置**同时检查**Extranet**并**Intranet**，例如。 单击 **“确定”** 。 (若要配置触发器按信赖方，请参阅下面的"创建使用 Windows PowerShell 的身份验证策略"。)

6.  请使用以下命令的结果：
    
    首次使用`Get-AdfsGlobalAuthenticationPolicy`。 你将看到您的提供程序名称为一个 AdditionalAuthenticationProvider 值。
    
    然后，使用`Get-AdfsAdditionalAuthenticationRule`。 应看到规则 Extranet 和 Intranet 配置由于策略管理员 UI 中选择的内容。

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>创建使用 Windows PowerShell 的身份验证策略

1.  首先，启用全局策略中的提供程序：
    
    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`
    

    > [!NOTE]
    > 请注意为 AdditionalAuthenticationProvider 参数提供的值为上面 Register-adfsauthenticationprovider cmdlet 中的"Name"参数提供的值以及与中的"Name"属性相对应Get AdfsAuthenticationProvider cmdlet 的输出。 
    > <P></P>

    
    示例：`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`

2.  接下来，配置全局或信赖方特定于规则触发 MFA:
    
    示例 1： 创建全局规则来要求执行 MFA 的外部请求：`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`
    
    示例 2： 创建 MFA 规则需要 MFA 的外部请求特定的信赖方。  （请注意，单个提供程序无法连接到 Windows Server 2012 R2 中的 AD FS 中的单个信赖方）。
    
        PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
        PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>通过使用您的适配器的 MFA 进行身份验证

最后，执行以下步骤来测试您的适配器：

1.  确保 AD FS 全局主要身份验证类型为 Extranet 和 Intranet （这将演示更轻松地以特定用户的身份验证） 配置为窗体身份验证
    
    1.  在 AD FS 管理单元中下,**身份验证策略**，在**主要身份验证**区域中，单击**编辑**旁边**全局设置**.
        
        1.  或者只需单击**主**tab 键在各**多重策略**UI。

2.  请确保**窗体身份验证**唯一的选项检查的 Extranet 和 Intranet 的身份验证方法。  单击 **“确定”** 。

3.  打开 IDP 发起的登录 html 页 (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) 并以在测试环境中有效的 AD 用户身份登录。

4.  输入凭据进行主要身份验证。

5.  应会看到显示的示例质询问题页 MFA 窗体。 
    
    如果有多个配置适配器时，您将看到 MFA 选择页与您从上面的友好名称。
    
    ![使用适配器进行身份验证](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "使用适配器进行身份验证")
    
    ![使用适配器进行身份验证](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "使用适配器进行身份验证")

此时的工作实现的接口，并且必须了解模型的工作原理。 你可以 trym 作为额外示例 BeginAuthentication TryEndAuthentication 中设置断点。  请注意 BeginAuthentication 的执行方式时用户首次进入 MFA 窗体，而在窗体的每个提交触发 TryEndAuthentication。

## <a name="update-the-adapter-for-successful-authentication"></a>更新成功进行身份验证适配器

等待 – 示例适配器将始终无法成功完成身份验证，但\!  这是因为在你的代码中为 nothing 为 TryEndAuthentication 将返回 null。

完成上述过程后，您将创建基本适配器实现，并将其添加到 AD FS 服务器。  可以获取 MFA 窗体页上，但您不能尚未身份验证，因为你具有尚未将正确的逻辑在 TryEndAuthentication 实现中。  因此，让我们添加的。

回想一下 TryEndAuthentication 实现：

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();
     
     }

让我们更新它，使它不会始终返回 MyPresentationForm()。  为此可以在类中创建一个简单的实用程序方法：

    static bool ValidateProofData(IProofData proofData, IAuthenticationContext authContext)
     {
     if (proofData == null || proofData.Properties == null || !proofData.Properties.ContainsKey("ChallengeQuestionAnswer"))
     {
     throw new ExternalAuthenticationException("Error - no answer found", authContext);
     }
     
     if ((string)proofData.Properties["ChallengeQuestionAnswer"] == "adfabric")
     {
     return true;
     }
     else
     {
     return false;
     }
     }

然后，更新 TryEndAuthentication 按如下所示：

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     outgoingClaims = new Claim[0];
     if (ValidateProofData(proofData, authContext))
     {
     //authn complete - return authn method
     outgoingClaims = new[] 
     {
     // Return the required authentication method claim, indicating the particulate authentication method used.
     new Claim( "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", 
     "http://example.com/myauthenticationmethod1" )
     };
     return null;
     }
     else
     {
     //authentication not complete - return new instance of IAdapterPresentationForm derived class
     return new MyPresentationForm();
     }
     }

现在您必须更新测试框上的适配器。  必须先撤消的 AD FS 策略，然后取消注册从 AD FS 和重新启动 AD FS 中，然后从 GAC 中，删除.dll 文件，然后将新.dll 添加到 GAC 中，然后在 AD FS 中注册、 重新启动 AD FS 中，并重新配置 AD FS 策略。

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>部署和测试 AD FS 计算机上配置更新后的适配器

### <a name="clear-ad-fs-policy"></a>清除 AD FS 策略

清除所有 MFA 相关 MFA UI，如下所示中的复选框，然后单击确定。

![清除策略](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "清除策略")

### <a name="unregister-provider-windows-powershell"></a>取消注册提供程序 (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

示例：`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

请注意，传递为"Name"是相同的值为"Name"的值提供给 Register-adfsauthenticationprovider cmdlet。  它也是从 Get AdfsAuthenticationProvider 输出的"Name"属性。

请注意，注销提供程序之前，你必须删除提供程序从 AdfsGlobalAuthenticationPolicy （不管是清除复选框签入 AD FS 管理管理单元或 Windows PowerShell。）

请注意在此操作后必须重新启动 AD FS 服务。

### <a name="remove-assembly-from-gac"></a>从 GAC 删除程序集

1.  首先，使用以下命令来查找项的完全限定的强名称：`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`
    
    示例：`C:\>.\gacutil.exe /l mfaadapter`

2.  然后，使用以下命令以将其从 gac 中：`.\gacutil /u “<output from the above command>”`
    
    示例：`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>将更新的程序集添加到 GAC

请确保首先粘贴本地更新的.dll 文件。 `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>GAC （命令行） 中的视图程序集

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>在 AD FS 中注册您的提供程序

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  重新启动 AD FS 服务。

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>创建使用 AD FS 管理管理单元中的身份验证策略

1.  打开 AD FS 管理管理单元中 (从服务器管理器**工具**菜单)。

2.  单击**身份验证策略**。

3.  下**多重身份验证**，单击**编辑**右侧的链接**全局设置**。

4.  下**选择附加身份验证方法**，提供程序的 AdminName 选中复选框。 单击 **“应用”** 。

5.  若要提供的"触发器"，以调用使用您的适配器的 MFA，在位置下两项全选**Extranet**并**Intranet**，例如。 单击 **“确定”** 。

### <a name="authenticate-with-mfa-using-your-adapter"></a>通过使用您的适配器的 MFA 进行身份验证

最后，执行以下步骤来测试您的适配器：

1.  确保 AD FS 全局主要身份验证类型配置为**窗体身份验证**Extranet 和 Intranet （这使得它更轻松地以特定用户的身份验证）。
    
    1.  在 AD FS 管理管理单元中下,**身份验证策略**，在**主要身份验证**区域中，单击**编辑**旁边**全局设置**.
        
        1.  或者只需单击**主**multi-factor 策略用户界面中的选项卡。

2.  请确保**窗体身份验证**两个选中的唯一选项**Extranet**并**Intranet**身份验证方法。  单击 **“确定”** 。

3.  打开 IDP 发起的登录 html 页 (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) 并以在测试环境中有效的 AD 用户身份登录。

4.  输入的凭据进行主要身份验证。

5.  应会看到 MFA 窗体显示使用示例质询文本页。
    
    1.  如果有多个配置适配器时，您将看到 MFA 选择页具有友好名称。

输入"adfabric"MFA 身份验证页时，应看到成功的单一登录。

![使用适配器登录](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "使用适配器登录")

![使用适配器登录](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "使用适配器登录")

## <a name="see-also"></a>请参阅

#### <a name="other-resources"></a>其他资源

[其他身份验证方法](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\))  
[使用针对敏感应用程序的附加多重身份验证管理风险](https://msdn.microsoft.com/en-us/library/dn280949\(v=msdn.10\))

