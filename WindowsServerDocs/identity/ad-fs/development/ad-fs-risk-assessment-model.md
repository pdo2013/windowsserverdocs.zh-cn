---
title: 用 AD FS 2019 风险评估模型生成插件
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: ''
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1a4569b1fa3791d1d3b412b0801f216c975aa4dc
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867610"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>用 AD FS 2019 风险评估模型生成插件

现在，你可以生成自己的插件来阻止或将风险评分分配给不同阶段的身份验证请求–请求接收、预身份验证和后身份验证。 这可以通过使用 AD FS 2019 引入的新风险评估模型来完成。 

## <a name="what-is-the-risk-assessment-model"></a>什么是风险评估模型？

风险评估模型是一组接口和类，使开发人员能够读取身份验证请求标头并实现其自己的风险评估逻辑。 然后，已实现的代码（插件） AD FS 身份验证过程中运行。 例如，通过使用模型中包含的接口和类，你可以根据请求标头中包含的客户端 IP 地址，实现代码来阻止或允许身份验证请求。 AD FS 将为每个身份验证请求执行代码，并根据实现的逻辑采取适当的操作。 

该模型允许在 AD FS 身份验证管道的三个阶段（如下所示）中的任意一阶段插件代码

![模型](media/ad-fs-risk-assessment-model/risk1.png)

1.  **请求已收到阶段**–当 AD FS 接收身份验证请求，即用户输入凭据之前，允许生成插件来允许或阻止请求。 你可以在此阶段使用可用的请求上下文（例如，客户端 IP、Http 方法、代理服务器 DNS 等）来执行风险评估。 例如，你可以生成一个插件来从请求上下文中读取 IP，并在 IP 位于风险 Ip 的预定义列表中时阻止身份验证请求。 

2.  **预身份验证阶段**–使生成插件能够允许或阻止请求，即用户提供凭据的点，但在 AD FS 评估它们之前。 在此阶段，除请求上下文外，还包含有关安全上下文（例如，用户令牌、用户标识符等）的信息以及要在风险评估逻辑中使用的协议上下文（例如，身份验证协议、clientid、resourceid 等）。 例如，你可以构建一个插件来防止密码喷涂攻击，方法是从用户令牌读取用户密码，并在密码处于有风险密码的预定义列表中时阻止身份验证请求。 

3.  **身份验证后**–在用户提供凭据并 AD FS 执行身份验证后，启用构建插件来评估风险。 在此阶段，除请求上下文、安全上下文和协议上下文外，还包含有关身份验证结果（成功或失败）的信息。 该插件可以根据可用信息评估风险评分，并将风险评分传递给理赔和策略规则以进行进一步评估。 

为了更好地了解如何生成风险评估插件并使其在 AD FS 过程中运行，我们构建了一个示例插件，用于阻止来自某些**extranet** IPs 的请求，这些 ip 被确定为有风险，将插件注册到 AD FS，最后测试性能. 

>[!NOTE]
>本演练只演示如何创建示例插件。 目前，解决方案是创建企业就绪解决方案。  

## <a name="building-a-sample-plug-in"></a>生成示例插件

### <a name="pre-requisites"></a>先决条件
下面是生成此示例插件所需的先决条件列表

- 安装和配置 AD FS 2019
- .NET Framework 4.7 及更高版本
- Visual Studio

### <a name="build-plug-in-dll"></a>生成插件 dll
以下过程将引导您生成一个示例插件 dll。

1. 下载示例插件，使用 Git Bash，并键入以下内容： 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. 在 AD FS 服务器上的任意位置创建一个 **.csv**文件（在我的示例中，我在**C:\extensions**创建了**authconfigdb**文件），并将要阻止的 ip 添加到此文件中。 

   示例插件将阻止来自此文件中列出的**Extranet ip**的任何身份验证请求。 

   >{!注意] 如果你有 AD FS 场，你可以在任何或所有 AD FS 服务器上创建文件。 任何文件均可用于将有风险的 Ip 导入 AD FS。 下面将详细讨论导入过程，请在下面的[AD FS 中注册插件 dll](#register-the-plug-in-dll-with-ad-fs) 。 

3. 使用 Visual Studio `ThreatDetectionModule.sln`打开项目

4. 从 " `Microsoft.IdentityServer.dll`解决方案资源管理器" 中删除，如下所示：</br>
   ![model](media/ad-fs-risk-assessment-model/risk2.png)

5. 向 AD FS 添加对`Microsoft.IdentityServer.dll`的引用，如下所示

   a.   在**解决方案资源管理器**中右键单击 "**引用**"，然后选择 "**添加引用 ...** "</br> 
   ![模式](media/ad-fs-risk-assessment-model/risk3.png)
   
   b.   在 "**引用管理器**" 窗口中选择 "**浏览**"。 在 "**选择要引用的文件 ...** " 对话框中， `Microsoft.IdentityServer.dll`选择 AD FS 安装文件夹（在 my case **C:\Windows\ADFS**中），然后单击 "**添加**"。
   
   >[!NOTE]
   >在我的示例中，我在 AD FS 服务器本身上构建了插件。 如果你的开发环境在不同的服务器上，请`Microsoft.IdentityServer.dll`将 AD FS server 上的 AD FS 安装文件夹中的复制到你的开发框。</br> 
   
   ![模型](media/ad-fs-risk-assessment-model/risk4.png)
   
   c.   确保`Microsoft.IdentityServer.dll`选中复选框后，在 "**引用管理器**" 窗口上单击 **"确定"**</br>
   ![model](media/ad-fs-risk-assessment-model/risk5.png)
 
6. 现在，所有类和引用都已准备就绪，可用于生成。   不过，由于此项目的输出是一个 dll，因此必须将其安装到 AD FS 服务器的**全局程序集缓存**（GAC）中，并且需要首先对该 dll 进行签名。 可按如下所示执行此操作：

   a.   **右键单击**项目名称 ThreatDetectionModule。 在菜单中单击 "**属性**"。</br>
   ![model](media/ad-fs-risk-assessment-model/risk6.png)
   
   b.   在 "**属性**" 页上，单击左侧的 "**签名**"，然后选中标记为 **"为程序集签名"** 的复选框。 从 "**选择强名称密钥文件**：" 下拉菜单中，选择 " **< 新建 ...">**</br>
   ![model](media/ad-fs-risk-assessment-model/risk7.png)

   c.   在 "**创建强名称密钥" 对话框**中，为密钥键入名称（可选择任何名称），取消选中 "**使用密码保护密钥文件**" 复选框。 然后单击 **"确定"** 。
   ![model](media/ad-fs-risk-assessment-model/risk8.png)</br>
 
   d.   保存项目，如下所示</br>
   ![model](media/ad-fs-risk-assessment-model/risk9.png)

7. 单击 "**生成**"，然后重新生成**解决方案**以生成项目，如下所示</br>
   ![model](media/ad-fs-risk-assessment-model/risk10.png)
 
   检查屏幕底部的 "**输出" 窗口**，查看是否发生了任何错误</br>
   ![model](media/ad-fs-risk-assessment-model/risk11.png)


插件（dll）现在可供使用，并且位于项目文件夹的 **\bin\Debug**文件夹（在本例中为**C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**）。 

下一步是将此 dll 注册到 AD FS，因此它将以 AD FS 身份验证过程运行。 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>将插件 dll 注册到 AD FS

我们需要在 AD FS 服务器上使用`Register-AdfsThreatDetectionModule` PowerShell 命令在 AD FS 中注册该 dll，但在注册之前，我们需要获取公钥标记。 此公钥标记是在创建密钥时创建的，并使用该密钥对 dll 进行签名。 若要了解 dll 的公钥标记是什么，可以使用**sn.exe** ，如下所示

1. 将 **\bin\Debug**文件夹中的 dll 文件复制到另一个位置（在本例中将其复制到**C:\extensions**）

2. 启动 Visual Studio 的**开发人员命令提示**，然后前往包含**sn.exe**的目录（在本例中，目录为**C:\Program Files （x86） \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**） ![model](media/ad-fs-risk-assessment-model/risk12.png)

3. 用 **-T**参数和文件（在我的示例`SN -T “C:\extensions\ThreatDetectionModule.dll”`中） ![模型的位置运行**SN**命令](media/ad-fs-risk-assessment-model/risk13.png)</br>
   此命令将为你提供公钥令牌（对于我，**公钥标记是 714697626ef96b35**）

4. 将 dll 添加到 AD FS 服务器的**全局程序集缓存**中我们最好的做法是为项目创建正确的安装程序，并使用安装程序将文件添加到 GAC 中。 另一种解决方案是在开发计算机上使用**gacutil.exe** （有关**gacutil.exe** [可用的](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)详细信息）。  由于我的 visual studio 与 AD FS 位于同一服务器上，因此我将使用**gacutil.exe** ，如下所示

   a.   在 Visual Studio 开发人员命令提示上，并中转到包含**gacutil.exe**的目录（在本例中，目录为**C:\Program Files （x86） \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**）

   b.   运行**gacutil.exe**命令（在我的示例`Gacutil /IF C:\extensions\ThreatDetectionModule.dll`中![为）模型](media/ad-fs-risk-assessment-model/risk14.png)
 
   >[!NOTE]
   >如果你有 AD FS 场，则需要在服务器场中的每个 AD FS 服务器上执行以上。 

5. 打开**Windows PowerShell**并运行以下命令以注册 dll
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>”
   ```
   在我的示例中，该命令为： 
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv”
   ```
 
   >[!NOTE]
   >只需注册一次 dll，即使有 AD FS 场。 

6. 注册 dll 后重新启动 AD FS 服务

就是这样，dll 现在已注册 AD FS 并且可供使用！

 >[!NOTE]
 > 如果对插件进行了任何更改，并且重新生成了项目，则需要重新注册更新后的 dll。 注册之前，需要使用以下命令取消注册当前 dll：</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> 在我的示例中，该命令为：
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>测试插件

1. 打开我们之前创建的**authconfig**文件（在**C:\extensions**位置的情况下），并添加要阻止的**Extranet ip** 。 每个 IP 都应在单独的行上，并且末尾不应有空格</br>
   ![model](media/ad-fs-risk-assessment-model/risk18.png)
 
2. 保存并关闭文件

3. 通过运行以下 PowerShell 命令，在 AD FS 中导入已更新的文件 

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```
 
   在我的示例中，该命令为： 
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```
 
4. 通过在**authconfig**中添加的相同 IP 启动来自服务器的身份验证请求。

   在本演示中，我将使用[AD FS 帮助声明 X 光工具](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)来启动请求。 如果要使用 X 光工具，请按照说明进行操作 

   输入 "联合服务器实例" 和 "命中**测试身份验证**" 按钮。</br> 
   ![模式](media/ad-fs-risk-assessment-model/risk15.png) 

5. 身份验证被阻止，如下所示。</br>
   ![model](media/ad-fs-risk-assessment-model/risk16.png)
 
现在，我们已经了解了如何生成和注册插件，接下来让我们演练插件代码，以了解如何使用模型引入的新接口和类实现。 

## <a name="plug-in-code-walkthrough"></a>插件代码演练

使用 Visual Studio `ThreatDetectionModule.sln`打开项目，然后在屏幕右侧的 "**解决方案资源管理器**" 中打开主文件**UserRiskAnalyzer.cs**</br>
![model](media/ad-fs-risk-assessment-model/risk17.png)
 
文件包含用于实现抽象类[ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019)和 interface [IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019)的主类 UserRiskAnalyzer，用于从请求上下文读取 ip，并将获得的 ip 与 ip 进行比较从 AD FS DB 加载，如果有 IP 匹配，则阻止请求。 让我们更详细地了解这些类型

### <a name="threatdetectionmodule-abstract-class"></a>ThreatDetectionModule 抽象类

此抽象类将插件加载到 AD FS 管道中，以便可以在 AD FS 过程中运行插件代码。 

```
public abstract class ThreatDetectionModule 
{ 
    protected ThreatDetectionModule(); 
 
    public abstract string VendorName { get; } 
    public abstract string ModuleIdentifier { get; } 
 
 public abstract void OnAuthenticationPipelineLoad(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData); 
 public abstract void OnAuthenticationPipelineUnload(ThreatDetectionLogger logger); 
  public abstract void OnConfigurationUpdate(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData); 
 }   
```
类包括以下方法和属性。

|方法 |type|定义|
|-----|-----|-----| 
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |导致|当插件加载到其管道中时 AD FS 调用| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |导致|当从其管道中卸载插件时 AD FS 调用| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| 导致|在配置更新时由 AD FS 调用 |
|**知识产权** |类型 |**定义**|
|[VendorName](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|String |获取拥有插件的供应商的名称|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|String |获取插件的标识符|

在我们的示例插件中，使用[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019)和[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)方法从 AD FS DB 读取预定义的 ip。 将插件注册到 AD FS 时，将调用[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) ，而当使用 `Import-AdfsThreatDetectionModuleConfiguration`cmdlet 导入 .csv 时调用 [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)。 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>IRequestReceivedThreatDetectionModule 接口

利用此[接口](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019)，你可以在 AD FS 接收身份验证请求，但用户在身份验证过程中收到请求阶段之前，在用户输入凭据之前实施风险评估。 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

接口包含[EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019)方法，该方法允许你使用 requestContext 输入参数中传递的身份验证请求的上下文来编写你的风险评估逻辑。 RequestContext 参数的类型为[requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)。 

传递的另一个输入参数是[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)类型的记录器。 参数可用于将错误、审核和/或调试消息写入 AD FS 日志。 

此方法返回[ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) （0，如果 NotEvaluated，1到 Block，2表示允许） AD FS 这会阻止或允许该请求。

在示例插件中， [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019)方法实现会分析[requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)参数中的[clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) ，并将其与从 AD FS DB 加载的所有 ip 进行比较。 如果找到匹配项，则方法将为**Block**返回2，否则对于**Allow**返回1。 根据返回的值，AD FS 阻止或允许请求。 

>[!NOTE]
>上面所述的示例插件仅实现了 IRequestReceivedThreatDetectionModule 接口。 但是，风险评估模型提供两个附加接口– IPreAuthenticationThreatDetectionModule （用于实现风险评估逻辑用预身份验证阶段）和 IPostAuthenticationThreatDetectionModule （实现风险在后期身份验证阶段评估逻辑）。 下面提供了有关这两个接口的详细信息。 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>IPreAuthenticationThreatDetectionModule 接口 

利用此[接口](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019)，你可以在用户提供凭据的位置实现风险评估逻辑，但在 AD FS 评估它们之前（即，预身份验证阶段）。 

```
public interface IPreAuthenticationThreatDetectionModule
{
Task<ThrottleStatus> EvaluatePreAuthentication (
ThreatDetectionLogger logger, 
RequestContext requestContext, 
SecurityContext securityContext, 
ProtocolContext protocolContext, 
IList<Claim> additionalClams
);
}
```
接口包含[EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019)方法，该方法允许你使用[RequestContext RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)、 [SecurityContext SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)、 [ProtocolContext ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)和中传递的信息。[IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2)输入参数，用于写入预身份验证风险评估逻辑。 

>[!NOTE]
>有关每个上下文类型传递的属性列表，请访问[RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)、 [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)和[ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)类定义。 

传递的另一个输入参数是[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)类型的记录器。 参数可用于将错误、审核和/或调试消息写入 AD FS 日志。

此方法返回[ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) （0，如果 NotEvaluated，1到 Block，2表示允许） AD FS 这会阻止或允许该请求。 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>IPostAuthenticationThreatDetectionModule 接口

利用此[接口](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019)，你可以在用户提供凭据并 AD FS 执行身份验证后（即身份验证后阶段）执行风险评估逻辑。 

```
public interface IPostAuthenticationThreatDetectionModule
{
Task<RiskScore> EvaluatePostAuthentication (
ThreatDetectionLogger logger, 
RequestContext requestContext, 
SecurityContext securityContext, 
ProtocolContext protocolContext, 
AuthenticationResult authenticationResult, 
IList<Claim> additionalClams
);
}
```

接口包含[EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019)方法，该方法允许你使用[RequestContext RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)、 [SecurityContext SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)、 [ProtocolContext ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)中传递的信息。和[IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2)输入参数，用于编写身份验证后的风险评估逻辑。 

>[!NOTE]
> 有关随每个上下文类型一起传递的属性的完整列表，请参阅[RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)、 [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)和[ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)类定义。 

传递的另一个输入参数是[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)类型的记录器。 参数可用于将错误、审核和/或调试消息写入 AD FS 日志。 

方法返回可用于 AD FS 政策和声明规则的[风险评分](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019)。 

>[!NOTE]
>若要运行插件，main 类（在本例中为 UserRiskAnalyzer）需要派生[ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019)抽象类，并且应至少实现上述三个接口中的一个。 注册 dll 后，AD FS 会检查要实现的接口并在管道中的适当阶段调用它们。

### <a name="faqs"></a>常见问题

**为什么应生成这些插件？**</br>
**答：** 这些插件不仅为你提供额外的功能来保护你的环境免受攻击（例如密码喷涂攻击），而且还能灵活地根据你的需求构建你自己的风险评估逻辑。 

**日志捕获到何处？**</br>
**答：** 可以使用 WriteAdminLogErrorMessage 方法将错误日志写入 "AD FS/Admin" 事件日志，使用 WriteAuditMessage 方法将审核日志写入 "AD FS 审核" 安全日志，使用 WriteDebugMessage 方法将日志调试到 "AD FS 跟踪" 调试日志。 

**添加这些插件是否可以增加 AD FS 身份验证过程延迟？**</br>
**答：** 延迟的影响取决于执行所实现的风险评估逻辑所需的时间。 建议在生产环境中部署插件之前，评估延迟的影响。 

**为什么不能 AD FS 建议有风险的 Ip、用户等的列表？**</br>
**答：** 虽然目前不可用，但我们正在努力构建智能，以在可插接式风险评估模型中提出有风险的 Ip、用户等。 我们将很快共享启动日期。 
