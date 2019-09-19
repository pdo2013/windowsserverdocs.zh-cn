---
title: 为 Windows Server 中的 AD FS 构建自定义身份验证方法
description: 此方案描述如何为 Windows Server 中的 AD FS 构建自定义身份验证方法。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fc71ca2b8d130ab00014f850ccae25e9138d501b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867568"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>为 Windows Server 中的 AD FS 构建自定义身份验证方法

本演练说明如何为 Windows Server 2012 R2 中的 AD FS 实现自定义身份验证方法。 有关详细信息，请参阅[附加身份验证方法](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\))。


> [!WARNING]
> 你可以在此处&nbsp;生成的示例仅供教育之用。 &nbsp;这些说明适用于公开模型所需元素的最简单的最小实现。&nbsp;没有身份验证后端、错误处理或配置数据。 
> <P></P>



## <a name="setting-up-the-development-box"></a>设置开发框

本演练使用 Visual Studio 2012。  可以使用可为 Windows 创建 .NET 类的任何开发环境生成项目。 项目必须面向 .NET 4.5 .NET Framework，因为**BeginAuthentication**和**TryEndAuthentication**方法使用**的类型为**""，这是该项目所需的一个引用：


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>引用 dll</strong></p></td>
<td><p><strong>查找位置</strong></p></td>
<td><p><strong>所需的</strong></p></td>
</tr>
<tr class="even">
<td><p>IdentityServer。</p></td>
<td><p>该 dll 位于安装了 AD FS 的 Windows Server 2012 R2 服务器上的%windir%\ADFS 中。</p>
<p></p>
<p>必须将此 dll 复制到开发计算机，并在该项目中创建一个显式引用。</p></td>
<td><p>接口类型，包括 IAuthenticationContext、IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>创建提供程序

1.  在 Visual Studio 2012 中：选择 "文件\>-新建\>-项目 ..."

2.  选择 "类库"，并确保以 .NET 4.5 为目标。

    ![创建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "创建提供程序")

3.  在 Windows Server 2012 R2 服务器上的% windir%\\ADFS 中创建 IdentityServer 的副本，其中已安装 AD FS，并将其粘贴到你的开发计算机上的项目文件夹中。

4.  在**解决方案资源管理器**中，右键单击 "**引用**"，然后单击 "**添加引用 ...** "

5.  浏览到**IdentityServer**的本地副本，然后**添加 ...**

6.  单击 **"确定"** 以确认新的引用：

    ![创建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "创建提供程序")

    现在应设置为解析提供程序所需的所有类型。 

7.  向项目中添加新类（右键单击项目，**添加 ...类 ...** ）并为其指定一个名称（如**MyAdapter**），如下所示：

    ![创建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "创建提供程序")

8.  在新文件 MyAdapter.cs 中，将现有代码替换为以下代码：

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

    现在，你应该能够在 IAuthenticationAdapter 上按 F12 （右键单击– "前往定义"）来查看所需的接口成员集。 

    接下来，您可以执行这些操作的简单实现。

9.  将类的全部内容替换为以下内容：

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

10. 我们尚未准备好生成 .。。还有两个接口可以继续。

    向项目添加另外两个类：一个用于元数据，另一个用于显示形式。  您可以将这些文件添加到上面的类所在的文件中。

        class MyMetadata : IAuthenticationAdapterMetadata
         {

         }

         class MyPresentationForm : IAdapterPresentationForm
         {

         }

11. 接下来，你可以为每个添加所需的成员。首先，元数据（包含有用的内嵌注释）

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

         /// Returns an array indicating the type of claim that the adapter uses to identify the user being authenticated.
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

    接下来，显示形式如下：

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


~~~
     }
~~~

12. 请注意上面的**FormPageHtml**元素的 "todo"。 

   你可以一分钟修复此问题，但首先，让我们根据新实现的类型将最后的必需 return 语句添加到初始 MyAdapter 类。  若要执行此操作，请在现有的 IAuthenticationAdapter 实现中将下面的*斜体*项添加到现有的实现：

       类 MyAdapter：IAuthenticationAdapter {public IAuthenticationAdapterMetadata Metadata {//get {return new <instance of IAuthenticationAdapterMetadata derived class>;}    get {return new MyMetadata （）;}    }

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

13. 现在，对于包含 html 片段的资源文件。 在项目文件夹中创建一个具有以下内容的新文本文件：

       <div id="loginArea">
        <form method="post" id="loginForm" >
        <!-- These inputs are required by the presentation framework. Do not modify or remove -->
        <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
        <input id="context" type="hidden" name="Context" value="%Context%"/>
        <!-- End inputs are required by the presentation framework. -->
        <p id="pageIntroductionText">此内容由 MFA 示例适配器提供。 质询输入如下所示。</p>
        <label for="challengeQuestionInput" class="block">问题文本</label>
        <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
        <div id="submissionArea" class="submitMargin">
        <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
        </div>
        </form>
        <div id="intro" class="groupMargin">
        <p id="supportEmail">支持信息</p>
        </div>
        <script type="text/javascript" language="JavaScript">
        //<![CDATA[
        function AuthPage() { }
        AuthPage.submitAnswer = function () { return true; };
        //]]>
        </script></div>

14. 然后，选择 **"项目\>-添加组件 ..."资源**文件并将文件命名为**资源**，然后单击 "**添加"：**

   ![创建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "创建提供程序")

15. 然后，在**Resources .resx**文件中，选择 "**添加资源 ..."添加现有文件**。  导航到上面保存的文本文件（包含 html 片段）。

   确保 GetFormHtml 代码通过资源文件（.resx 文件）名称前缀后跟资源本身的名称，正确解析新资源的名称：

       public string GetFormHtml （int lcid） {string htmlTemplate = MfaFormHtml;//Resxfilename.resourcename return htmlTemplate;   }

   现在应能够生成。

## <a name="build-the-adapter"></a>构建适配器

适配器应内置于可安装到 Windows 中 GAC 中的强命名 .NET 程序集中。  若要在 Visual Studio 项目中实现此目的，请完成以下步骤：

1.  在解决方案资源管理器中右键单击项目名称，然后单击 "**属性**"。

2.  在 "**签名**" 选项卡上，选中 **"为程序集签名"** ，然后选择 **\<"新建 ..."在\>** "**选择强名称密钥文件**" 下：输入密钥文件名和密码，然后单击 **"确定"** 。  然后确保选中 **"为程序集签名"** ，并取消选中 "**延迟签名**"。  "属性**签名**" 页应如下所示：

    ![构建提供程序](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "构建提供程序")

3.  然后生成解决方案。

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>将适配器部署到 AD FS 测试计算机

在 AD FS 调用外部提供程序之前，必须在系统中注册该提供程序。  适配器提供程序必须提供安装程序，该程序执行必要的安装操作，包括安装在 GAC 中，安装程序必须支持在 AD FS 中进行注册。  如果未执行此操作，则管理员需要执行下面的 Windows PowerShell 步骤。  可以在实验室中使用这些步骤来启用测试和调试。

### <a name="prepare-the-test-ad-fs-machine"></a>准备测试 AD FS 计算机

复制文件并将其添加到 GAC 中。

1.  确保你有 Windows Server 2012 R2 计算机或虚拟机。

2.  安装 AD FS 角色服务，并使用至少一个节点配置场。

    有关在实验室环境中设置联合服务器的详细步骤，请参阅[Windows server 2012 R2 AD FS 部署指南](https://msdn.microsoft.com/library/dn486820\(v=msdn.10\))。

3.  将 Gacutil.exe 工具复制到服务器。

    Gacutil.exe 可以在 windows 8 计算机上的 **% homedrive\\% Program Files （x86\\） Microsoft\\sdk\\Windows v2.0 a\\bin\\NETFX 4.0 工具\\** 中找到。  你将需要**gacutil.exe**文件本身以及**1033**、 **en-us**和**NETFX 4.0 工具**位置下面的其他本地化资源文件夹。

4.  将提供程序文件（一个或多个强名称签名 .dll 文件）复制到与**gacutil.exe**相同的文件夹位置（该位置只是为了方便）

5.  将 .dll 文件添加到场中每个 AD FS 联合服务器上的 GAC 中：

    示例：使用命令行工具 Gacutil.exe 将 dll 添加到 GAC：`C:\>.\gacutil.exe /if .\<yourdllname>.dll`

    查看 GAC 中的生成条目：`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>将提供程序注册到 AD FS

满足上述先决条件后，请在联合服务器上打开 Windows PowerShell 命令窗口并输入以下命令（请注意，如果使用的是使用 Windows 内部数据库的联合服务器场，则必须在场的主联合服务器）：

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    其中，YourTypeName 是 .NET 强类型名称："YourDefaultNamespace，YourIAuthenticationAdapterImplementationClassName，; Yourassemblyname&gt，Version = YourAssemblyVersion，Culture = 中立，PublicKeyToken = YourPublicKeyTokenValue，processorArchitecture = MSIL"

    这会将你的外部提供程序注册到 AD FS，并将其名称作为 AnyNameYouWish 提供给你。

2.  重新启动 AD FS 服务（例如，使用 Windows 服务管理单元）。

3.  运行以下命令: `Get-AdfsAuthenticationProvider`。

    这会将提供程序显示为系统中的某个提供程序。

    例如：

        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv

    如果在 AD FS 环境中启用了设备注册服务，则还需执行以下操作：`PS C:\>net start drs`

    若要验证注册的提供程序，请使用以下`PS C:\>Get-AdfsAuthenticationProvider`命令：。

    这会将提供程序显示为系统中的某个提供程序。

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>创建调用适配器的 AD FS 身份验证策略

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>使用 "AD FS 管理" 管理单元创建身份验证策略

1.  从 "服务器管理器**工具**" 菜单中打开 "AD FS 管理" 管理单元。

2.  单击 "**身份验证策略**"。

3.  在中心窗格中的 "**多重身份验证**" 下，单击 "**全局设置**" 右侧的 "**编辑**" 链接。

4.  在页面底部的 "**选择其他身份验证方法**" 下，选中提供商的 AdminName 的复选框。 单击 **“应用”** 。

5.  若要提供 "触发器" 来使用适配器调用 MFA，请在 "**位置**" 下检查**Extranet**和**Intranet**，例如。 单击 **“确定”** 。 （若要配置每个信赖方的触发器，请参阅下面的 "使用 Windows PowerShell 创建身份验证策略"。）

6.  使用以下命令检查结果：

    第一`Get-AdfsGlobalAuthenticationPolicy`次使用。 你应看到提供程序名称为 AdditionalAuthenticationProvider 值之一。

    然后使用`Get-AdfsAdditionalAuthenticationRule`。 你应看到配置为 "管理员" UI 中策略选择后的 Extranet 和 Intranet 的规则。

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>使用 Windows PowerShell 创建身份验证策略

1.  首先，在全局策略中启用提供程序：

    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`


~~~
> [!NOTE]
> Note that the value provided for the AdditionalAuthenticationProvider parameter corresponds to the value you provided for the “Name” parameter in the Register-AdfsAuthenticationProvider cmdlet above and to the “Name” property from Get-AdfsAuthenticationProvider cmdlet output. 
> <P></P>


Example:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
~~~

2. 接下来，配置全局或信赖方特定的规则以触发 MFA：

   示例1：创建全局规则以要求对外部请求进行 MFA：`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`

   示例2：创建 MFA 规则以要求对特定信赖方的外部请求进行 MFA。  （请注意，单个提供程序无法连接到 Windows Server 2012 R2 的 AD FS 中的单个依赖方。

       PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
       PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>使用适配器通过 MFA 进行身份验证

最后，执行以下步骤来测试适配器：

1.  确保为 Extranet 和 Intranet 将 AD FS 全局主要身份验证类型配置为窗体身份验证（这使得演示更易于作为特定用户进行身份验证）

    1.  在 "AD FS" 管理单元中，在 "**身份验证策略**" 下的 "**主要身份验证**" 区域中，单击 "**全局设置**" 旁边的 "**编辑**"。

        1.  或者只需单击**多因素策略**UI 中的 "**主要**" 选项卡即可。

2.  确保为 Extranet 和 Intranet 身份验证方法都同时检查**Forms 身份验证**。  单击 **“确定”** 。

3.  打开 "IDP 发起的登录" html 页（ https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm），并以有效的 AD 用户身份登录到测试环境中。

4.  输入用于主要身份验证的凭据。

5.  应会看到 "MFA 窗体" 页，其中显示了示例质询问题。 

    如果配置了多个适配器，则会看到 "MFA" 选项页，其中包含上面的友好名称。

    ![通过适配器进行身份验证](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "通过适配器进行身份验证")

    ![通过适配器进行身份验证](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "通过适配器进行身份验证")

现在，你已有了接口的工作实现，并且你了解了该模型的工作原理。 您可以 trym 为在 BeginAuthentication 和 TryEndAuthentication 中设置断点的额外示例。  请注意，当用户首次输入 MFA 窗体时，如何执行 BeginAuthentication，而在每次提交窗体时都会触发 TryEndAuthentication。

## <a name="update-the-adapter-for-successful-authentication"></a>更新适配器以成功进行身份验证

但请等待–你的示例适配器将永远不会成功进行身份验证\!  这是因为代码中的任何内容都不会为 TryEndAuthentication 返回 null。

完成上述过程后，创建了一个基本的适配器实现并将其添加到 AD FS 服务器。  你可以获取 "MFA 窗体" 页，但仍无法进行身份验证，因为尚未将正确的逻辑放入 TryEndAuthentication 实现中。  接下来，让我们添加。

回忆一下你的 TryEndAuthentication 实现：

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();

     }

让我们进行更新，使其不会始终返回 MyPresentationForm （）。  为此，可以在类中创建一个简单的实用工具方法：

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

然后，按如下所示更新 TryEndAuthentication：

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

现在，您必须更新 "测试" 框中的适配器。  您必须先撤消 AD FS 策略，然后取消注册 AD FS 并重新启动 AD FS，然后从 GAC 中删除该 .dll，然后将新 .dll 添加到 GAC 中，然后在 AD FS 中注册它，重新启动 AD FS 并重新配置 AD FS 策略。

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>在测试 AD FS 计算机上部署和配置已更新的适配器

### <a name="clear-ad-fs-policy"></a>清除 AD FS 策略

清除 MFA UI 中所有与 MFA 相关的复选框，如下所示，然后单击 "确定"。

![清除策略](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "清除策略")

### <a name="unregister-provider-windows-powershell"></a>撤消注册提供程序（Windows PowerShell）

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

实例`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

请注意，你为 "Name" 传递的值与你向 Register-adfsauthenticationprovider cmdlet 提供的 "名称" 的值相同。  它也是从 Register-adfsauthenticationprovider 输出的 "Name" 属性。

请注意，在取消注册某个提供程序之前，必须从 AdfsGlobalAuthenticationPolicy 中删除该提供程序（通过清除你在 AD FS 管理 "管理单元或使用 Windows PowerShell 中签入的复选框）。

请注意，在完成此操作后，必须重新启动 AD FS 服务。

### <a name="remove-assembly-from-gac"></a>从 GAC 中删除程序集

1.  首先，使用以下命令查找条目的完全限定强名称：`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`

    实例`C:\>.\gacutil.exe /l mfaadapter`

2.  然后，使用以下命令将其从 GAC 中删除：`.\gacutil /u “<output from the above command>”`

    实例`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>将更新后的程序集添加到 GAC

请确保先在本地粘贴更新的 .dll。 `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>查看 GAC 中的程序集（cmd 行）

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>将提供程序注册到 AD FS

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  重新启动 AD FS 服务。

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>使用 "AD FS 管理" 管理单元创建身份验证策略

1.  从 "服务器管理器**工具**" 菜单中打开 "AD FS 管理" 管理单元。

2.  单击 "**身份验证策略**"。

3.  在 "**多重身份验证**" 下，单击 "**全局设置**" 右侧的 "**编辑**" 链接。

4.  在 "**选择其他身份验证方法**" 下，选中提供程序的 AdminName 的复选框。 单击 **“应用”** 。

5.  若要提供 "触发器" 来使用适配器调用 MFA，请在 "位置" 下检查**Extranet**和**Intranet**，例如。 单击 **“确定”** 。

### <a name="authenticate-with-mfa-using-your-adapter"></a>使用适配器通过 MFA 进行身份验证

最后，执行以下步骤来测试适配器：

1.  确保为 Extranet 和 Intranet 将 AD FS 全局主要身份验证类型配置为**窗体身份验证**（这样可以更容易地以特定用户身份进行身份验证）。

    1.  在 AD FS 管理 "管理单元中，在"**身份验证策略**"下的"**主要身份验证**"区域中，单击"**全局设置**"旁边的"**编辑**"。

        1.  或者只需单击多因素策略 UI 中的 "**主要**" 选项卡即可。

2.  确保为**Extranet**和**Intranet**身份验证方法都同时检查**Forms 身份验证**。  单击 **“确定”** 。

3.  打开 "IDP 发起的登录" html 页（ https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm），并以有效的 AD 用户身份登录到测试环境中。

4.  输入用于主要身份验证的凭据。

5.  应会看到 "MFA 窗体" 页，其中显示了示例 "质询" 文本。

    1.  如果配置了多个适配器，则会看到具有友好名称的 MFA 选项页。

在 MFA 身份验证页上输入 "adfabric" 时，应会看到成功的登录。

![用适配器登录](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "用适配器登录")

![用适配器登录](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "用适配器登录")

## <a name="see-also"></a>请参阅

#### <a name="other-resources"></a>其他资源

[其他身份验证方法](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\))  
[使用适用于敏感应用程序的附加多重身份验证管理风险](https://msdn.microsoft.com/library/dn280949\(v=msdn.10\))

