---
title: "安装和配置 Windows Server Essentials 或 Windows Server Essentials 体验"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8a2310b178663c6ca32a4e07d11656f1aaf2a11b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>安装和配置 Windows Server Essentials 或 Windows Server Essentials 体验

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

Windows ServerEssentials 是第一个理想 25 最多的用户和 50 设备小型企业服务器。 对于高达 100 用户和 200 设备，你现在可以使用 Windows Server 2012 R2 安装了 Windows Server Essentials 体验角色。 本主题介绍了这两种情况。  
  
Windows Server 软件包体验是在使您可以利用（如远程访问 Web 和电脑备份），可供您在 Windows Server Essentials 不显示锁和强制执行 Windows Server Essentials 中的限制中的所有功能的 Windows Server 2016 中的角色。 此服务器角色也适用于 Windows Server Essentials 并且默认启用。
  
Windows Server Essentials 或 Essentials 体验角色安装之前，请注意以下限制。  
  
|Windows Server 在 Windows Server Essentials essentials 体验|Windows Server 在 Windows Server 2016 essentials 体验
|----|----|
|-必须根部森林和域，域控制器按住必须所有域控制器。<br /><br /> -无法安装与以前存在的 Active Directory 域（但是，并执行迁移 21 天的宽限期内）的环境中。|-没有安装与以前存在的 Active Directory 域的环境中为域控制器。<br /><br /> -如果不存在的 Active Directory 域，安装角色将创建一个 Active Directory 域中，并服务器将成为根处的森林和域中，按所有域控制器域控制器。  
|仅部署到单个域。|仅部署到单个域。  
|仅阅读域控制器不能在域中存在。|仅阅读域控制器不能在域中存在。

> [!NOTE]
>  若要下载的操作系统评估版本，请访问 TechNet 评估中心：  
>   
>  [下载 Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [下载 Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>安装选项  
 本文提供了有关安装和配置 Windows Server Essentials 的分步说明进行操作。 根据网络环境中，你有可供您以下安装选项：  
  
-    Windows Server（与 Windows Server Essentials 体验角色默认情况下启用）软件包  
  
-    安装了 Windows Server Essentials 体验角色与 Windows Server 2016  
 
|部署环境|描述|相关的部分。|  
|----------------------------|-----------------|---------------------|  
|新的 Active Directory 环境|你可以安装的 Windows Server Essentials 创建新的 Active Directory 环境。|[部署 Windows Server Essentials 设置新的 Active Directory 环境](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|现有的 Active Directory 环境|你可以在现有的 Active Directory 环境中安装了 Windows Server Essentials。|[部署 Windows Server Essentials 现有的 Active Directory 环境中](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|虚拟环境|你可以为虚拟机部署 Windows Server Essentials。|[虚拟化你环境](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|自动的部署|你可以通过使用 Windows PowerShell 自动化部署 Windows Server Essentials。|[安装和配置 Windows Server Essentials 通过使用 Windows PowerShell](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>在开始之前  
 在开始安装之前，请查看下面的文档：  
  
-   [Windows Server 软件包产品概述](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Windows Server Essentials 系统要求](../get-started/system-requirements.md)   

  
##  <a name="BKMK_NewAD"></a>部署 Windows Server Essentials 设置新的 Active Directory 环境  
 Windows Server 软件包提供快速设置 Active Directory 环境和服务器相关的功能的方法。  
  
###  <a name="BKMK_WSEDeploy"></a>部署 Windows Server Essentials  
 如果你使用的 Windows Server Essentials，Windows Server Essentials 体验已启用。 但是，你必须完成配置服务器一些步骤。  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>若要配置 Windows Server Essentials 物理服务器上  
  
1.  Windows 后**欢迎**页上，**配置 Windows Server Essentials 向导**桌面上可见。  
  
2.  按照说明来完成向导，如下所示：  
  
    1.  在**配置 Windows Server Essentials**页上，单击**下一步**。  
  
    2.  在**时间设置**，请确保你的日期、时间和时区是正确的，然后单击**下一步**。  
  
    3.  在**公司信息**，键入你的公司名称，如**Contoso，Ltd.**，然后单击**下一步**。 （可选）你还可以更改的内部域名和服务器名称。  
  
    4.  在**创建网络管理员**，键入新管理员帐户名称和密码。  
  
        > [!NOTE]
        >  不要使用默认值**管理员**帐户名称和密码。  
  
    5.  单击**配置**。  
  
3.  配置过程，服务器将重启多次，并且配置完成前，将自动你登录。 此过程需要大约 20 分钟。  
  
4.  在桌面上，请单击仪表板图标以启动服务器仪表板。 在**家庭**页上，完成**入门**上列出的任务**设置**选项卡。  
  
 你已经完成了服务器配置后，运行的 Windows Server Essentials 服务器将设置为域控制器。  
  
###  <a name="BKMK_DeployWSERole"></a>部署 Windows Server 2012 R2 Standard 和数据中心中的 Windows Server Essentials 体验角色  
 你可以使用 Server 管理器中启用并配置 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 中的 Windows Server Essentials 体验角色，通过以下步骤。  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>部署 Windows Server Essentials 体验角色，在 Windows Server 2012 R2  
  
1.  本地管理员身份登录到你的服务器上。  
  
2.  打开**服务器管理器**，然后单击**添加角色和功能**。  
  
3.  在**选择服务器角色**、选择**Windows Server Essentials 体验**角色。 在对话框中，单击**添加功能**，然后单击**下一步**。  
  
4.  在**功能**，单击**下一步**。  
  
5.  查看**Windows Server Essentials 体验**角色描述，然后单击**下一步**。  
  
6.  在页面，请按照，单击**下一步**，然后在确认页中，单击**安装**。  
  
7.  安装完毕后，应服务器角色中服务器管理器中列为 Windows Server Essentials 体验。  
  
8.  在标志通知区域中服务器管理器中，单击标记，，然后单击**配置 Windows Server Essentials**。  
  
9. （可选）如果需要，更改服务器的名称。  
  
    > [!IMPORTANT]
    >  配置 Windows Server Essentials 之后，你无法更改服务器的名称。  
  

10. 按照该向导配置 Windows Server Essentials 之前中所述[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)部分。  

10. 按照该向导配置 Windows Server Essentials 之前中所述[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)部分。  

  
##  <a name="BKMK_ExistingAD"></a>部署 Windows Server Essentials 现有的 Active Directory 环境中  
 如果你的组织已经有现有的 Active Directory 环境，还可以部署 Windows Server Essentials。 此外，你可以选择是否要为域控制器部署 Windows Server Essentials。  
  
> [!IMPORTANT]
>  此选项才可用如果部署 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 数据中心中的 Windows Server Essentials 体验角色。  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>部署 Windows Server Essentials 现有的 Active Directory 环境中  
  
1.  （可选）如果需要，更改服务器的名称。  
  
    > [!IMPORTANT]
    >  配置 Windows Server Essentials 之后，你无法更改服务器的名称。  
  
2.  加入你运行的服务器 Windows Server Essentials 到现有域，如下所示：  
  
    1.  如果你希望该服务器域控制器，设置为副本域控制器服务器。  
  
    2.  如果你不希望该服务器域控制器，加入该服务器你域通过使用 Windows 的原始工具。  
  
3.  重新启动你的服务器，并登录到作为域管理员服务器上。  
  
4.  打开服务器管理器，然后单击**添加角色和功能**。  
  
5.  按照，请单击页面上**下一步**。  
  
6.  在**选择服务器角色**、选择**Windows Server Essentials 体验**。 在对话框中，单击**添加功能**，然后单击**下一步**。  
  
7.  在**功能**，单击**下一步**。  
  
8.  查看**Windows Server Essentials 体验**说明，，然后单击**下一步**。  
  
9. 在页面，请按照，单击**下一步**，然后在确认页中，单击**安装**。  
  
10. 安装完毕后，Windows Server Essentials 体验将列为服务器角色中服务器管理器。  
  
11. 在通知区域中的标志**服务器管理器**、 单击标志，，然后单击**配置 Windows Server Essentials**。  
  
12. 按照该向导配置 Windows Server Essentials。 根据你的 Active Directory 配置，将通知是否要配置 Windows Server Essentials 域控制器上或域成员。 单击**配置**开始进行配置。 配置过程需要大约 10 分钟的时间才能完成。  
  
##  <a name="BKMK_VirtualWSE"></a>虚拟化你环境  
  Windows Server 软件包、Windows Server 2012 R2 Standard 和 Windows Server 2012 R2 Datacenter 可以作为虚拟机运行。 你可以使用运行的 Hyper-V 服务器上的 Hyper-V 管理工具运行虚拟机。 从授权的角度而言，Windows Server Essentials 允许你设置的 Hyper-V 角色和虚拟化你环境。 许可允许你设置另一台来宾操作系统的运行的 Windows Server Essentials。 根据你的系统提供商"美分的配置中，Windows Server Essentials 使您无缝建立虚拟化环境。  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>若要为虚拟机部署 Windows Server Essentials  
  
1.  在欢迎使用 Windows 页面（具体取决于您的系统提供商"美分的配置）之后,**在开始之前**页面将提供选项来设置 Windows Server Essentials 虚拟实例或在物理硬件上。 系统提供商预定义的可用性这些选项，这两个选项可能始终不可用。 来在虚拟机，以安装 Windows Server Essentials**安装 Windows Server Essentials**，选择**安装作为虚拟实例**，然后单击**配置**。  
  
2.  该向导将配置虚拟机的大约 5 分钟。  
  

3.  接下来，在之前所述配置 Windows Server Essentials[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)部分。  

3.  接下来，在之前所述配置 Windows Server Essentials[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)部分。  

  
##  <a name="BKMK_PowerShell"></a>安装和配置 Windows Server Essentials 通过使用 Windows PowerShell  
 你可以通过使用 Windows PowerShell cmdlet 自动 Windows Server Essentials 安装。  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>若要使用的 Windows PowerShell 安装 Windows Server Essentials  
  
1.  从提升了权限的命令提示符下，打开 Windows PowerShell 主机。  
  
2.  使用以下命令以安装 Windows Server Essentials 体验角色：  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  运行`Get-Help Start-WssConfigurationService`。  
  
    1.  运行以下命令以启动配置为域控制器设置 Windows Server Essentials:  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  运行以下命令以启动以设置 Windows Server Essentials 作为现有域成员的配置。 你必须是管理员企业组和域管理员组 Active Directory 中的执行此任务中的成员：  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  监视安装的进度，使用以下命令：  
  
    -   若要安装进度栏上显示的状态，运行`Get-WssConfigurationStatus  œShowProgress`。  
  
    -   若要获取不进度栏的情况下即时进度，运行`Get-WssConfigurationStatus`。  
  
## <a name="see-also"></a>请参阅  
  
-   [什么是 Windows Server Essentials 中的新增功能](../get-started/what-s-new.md)  
  
-   [安装 Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [Windows Server Essentials 入门](../get-started/get-started.md)
