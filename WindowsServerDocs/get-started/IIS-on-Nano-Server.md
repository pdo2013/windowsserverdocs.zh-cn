---
title: Nano Server 上的 IIS
description: 在 Nano Server 上配置 IIS 的详细信息
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/06/2017
ms.assetid: 16984724-2d77-4d7b-9738-3dff375ed68c
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 54c8d05c028cbca364b6a46052d12cdcb12c01b0
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443613"
---
# <a name="iis-on-nano-server"></a>Nano Server 上的 IIS

>适用于：Windows Server 2016

> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解这意味着什么。 

也可以通过结合使用 -Package 参数和 Microsoft-NanoServer-IIS-Package 在 Nano Server 上安装 Internet Information Services (IIS) 服务器角色。 有关配置 Nano Server 的信息，包括安装程序包，请参阅[安装 Nano Server](Getting-Started-with-Nano-Server.md)。  

在此发行版的 Nano Server 中，以下 IIS 功能可用：  

|功能|默认情况下启用|  
|-----------|----------------------|  
|**常用 HTTP 功能**||  
|默认文档|x|  
|目录浏览|x|  
|HTTP 错误|x|  
|静态内容|x|  
|HTTP 重定向||  
|**运行状况和诊断**||  
|HTTP 日志记录|x|  
|自定义日志记录||  
|请求监视器||  
|跟踪||  
|**性能**||  
|静态内容压缩|x|  
|动态内容压缩||  
|**安全性**||  
|请求筛选|x|  
|基本身份验证||  
|客户端证书映射身份验证||  
|摘要式身份验证||  
|IIS 客户端证书映射身份验证||  
|IP 和域限制||  
|URL 授权||  
|Windows 身份验证||  
|**应用程序开发**||  
|应用程序初始化||  
|CGI||  
|ISAPI 扩展||  
|ISAPI 筛选器||  
|服务器端包含||  
|WebSocket 协议||  
|**管理工具**||  
|Windows PowerShell 的 IISAdministration 模块|x|  

有关 IIS 的其他配置（例如使用 ASP.NET、PHP 和 Java）的一系列文章以及其他相关内容发布在 [http://iis.net/learn](http://iis.net/learn)。  

## <a name="installing-iis-on-nano-server"></a>在 Nano Server 上安装 IIS  
可以脱机（Nano Server 断开时）或联机（Nano Server 运行时）安装此服务器角色；脱机安装是推荐选项。  

对于脱机安装，使用程序包参数 New-NanoServerImage 添加程序包，如本示例中所示：  

`New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1.vhd -ComputerName Nano1 -Package Microsoft-NanoServer-IIS-Package`  

如果有现有的 VHD 文件，则可以通过装载 VHD，然后使用**添加程序包**选项来利用 DISM.exe 脱机安装 IIS。   
以下示例步骤假定正在从 BasePath 指定的目录运行，且该目录是在运行 New-NanoServerImage 之后创建的。  

1.  mkdir mountdir
2.  .\Tools\dism.exe /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir
3.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\Microsoft-NanoServer-IIS-Package.cab /Image:.\mountdir
4.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab /Image:.\mountdir
5.  .\Tools\dism.exe /Unmount-Image /MountDir:.\MountDir /Commit


> [!NOTE]  
> 请注意，步骤 4 添加了语言包，此示例安装的是 EN-US。  

此时可以使用 IIS 启动 Nano Server。  

### <a name="installing-iis-on-nano-server-online"></a>在 Nano Server 上联机安装 IIS  
虽然建议脱机安装服务器角色，但可能需要在容器方案中联机对其进行安装（运行 Nano Server 时）。 要实现这一点，请执行下列操作：  

1.  将程序包文件夹从安装媒体本地复制到运行的 Nano Server（例如：到 C:\packages）。  

2.  在另一台计算机上创建新的 Unattend.xml 文件，然后将其复制到 Nano Server。 可以将此 XML 内容复制并粘贴到创建的 XML 文件中：  



```  

    <unattend xmlns="urn:schemas-microsoft-com:unattend">  
    <servicing>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" />  
            <source location="c:\packages\Microsoft-NanoServer-IIS-Package.cab" />  
        </package>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="en-US" />  
            <source location="c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab" />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source="" xmlns:cpi="urn:schemas-microsoft-com:cpi" />  
</unattend>  
```  




3. 在创建（或复制）的新 XML 文件中，将 C:\packages 编辑为程序包内容复制到的目录。  

4. 使用新创建的 XML 文件切换到目录并运行  

   **dism /online /apply-unattend:.\unattend.xml**  


5. 通过运行以下命令确定 IIS 程序包及其关联的语言包已正确安装：  

   **dism /online /get-packages**  

   应会看到“程序包标识符:Microsoft-NanoServer-IIS-Package~31bf3856ad364e35~amd64~~10.0.14393.1000”列出两次，一次用于”发布类型:语言包”，一次用于“发布类型:功能包”。  

6. 使用 **net start w3svc** 或通过重新启动 Nano Server 启动 W3SVC 服务。  

## <a name="starting-iis"></a>启动 IIS  
安装 IIS 后并且运行时，它已准备好为 Web 请求提供服务。 通过浏览默认的 IIS 网页（网址： http://\<IP address of Nano Server> 验证 IIS 是否在运行。 在物理计算机上，可以通过使用恢复控制台确定 IP 地址。 在虚拟计算机上，通过使用 Windows PowerShell 提示符和运行以下各项可获得 IP 地址：  

`Get-VM -name <VM name> | Select -ExpandProperty networkadapters | select IPAddresses`  

如果无法访问默认的 IIS 网页，通过在 Nano Server 上查找 **c:\inetpub** 目录复核 IIS 安装。  

## <a name="enabling-and-disabling-iis-features"></a>启用和禁用 IIS 功能  
安装 IIS 角色时默认启用大量的 IIS 功能（请参阅本主题的“Nano Server 上的 IIS 概述”部分中的表）。 可以使用 DISM.exe 启用（或禁用）其他功能

IIS 的每个功能作为一组配置元素存在。 例如，Windows 身份验证功能包含下列元素：  

|部分|配置元素|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name="WindowsAuthenticationModule" image="%windir%\System32\inetsrv\authsspi.dll`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" \/>`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled="false" authPersistNonNTLM\="true"><providers><add value="Negotiate" /><add value="NTLM" /><br /></providers><br /></windowsAuthentication>`|  

本主题的附录 1 中包含完整的 IIS 子功能集，其相应的配置元素包含在本主题的附录 2 中。  


### <a name="example-installing-windows-authentication"></a>示例：安装 Windows 身份验证  

1.  在 Nano Server上打开 Windows PowerShell 远程会话控制台。  

2.  使用 `DISM.exe` 安装 Windows 身份验证模块：

    ```
    dism /Enable-Feature /online /featurename:IIS-WindowsAuthentication /all
    ```

    `/all` 交换机将安装选定的功能所依赖的任何功能。

### <a name="example-uninstalling-windows-authentication"></a>示例：卸载 Windows 身份验证  

1.  在 Nano Server上打开 Windows PowerShell 远程会话控制台。  

2.  使用 `DISM.exe` 卸载 Windows 身份验证模块：

    ```
    dism /Disable-Feature /online /featurename:IIS-WindowsAuthentication
    ```

## <a name="other-common-iis-configuration-tasks"></a>其他常见的 IIS 配置任务  
**创建网站**  

使用此 cmdlet：  

`PS D:\> New-IISSite -Name TestSite -BindingInformation "*:80:TestSite" -PhysicalPath c:\test`  

然后可以运行 `Get-IISSite` 以验证该站点的状态（返回网站名称、ID、状态、物理路径和绑定）。  

**删除网站**  

运行 `Remove-IISSite -Name TestSite -Confirm:$false`。  

**创建虚拟目录**  

可以通过使用 Get-IISServerManager 返回的 IISServerManager 对象来创建虚拟目录，该对象公开了 NET Microsoft.Web.Administration.ServerManager API。 在此示例中，这些命令访问站点集合的“Default Web Site”元素和应用程序部分的根应用程序元素 ("/")。 然后这些命令为该应用程序元素调用 VirtualDirectories 集合的 Add() 方法，以便创建新目录：  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.Sites["Default Web Site"].Applications["/"].VirtualDirectories.Add("/DemoVirtualDir1", "c:\test\virtualDirectory1")  
PS C:\> $sm.Sites["Default Web Site"].Applications["/"].VirtualDirectories.Add("/DemoVirtualDir2", "c:\test\virtualDirectory2")  
PS C:\> $sm.CommitChanges()  
```  

**创建应用程序池**  

同样可以使用 Get-IISServerManager 创建应用程序池：  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.ApplicationPools.Add("DemoAppPool")  
```  

**配置 HTTPS 和证书**  

使用 Certoc.exe 实用程序导入证书，如本示例中所示，其中显示了在 Nano Server 上为网站配置 HTTPS：  

1.  在另一台没有运行 Nano Server 的计算机上，创建证书（使用自己的证书名称和密码），然后再将其导出到 c:\temp\test.pfx。  

    `$newCert = New-SelfSignedCertificate -DnsName "www.foo.bar.com" -CertStoreLocation cert:\LocalMachine\my`  

    `$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText`  

    `Export-PfxCertificate -FilePath c:\temp\test.pfx -Cert $newCert -Password $mypwd`  

2.  将 test.pfx 文件复制到 Nano Server 计算机上。  

3.  在 Nano Server上，使用以下命令将证书导入到“My”存储区：  

    **certoc.exe -ImportPFX -p YOUR_PFX_PASSWD My c:\temp\test.pfx**  

4.  使用 `Get-ChildItem Cert:\LocalMachine\my` 检索此新证书（在本示例中为 61E71251294B2A7BB8259C2AC5CF7BA622777E73）的指纹。  

5.  通过使用以下 Windows PowerShell 命令将 HTTPS 绑定添加到默认网站（或想要添加绑定的任何网站）：  

    ```  
    $certificate = get-item Cert:\LocalMachine\my\61E71251294B2A7BB8259C2AC5CF7BA622777E73  
    # Use your actual thumbprint instead of this example  
    $hash = $certificate.GetCertHash()  

    Import-Module IISAdministration  
    $sm = Get-IISServerManager  
    $sm.Sites["Default Web Site"].Bindings.Add("*:443:", $hash, "My", "0")    # My is the certificate store name  
    $sm.CommitChanges()  
    ```  

    还可以使用此语法将服务器名称指示 (SNI) 和特定主机名配合使用：`$sm.Sites["Default Web Site"].Bindings.Add("*:443:www.foo.bar.com", $hash, "My", "Sni".`  

## <a name="appendix-1-list-of-iis-sub-features"></a>附录 1：IIS 子功能列表

- IIS-WebServer
- IIS-CommonHttpFeatures
- IIS-StaticContent
- IIS-DefaultDocument
- IIS-DirectoryBrowsing
- IIS-HttpErrors
- IIS-HttpRedirect
- IIS-ApplicationDevelopment
- IIS-CGI
- IIS-ISAPIExtensions
- IIS-ISAPIFilter
- IIS-ServerSideIncludes
- IIS-WebSockets
- IIS-ApplicationInit
- IIS-Security
- IIS-BasicAuthentication
- IIS-WindowsAuthentication
- IIS-DigestAuthentication
- IIS-ClientCertificateMappingAuthentication
- IIS-IISCertificateMappingAuthentication
- IIS-URLAuthorization
- IIS-RequestFiltering
- IIS-IPSecurity
- IIS-CertProvider
- IIS-Performance
- IIS-HttpCompressionStatic
- IIS-HttpCompressionDynamic
- IIS-HealthAndDiagnostics
- IIS-HttpLogging
- IIS-LoggingLibraries
- IIS-RequestMonitor
- IIS-HttpTracing
- IIS-CustomLogging

## <a name="appendix-2-elements-of-http-features"></a>附录 2：HTTP 功能的元素  
IIS 的每个功能作为一组配置元素存在。 本附录列出了此版本 Nano Server 中的所有功能的配置元素  

### <a name="common-http-features"></a>常用 HTTP 功能  
**默认文档**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="DefaultDocumentModule" image="%windir%\System32\inetsrv\defdoc.dll" />`|  
|`<modules>`|`<add name="DefaultDocumentModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="DefaultDocumentModule" resourceType="EiSecther" requireAccess="Read" />`|  
|`<defaultDocument>`|`<defaultDocument enabled="true"><br /><files><br /> <add value="Default.htm" /><br />        <add value="Default.asp" /><br />        <add value="index.htm" /><br />        <add value="index.html" /><br />        <add value="iisstart.htm" /><br />    </files><br /></defaultDocument>`|  

`StaticFile <handlers>` 条目可能已经存在；如果是这样，只需将“DefaultDocumentModule”添加到 \<modules> 属性中，用逗号隔开。  

**目录浏览**  

|部分|配置元素|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="DirectoryListingModule" image="%windir%\System32\inetsrv\dirlist.dll" />`|  
|`<modules>`|`<add name="DirectoryListingModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="DirectoryListingModule" resourceType="Either" requireAccess="Read" />`|  

`StaticFile <handlers>` 条目可能已经存在；如果是这样，只需将“DirectoryListingModule”添加到 \<modules> 属性中，用逗号隔开。  

**HTTP 错误**  

|部分|配置元素|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="CustomErrorModule" image="%windir%\System32\inetsrv\custerr.dll" />`|  
|`<modules>`|`<add name="CustomErrorModule" lockItem="true" />`|  
|`<httpErrors>`|`<httpErrors lockAttributes="allowAbsolutePathsWhenDelegated,defaultPath"><br />    <error statusCode="401"    prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="401.htm" ><br />    <error statusCode="403" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="403.htm" /><br />    <error statusCode="404" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="404.htm" /><br />    <error statusCode="405" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="405.htm" /><br />    <error statusCode="406" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="406.htm" /><br />    <error statusCode="412" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="412.htm" /><br />    <error statusCode="500" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="500.htm" /><br />    <error statusCode="501" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="501.htm" /><br />    <error statusCode="502" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="502.htm" /><br /></httpErrors>`|  

**静态内容**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="StaticFileModule" image="%windir%\System32\inetsrv\static.dll" />`|  
|`<modules>`|`<add name="StaticFileModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="StaticFileModule" resourceType="Either" requireAccess="Read" />`|  

`StaticFile \<handlers>` 条目可能已经存在；如果是这样，只需将“StaticFileModule”添加到 \<modules> 属性中，用逗号隔开。  

**HTTP 重定向**  

|部分|配置元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="HttpRedirectionModule" image="%windir%\System32\inetsrv\redirect.dll" />`|  
|`<modules>`|`<add name="HttpRedirectionModule" lockItem="true" />`|  
|`<httpRedirect>`|`<httpRedirect enabled="false" />`|  

### <a name="health-and-diagnostics"></a>运行状况和诊断  
**HTTP 日志记录**  

|部分|配置元素|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="HttpLoggingModule" image="%windir%\System32\inetsrv\loghttp.dll" />`|  
|`<modules>`|`<add name="HttpLoggingModule" lockItem="true" />`|  
|`<httpLogging>`|`<httpLogging dontLog="false" />`|  

**自定义日志记录**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CustomLoggingModule" image="%windir%\System32\inetsrv\logcust.dll" />`|  
|`<modules>`|`<add name="CustomLoggingModule" lockItem="true" />`|  

**请求监视器**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="RequestMonitorModule" image="%windir%\System32\inetsrv\iisreqs.dll" />`|  

**跟踪**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="TracingModule" image="%windir%\System32\inetsrv\iisetw.dll" \/><br /><add name="FailedRequestsTracingModule" image="%windir%\System32\inetsrv\iisfreb.dll" />`|  
|`<modules>`|`<add name="FailedRequestsTracingModule" lockItem="true" />`|  
|`<traceProviderDefinitions>`|`<traceProviderDefinitions><br />    <add name="WWW Server" guid\="{3a2a4e84-4c21-4981-ae10-3fda0d9b0f83}"><br />        <areas><br />            <clear /><br />            <add name="Authentication" value="2" /><br />            <add name="Security" value="4" /><br />            <add name="Filter" value="8" /><br />            <add name="StaticFile" value="16" /><br />            <add name="CGI" value="32" /><br />            <add name="Compression" value="64" /><br />            <add name="Cache" value="128" /><br />            <add name="RequestNotifications" value="256" /><br />            <add name="Module" value="512" /><br />            <add name="FastCGI" value="4096" /><br />            <add name="WebSocket" value="16384" /><br />        </areas><br />    </add><br />    <add name="ISAPI Extension" guid="{a1c2040e-8840-4c31-ba11-9871031a19ea}"><br />        <areas><br />            <clear /><br />        </areas><br />    </add><br /></traceProviderDefinitions>`|  

### <a name="performance"></a>性能  
**静态内容压缩**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="StaticCompressionModule" image="%windir%\System32\inetsrv\compstat.dll" />`|  
|`<modules>`|`<add name="StaticCompressionModule" lockItem="true" />`|  
|`<httpCompression>`|`<httpCompression directory="%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files"><br />    <scheme name="gzip" dll="%Windir%\system32\inetsrv\gzip.dll" /><br />   <staticTypes><br />        <add mimeType="text/*" enabled="true" /><br />        <add mimeType="message/*" enabled="true" /><br />        <add mimeType="application/javascript" enabled="true" \/><br />        <add mimeType="application/atom+xml" enabled="true" /><br />        <add mimeType="application/xaml+xml" enabled="true" /><br />        <add mimeType="\*\*" enabled="false" /><br />    </staticTypes><br /></httpCompression>`|  

**动态内容压缩**  

|部分|配置元素|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name="DynamicCompressionModule" image="%windir%\System32\inetsrv\compdyn.dll" />`|  
|`<modules>`|`<add name="DynamicCompressionModule" lockItem="true" />`|  
|`<httpCompression>`|`<httpCompression directory\="%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files"><br />    <scheme name="gzip" dll="%Windir%\system32\inetsrv\gzip.dll" \/><br />    \<dynamicTypes><br />        <add mimeType="text/*" enabled="true" \/><br />        <add mimeType="message/*" enabled="true" /><br />        <add mimeType="application/x-javascript" enabled="true" /><br />        <add mimeType="application/javascript" enabled="true" /><br />        <add mimeType="*/*" enabled="false" /><br />    <\/dynamicTypes><br /></httpCompression>`|  

### <a name="security"></a>安全性  
**请求筛选**  


|       部分        |                                                                                                                                        配置元素                                                                                                                                        |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  `<globalModules>`   |                                                                                                        `<add name="RequestFilteringModule" image="%windir%\System32\inetsrv\modrqflt.dll" />`                                                                                                        |
|     `<modules>`      |                                                                                                                       `<add name="RequestFilteringModule" lockItem="true" />`                                                                                                                        |
| \`<requestFiltering> | `<requestFiltering><br />    <fileExtensions allowUnlisted="true" applyToWebDAV="true" /><br />    <verbs allowUnlisted="true" applyToWebDAV="true" /><br />    <hiddenSegments applyToWebDAV="true"><br />        <add segment="web.config" /><br />    </hiddenSegments><br /></requestFiltering>` |

**基本身份验证**  

|部分|配置元素|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="BasicAuthenticationModule" image="%windir%\System32\inetsrv\authbas.dll" />`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" />`|  
|`<basicAuthentication>`|`<basicAuthentication enabled="false" />`|  

**客户端证书映射身份验证**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CertificateMappingAuthentication" image="%windir%\System32\inetsrv\authcert.dll" />`|  
|`<modules>`|`<add name="CertificateMappingAuthenticationModule" lockItem="true" />`|  
|`<clientCertificateMappingAuthentication>`|`<clientCertificateMappingAuthentication enabled="false" />`|  

**摘要式身份验证**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="DigestAuthenticationModule" image="%windir%\System32\inetsrv\authmd5.dll" />`|  
|`<modules>`|`<add name="DigestAuthenticationModule" lockItem="true" />`|  
|`<other>`|`<digestAuthentication enabled="false" />`|  

**IIS 客户端证书映射身份验证**  


|                  部分                   |                                         配置元素                                         |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------|
|             `<globalModules>`              | `<add name="CertificateMappingAuthenticationModule" image="%windir%\System32\inetsrv\authcert.dll" />` |
|                `<modules>`                 |               `<add name="CertificateMappingAuthenticationModule" lockItem="true" `/>\`                |
| `<clientCertificateMappingAuthentication>` |                      `<clientCertificateMappingAuthentication enabled="false" />`                      |

**IP 和域限制**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|```<add name="IpRestrictionModule" image="%windir%\System32\inetsrv\iprestr.dll" /><br /><add name="DynamicIpRestrictionModule" image="%windir%\System32\inetsrv\diprestr.dll" />```|  
|`<modules>`|`<add name="IpRestrictionModule" lockItem="true" \/><br /><add name="DynamicIpRestrictionModule" lockItem="true" \/>`|  
|`<ipSecurity>`|`<ipSecurity allowUnlisted="true" />`|  

**URL 授权**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="UrlAuthorizationModule" image="%windir%\System32\inetsrv\urlauthz.dll" />`|  
|`<modules>`|`<add name="UrlAuthorizationModule" lockItem="true" />`|  
|`<authorization>`|`<authorization><br />    <add accessType="Allow" users="*" /><br /></authorization>`|  

**Windows 身份验证**  

|部分|配置元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="WindowsAuthenticationModule" image="%windir%\System32\inetsrv\authsspi.dll" />`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" />`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled="false" authPersistNonNTLM\="true"><br />    <providers><br />        <add value="Negotiate" /><br />        <add value="NTLM" /><br />    <\providers><br /><\windowsAuthentication><windowsAuthentication enabled="false" authPersistNonNTLM\="true"><br />    <providers><br />        <add value="Negotiate" /><br />        <add value="NTLM" /><br />    <\/providers><br /><\/windowsAuthentication>`|  

### <a name="application-development"></a>应用程序开发  
**应用程序初始化**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="ApplicationInitializationModule" image="%windir%\System32\inetsrv\warmup.dll" />`|  
|`<modules>`|`<add name="ApplicationInitializationModule" lockItem="true" />`|  

**CGI**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CgiModule" image="%windir%\System32\inetsrv\cgi.dll" /><br /><add name="FastCgiModule" image="%windir%\System32\inetsrv\iisfcgi.dll" />`|  
|`<modules>`|`<add name="CgiModule" lockItem="true" /><br /><add name="FastCgiModule" lockItem="true" />`|  
|`<handlers>`|`<add name="CGI-exe" path="*.exe" verb="\*" modules="CgiModule" resourceType="File" requireAccess="Execute" allowPathInfo="true" />`|  

**ISAPI 扩展**  

|部分|配置元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="IsapiModule" image="%windir%\System32\inetsrv\isapi.dll" />`|  
|`<modules>`|`<add name="IsapiModule" lockItem="true" />`|  
|`<handlers>`|`<add name="ISAPI-dll" path="*.dll" verb="*" modules="IsapiModule" resourceType="File" requireAccess="Execute" allowPathInfo="true" />`|  

**ISAPI 筛选器**  

|部分|配置元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="IsapiFilterModule" image="%windir%\System32\inetsrv\filter.dll" />`|  
|`<modules>`|`<add name="IsapiFilterModule" lockItem="true" />`|  

**服务器端包含**  

|部分|配置元素|  
|----------------|--------------------------|  
|`<globalModules>`|<`add name="ServerSideIncludeModule" image="%windir%\System32\inetsrv\iis_ssi.dll" />`|  
|`<modules>`|`<add name="ServerSideIncludeModule" lockItem="true" />`|  
|`<handlers>`|`<add name="SSINC-stm" path="*.stm" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" \/><br /><add name="SSINC-shtm" path="*.shtm" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" /><br /><add name="SSINC-shtml" path="*.shtml" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" />`|  
|`<serverSideInclude>`|`<serverSideInclude ssiExecDisable="false" />`|  

**WebSocket 协议**  

|部分|配置元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="WebSocketModule" image="%windir%\System32\inetsrv\iiswsock.dll" />`|  
|`<modules>`|`<add name="WebSocketModule" lockItem="true" />`|  