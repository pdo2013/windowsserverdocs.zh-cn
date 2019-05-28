---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Windows Server 2012 R2 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2f597994aa74f453903e09f7d3eefd83f26faba
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192269"
---
# <a name="configure-a-federation-server"></a>配置联合服务器

安装 Active Directory 联合身份验证服务后\(AD FS\)角色服务在计算机上已准备好将该计算机成为联合服务器配置。 执行下面其中一项操作：  
  
-   [在新的联合服务器场中配置的第一个联合身份验证服务器](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [向现有的联合服务器场中添加联合身份验证服务器](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>在新的联合服务器场中配置的第一个联合身份验证服务器  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>若要使用 Active Directory 联合身份验证服务配置向导在新的联合服务器场中配置的第一个联合身份验证服务器  
  
> [!NOTE]  
> 请确保你拥有域管理员权限或具有可用的域管理员凭据，然后执行此过程。  
  
1.  在服务器管理器“仪表板”  页面上，单击“通知”  标志，然后单击“在服务器上配置联合身份验证服务”  。  
  
    打开“Active Directory 联合身份验证服务配置向导”  。  
  
2.  在“欢迎使用”  页面上，选择“在联合服务器场中创建第一个联合服务器”  ，然后单击“下一步”  。  
  
3.  上**连接到 AD DS**页上，指定通过使用 Active Directory 域管理员权限的帐户\(AD\)此计算机所加入的域，然后单击**下一步**.  
  
4.  在“指定服务属性”  页面上，执行以下操作，然后再单击“下一步”  ：  
  
    -   导入.pfx 文件包含安全套接字层\(SSL\)证书和你前面获取的密钥。 在[步骤 2:为 AD FS 注册 SSL 证书](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)，获取此证书并将其复制到你想要将配置为联合身份验证服务器的计算机上。 若要通过该向导将.pfx 文件导入，请单击**导入**，然后浏览到该文件的位置。 输入在得到提示时的.pfx 文件的密码。  
  
    -   提供联合身份验证服务的名称。 例如， **fs.contoso.com**。 此名称必须与使用者或证书中的使用者可选名称之一匹配。  
  
    -   提供联合身份验证服务的显示名称。 例如， **Contoso Corporation**。 用户看到此名称在 Active Directory 联合身份验证服务\(AD FS\)登录\-页中。  
  
5.  上**指定服务帐户**页上，指定服务帐户。 您可以创建或使用现有的组托管服务帐户\(gMSA\)或使用现有的域用户帐户。 如果选择创建新的 gMSA 帐户的选项，指定新帐户的名称。 如果选择使用现有的 gMSA 或域帐户的选项，则单击**选择**选择一个帐户。  
  
    > [!NOTE]  
    > 使用 gMSA 帐户的好处是其自动\-协商密码更新功能。  
  
    > [!WARNING]  
    > 如果你想要使用 gMSA 帐户，您必须至少一个域控制器运行 Windows Server 2012 操作系统的环境中。  
    >   
    > 如果禁用了 gMSA 选项，并且你看到错误消息，如**组托管服务帐户不可用，因为尚未设置 KDS 根密钥**，可以通过运行以下 Windows 域中启用 gMSA在域控制器上，运行 Windows Server 2012 或更高版本，Active Directory 域中的 PowerShell 命令： `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 然后返回到向导中，单击**上一步**，然后单击**下一步**到重新\-输入**指定服务帐户**页。 现在应启用 gMSA 选项。 可以选择它，输入你想要使用的 gMSA 帐户名称。  
  
6.  上**指定配置数据库**页上，指定 AD FS 配置数据库，，然后单击**下一步**。 您可以创建一个数据库在此计算机上使用 Windows 内部数据库\(WID\)，也可以指定的位置和 Microsoft SQL Server 实例名称。  
  
    有关详细信息，请参阅 [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
    > [!IMPORTANT]  
    > 如果你想要创建的 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
7.  在“查看选项”  页面上，验证你的配置选择，然后单击“下一步”  。  
  
8.  上**Pre\-必备项检查**页上，验证所有先决条件检查都成功完成，然后单击**配置**。  
  
9. 上**结果**页上，查看结果并检查是否已成功，完成配置，然后单击**完成联合身份验证服务部署所需的下一步步骤**。 有关详细信息，请参阅[后续步骤来完成 AD FS 安装](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 单击**关闭**以退出向导。  
  
### <a name="BKMK_3"></a>若要通过 Windows PowerShell 新联合服务器场中配置的第一个联合身份验证服务器  
可以通过使用新的或现有的 gMSA 帐户或现有的域用户帐户创建新的联合服务器场。  
  
-   **如果你想要使用新的 gMSA 帐户创建新的联合身份验证服务器，请执行以下操作：**  
  
    > [!IMPORTANT]  
    > 必须具有域管理员权限才能在新的联合服务器场中创建第一个联合身份验证服务器。  
  
    1.  在你想要将配置为联合身份验证服务器的计算机，请确保所需的 SSL 证书已导入**本地计算机\\应用商店的 My**目录。 你可以验证是否通过在 Windows PowerShell 命令窗口中运行以下命令导入 SSL 证书： `dir Cert:\LocalMachine\My`。 证书指纹的形式列出**本地计算机\\应用商店的 My**目录。  
  
    2.  在域控制器上，打开 Windows PowerShell 命令窗口并运行以下命令以验证是否已在域中创建 KDS 根密钥： `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 如果它尚未创建，以便输出不显示任何信息，运行以下命令来创建该密钥： `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。  
  
    3.  在你想要将配置为联合身份验证服务器的计算机，打开 Windows PowerShell 命令窗口中，并运行以下命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > `$`登录前一命令的末尾是必需的。  
  
        若要获取的值`<certificate_thumbprint>`，请运行`dir Cert:\LocalMachine\My`，然后选择你的 SSL 证书的指纹。 值`<federation_service_name>`是联合身份验证服务的名称。 例如， **fs.contoso.com**。  
  
        > [!NOTE]  
        > 如果这不是第一次运行此命令，添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 前一个命令创建一个 WID 场。 如果你想要创建 SQL Server 的服务器场，则必须已安装且正常运行的 SQL Server 的实例。  
        >   
        > 可以使用以下命令以创建第一个联合服务器使用的 SQL Server 实例的新场中：`Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"`其中 **< SQL\_主机\_名称 >** 上的 SQL server 的名称服务器正在运行，并 **< SQL\_实例\_名称 >** 是 SQL Server 实例的名称。 如果使用 SQL Server 的默认实例，使用**SQLConnectionString**的值"**数据源\=< SQL\_主机\_名称 >; Integrated Security\=，则返回 True**".  
  
        > [!IMPORTANT]  
        > 如果需要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。  
  
-   **如果你想要使用现有的域用户帐户创建新的联合身份验证服务器，请执行以下操作：**  
  
    1.  在你想要将配置为联合身份验证服务器的计算机，请确保所需的 SSL 证书已导入**本地计算机\\应用商店的 My**目录。 你可以验证是否通过在 Windows PowerShell 命令窗口中运行以下命令导入 SSL 证书： `dir Cert:\LocalMachine\My`。 证书指纹的形式列出**本地计算机\\应用商店的 My**目录。  
  
    2.  在你想要将配置为联合身份验证服务器的计算机，打开 Windows PowerShell 命令窗口，然后运行以下命令： `$fscred = Get-Credential`。 输入你想要使用的格式域中的联合身份验证服务帐户的域用户帐户凭据\\用户名称。  
  
    3.  在同一个 Windows PowerShell 命令窗口中运行以下命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        若要获取的值 **< 证书\_指纹 >** ，请运行`dir Cert:\LocalMachine\My`，然后选择你的 SSL 证书的指纹。 值 **< 联合身份验证\_服务\_名称 >** 是你的联合身份验证服务 （例如 fs.contoso.com） 的名称。  
  
        > [!NOTE]  
        > 如果这不是第一次运行此命令，添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 前一个命令创建一个 WID 场。 如果你想要创建 SQL Server 场，则必须已安装且正常运行的 SQL Server 的实例。  
        >   
        > 可以使用以下命令以创建第一个联合服务器使用的 SQL Server 实例的新场中：`Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`其中**SQL\_主机\_名称**是 SQL Server 的服务器的名称运行，并**SQL\_实例\_名称**是 SQL Server 实例的名称。 如果使用 SQL Server 的默认实例，使用**SQLConnectionString**的值"**数据源\=< SQL\_主机\_名称 >; Integrated Security\=，则返回 True**".  
  
        > [!IMPORTANT]  
        > 如果你想要创建的 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="BKMK_2"></a>向现有的联合服务器场中添加联合身份验证服务器  
  
> [!IMPORTANT]  
> 确保已完成[步骤 3:安装 AD FS 角色服务](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)之前在本部分中启动任何过程。  
  
> [!IMPORTANT]  
> 请确保你已获得有效的 SSL 服务器身份验证证书之前完成此过程。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>若要将联合服务器添加到 Active Directory 联合身份验证服务配置向导通过现有的联合服务器场  
  
1.  在服务器管理器“仪表板”  页面上，单击“通知”  标志，然后单击“在服务器上配置联合身份验证服务”  。  
  
    打开“Active Directory 联合身份验证服务配置向导”  。  
  
2.  上**欢迎**页上，选择**将联合身份验证服务器添加到联合服务器场**，然后单击**下一步**。  
  
3.  上**连接到 AD DS**页上，指定的帐户使用域管理员权限为此计算机所加入的 AD 域，然后单击**下一步**。  
  
4.  上**指定场**页上，提供使用 WID 的场中的主联合服务器的名称或指定数据库主机名称和现有的联合服务器场使用 SQL Server 数据库实例名称。  
  
    > [!WARNING]  
    > 在 Windows Server® 2012 R2 中，没有一种解决方法，若要指定 SQL Server 的默认实例。 解决方法是使用用户界面。 而是使用中的步骤[若要通过 Windows PowerShell 新联合服务器场中配置的第一个联合身份验证服务器](Configure-a-Federation-Server.md#BKMK_3)。  
  
    > [!IMPORTANT]  
    > 如果需要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。  
  
5.  上**指定 SSL 证书**页上，导入包含的 SSL 证书和前面获取的密钥的.pfx 文件。 此证书是所需的服务身份验证证书。 在[步骤 2:为 AD FS 注册 SSL 证书](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)，获取此证书并将其复制到你想要将配置为联合身份验证服务器的计算机。 若要通过该向导将.pfx 文件导入，请单击**导入**并浏览到文件的位置。 输入在得到提示时的.pfx 文件的密码。  
  
6.  上**指定服务帐户**页上，指定服务器场中创建第一个联合身份验证服务器时配置的相同服务帐户。 可以使用现有的组托管服务帐户或现有的域用户帐户。  
  
    > [!IMPORTANT]  
    > 您指定的帐户必须与此服务器场中主联合服务器使用的帐户相同的帐户。  
  
7.  在“查看选项”  页面上，验证你的配置选择，然后单击“下一步”  。  
  
8.  上**Pre\-必备项检查**页上，验证所有先决条件检查都成功完成，然后单击**配置**。  
  
9. 上**结果**页上，查看结果并检查是否已成功，完成配置，然后单击**完成联合身份验证服务部署所需的下一步步骤**。 有关详细信息，请参阅[后续步骤来完成 AD FS 安装](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 单击**关闭**以退出向导。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>若要将联合服务器添加到现有的联合服务器场通过 Windows PowerShell  
通过使用现有的 gMSA 帐户或现有的域用户帐户，可以向现有场添加联合身份验证服务器。  
  
-   如果你想要使用现有的 gMSA 帐户加入到一个场的联合身份验证服务器，请执行以下操作：  
  
    1.  在你想要将配置为联合身份验证服务器的计算机，请确保所需的 SSL 证书已导入**本地计算机\\应用商店的 My**目录。 你可以验证是否通过在 Windows PowerShell 命令窗口中运行以下命令导入 SSL 证书： `dir Cert:\LocalMachine\My`。 证书指纹的形式列出**本地计算机\\应用商店的 My**目录。  
  
    2.  在你想要将配置为联合身份验证服务器的计算机，打开 Windows PowerShell 命令窗口中，并运行以下命令。  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` 是你的 AD 域和 gMSA 帐户，该域中的名称。 `<first_federation_server_hostname>` 是此现有场中的主联合服务器的主机名。  
  
        你可以获取的值`<certificate_thumbprint>`通过运行`dir Cert:\LocalMachine\My`在上一步。  
  
        > [!NOTE]  
        > 如果这不是第一次运行此命令，添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 前一个命令创建一个 WID 场节点。 如果你想要创建运行 SQL Server 的计算机的一个服务器场节点，则必须已安装且正常运行的 SQL Server 的实例。  
        >   
        > 可以使用以下命令将联合身份验证服务器添加到现有场正在使用的 SQL Server 实例：`Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`其中**SQL\_主机\_名称**是 SQL Server 的服务器的名称运行，并**SQL\_实例\_名称**是 SQL Server 实例的名称。 如果使用 SQL Server 的默认实例，使用**SQLConnectionString**的值"**数据源\=< SQL\_主机\_名称 >; Integrated Security\=，则返回 True**".  
  
        > [!IMPORTANT]  
        > 如果你想要创建的 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
-   如果你想要使用现有的域用户帐户加入到一个场的联合身份验证服务器，请执行以下操作：  
  
    1.  在你想要将配置为联合身份验证服务器的计算机，打开 Windows PowerShellcommand 窗口中，并运行以下命令： `$fscred = get-credential`。 输入你想要使用的格式域中的联合身份验证服务帐户的域用户帐户凭据\\用户名称。  
  
    2.  在你想要将配置为联合身份验证服务器的计算机，请确保所需的 SSL 证书已导入**本地计算机\\应用商店的 My**目录。 你可以验证是否通过 Windows PowerShellcommand 窗口中运行以下命令导入 SSL 证书： `dir Cert:\LocalMachine\My`。 证书指纹的形式列出**本地计算机\\应用商店的 My**目录。  
  
    3.  在同一个 Windows PowerShell 命令窗口中，运行以下命令。  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > 如果这不是第一次运行此命令，添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 前一个命令创建一个 WID 场节点。 如果你想要创建运行 SQL Server 的计算机的一个服务器场节点，则必须已安装且正常运行的 SQL Server 的实例。 可以使用以下命令使用的 SQL Server 实例到现有场添加联合身份验证服务器：`Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`其中**SQL\_主机\_名称**是在其上的服务器的名称的 SQL 实例服务器正在运行，并**SQL\_实例\_名称**是 SQL Server 实例的名称。 如果使用 SQL Server 的默认实例，使用**SQLConnectionString**的值"**数据源\=< SQL\_主机\_名称 >; Integrated Security\=，则返回 True**".  
  
        > [!IMPORTANT]  
        > 如果你想要创建的 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="see-also"></a>请参阅 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

