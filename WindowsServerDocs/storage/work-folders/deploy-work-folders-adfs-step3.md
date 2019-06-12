---
title: 使用 AD FS 和 Web 应用程序代理部署工作文件夹 - 步骤 3，设置工作文件夹
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 5a43b104-4d02-4d73-a385-da1cfb67e341
ms.openlocfilehash: d6b21579fb1dedc777733317e7222debd8d944a1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812664"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-3-set-up-work-folders"></a>部署工作文件夹使用 AD FS 和 Web 应用程序代理：第 3 步： 设置工作文件夹

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍使用 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理部署工作文件夹的第三个步骤。 你可以在这些主题中查找这一过程的其他步骤：  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：概述](deploy-work-folders-adfs-overview.md)  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：第 1 步： 设置 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 2 中，AD FS 配置后工作](deploy-work-folders-adfs-step2.md)  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 4 中，设置 Web 应用程序代理](deploy-work-folders-adfs-step4.md)  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 5 中，设置客户端](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   在本部分中介绍的说明适用于 Windows Server 2019 或 Windows Server 2016 环境。 如果你使用的是 Windows Server 2012 R2，请遵循 [Windows Server 2012 R2 说明](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。

若要设置工作文件夹，请使用以下步骤。  
  
## <a name="pre-installment-work"></a>预安装工作  
为了安装工作文件夹，必须拥有一个已加入域并运行 Windows Server 2016 的服务器。 服务器必须具有有效的网络配置。  
  
对于测试示例，将运行工作文件夹的计算机加入到 Contoso 域，并按以下各节的描述设置网络接口。 

### <a name="set-the-server-ip-address"></a>设置服务器的 IP 地址  
将服务器的 IP 地址更改为静态 IP 地址。 测试示例中，使用 IP 类，该类是 192.168.0.170 / 子网掩码：255.255.0.0/默认网关：192.168.0.1/首选 DNS:192.168.0.150 （你的域控制器的 IP 地址）。 
  
### <a name="create-the-cname-record-for-work-folders"></a>为工作文件夹创建 CNAME 记录  
要为工作文件夹创建 CNAME 记录，请遵循下列步骤：  
  
1.  在域控制器上，打开 **DNS 管理器**。  
  
2.  展开“正向查找区域”文件夹，右键单击你的域，并单击**新别名 (CNAME)** 。  
  
3.  在**新资源记录**窗口的**别名**字段中，输入工作文件夹的别名。 在测试示例中，别名为 **workfolders**。  
  
4.  在**完全限定的域名**字段中，该值应为 **workfolders.contoso.com**。  
  
5.  在**目标主机的完全限定的域名**字段中，输入工作文件夹服务器的 FQDN。 在测试示例中，FQDN 为 **2016-WF.contoso.com**。  
  
6.  单击 **“确定”** 。  
  
要通过 Windows PowerShell 完成相同的步骤，请使用以下命令。 该命令必须在域控制器上执行。  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name workfolders -CName  -HostNameAlias 2016-wf.contoso.com   
```  
  
### <a name="install-the-ad-fs-certificate"></a>安装 AD FS 证书  
使用以下步骤将 AD FS 设置期间创建的 AD FS 证书安装到本地计算机证书存储中：  
  
1.  单击 **“开始”** ，然后单击 **“运行”** 。  
  
2.  键入 **MMC**。  
  
3.  在“文件”  菜单上，单击“添加/删除管理单元”  。  
  
4.  在**可用的管理单元**列表中，单击**证书**，然后单击**添加**。 证书管理单元向导启动。  
  
5.  选择“计算机帐户”  ，然后单击“下一步”  。  
  
6.  选择**本地计算机：（运行此控制台的计算机）** ，然后单击**完成**。  
  
7.  单击 **“确定”** 。  
  
8.  展开文件夹**控制台根节点\证书\(本地计算机)\个人\证书**。  
  
9. 右键单击**证书**，单击**所有任务**，然后单击**导入**。  
  
10. 浏览到包含 AD FS 证书的文件夹，然后遵循向导中的说明导入文件，并将其放置在证书存储中。

11. 展开文件夹**控制台根节点\证书\(本地计算机)\受信任的根证书颁发机构\证书**。  
  
12. 右键单击**证书**，单击**所有任务**，然后单击**导入**。  
  
13. 浏览到包含 AD FS 证书的文件夹，然后遵循向导中的说明导入文件，并将其放置在受信任的根证书颁发机构存储中。  
  
### <a name="create-the-work-folders-self-signed-certificate"></a>创建工作文件夹自签名证书  
要创建工作文件夹自签名证书，请遵循下列步骤：  
  
1.  下载[使用 AD FS 和 Web 应用程序代理部署工作文件夹](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap)博客文章中提供的脚本，然后将文件 makecert.ps1 复制到工作文件夹计算机。  
  
2.  使用管理员权限打开 Windows PowerShell 窗口。  
  
3.  将执行策略设置为无限制：  
  
    ```powershell  
    PS C:\temp\scripts> Set-ExecutionPolicy -ExecutionPolicy Unrestricted   
    ```  
  
4.  更改为从中复制脚本的目录。  
  
5.  执行 makeCert 脚本：  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1  
    ```  
  
6.  当提示更改主题证书时，请为主题输入新值。 在此示例中，该值为 **workfolders.contoso.com**。  
  
7.  当系统提示你输入使用者可选名称 (SAN) 名称时，按 Y ，然后一次输入一个 SAN 名称。  
  
    对于此示例，键入 **workfolders.contoso.com**，然后按 Enter 键。 键入 **2016-WF.contoso.com**，然后按 Enter 键。  
  
    当所有 SAN 名称输入完毕后，请在空行上按 Enter。  
  
8.  当提示将证书安装至受信任的根证书机构存储时，按 Y。  
  
工作文件夹证书必须是具有以下值的 SAN 证书：  
  
-   **workfolders**.**domain**  
  
-   **machine name**.**domain**  
  
在测试示例中，这些值为：  
  
-   **workfolders.contoso.com**  
  
-   **2016-WF.contoso.com**  
  
## <a name="install-work-folders"></a>安装工作文件夹  
要安装工作文件夹角色，请遵循下列步骤：  
  
1.  打开**服务器管理器**，单击**添加角色和功能**，然后单击**下一步**。  
  
2.  在**安装类型**页上，选择**基于角色或基于功能的安装**，然后单击**下一步**。  
  
3.  在**服务器选择**页上，选择当前服务器，然后单击**下一步**。  
  
4.  在**服务器角色**页上，依次展开**文件和存储服务**及**文件和 iSCSI 服务**，然后选择**工作文件夹**。  
  
5.  在**添加角色和功能向导**页上，单击**添加功能**，然后单击**下一步**。  
  
6.  在**功能**页上，单击**下一步**。  
  
7.  在“确认”  页上，单击“安装”  。  
  
## <a name="configure-work-folders"></a>配置工作文件夹  
要配置工作文件夹，请遵循下列步骤：  
  
1.  打开**服务器管理器**。  
  
2.  选择**文件和存储服务**，然后选择**工作文件夹**。  
  
3.  在**工作文件夹**页上，启动**新同步共享向导**，然后单击**下一步**。  
  
4.  在**服务器和路径**页上，选择要创建同步共享的服务器，输入存储工作文件夹数据的本地路径，然后单击**下一步**。  
  
    如果路径不存在，系统将提示你创建该路径。 单击 **“确定”** 。  
  
5.  在**用户文件夹结构**页上，选择**用户别名**，然后单击**下一步**。  
  
6.  在**同步共享名称**页中，输入同步共享的名称。 对于测试示例，名称为 **WorkFolders**。 单击“下一步”  。  
  
7.  在**同步访问权限**页上，添加可以访问新同步共享的用户或组。 对于测试示例，授予对所有域用户的访问权限。 单击“下一步”  。  
  
8.  在**电脑的安全策略**页上，选择**加密工作文件夹**和**自动锁定页面并需要密码**。 单击“下一步”  。  
  
9. 在**确认**页上，单击**创建**以完成配置过程。  
  
## <a name="work-folders-post-configuration-work"></a>工作文件夹配置后工作  
要完成设置工作文件夹，请完成以下其他步骤：  
  
-   将工作文件夹证书绑定到 SSL 端口  
  
-   配置工作文件夹以使用 AD FS 身份验证  
  
-   导出工作文件夹证书（如果你使用的是自签名证书）  
  
### <a name="bind-the-certificate"></a>绑定证书  
工作文件夹仅通过 SSL 进行通信，并且必须将先前创建的自签名证书（或你的证书颁发机构颁发的证书）绑定到端口。  
  
有两种方法可用于将证书绑定到通过 Windows PowerShell 的端口：IIS cmdlet 和 netsh。  
  
#### <a name="bind-the-certificate-by-using-netsh"></a>使用 netsh 绑定证书  
要在 Windows PowerShell 中使用 netsh 命令行脚本实用工具，必须通过管道将此命令传递给 netsh。 以下示例脚本查找具有 **workfolders.contoso.com** 主题的证书，并使用 netsh 将其绑定到端口 443：  
  
```powershell  
$subject = "workfolders.contoso.com"   
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
$Command = "http add sslcert ipport=0.0.0.0:443 certhash=$thumbprint appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY"  
$Command | netsh  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
#### <a name="bind-the-certificate-by-using-iis-cmdlets"></a>使用 IIS cmdlet 绑定证书  
你还可以使用 IIS 管理 cmdlet 将证书绑定到端口，如果安装了 IIS 管理工具和脚本，则可以使用该 cmdlet。  
  
> [!NOTE]  
> 安装 IIS 管理工具不能在工作文件夹计算机上启用完整版本的 Internet 信息服务 (IIS)；它只能启用管理 cmdlet。 这个设置有一些可能的好处。 例如，如果你正在寻找 cmdlet 来提供从 netsh 获得的功能。 当证书通过 New-WebBinding cmdlet 绑定到端口时，绑定不以任何方式依赖于 IIS。 完成绑定后，你甚至可以删除 Web-Mgmt-Console 功能，证书仍将绑定到该端口。 可以通过键入 **netsh http show sslcert** 验证通过 netsh 进行的绑定。  
  
以下示例使用 New-WebBinding cmdlet 查找具有 **workfolders.contoso.com** 主题的证书，并将其绑定到端口 443：  
  
```powershell  
$subject = "workfolders.contoso.com"  
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert =Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject } | sort $_.NotAfter -Descending | select -first 1   
$thumbprint = $cert.Thumbprint  
New-WebBinding -Name "Default Web Site" -IP * -Port 443 -Protocol https  
#The default IIS website name must be used for the binding. Because Work Folders uses Hostable Web Core and its own configuration file, its website name, 'ECSsite', will not work with the cmdlet. The workaround is to use the default IIS website name, even though IIS is not enabled, because the NewWebBinding cmdlet looks for a site in the default IIS configuration file.   
Push-Location IIS:\SslBindings  
Get-Item cert:\LocalMachine\MY\$thumbprint | new-item *!443  
Pop-Location  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
### <a name="set-up-ad-fs-authentication"></a>设置 AD FS 身份验证  
要配置工作文件夹以使用 AD FS 进行身份验证，请遵循下列步骤：  
  
1.  打开**服务器管理器**。  
  
2.  单击**服务器**，然后在列表中选择你的工作文件夹服务器。  
  
3.  右键单击服务器名称，然后单击**工作文件夹设置**。  
  
4.  在**工作文件夹设置**窗口中，选择**Active Directory 联合身份验证服务**，然后键入联合身份验证服务 URL。 单击 **“应用”** 。  
  
    在测试的示例中，URL 是 **https://blueadfs.contoso.com** 。  
  
通过 Windows PowerShell 完成相同任务的 cmdlet 是：  
  
```powershell  
Set-SyncServerSetting -ADFSUrl "https://blueadfs.contoso.com"   
```  
  
如果你使用自签名证书设置 AD FS，则可能会收到一条错误消息，指出联合身份验证服务 URL 不正确、无法访问、或尚未设置信赖方信任。  
  
如果工作文件夹服务器上未安装 AD FS 证书或 AD FS 的 CNAME 未正确设置，也可能会发生此错误。 你必须纠正这些问题才能继续。  
  
### <a name="export-the-work-folders-certificate"></a>导出工作文件夹证书  
必须导出自签名的工作文件夹证书，以便稍后将其安装在测试环境中的以下计算机上：  
  
-   用于 Web 应用程序代理的服务器  
  
-   加入域的 Windows 客户端  
  
-   未加入域的 Windows 客户端  
  
若要导出的证书，请按照用于导出 AD FS 证书更早版本，如中所述的相同步骤[使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 2 中，AD FS 配置后工作](deploy-work-folders-adfs-step2.md)，导出 AD FS 证书。  
  
下一步：[部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 4 中，设置 Web 应用程序代理](deploy-work-folders-adfs-step4.md)  
  
## <a name="see-also"></a>请参阅  
[工作文件夹概述](Work-Folders-Overview.md)  
  

