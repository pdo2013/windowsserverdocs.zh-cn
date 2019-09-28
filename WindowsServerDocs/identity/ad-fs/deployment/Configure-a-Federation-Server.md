---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Windows Server 2012 R2 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c549b52f697db889bf638470aca03f02e1323ed5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359727"
---
# <a name="configure-a-federation-server"></a>配置联合服务器

在您的计算机上\(安装\) Active Directory 联合身份验证服务 AD FS 角色服务后，您就可以将此计算机配置为联合服务器了。 执行下面其中一项操作：  
  
-   [配置新联合服务器场中的第一台联合服务器](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [将联合服务器添加到现有的联合服务器场](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>配置新联合服务器场中的第一台联合服务器  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>使用 Active Directory 联合身份验证服务配置向导在新的联合服务器场中配置第一台联合服务器  
  
> [!NOTE]  
> 执行此过程之前，请确保你具有域管理员权限，或具有可用的域管理员凭据。  
  
1.  在服务器管理器“仪表板”页面上，单击“通知”标志，然后单击“在服务器上配置联合身份验证服务”。  
  
    打开“Active Directory 联合身份验证服务配置向导” 。  
  
2.  在“欢迎使用”页面上，选择“在联合服务器场中创建第一个联合服务器”，然后单击“下一步”。  
  
3.  在 "**连接到 AD DS** " 页上，使用 "域管理员" 权限为此计算机\(加入\)的 Active Directory AD 域指定一个帐户，然后单击 "**下一步**"。  
  
4.  在“指定服务属性” 页面上，执行以下操作，然后再单击“下一步”：  
  
    -   导入 .pfx 文件，其中包含之前获取的安全\(套\)接字层 SSL 证书和密钥。 在[步骤 2:注册 AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)的 SSL 证书，已获取此证书，并将其复制到要配置为联合服务器的计算机上。 若要通过向导导入 .pfx 文件，请单击 "**导入**"，然后浏览到文件的位置。 出现提示时，请输入 .pfx 文件的密码。  
  
    -   提供联合身份验证服务的名称。 例如， **fs.contoso.com**。 此名称必须与证书中的使用者或使用者备用名称之一匹配。  
  
    -   提供联合身份验证服务的显示名称。 例如**Contoso Corporation**。 用户将在 Active Directory 联合身份验证服务\(AD FS\)登录\-页上看到此名称。  
  
5.  在 "**指定服务帐户**" 页上，指定服务帐户。 你可以创建或使用现有的组托管服务帐户\(gMSA\)或使用现有的域用户帐户。 如果选择 "创建新的 gMSA 帐户" 选项，请指定新帐户的名称。 如果选择 "使用现有 gMSA 或域帐户" 选项，请单击 "**选择**" 以选择帐户。  
  
    > [!NOTE]  
    > 使用 gMSA 帐户的好处是它的自动\-协商密码更新功能。  
  
    > [!WARNING]  
    > 如果要使用 gMSA 帐户，你的环境中必须至少有一个域控制器运行 Windows Server 2012 操作系统。  
    >   
    > 如果 "gMSA" 选项已禁用，并且你看到一条错误消息，如 **"组托管服务帐户" 不可用，因为未设置 KDS 根密钥**，你可以通过在域上运行以下 Windows PowerShell 命令在域中启用 gMSA在 Active Directory 域中运行 Windows Server 2012 或更高版本的控制器： `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 然后返回到向导，单击 "**上一**步"，然后单击 "\-**下一步**" 以重新输入 "**指定服务帐户**" 页。 现在应启用 gMSA 选项。 可以选择该名称，并输入要使用的 gMSA 帐户名称。  
  
6.  在 "**指定配置数据库**" 页上，指定 AD FS 配置数据库，然后单击 "**下一步**"。 您可以使用 Windows 内部数据库\(WID\)在此计算机上创建数据库，也可以指定 Microsoft SQL Server 的位置和实例名称。  
  
    有关详细信息，请参阅 [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
    > [!IMPORTANT]  
    > 如果要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
7.  在“查看选项”页面上，验证你的配置选择，然后单击“下一步”。  
  
8.  在 "**先决条件\-检查**" 页上，验证是否已成功完成所有先决条件检查，然后单击 "**配置**"。  
  
9. 在 "**结果**" 页上，查看结果并检查配置是否已成功完成，然后单击 "**完成联合身份验证服务部署所需的后续步骤**"。 有关详细信息，请参阅[完成 AD FS 安装的后续步骤](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 单击 "**关闭**" 退出向导。  
  
### <a name="BKMK_3"></a>通过 Windows PowerShell 配置新联合服务器场中的第一台联合服务器  
您可以使用新的或现有的 gMSA 帐户或现有的域用户帐户创建新的联合服务器场。  
  
-   **如果要使用新的 gMSA 帐户创建新的联合服务器，请执行以下操作：**  
  
    > [!IMPORTANT]  
    > 您必须具有域管理员权限才能在新的联合服务器场中创建第一个联合服务器。  
  
    1.  在要配置为联合服务器的计算机上，确保所需的 SSL 证书已导入到**本地计算机\\的 "我的应用商店"** 目录中。 你可以通过在 Windows PowerShell 命令窗口中运行以下命令来验证是否已导入 SSL 证书： `dir Cert:\LocalMachine\My`。 证书在**本地计算机\\的 "我的应用商店**" 目录中按其指纹列出。  
  
    2.  在域控制器上，打开 Windows PowerShell 命令窗口并运行以下命令，验证是否已在域中创建 KDS 根密钥： `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 如果尚未创建它，因此输出不显示任何信息，请运行以下命令来创建密钥： `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。  
  
    3.  在要配置为联合服务器的计算机上，打开 Windows PowerShell 命令窗口，并运行以下命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > 需要前一个命令末尾的符号。`$`  
  
        若要获取的值`<certificate_thumbprint>`，请`dir Cert:\LocalMachine\My`运行，然后选择 SSL 证书的指纹。 的值`<federation_service_name>`是联合身份验证服务的名称，例如**fs.contoso.com**。  
  
        > [!NOTE]  
        > 如果这不是第一次运行此命令，请添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 上述命令将创建一个 WID 场。 如果要创建 SQL Server 服务器场，则必须已安装和运行 SQL Server 的实例。  
        >   
        > 您可以使用以下命令在使用 SQL Server 实例的新场中创建第一个联合服务器： `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"`其中 **< SQL\_主机\_名 >** 是运行 SQL Server 的服务器的名称， **< "\_SQL\_实例名称" >** 是 SQL Server 实例的名称。 如果使用 SQL Server 的默认实例，请使用 "**数据\=源 < SQL\_\_主机名 >; 集成\=安全性 True**" 的**SQLConnectionString**值。  
  
        > [!IMPORTANT]  
        > 如果需要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。  
  
-   **如果要使用现有的域用户帐户创建新的联合服务器，请执行以下操作：**  
  
    1.  在要配置为联合服务器的计算机上，确保所需的 SSL 证书已导入到**本地计算机\\的 "我的应用商店"** 目录中。 你可以通过在 Windows PowerShell 命令窗口中运行以下命令来验证是否已导入 SSL 证书： `dir Cert:\LocalMachine\My`。 证书在**本地计算机\\的 "我的应用商店**" 目录中按其指纹列出。  
  
    2.  在要配置为联合服务器的计算机上，打开 Windows PowerShell 命令窗口，然后运行以下命令： `$fscred = Get-Credential`。 输入要用于联合身份验证服务帐户的域用户帐户凭据，格式为 "域\\用户名"。  
  
    3.  在同一个 Windows PowerShell 命令窗口中运行以下命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        若要获取 **\_< 证书指纹**的值 >，请`dir Cert:\LocalMachine\My`运行，然后选择 SSL 证书的指纹。 **< 联合身份验证\_服务\_名称 >** 的值是联合身份验证服务的名称，例如 fs.contoso.com。  
  
        > [!NOTE]  
        > 如果这不是第一次运行此命令，请添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 上述命令将创建一个 WID 场。 如果你想要创建 SQL Server 场，你必须已安装 SQL Server 实例且该实例可操作。  
        >   
        > 您可以使用以下命令在使用 SQL Server 实例的新场中创建第一个联合服务器： `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`其中， **SQL\_主机\_名**是运行 SQL Server 的服务器的名称， **sql实例\_名是 SQL Server 实例的名称。 \_** 如果使用 SQL Server 的默认实例，请使用 "**数据\=源 < SQL\_\_主机名 >; 集成\=安全性 True**" 的**SQLConnectionString**值。  
  
        > [!IMPORTANT]  
        > 如果要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="BKMK_2"></a>将联合服务器添加到现有的联合服务器场  
  
> [!IMPORTANT]  
> 确保已完成[步骤3：在开始本部分中](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)的任何过程之前，请安装 AD FS 角色服务。  
  
> [!IMPORTANT]  
> 在完成此过程之前，请确保已获得有效的 SSL 服务器身份验证证书。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>通过 Active Directory 联合身份验证服务配置向导将联合服务器添加到现有的联合服务器场  
  
1.  在服务器管理器“仪表板”页面上，单击“通知”标志，然后单击“在服务器上配置联合身份验证服务”。  
  
    打开“Active Directory 联合身份验证服务配置向导” 。  
  
2.  在 "**欢迎**" 页上，选择 "**将联合服务器添加到联合服务器场**"，然后单击 "**下一步**"。  
  
3.  在 "**连接到 AD DS** " 页上，使用此计算机加入的 AD 域的 "域管理员" 权限指定帐户，然后单击 "**下一步**"。  
  
4.  在 "**指定场**" 页上，提供在场中使用 WID 的主联合服务器的名称，或者指定使用 SQL Server 的现有联合服务器场的数据库主机名和数据库实例名称。  
  
    > [!WARNING]  
    > 在 Windows Server® 2012 R2 中，有一种方法可以指定 SQL Server 的默认实例。 解决方法是不使用用户界面。 而应使用[通过 Windows PowerShell 配置新联合服务器场中的第一个联合服务器](Configure-a-Federation-Server.md#BKMK_3)的步骤。  
  
    > [!IMPORTANT]  
    > 如果需要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。  
  
5.  在 "**指定 Ssl 证书**" 页上，导入包含你之前获取的 SSL 证书和密钥的 .pfx 文件。 此证书是所需的服务身份验证证书。 在[步骤 2:注册 AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)的 SSL 证书，已获取此证书，并将其复制到要配置为联合服务器的计算机。 若要通过向导导入 .pfx 文件，请单击 "**导入**" 并浏览到文件的位置。 出现提示时，请输入 .pfx 文件的密码。  
  
6.  在 "**指定服务帐户**" 页上，指定你在服务器场中创建第一台联合服务器时配置的相同服务帐户。 你可以使用现有的组托管服务帐户或现有的域用户帐户。  
  
    > [!IMPORTANT]  
    > 你指定的帐户必须与在此服务器场中的主联合服务器上使用的帐户相同。  
  
7.  在“查看选项”页面上，验证你的配置选择，然后单击“下一步”。  
  
8.  在 "**先决条件\-检查**" 页上，验证是否已成功完成所有先决条件检查，然后单击 "**配置**"。  
  
9. 在 "**结果**" 页上，查看结果并检查配置是否已成功完成，然后单击 "**完成联合身份验证服务部署所需的后续步骤**"。 有关详细信息，请参阅[完成 AD FS 安装的后续步骤](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 单击 "**关闭**" 退出向导。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>通过 Windows PowerShell 将联合服务器添加到现有的联合服务器场  
您可以使用现有的 gMSA 帐户或现有的域用户帐户将联合服务器添加到现有场。  
  
-   如果要使用现有 gMSA 帐户将联合服务器加入到场中，请执行以下操作：  
  
    1.  在要配置为联合服务器的计算机上，确保所需的 SSL 证书已导入到**本地计算机\\的 "我的应用商店"** 目录中。 你可以通过在 Windows PowerShell 命令窗口中运行以下命令来验证是否已导入 SSL 证书： `dir Cert:\LocalMachine\My`。 证书在**本地计算机\\的 "我的应用商店**" 目录中按其指纹列出。  
  
    2.  在要配置为联合服务器的计算机上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>`是你的 AD 域以及该域中 gMSA 帐户的名称。 `<first_federation_server_hostname>`此现有场中的主联合服务器的主机名。  
  
        您可以`<certificate_thumbprint>`通过在上一步中`dir Cert:\LocalMachine\My`运行来获取的值。  
  
        > [!NOTE]  
        > 如果这不是第一次运行此命令，请添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 上述命令将创建一个 WID 场节点。 如果要创建运行 SQL Server 的计算机的服务器场节点，则必须已安装了 SQL Server 实例且该实例可操作。  
        >   
        > 您可以使用以下命令向使用 SQL Server 实例的现有场中添加联合服务器： `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`其中， **SQL\_主机\_名**是运行 SQL Server 的服务器的名称， **sql实例\_名是 SQL Server 实例的名称。 \_** 如果使用 SQL Server 的默认实例，请使用 "**数据\=源 < SQL\_\_主机名 >; 集成\=安全性 True**" 的**SQLConnectionString**值。  
  
        > [!IMPORTANT]  
        > 如果要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
-   如果要使用现有的域用户帐户将联合服务器加入到场中，请执行以下操作：  
  
    1.  在要配置为联合服务器的计算机上，打开 "Windows PowerShellcommand" 窗口，并运行以下命令： `$fscred = get-credential`。 输入要用于联合身份验证服务帐户的域用户帐户凭据，格式为 "域\\用户名"。  
  
    2.  在要配置为联合服务器的计算机上，确保所需的 SSL 证书已导入到**本地计算机\\的 "我的应用商店"** 目录中。 你可以通过在 Windows PowerShellcommand 窗口中运行以下命令来验证是否已导入 SSL 证书： `dir Cert:\LocalMachine\My`。 证书在**本地计算机\\的 "我的应用商店**" 目录中按其指纹列出。  
  
    3.  在同一个 Windows PowerShell 命令窗口中运行以下命令。  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > 如果这不是第一次运行此命令，请添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 上述命令将创建一个 WID 场节点。 如果要创建运行 SQL Server 的计算机的服务器场节点，则必须已安装了 SQL Server 实例且该实例可操作。 您可以使用以下命令使用 SQL Server 的实例将联合服务器添加到现有场： `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`其中， **SQL\_主机\_名**是运行 SQL Server 实例的服务器的名称，**SQL\_实例\_名称**是 SQL Server 实例的名称。 如果使用 SQL Server 的默认实例，请使用 "**数据\=源 < SQL\_\_主机名 >; 集成\=安全性 True**" 的**SQLConnectionString**值。  
  
        > [!IMPORTANT]  
        > 如果要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="see-also"></a>请参阅 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

