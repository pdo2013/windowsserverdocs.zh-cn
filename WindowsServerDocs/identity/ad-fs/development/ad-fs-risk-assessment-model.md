---
title: 构建插件与 AD FS 2019 风险评估模型
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: ''
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 80f42695af917084ee63297df052adc069340bb3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190520"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>构建插件与 AD FS 2019 风险评估模型

你现在可以构建自己的插件来阻止或在各个阶段 – 请求已接收、 预身份验证和后身份验证期间分配给身份验证请求的风险评分。 这可以通过与 AD FS 2019 引入的新风险评估模型。 

## <a name="what-is-the-risk-assessment-model"></a>风险评估模型是什么？

风险评估模型是一组接口和使开发人员能够阅读身份验证的请求标头并实施其自己的风险评估逻辑的类。 然后，（插件） 的实现的代码运行符合 AD FS 身份验证过程。 例如，使用的接口和该模型中包含的类，可以实现对任一块的代码或允许基于请求标头中包含的客户端 IP 地址的身份验证请求。 AD FS 将执行每个身份验证请求的代码，并采取相应的措施根据实现的逻辑。 

在 AD FS 身份验证管道，如下所示的三个任何的阶段模型允许插件代码

![模型](media\ad-fs-risk-assessment-model\risk1.png)

1.  **请求收到阶段**– 允许构建插件，以允许或阻止请求时 AD FS 身份验证请求即之前接收用户输入凭据。 可以使用的请求上下文 (例如，客户端 IP、 Http 方法、 代理服务器 DNS 等) 可在此阶段执行风险评估。 例如，可以生成插件以从请求上下文中读取 IP 和阻止身份验证请求，如果 IP 风险 Ip 的预定义列表中。 

2.  **预身份验证阶段**– 允许构建插件，以允许或阻止在该点的请求，其中用户提供凭据，但 AD FS 对它们进行评估之前。 在此阶段，除了请求上下文也可以在安全上下文 （例如，用户令牌，用户标识符，等等） 和要在风险评估逻辑中使用的协议上下文 （例如，身份验证协议、 clientid、 资源 id 等） 上的信息。 例如，可以生成插件以防止密码喷射攻击的用户令牌读取用户密码和密码有风险的密码预定义列表中阻止身份验证请求。 

3.  **后身份验证**– 允许构建插件，用户已提供凭据和 AD FS 执行了身份验证后评估风险。 在此阶段，请求上下文、 安全上下文，以及协议上下文中，你还可以在身份验证结果 （成功或失败） 的信息。 该插件可以评估风险评分基于可用的信息，并将传递的风险评分来声明和进一步评估的策略规则。 

若要更好地了解如何生成插件的风险评估并根据 AD FS 进程运行，让我们构建示例插件可阻止来自某些请求**extranet** Ip 标识为存在风险，注册该插件使用 AD FS和最后测试的功能。 

>[!NOTE]
>本演练只是向您展示如何创建一个示例插件。 不是我们要创建企业就绪解决方案的解决方案。  

## <a name="building-a-sample-plug-in"></a>生成示例插件

### <a name="pre-requisites"></a>先决条件
下面是生成此示例插件所需的系统必备组件的列表

- 安装并配置了 AD FS 2019
- .NET framework 4.7 及更高版本
- Visual Studio

### <a name="build-plug-in-dll"></a>构建插件 dll
以下过程将引导您完成生成示例插件 dll。

 1. 下载插件示例、 使用 Git Bash 并键入以下命令： 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

 2. 创建 **.csv** AD FS 服务器上的任何位置的文件 (在本例中，我创建了**authconfigdb.csv**文件**C:\extensions**)，并添加你想要阻止对此文件的 Ip。 

   插件示例会阻止来自任何身份验证请求**Extranet Ip**此文件中列出。 

   >{!请注意] 如果你有 AD FS 场，可以创建任何或所有 AD FS 服务器上的文件。 可以使用的任何文件导入 AD FS 的风险的 Ip。 导入过程中的详细信息中，我们将讨论[向 AD FS 注册插件 dll](#register-the-plug-in-dll-with-ad-fs)下面一节。 

 3. 打开项目`ThreatDetectionModule.sln`使用 Visual Studio

 4. 删除`Microsoft.IdentityServer.dll`从解决方案资源管理器，如下所示：</br>
 ![model](media\ad-fs-risk-assessment-model\risk2.png)

 5. 将引用添加到`Microsoft.IdentityServer.dll`的 AD FS，如下所示

   a.   右键单击**引用**中**解决方案资源管理器**，然后选择**添加引用...**</br> 
   ![模型](media\ad-fs-risk-assessment-model\risk3.png)
   
   b.   上**引用管理器**窗口中选择**浏览**。 在**选择要引用的文件...** 对话框中，选择`Microsoft.IdentityServer.dll`从你的 AD FS 安装文件夹 (在我的示例**C:\Windows\ADFS**)，单击**添加**。
   
   >[!NOTE]
    >在本例中我要构建该插件在 AD FS 服务器本身上。 如果你的开发环境位于不同服务器上，复制`Microsoft.IdentityServer.dll`到开发环境中的 AD FS 服务器上的 AD FS 安装文件夹中。</br> 
   
   ![模型](media\ad-fs-risk-assessment-model\risk4.png)
   
   c.   单击**确定**上**引用管理器**后确保窗口`Microsoft.IdentityServer.dll`选中复选框</br>
   ![model](media\ad-fs-risk-assessment-model\risk5.png)
 
 6. 所有类和引用都现到位，才能执行的生成。   但是，由于此项目的输出是一个 dll，它必须将安装到**全局程序集缓存**，或 GAC 中，AD FS 服务器和 dll 的需要进行第一次签名。 这可以按如下所示：

   a.   **右键单击**ThreatDetectionModule 项目的名称。 从菜单中，单击**属性**。</br>
   ![model](media\ad-fs-risk-assessment-model\risk6.png)
   
   b.   从**属性**页上，单击**签名**，在左侧，然后检查相应的复选框标记**程序集签名**。 从**选择强名称密钥文件**： 展开下拉列表菜单中，选择 **< 新建...>**</br>
   ![model](media\ad-fs-risk-assessment-model\risk7.png)

   c.   在中**创建强名称密钥对话框**中，键入密钥 （可以选择任何名称） 的名称，请取消选中该复选框**保护密钥文件与密码**。 然后，单击**确定**。
   ![model](media\ad-fs-risk-assessment-model\risk8.png)</br>
 
   d.   保存项目，如下所示</br>
   ![model](media\ad-fs-risk-assessment-model\risk9.png)

 7. 通过单击生成项目**构建**，然后**重新生成解决方案**，如下所示</br>
 ![model](media\ad-fs-risk-assessment-model\risk10.png)
 
 检查**输出窗口**，在屏幕上，若要查看是否有任何错误发生的底部</br>
 ![model](media\ad-fs-risk-assessment-model\risk11.png)


插件 (dll) 现已准备好使用和处于 **\bin\Debug**项目文件夹的文件夹 (在本例中，这**C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**)。 

下一步是向 AD FS 中，注册此 dll，使其符合 AD FS 身份验证过程中运行。 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>向 AD FS 注册插件 dll

我们需要使用在 AD FS 中注册该 dll `Register-AdfsThreatDetectionModule` PowerShell 命令在 AD FS 服务器上，但是，我们注册之前，我们需要先获取公钥标记。 我们创建了密钥密钥和签名使用该密钥的 dll 时创建此公钥标记。 若要了解什么公钥标记的 dll 是，可以使用**SN.exe** ，如下所示

 1. 从 dll 文件复制 **\bin\Debug**到另一个位置的文件夹 (在本例中复制到**C:\extensions**)

 2. 启动**开发人员命令提示**for Visual Studio 和转到目录包含**sn.exe** (在本例中的目录是**C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 工具**)![模型](media\ad-fs-risk-assessment-model\risk12.png)

 3. 运行**SN**命令 **-T**参数和文件的位置 (在我的示例`SN -T “C:\extensions\ThreatDetectionModule.dll”`)![模型](media\ad-fs-risk-assessment-model\risk13.png)</br>
 该命令将提供的公钥标记 (对于我来说，**公钥标记是 714697626ef96b35**)

 4. 添加到 dll**全局程序集缓存**我们最佳的做法是 AD FS 服务器为你的项目创建正确的安装程序，并使用安装程序将文件添加到 gac 中。 另一种解决方案是使用**Gacutil.exe** (的详细信息**Gacutil.exe**可用[此处](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)) 在开发计算机上。  由于我在 AD FS 所在的同一服务器上有 visual studio，我将使用**Gacutil.exe** ，如下所示

   a.   在开发人员命令提示符处为 Visual Studio，然后转到目录包含**Gacutil.exe** (在本例中的目录是**C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**)

   b.   运行**Gacutil**命令 (在我的示例`Gacutil /IF C:\extensions\ThreatDetectionModule.dll`)![模型](media\ad-fs-risk-assessment-model\risk14.png)
 
 >[!NOTE]
 >如果有更高版本的 AD FS 场需要在场中每个 AD FS 服务器上执行。 

 5. 打开**Windows PowerShell**并运行以下命令以注册 dll
    ```
    Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>”
    ```
    在本例中，该命令是： 
    ```
    Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv”
    ```
 
    >[!NOTE]
    >需要注册 dll，一次，即使具有 AD FS 场。 

 6. 注册 dll 后重新启动 AD FS 服务

Dll 现在注册以及 AD FS 并使用准备好，就这么简单 ！

 >[!NOTE]
 > 如果对插件进行任何更改并重新生成项目，则需要再次注册已更新的 dll。 在注册之前，你将需要注销当前 dll 使用以下命令：</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> 在本例中，该命令是：
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>测试插件

 1. 打开**authconfig.csv**我们之前创建的文件 (在我的示例位置**C:\extensions**)，并添加**Extranet Ip**你想要阻止。 在单独的行应为每个 IP 和结束时应没有空格</br>
 ![model](media\ad-fs-risk-assessment-model\risk18.png)
 
 2. 保存并关闭文件

 3. 通过运行以下 PowerShell 命令导入 AD FS 中更新的文件 

  ```
  Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
  ```
 
  在本例中，该命令是： 
  ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
 ```
 
 4. 启动身份验证请求中具有相同的 IP 中添加的服务器**authconfig.csv**。

 对于本演示中，我将使用[AD FS 帮助 Claims X-Ray 工具](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)来启动一个请求。 如果你想要使用 x 射线工具，请按照的说明 

 输入联合身份验证服务器实例和命中**测试身份验证**按钮。</br> 
 ![模型](media\ad-fs-risk-assessment-model\risk15.png) 

 5. 身份验证被阻止，如下所示。</br>
 ![model](media\ad-fs-risk-assessment-model\risk16.png)
 
现在，我们知道如何生成和注册插件，让我们演练插件代码，以了解使用新的接口和类的实现中引入了该模型。 

## <a name="plug-in-code-walkthrough"></a>插件代码演练

打开项目`ThreatDetectionModule.sln`使用 Visual Studio，然后打开主文件**UserRiskAnalyzer.cs**从**解决方案资源管理器**在屏幕的右侧</br>
![model](media\ad-fs-risk-assessment-model\risk17.png)
 
该文件包含主类可实现抽象类 UserRiskAnalyzer [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019)和界面[IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019)以从请求中读取 IP上下文中，比较从 AD FS 数据库中加载的 ip 的获得的 IP，并阻止请求，如果 IP 匹配项。 这些类型更详细地介绍一下

### <a name="threatdetectionmodule-abstract-class"></a>ThreatDetectionModule 抽象类

此抽象类到 AD FS 管道，从而可以运行符合 AD FS 进程的插件代码加载插件。 

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

|方法 |在任务栏的搜索框中键入|定义|
|-----|-----|-----| 
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |Void|由 AD FS 中将插件加载到其管道中时调用| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |Void|由 AD FS 时从其管道中卸载该插件调用| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| Void|由 AD FS 配置更新上调用 |
|**属性** |**Type** |**定义**|
|[VendorName](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|字符串 |获取拥有该插件的供应商名称|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|字符串 |获取该插件的标识符|

我们将使用我们的示例插件[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019)并[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)方法来从 AD FS 数据库中读取的预定义的 Ip。 [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019)插件是否已注册时的 AD FS 时，将调用[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) .csv 导入使用时，将调用`Import-AdfsThreatDetectionModuleConfiguration`cmdlet。 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>IRequestReceivedThreatDetectionModule Interface

这[接口](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019)使您能够实施风险评估点处，其中 AD FS 接收身份验证请求，但之前用户输入凭据即在收到请求的身份验证过程的阶段。 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

该接口包含[EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019)要编写风险评估逻辑的 requestContext 输入参数传入方法，这样即可使用身份验证请求的上下文。 RequestContext 参数的类型是[RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)。 

传递的其他输入的参数是该类型的记录器[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)。 该参数可用于写入错误，审核和/或调试到 AD FS 日志消息。 

该方法返回[ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 NotEvaluated，块中，为 1 和 2 可允许向) 到 AD FS，然后可以阻止或允许该请求。

在本示例中插件[EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019)方法的实现分析[clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses)从[requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)参数并将其与所有 Ip从 AD FS 数据库中加载。 如果找到匹配项，方法将返回为 2**块**，否则它将返回 1 表示**允许**。 根据返回的值，AD FS 可以阻止或允许该请求。 

>[!NOTE]
>上述实现唯一 IRequestReceivedThreatDetectionModule 接口讨论示例插件。 但是，风险评估模型提供了两个其他接口 – IPreAuthenticationThreatDetectionModule （若要实现风险评估逻辑 duing 预身份验证阶段） 和 IPostAuthenticationThreatDetectionModule （若要实现风险评估逻辑阶段后身份验证）。 下面提供了两个接口上的详细信息。 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>IPreAuthenticationThreatDetectionModule 接口 

这[接口](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019)，用户可以实现在点的风险评估逻辑，其中用户提供凭据，但 AD FS 它们进行评估即预身份验证阶段之前。 

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
该接口包含[EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019)传入方法，这样即可使用的信息[RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)， [SecurityContext securityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)， [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)，和[IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2)的输入参数编写预身份验证的风险评估逻辑。 

>[!NOTE]
>使用每个上下文类型传递的属性的列表，请访问[RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)， [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)，并[ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)类定义。 

传递的其他输入的参数是该类型的记录器[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)。 该参数可用于写入错误，审核和/或调试到 AD FS 日志消息。

该方法返回[ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 NotEvaluated，块中，为 1 和 2 可允许向) 到 AD FS，然后可以阻止或允许该请求。 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>IPostAuthenticationThreatDetectionModule 接口

这[接口](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019)使您能够实施风险评估逻辑后用户已提供凭据和 AD FS 执行了身份验证即后身份验证阶段。 

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

该接口包含[EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019)传入方法，这样即可使用的信息[RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)， [SecurityContext securityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)， [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)，和[IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2)的输入参数编写后身份验证的风险评估逻辑。 

>[!NOTE]
> 使用每个上下文类型传递的属性的完整列表，请参阅[RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)， [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)，并[ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)类定义。 

传递的其他输入的参数是该类型的记录器[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)。 该参数可用于写入错误，审核和/或调试到 AD FS 日志消息。 

该方法返回[风险评分](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019)这可以使用 AD FS 策略中，声明的规则。 

>[!NOTE]
>为插件起作用，（在本例中 UserRiskAnalyzer） 的主类需要派生[ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019)抽象类，并应实现上面所述的三个接口中至少一个。 Dll 注册后，AD FS 将检查该接口的实现，并在管道中的适当阶段调用它们。

### <a name="faqs"></a>常见问题

**为什么应生成这些插件？**</br>
**答：** 这些插件不只提供其他功能来保护环境免受密码喷射攻击等攻击，但还可以灵活地构建您自己基于自身要求的风险评估逻辑。 

**在其中捕获日志？**</br>
**答：** 可以向使用 WriteAdminLogErrorMessage 方法的"AD FS/Admin"事件日志写入错误日志，审核日志以"AD FS 审核"安全日志使用 WriteAuditMessage 方法和调试日志传输到"AD FS 跟踪"调试日志使用 WriteDebugMessage 方法。 

**可以添加这些插件增加 AD FS 身份验证进程延迟？**</br>
**答：** 延迟的影响将确定执行风险评估逻辑实现所花的时间。 我们建议部署插件在生产环境中之前评估的延迟影响。 

**AD FS 不能为什么推荐用户等的风险 Ip 的列表？**</br>
**答：** 尽管不目前不可用，我们正在努力构建的智能建议风险 Ip，用户可插入的风险评估模型中等。 我们将分享启动推出日期。 
