---
title: "在 Windows Server Essentials 配置 DirectAccess"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cc336dcd2a5418aa79254108c941a02147112e8f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>在 Windows Server Essentials 配置 DirectAccess

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本主题提供的分步说明配置 Windows Server Essentials 中的 DirectAccess 以启用你的移动员工无缝地从连接到你的组织的 s 网络任何 Internet 配备远程位置而无需建立虚拟专用网络 (VPN) 连接。 DirectAccess 可以提供外出人员相同的连接体验 office 内外从他们的 Windows 8.1、 Windows 8 和 Windows 7 的计算机。  
  
 Windows Server Essentials，如果域包含多个 Windows Server Essentials 服务器，DirectAccess 必须配置该域控制器上。  
  
> [!NOTE]
>  本主题提供用于你的 Windows Server Essentials 服务器时域控制器配置 DirectAccess 的说明进行操作。 如果 Windows Server Essentials 服务器域成员，请按照说明进行操作，配置在域中的某个成员 DirectAccess[添加到现有远程访问 (VPN) 部署的 DirectAccess](https://technet.microsoft.com/library/jj574220.aspx)改为。  
  
## <a name="process-overview"></a>进程概述  
 若要在 Windows Server Essentials 配置 DirectAccess，完成以下步骤。  
  
> [!IMPORTANT]
>  你使用本指南中过程配置 Windows Server Essentials 中的 DirectAccess 之前，你必须启用 VPN 服务器上。 有关说明，请参阅[管理 VPN](Manage-VPN-in-Windows-Server-Essentials.md)。  
  
-   [第 1 步： 添加到服务器远程访问管理工具](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [第 2 步： 为静态 IP 地址更改服务器的网络适配器地址](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [第 3 步： 准备证书和 DNS 记录网络位置服务器](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [单步执行 3a： 的 Web 服务器的证书模板授予身份验证用户完整的权限](#BKMK_GrantFullPermissions)  
  
    -   [单步执行 3b： 注册证书网络位置服务器不可从外部网络解析常见名称](#BKMK_EnrollaCertificate)  
  
    -   [步骤 3 c： 添加新的主 DNS 服务器上，并将其映射到 Windows Server Essentials 服务器的地址](#BKMK_MapNewHosttoServerAddress)  
  
-   [第 4 步： 为创建一个安全组 DirectAccess 客户端计算机](#BKMK_AddSecurityGroup)  
  
-   [第 5 步： 启用并配置 DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [第 5a 步： 使用远程访问管理控制台启用 DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [步骤 5b： 删除 RRAS GPO (仅 Windows Server Essentials) 中的无效 IPv6Prefix](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [步骤 5 c： 启用运行 Windows 7 企业版，若要使用 DirectAccess 客户端计算机](#BKMK_Step4cWindows7Setup)  
  
    -   [步骤 5 d： 配置网络位置服务器](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [步骤 5e： 添加绕过 CA 认证建立 IPsec 信道时的注册表项](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [第 6 步： 配置名称分辨率策略表 DirectAccess 服务器设置](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [第 7 步： 配置 DirectAccess 服务器 Gpo TCP 和 UDP 防火墙规则](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [第 8 步： 更改 DNS64 配置若要收听 IP HTTPS 界面](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [第 9 步： 保留 WinNat 送修端口](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [第 10 步： 重启 WinNat 服务](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [附录： 通过使用 Windows PowerShell 设置 DirectAccess](#BKMK_AppendixBPowerShellScript)提供了一个可用于执行 DirectAccess 安装的 Windows PowerShell 脚本。  
  
##  <a name="BKMK_AddRAM"></a>第 1 步： 添加到服务器远程访问管理工具  
  
#### <a name="to-add-remote-access-management-tools"></a>若要添加远程访问管理工具  
  
1.  在服务器上，在左下角的开始页面上，单击**服务器管理器**图标。  
  
     在 Windows Server Essentials，你将需要搜索为服务器管理器打开它。 在开始页面中，键入**服务器管理器**，然后单击**服务器管理器**搜索结果中。 若要将服务器管理器固定到开始页，请服务器管理器中搜索结果中，右键单击，然后单击**固定到 ' 开始**。  
  
2.  如果**用户帐户控制**显示条警告消息，请单击**是**。  
  
3.  在服务器管理器仪表板中，单击**管理**，然后单击**添加角色和功能**。  
  
4.  在添加角色和功能向导中，执行以下操作：  
  
    1.  在**安装类型**页上，单击**角色基于或功能的安装**。  
  
    2.  在**选择服务器页面**(或**选择目标服务器**Windows Server Essentials 中的页面)，单击**从服务器池选择服务器**。  
  
    3.  在**功能**页面上，展开**远程服务器管理工具 （已安装）**，展开**远程访问管理工具 （已安装）**，展开**角色管理工具 （已安装）**，展开**远程访问管理工具**，然后选择**远程访问 GUI 和命令行工具**。  
  
    4.  按照说明来完成向导。  
  
##  <a name="BKMK_AddStaticIP"></a>第 2 步： 为静态 IP 地址更改服务器的网络适配器地址  
 DirectAccess 要求适配器的静态 IP 地址。 你需要更改本地网络适配器服务器上的 IP 地址。  
  
#### <a name="to-add-a-static-ip-address"></a>若要添加的静态 IP 地址  
  
1.  在开始页面上，打开**控制面板**。  
  
2.  单击**网络和 Internet**，然后单击**查看网络状态和任务**。  
  
3.  在任务窗格中**网络和共享中心**，单击**更改适配器设置**。  
  
4.  本地网络适配器，右键单击，然后单击**属性**。  
  
5.  在**网络**选项卡上，单击**Internet 协议版本 4 (TCP/IPv4)**，然后单击**属性**。  
  
6.  在**常规**选项卡上，单击**使用以下 IP 地址**，然后键入你想要使用的 IP 地址。  
  
     默认值为子网掩码显示将显示在自动**子网掩码**框。 接受默认值，或键入你想要使用的子网掩码值。  
  
7.  在**默认网关**框中，键入你的默认网关的 IP 地址。  
  
8.  在**首选 DNS 服务器**框中，键入你 DNS 服务器的 IP 地址。  
  
    > [!NOTE]
    >  使用的 IP 地址分配给你的网络适配器由 DHCP (例如，192.168.X.X) 而不是回送网络 (例如，127.0.0.1)。 若要查明分配的 IP 地址，运行**ipconfig**在命令提示符。  
  
9. 在**备用 DNS 服务器**框中，键入备用 DNS 服务器的 IP 地址 （如果有。  
  
10. 单击**确定**，然后单击**关闭**。  
  
> [!IMPORTANT]
>  确保你配置前进端口 80 和到服务器的新的静态 IP 地址的 443 到路由器。  
  
##  <a name="BKMK_DNS"></a>第 3 步： 准备证书和 DNS 记录网络位置服务器  
 要证书和 DNS 记录准备网络位置服务器，请执行以下任务：  
  
-   [单步执行 3a： 的 Web 服务器的证书模板授予身份验证用户完整的权限](#BKMK_GrantFullPermissions)  
  
-   [单步执行 3b： 注册证书网络位置服务器不可从外部网络解析常见名称](#BKMK_EnrollaCertificate)  
  
-   [步骤 3 c： 添加新的主 DNS 服务器上，并将其映射到 Windows Server Essentials 服务器地址。](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="BKMK_GrantFullPermissions"></a>单步执行 3a： 的 Web 服务器的证书模板授予身份验证用户完整的权限  
 您的首要任务是授予完整 Web 服务器的证书模板中的证书颁发机构的用户进行身份验证的权限。  
  
####  <a name="BKMK_ToGrantFullPermissions"></a>完整向授予权限的用户身份验证的 Web 服务器 s 证书模板  
  
1.  在**开始**页上，打开**证书颁发机构**。  
  
2.  控制台树中，在下**证书颁发机构 （本地）**，展开**< servername\ >-CA**，右键单击**证书模板**，然后单击**管理**。  
  
3.  在**证书颁发机构 （本地）**，右键单击**Web 服务器**，然后单击**属性**。  
  
4.  中的 Web 服务器属性上**安全**选项卡上，单击**用户身份验证**，选择**完全控制**，然后单击**确定**。  
  
5.  重启**Active Directory 证书服务**。 在控制面板中，打开**查看本地服务**。 在服务列表中，右键单击**Active Directory 证书服务**，然后单击**重启**。  
  
###  <a name="BKMK_EnrollaCertificate"></a>单步执行 3b： 注册证书网络位置服务器不可从外部网络解析常见名称  
 接下来，注册证书网络位置服务器不可从外部网络解析通用名称。  
  
####  <a name="BKMK_ToEnrollaCertificate"></a>若要注册证书网络位置服务器  
  
1.  在**开始**页上，打开**mmc** （Microsoft 管理控制台）。  
  
2.  如果**用户帐户控制**警告消息出现时，单击**是**。  
  
     打开 Microsoft 管理控制台 (MMC)。  
  
3.  在**文件**菜单上，单击**添加/删除管理单元**。  
  
4.  在**添加或远程单元**框中，单击**证书**，然后单击**添加**。  
  
5.  在**证书管理单元**页上，单击**计算机帐户**，然后单击**下一步**。  
  
6.  在**选择计算机**页上，单击**本地计算机**，单击**完成**，然后单击**确定**。  
  
7.  控制台树中，展开**证书 （本地计算机）**，展开**个人**，右键单击**证书**，然后在**所有任务**，单击**申请新证书**。  
  
8.  出现证书的注册向导后，单击**下一步**。  
  
9. 在**选择证书的注册策略**页上，单击**下一步**。  
  
10. 在**请求证书**页上，选择**Web 服务器**复选框，然后依次单击**该证书的注册所需的详细信息**。  
  
11. 在**证书属性**框中，输入的以下设置**主题名称**:  
  
    1.  对于**类型**、 选择**常见名称**。  
  
    2.  对于**值**、 键入网络位置服务器 (例如，DirectAccess-NLS.contoso.local) 的名称，然后单击**添加**。  
  
    3.  单击**确定**，然后单击**注册**。  
  
12. 当证书的注册完成后时，单击**完成**。  
  
###  <a name="BKMK_MapNewHosttoServerAddress"></a>步骤 3 c： 添加新的主 DNS 服务器上，并将其映射到 Windows Server Essentials 服务器的地址  
 若要完成 DNS 配置，添加新的主 DNS 服务器上，并将其映射到 Windows Server Essentials 服务器的地址。  
  
####  <a name="BKMK_ToMapNewHosttoServerAddress"></a>若要将映射到 Windows Server Essentials 服务器地址新主机  
  
1.  在开始页面上，打开 DNS 管理器。 若要打开 DNS 管理器中，搜索**dnsmgmt.msc**，然后单击**dnsmgmt.msc**在结果中。  
  
2.  在 DNS 管理控制台树中，依次展开本地服务器、**前进查找区域**，右键单击具有服务器的域职务的区域，然后单击**（A 或 AAAA） 的新主机**。  
  
3.  键入名称 (例如，DirectAccess NLS.contoso.local) 服务器的 IP 地址和其对应服务器的地址 (例如，192.168.x.x)。  
  
4.  单击**添加主机**，单击**确定**，然后单击**完成**。  
  
##  <a name="BKMK_AddSecurityGroup"></a>第 4 步： 为创建一个安全组 DirectAccess 客户端计算机  
 接下来，创建一个安全组用于 DirectAccess 客户端计算机，并将计算机的帐户添加到组。  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>若要添加为使用 DirectAccess 客户端计算机安全组  
  
1.  在服务器管理器仪表板中，单击**工具**，然后单击**Active Directory 用户和计算机**。  
  
    > [!NOTE]
    >  如果你没有看到**Active Directory 用户和计算机**上**工具**菜单上，你需要安装该功能。 若要安装 Active Directory 用户和组，以管理员身份运行以下 Windows PowerShell cmdlet: `Install-WindowsFeature RSAT-ADDS-Tools`。 有关详细信息，请参阅[安装或删除远程服务器管理工具包](https://technet.microsoft.com/library/cc730825.aspx)。  
  
2.  在控制台树中，展开服务器、 右键单击**用户**，单击**新建**，然后单击**组**。  
  
3.  输入一个组的名称、 组 scope，并且组类型 （创建一个安全组），然后单击**确定**。  
  
 新的安全组添加到**用户**文件夹。  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>若要将计算机的帐户添加到安全组  
  
1.  在服务器管理器仪表板中，单击**工具**，然后单击**Active Directory 用户和计算机**。  
  
2.  控制台树中，展开服务器，，然后单击**用户**。  
  
3.  在用户帐户和服务器上的安全组列表中，右键单击 DirectAccess，为你创建的安全组，然后单击**属性**。  
  
4.  在**成员**选项卡上，单击**添加**。  
  
5.  在对话框中，键入你想要添加到组中，分号 （;） 的分隔名计算机帐户名称。 然后单击**检查名称**。  
  
6.  验证计算机帐户后，单击**确定**。 然后单击**确定**再次。  
  
> [!NOTE]
>  你还可以使用**的成员**计算机帐户属性帐户添加安全组到选项卡。  
  
##  <a name="BKMK_EnableConfigureDA"></a>第 5 步： 启用并配置 DirectAccess  
 若要启用并配置 Windows Server Essentials 中的 DirectAccess，你必须完成以下步骤：  
  
-   [第 5a 步： 使用远程访问管理控制台启用 DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [步骤 5b： 删除 RRAS GPO (仅 Windows Server Essentials) 中的无效 IPv6Prefix](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [步骤 5 c： 启用运行 Windows 7 企业版，若要使用 DirectAccess 客户端计算机](#BKMK_Step4cWindows7Setup)  
  
-   [步骤 5 d： 配置网络位置服务器](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [步骤 5e： 添加绕过 CA 认证建立 IPsec 信道时的注册表项](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="BKMK_EnableDA"></a>第 5a 步： 使用远程访问管理控制台启用 DirectAccess  
 此部分中提供的分步说明，以便在 Windows Server Essentials DirectAccess。 如果尚未服务器上不具有配置 VPN，你应该执行该操作前开始此过程。 有关说明，请参阅[管理 VPN](Manage-VPN-in-Windows-Server-Essentials.md)。  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>使用远程访问管理控制台启用 DirectAccess  
  
1.  在开始页面上，打开**远程访问管理**。  
  
2.  在启用 DirectAccess 向导中，请执行以下操作：  
  
    1.  查看**DirectAccess 先决条件**，然后单击**下一步**。  
  
    2.  在**选择组**选项卡，添加你之前创建了 DirectAccess 客户端的安全组。 (如果你未创建安全组，请参阅[第 4 步： 创建计算机的 DirectAccess 客户端的安全组](#BKMK_AddSecurityGroup)的说明进行操作。)  
  
    3.  在**选择组**选项卡上，单击**移动计算机仅用于启用 DirectAccess**如果你想要启用移动计算机使用 DirectAccess 远程访问服务器，，然后单击**下一步**。  
  
    4.  在**网络拓扑**、 选择服务器，拓扑，然后单击**下一步**。  
  
    5.  在**DNS 职务搜索列表**，如果需要请添加的客户端计算机中，其他 DNS 职务，然后单击**下一步**。  
  
        > [!NOTE]
        >  默认情况下，启用 DirectAccess 向导已添加了有关当前域 DNS 职务。 但是，你可以更多如果需要添加。  
  
    6.  查看组策略对象 (Gpo)，将应用，并根据需要对其进行修改。  
  
    7.  单击**下一步**，然后单击**完成**。  
  
    8.  通过在提升了权限的模式下运行以下 Windows PowerShell 命令，重新启动远程访问权限管理服务：  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="BKMK_RemoveIPv6"></a>步骤 5b： 删除 RRAS GPO (仅 Windows Server Essentials) 中的无效 IPv6Prefix  
  本部分适用于运行 Windows Server Essentials 的服务器。  
  
 打开 Windows PowerShell 以管理员身份登录，并运行以下命令：  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="BKMK_Step4cWindows7Setup"></a>步骤 5 c： 启用运行 Windows 7 企业版，若要使用 DirectAccess 客户端计算机  
 如果你有运行 Windows 7 企业版客户端计算机，完成以下过程，以使 DirectAccess 来自这些计算机。  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>若要使 Windows 7 企业版计算机使用 DirectAccess  
  
1.  在服务器的开始页面上，打开**远程访问管理**。  
  
2.  在远程访问管理控制台中，单击**配置**。 然后，在**设置的详细信息**窗格下**第 2 步**，单击**编辑**。  
  
     打开远程访问服务器设置向导。  
  
3.  在**身份验证**选项卡上，选择将为受信任的根证书 （你可以选择的 Windows Server Essentials 服务器 CA 证书） 的证书颁发机构证书。 单击**启用 Windows 7 客户端计算机连接通过 DirectAccess**，然后单击**下一步**。  
  
4.  按照说明来完成向导。  
  
> [!IMPORTANT]
>  没有为 DirectAccess 在连接，如果 Windows Server Essentials 服务器未配备 UR1 预装的 Windows 7 企业版计算机一个已知的问题。 若要启用此环境中的 DirectAccess 连接，你必须执行这些额外的步骤：  
>   
>  1.  安装修补程序，述[Microsoft 知识库 (KB) 的文章 2796394](https://support.microsoft.com/kb/2796394) Windows Server Essentials 服务器上。 然后重新启动的服务器。  
> 2.  然后安装修补程序，述[Microsoft 知识库 (KB) 的文章 2615847](https://support.microsoft.com/kb/2615847)每个 Windows 7 的计算机上。  
>   
>      在 Windows Server Essentials 解决此问题。  
  
###  <a name="BKMK_NLS"></a>步骤 5 d： 配置网络位置服务器  
 此部分中提供的分步说明配置网络位置服务器设置。  
  
> [!NOTE]
>  在开始之前，将 < SystemDrive\ > \inetpub\wwwroot 文件夹中的内容复制到 < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site\insideoutside 文件夹中。 此外将从 < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site 文件夹的 default.aspx 文件复制到 < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site\insideoutside 文件夹中。  
  
##### <a name="to-configure-the-network-location-server"></a>若要配置网络位置服务器  
  
1.  在开始页面上，打开**远程访问管理**。  
  
2.  在远程访问管理控制台中，单击**配置**，并在**远程访问设置**中的详细信息窗格中，**第 3 步**，单击**编辑**。  
  
3.  在远程访问服务器设置向导中，在**网络位置服务器**选项卡上，选择**网络位置服务器部署在远程访问服务器**，，然后选择以前由于证书 (在[第 3 步： 准备网络位置服务器证书和 DNS 记录](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS))。  
  
4.  按照说明进行操作，以完成向导中，然后单击**完成**。  
  
###  <a name="BKMK_CA"></a>步骤 5e： 添加绕过 CA 认证建立 IPsec 信道时的注册表项  
 你下一步是配置绕过 CA 认证建立 IPsec 信道时的服务器。  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>若要添加注册表项来跳过 CA 证书  
  
1.  在开始页面上，打开**regedit** （注册表编辑器中）。  
  
2.  在注册表编辑器中，展开**HKEY_LOCAL_MACHINE**，展开**系统**，展开**开目前控制设置**，展开**服务**，然后展开**IKEEXT**。  
  
3.  下**IKEEXT**，右键单击**参数**，单击**新建**，然后单击**DWORD （32 位） 值**。  
  
4.  重命名到新添加的值**ikeflags**。  
  
5.  双击**ikeflags**、 设置**类型**到**十六进制**，将设置为**8000**，然后单击**确定**。  
  
> [!NOTE]
>  你可用于 Windows PowerShell 以下命令以提升模式添加此注册表项：  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="BKMK_NRPT"></a>第 6 步： 配置名称分辨率策略表 DirectAccess 服务器设置  
 此部分中提供的说明进行编辑内部地址 （例如，contoso.local 职务的条目） 的名称分辨率策略表 (NPRT) 项，以 DirectAccess 客户端 Gpo，并设置 IPHTTPS 界面地址。  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>若要配置名称分辨率策略表条目  
  
1.  在开始页面上，打开**组策略管理**。  
  
2.  在组策略管理控制台中，单击默认森林和域中，右键单击**DirectAccess 客户端设置**，然后单击**编辑**。  
  
3.  单击**计算机配置**，单击**策略**，单击**Windows 设置**，然后单击**名称分辨率策略**。 选择具有与您 DNS 职务，命名空间的条目，然后单击**编辑规则**。  
  
4.  单击**DirectAccess DNS 设置**选项卡;然后选择**启用 DNS 设置 DirectAccess 在此规则**。 在 DNS 服务器列表中添加 IP HTTPS 界面 IPv6 地址。  
  
    > [!NOTE]
    >  你可以使用 Windows PowerShell 以下命令以获取 IPv6 地址：  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="BKMK_TCPUDP"></a>第 7 步： 配置 DirectAccess 服务器 Gpo TCP 和 UDP 防火墙规则  
 此部分中包括配置 DirectAccess 服务器 Gpo TCP 和 UDP 防火墙规则的分步说明进行操作。  
  
#### <a name="to-configure-firewall-rules"></a>若要配置防火墙规则  
  
1.  在开始页面上，打开**组策略管理**。  
  
2.  在组策略管理控制台中，单击默认森林和域中，右键单击**DirectAccess 服务器设置**，然后单击**编辑**。  
  
3.  单击**计算机配置**，单击**策略**，单击**Windows 设置**，单击**安全设置**，单击**高级安全 Windows 防火墙**，单击级别下一章**高级安全 Windows 防火墙**，然后单击**归巢的规则**。 右键单击**域名 （TCP 中） 服务器**，然后单击**属性**。  
  
4.  单击**范围**选项卡，然后在**本地 IP 地址**列表中添加 IP HTTPS 接口 IPv6 地址。  
  
5.  重复步骤来**域名 （UDP 在） 服务器**。  
  
##  <a name="BKMK_DNS64"></a>第 8 步： 更改 DNS64 配置若要收听 IP HTTPS 界面  
 你必须更改 DNS64 配置若要听到声音 IP HTTPS 界面通过使用以下 Windows PowerShell 命令。  
  
```powershell  
Set-NetDnsTransitionConfiguration  �AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="BKMK_ExemptPort"></a>第 9 步： 保留 WinNat 送修端口  
 使用下面的 Windows PowerShell 命令保留 WinNat 服务的端口。 替换你的 Windows Server Essentials 服务器的实际 IPv4 地址"192.168.1.100"。  
  
```powershell  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  要避免端口冲突与应用程序，请确保你预订 WinNat 送修端口范围不包括 6602 端口。  
  
##  <a name="BKMK_WinNAT"></a>第 10 步： 重启 WinNat 服务  
 通过运行以下 Windows PowerShell 命令，重新启动 Windows NAT 驱动程序 (WinNat) 的服务。  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="BKMK_AppendixBPowerShellScript"></a>附录： 通过使用 Windows PowerShell DirectAccess 设置  
 此部分中介绍了如何设置并使用 Windows PowerShell 配置 DirectAccess。  
  
### <a name="preparation"></a>准备  
 配置服务器的 DirectAccess 之前，你必须完成以下：  
  
1.  按照中的步骤[第 3 步： 准备网络位置服务器证书和 DNS 记录](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)注册一个名为证书**DirectAccess NLS.contoso.com** (其中**contoso.com**替换为你实际内部域名)，以及添加网络位置服务器 (NLS) DNS 记录。  
  
2.  添加了一个名为安全组**DirectAccessClients**中 Active Directory，然后添加所需以提供 DirectAccess 功能的客户端计算机。 有关详细信息，请参阅[第 4 步： 创建计算机的 DirectAccess 客户端的安全组](#BKMK_AddSecurityGroup)。  
  
### <a name="commands"></a>命令  
  
> [!IMPORTANT]
>  Windows Server Essentials，不需要删除不必要的 IPv6 前缀 GPO。 删除明代码以下标签： `# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`。  
  
```powershell  
# Add Remote Access role if not installed yet  
$ra = Get-WindowsFeature RemoteAccess  
If ($ra.Installed -eq $FALSE) { Add-WindowsFeature RemoteAccess }  
  
# Server may need to restart if you installed RemoteAccess role in the above step  
  
# Set the internet domain name to access server, replace contoso.com below with your own domain name  
$InternetDomain = "www.contoso.com"  
#Set the SG name which you create for DA clients  
$DaSecurityGroup = "DirectAccessClients"  
#Set the internal domain name  
$InternalDomain = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Name  
  
# Set static IP and DNS settings  
$NetConfig = Get-WmiObject Win32_NetworkAdapterConfiguration -Filter "IPEnabled=$true"  
$CurrentIP = $NetConfig.IPAddress[0]  
$SubnetMask = $NetConfig.IPSubnet | Where-Object{$_ -like "*.*.*.*"}  
$NetConfig.EnableStatic($CurrentIP, $SubnetMask)  
$NetConfig.SetGateways($NetConfig.DefaultIPGateway)  
$NetConfig.SetDNSServerSearchOrder($CurrentIP)  
  
# Get physical adapter name and the certificate for NLS server  
$Adapter = (Get-WmiObject -Class Win32_NetworkAdapter -Filter "NetEnabled=$true").NetConnectionId  
$Certs = dir cert:\LocalMachine\My  
$nlscert = $certs | Where-Object{$_.Subject -like "*CN=DirectAccess-NLS*"}  
  
# Add regkey to bypass CA cert for IPsec authentication  
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000  
  
# Install DirectAccess.   
Install-RemoteAccess -NoPrerequisite -DAInstallType FullInstall -InternetInterface $Adapter -InternalInterface $Adapter -ConnectToAddress $InternetDomain -nlscertificate $nlscert -force  
  
#Restart Remote Access Management service  
Restart-Service RaMgmtSvc  
  
# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO   
  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
  
# Enable client computers running Windows 7 to use DirectAccess  
$allcertsinroot = dir cert:\LocalMachine\root  
$rootcert = $allcertsinroot | Where-Object{$_.Subject -like "*-CAA*"}  
Set-DAServer  �IPSecRootCertificate $rootcert[0]  
Set  �DAClient  �OnlyRemoteComputers Disabled -Downlevel Enabled  
  
#Set the appropriate security group used for DA client computers. Replace the group name below with the one you created for DA clients  
Add-DAClient -SecurityGroupNameList $DaSecurityGroup   
Remove-DAClient -SecurityGroupNameList "Domain Computers"  
  
# Gather DNS64 IP address information  
$Remoteaccess = get-remoteaccess  
$IPinterface = get-netipinterface -InterfaceAlias IPHTTPSInterface | get-netipaddress -PrefixLength 128  
$DNS64IP=$IPInterface[1].IPaddress  
$Natconfig = Get-NetNatTransitionConfiguration  
  
# Configure TCP and UDP firewall rules for the DirectAccess server GPO  
$GpoName = 'GPO:'+$InternalDomain+'\DirectAccess Server Settings'  
Get-NetFirewallRule -PolicyStore $GpoName -Displayname "Domain Name Server (TCP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
Get-NetFirewallrule -PolicyStore $GpoName -Displayname "Domain Name Server (UDP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
  
# Configure the name resolution policy settings for the DirectAccess server, replace the DNS suffix below with the one in your domain  
$Suffix = '.' + $InternalDomain  
set-daclientdnsconfiguration -DNSsuffix $Suffix -DNSIPAddress $DNS64IP  
  
# Change the DNS64 configuration to listen to IP-HTTPS interface  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
  
# Copy the necessary files to NLS site folder  
XCOPY 'C:\inetpub\wwwroot' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /E /Y  
XCOPY 'C:\Program Files\Windows Server\Bin\WebApps\Site\Default.aspx' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /Y  
  
# Reserve ports for the WinNat service  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>请参阅  
  
-   [管理任意位置的访问权限](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
