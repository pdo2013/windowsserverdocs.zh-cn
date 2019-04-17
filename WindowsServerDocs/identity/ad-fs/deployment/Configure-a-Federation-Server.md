---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: "Windows Server 2012R2 广告 FS 部署指导"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-federation-server"></a>配置联盟服务器

>适用于：Windows Server 2016，Windows Server 2012 R2

在你的计算机上安装了 Active Directory 联合身份验证服务 \(AD FS\) 角色服务后，你就可以配置变得联合服务器此计算机。 你可以执行以下任一操作：  
  
-   [在新的联合 server 场配置的第一个联盟服务器](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [添加到现有联盟服务器场联合服务器](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>在新的联合 server 场配置的第一个联盟服务器  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>若要使用 Active Directory 联合身份验证服务配置向导新联合身份验证的服务器场在配置的第一个联盟服务器  
  
> [!NOTE]  
> 确保你具有管理员权限的域，或者有域管理员凭据可用之前你执行此过程。  
  
1.  在服务器管理器中**仪表板**页上，单击**通知**标记，，然后单击**配置服务器上的联合身份验证服务**。  
  
    **Active Directory 联合身份验证服务配置向导**打开。  
  
2.  在**欢迎**页上，选择**联合身份验证的服务器场中创建的第一个联盟服务器**，然后单击**下一步**。  
  
3.  在**连接到广告 DS**页面上，通过使用管理员权限的域的 Active Directory \(AD\) 加入的域此计算机是，，然后单击指定帐户**下一步**。  
  
4.  在**指定的服务属性**页上，执行以下操作，然后依次单击**下一步**:  
  
    -   包含安全套接字层 \(SSL\) 证书，之前你已获得的密钥.pfx 文件导入。 在[第 2 步：广告 FS 注册 SSL 证书](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)，你已获取此证书，并将其复制到你想要配置为服务器联合身份验证的计算机上。 若要导入该向导通过.pfx 文件，请单击**导入**，然后浏览到该文件的位置。 当系统提示你.pfx 文件输入密码。  
  
    -   提供联合身份验证服务的名称。 例如，**fs.contoso.com**。该名称必须匹配之一证书中的主题备用名称或主题。  
  
    -   提供的显示名称联合身份验证服务。 例如，**Contoso Corporation**。 用户看到此名称 Active Directory 联合身份验证服务 \(AD FS\) sign\ 上的页面中。  
  
5.  在**指定的服务帐户**页面上，指定的服务的帐户。 你可以创建或使用现有的组托管服务帐户 \(gMSA\) 或使用现有的域的用户帐户。 如果你选择的选项来创建一个新 gMSA 帐户，指定的名称的新帐户。 如果你选择要使用现有 gMSA 或域帐户的选项，请单击**选择**选择一个帐户。  
  
    > [!NOTE]  
    > 使用 gMSA 帐户的优势是它 auto\ 协商密码更新功能。  
  
    > [!WARNING]  
    > 如果你想要使用 gMSA 帐户，你必须至少一个域控制器在你运行的 Windows Server 2012 操作系统的环境中。  
    >   
    > 如果已禁用 gMSA 选项，并且你看到一条错误消息，如**组托管服务帐户不可用，因为尚未设置 KDS 根密钥**，你可以通过运行以下 Windows PowerShell 命令，域控制器，运行 Windows Server 2012 或更高版本，在您的域的 Active Directory 在域中启用 gMSA: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 然后返回到该向导中，单击**上一个**，然后单击**下一步**re\ 到-输入**指定的服务帐户**页面。 现在应已启用 gMSA 选项。 你可以选择它，并输入你想要使用的 gMSA 帐户名称。  
  
6.  在**指定配置数据库**页、指定广告 FS 配置数据库中，，然后单击**下一步**。 您可以创建数据库到此计算机上使用 Windows 内部数据库 \(WID\) 或者你可以指定的位置和 Microsoft SQL server 实例名称。  
  
    有关详细信息，请参阅[广告 FS 配置数据库中的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
    > [!IMPORTANT]  
    > 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，你可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012 并且 SQL Server 2014。  
  
7.  在**回顾选项**页面，验证你的配置选择，然后单击**下一步**。  
  
8.  在**Pre\ 若要检查**页上，确保所有必要检查成功完成后，然后单击**配置**。  
  
9. 在**结果**页上，检查结果并检查是否已成功，完成的配置，然后单击**所需的完成联合身份验证服务部署后续步骤**。 有关详细信息，请参阅[接下来的完成广告 FS 安装步骤](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 单击**关闭**退出向导。  
  
### <a name="BKMK_3"></a>在新的 Windows PowerShell 通过联盟服务器场配置的第一个联盟服务器  
你可以通过使用新的或现有 gMSA 帐户或现有的域用户帐户创建新的联合 server 场。  
  
-   **如果你想要使用新的 gMSA 帐户创建新联合身份验证的服务器，请执行以下操作：**  
  
    > [!IMPORTANT]  
    > 你必须域管理员权限在新的联合 server 场中创建的第一个联合身份验证的服务器。  
  
    1.  你想要配置为服务器联合身份验证的计算机，请确保所需的 SSL 证书，已导入**本地 Computer\\My 官方商城**目录。 你可以采用验证是否由 Windows PowerShell 命令窗口中运行以下命令导入 SSL 证书：`dir Cert:\LocalMachine\My`。 通过在其指纹列出了证书**本地 Computer\\My 官方商城**目录。  
  
    2.  在你域控制器上，打开 Windows PowerShell 命令窗口，然后运行以下命令以确认是否已在你的域中创建 KDS 根密钥：`Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 如果它尚未创建以便输出中显示的任何信息，运行以下命令以创建该密钥：`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。  
  
    3.  你想要配置为服务器联合身份验证的计算机中,，打开 Windows PowerShell 命令窗口中，并运行以下命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > `$`是必需的上一个命令末尾登录。  
  
        若要获得的值`<certificate_thumbprint>`、运行`dir Cert:\LocalMachine\My`，然后选择 SSL 证书的指纹。 值`<federation_service_name>`是您的联合身份验证服务的名称，例如，**fs.contoso.com**。  
  
        > [!NOTE]  
        > 如果这不是第一次运行以下命令，添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 上一个命令中创建 WID 场。 如果你想要创建 SQL Server 服务器电场的日落，则你必须已安装并运行的 SQL Server 实例。  
        >   
        > 你可以使用以下命令以在使用的 SQL Server 实例新场中创建的第一个联合身份验证的服务器：`Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"`位置**< SQL\_Host\_Name >**运行 SQL Server 服务器的名称和**< SQL\_instance\_name >** SQL server 实例名称。 如果你使用默认的 SQL Server 实例，使用**可以**值"**数据 Source\ = < SQL\_Host\_Name >; 集成 Security\ = 真**"。  
  
        > [!IMPORTANT]  
        > 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，您可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012。  
  
-   **如果你想要使用现有的域用户帐户创建新联合身份验证的服务器，请执行以下操作：**  
  
    1.  你想要配置为服务器联合身份验证的计算机，请确保所需的 SSL 证书，已导入**本地 Computer\\My 官方商城**目录。 你可以采用验证是否由 Windows PowerShell 命令窗口中运行以下命令导入 SSL 证书：`dir Cert:\LocalMachine\My`。 通过在其指纹列出了证书**本地 Computer\\My 官方商城**目录。  
  
    2.  你想要配置为服务器联合身份验证的计算机，打开 Windows PowerShell 命令窗口中，并运行以下命令：`$fscred = Get-Credential`。 输入你想要使用联合身份验证服务帐户中的格式域 \\ 名称域用户帐户凭据。  
  
    3.  在同一 Windows PowerShell 命令窗口中，运行以下命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        若要获得的值**< certificate\_thumbprint >**、运行`dir Cert:\LocalMachine\My`，然后选择 SSL 证书的指纹。 值**< federation\_service\_name >**是你联合身份验证服务，例如，fs.contoso.com 的名称。  
  
        > [!NOTE]  
        > 如果这不是第一次运行以下命令，添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 上一个命令中创建 WID 场。 如果你想要创建 SQL Server 电场的日落，则你必须已安装并运行的 SQL Server 实例。  
        >   
        > 你可以使用以下命令以在使用的 SQL Server 实例新场中创建的第一个联合身份验证的服务器：`Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`位置**SQL\_Host\_Name**运行 SQL Server 服务器的名称和**SQL\_instance\_name** SQL server 实例名称。 如果你使用默认的 SQL Server 实例，使用**可以**值"**数据 Source\ = < SQL\_Host\_Name >; 集成 Security\ = 真**"。  
  
        > [!IMPORTANT]  
        > 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，你可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012 并且 SQL Server 2014。  
  
## <a name="BKMK_2"></a>添加到现有联盟服务器场联合服务器  
  
> [!IMPORTANT]  
> 确保你已完成[第 3 步：安装广告 FS 角色服务](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)之前你开始在此部分中的任何步骤。  
  
> [!IMPORTANT]  
> 确保你已获得有效 SSL 服务器身份验证证书之前完成此过程。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>若要添加到现有联盟服务器场通过 Active Directory 联合身份验证服务配置向导联合服务器  
  
1.  在服务器管理器中**仪表板**页上，单击**通知**标记，，然后单击**配置服务器上的联合身份验证服务**。  
  
    **Active Directory 联合身份验证服务配置向导**打开。  
  
2.  在**欢迎**页上，选择**添加到联合身份验证的服务器场联合服务器**，然后单击**下一步**。  
  
3.  在**连接到广告 DS**页面上，通过使用这台计算机加入的广告域，然后单击域管理员权限指定帐户**下一步**。  
  
4.  在**指定电场的日落**页面上，提供中使用 WID 农场里的主要联合服务器名称或指定数据库主机名称并使用 SQL Server 现有联盟服务器场数据库实例名称。  
  
    > [!WARNING]  
    > 在 Windows Server® R2、2012 年没有一种解决方法，若要指定默认 SQL Server 实例。 解决方法是使用用户界面。 改为使用中的步骤[配置新联盟服务器场通过 Windows PowerShell 中的第一个联盟服务器](Configure-a-Federation-Server.md#BKMK_3)。  
  
    > [!IMPORTANT]  
    > 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，您可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012。  
  
5.  在**指定 SSL 证书**页上，导入.pfx 文件，其中包含 SSL 证书和您之前已经获得的键。 此证书已所需的服务身份验证证书。 在[第 2 步：广告 FS 注册 SSL 证书](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)，你已获取此证书，并将其复制到你想要配置为服务器联合身份验证的计算机。 若要导入该向导通过.pfx 文件，请单击**导入**并浏览到该文件的位置。 当系统提示你.pfx 文件输入密码。  
  
6.  在**指定的服务帐户**页面上，指定同一设备在一场中创建的第一个联合身份验证的服务器时的服务帐户。 你可以使用现有组托管服务帐户或现有的域用户帐户。  
  
    > [!IMPORTANT]  
    > 指定你的帐户必须相同帐户的主要联合服务器此一场中使用的帐户。  
  
7.  在**回顾选项**页面，验证你的配置选择，然后单击**下一步**。  
  
8.  在**Pre\ 若要检查**页上，确保所有必要检查成功完成后，然后单击**配置**。  
  
9. 在**结果**页上，检查结果并检查是否已成功，完成的配置，然后单击**所需的完成联合身份验证服务部署后续步骤**。 有关详细信息，请参阅[接下来的完成广告 FS 安装步骤](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 单击**关闭**退出向导。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>若要添加到现有联盟服务器场通过 Windows PowerShell 联合服务器  
通过使用现有 gMSA 帐户或现有的域用户帐户，你可以添加到现有场联合服务器。  
  
-   如果你想要使用现有 gMSA 帐户到场加入联合服务器，请执行以下操作：  
  
    1.  你想要配置为服务器联合身份验证的计算机，请确保所需的 SSL 证书，已导入**本地 Computer\\My 官方商城**目录。 你可以采用验证是否由 Windows PowerShell 命令窗口中运行以下命令导入 SSL 证书：`dir Cert:\LocalMachine\My`。 通过在其指纹列出了证书**本地 Computer\\My 官方商城**目录。  
  
    2.  你想要配置为服务器联合身份验证的计算机，打开 Windows PowerShell 命令窗口中，并运行以下命令。  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` 是你的广告域和 gMSA 该域你名称。 `<first_federation_server_hostname>` 是此现有电场的日落中的主要联合服务器的主名称。  
  
        你可以获得的值`<certificate_thumbprint>`通过运行`dir Cert:\LocalMachine\My`在上一步。  
  
        > [!NOTE]  
        > 如果这不是第一次运行以下命令，添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 上一个命令中创建 WID 电场的日落节点。 如果你想要创建的计算机运行的 SQL Server 服务器电场的日落节点，则你必须已安装并运行的 SQL Server 实例。  
        >   
        > 你可以使用以下命令添加到正在使用的 SQL Server 实例现有场联合服务器：`Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`位置**SQL\_Host\_Name**运行 SQL Server 服务器的名称和**SQL\_instance\_name** SQL server 实例名称。 如果你使用默认的 SQL Server 实例，使用**可以**值"**数据 Source\ = < SQL\_Host\_Name >; 集成 Security\ = 真**"。  
  
        > [!IMPORTANT]  
        > 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，你可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012 并且 SQL Server 2014。  
  
-   如果你想要加入电场的日落使用现有的域用户帐户的联合身份验证的服务器，请执行以下操作：  
  
    1.  你想要配置为服务器联合身份验证的计算机，打开 Windows PowerShellcommand 窗口中，并运行以下命令：`$fscred = get-credential`。 输入你想要使用联合身份验证服务帐户中的格式域 \\ 名称域用户帐户凭据。  
  
    2.  你想要配置为服务器联合身份验证的计算机，请确保所需的 SSL 证书，已导入**本地 Computer\\My 官方商城**目录。 你可以采用验证是否通过运行以下命令，在 Windows PowerShellcommand 窗口中导入 SSL 证书：`dir Cert:\LocalMachine\My`。 通过在其指纹列出了证书**本地 Computer\\My 官方商城**目录。  
  
    3.  在同一 Windows PowerShell 命令窗口中，运行以下命令。  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > 如果这不是第一次运行以下命令，添加`OverwriteConfiguration`参数。  
  
        > [!NOTE]  
        > 上一个命令中创建 WID 电场的日落节点。 如果你想要创建的计算机运行的 SQL Server 服务器电场的日落节点，则你必须已安装并运行的 SQL Server 实例。 你可以使用以下命令以使用 SQL Server 实例添加到现有场联合服务器：`Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`位置**SQL\_Host\_Name**是运行的 SQL Server 实例服务器的名称和**SQL\_instance\_name** SQL server 实例名称。 如果你使用默认的 SQL Server 实例，使用**可以**值"**数据 Source\ = < SQL\_Host\_Name >; 集成 Security\ = 真**"。  
  
        > [!IMPORTANT]  
        > 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，你可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012 并且 SQL Server 2014。  
  
## <a name="see-also"></a>请参阅 

[广告 FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012R2 广告 FS 部署指导](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联盟服务器电场的日落](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

