---
title: 安装和配置 Windows Server Essentials 或 Windows Server Essentials 体验
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: f5593c21b99f4f8cb22979d5dc201a38e54be84c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433486"
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>安装和配置 Windows Server Essentials 或 Windows Server Essentials 体验

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

Windows Server Essentials 是理想的首要服务器具有最多 25 个用户和 50 台设备的小型企业。 对于具有最多 100 个用户和 200 台设备的组织，你现在可以使用 Windows Server 2012 R2 安装了 Windows Server Essentials 体验角色。 本主题将介绍这两种方案。  
  
Windows Server Essentials 体验是使您能够充分利用 （如远程 Web 访问和电脑备份），可供您在 Windows Server Essentials 中而不锁定和限制中强制实施的所有功能的 Windows Server 2016 中的角色 Windows Server Essentials。 此服务器角色还在 Windows Server Essentials 中可用，并且默认情况下启用。
  
在安装 Windows Server Essentials 或 Essentials 体验角色之前，请注意以下限制。  
  
|Windows Server Essentials 中的 Windows Server Essentials 体验|Windows Server 2016 中的 Windows Server Essentials 体验
|----|----|
|-必须是位于林和域中，根的域控制器，并且必须保留所有 FSMO 角色。<br /><br /> -不能具有预先存在的 Active Directory 域 （但是，不存在用于执行迁移的 21 天的宽限期） 的环境中安装。|-无需成为域控制器，如果安装在具有预先存在的 Active Directory 域的环境。<br /><br /> -如果 Active Directory 域不存在，安装角色将创建的 Active Directory 域，并且服务器将成为根处的林和域，保留所有 FSMO 角色的域控制器。  
|只能部署到单个域中。|只能部署到单个域中。  
|只读域控制器不能存在于域中。|只读域控制器不能存在于域中。

> [!NOTE]
>  若要下载评估版本的操作系统，请访问 TechNet 评估中心：  
>   
>  [下载 Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [下载 Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>安装选项  
 本文档提供有关安装和配置 Windows Server Essentials 的分步说明。 根据你的网络环境，可以向你提供以下安装选项：  
  
-    Windows Server Essentials （与默认情况下启用的 Windows Server Essentials 体验角色）  
  
-    安装了 Windows Server Essentials 体验角色的 Windows Server 2016  
 
|部署环境|描述|相关部分|  
|----------------------------|-----------------|---------------------|  
|新的 Active Directory 环境|可以安装 Windows Server Essentials 来创建新的 Active Directory 环境。|[部署 Windows Server Essentials 来设置新的 Active Directory 环境](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|现有 Active Directory 环境|可以在现有 Active Directory 环境中安装 Windows Server Essentials。|[在现有 Active Directory 环境中部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|虚拟环境|可以将 Windows Server Essentials 部署为虚拟机。|[虚拟化环境](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|自动部署|可以使用 Windows PowerShell 自动部署 Windows Server Essentials。|[安装和配置使用 Windows PowerShell 的 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>开始之前  
 在开始安装之前，请查看以下文档：  
  
-   [Windows Server Essentials 产品概述](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Windows Server Essentials 的系统要求](../get-started/system-requirements.md)   

  
##  <a name="BKMK_NewAD"></a> 部署 Windows Server Essentials 来设置新的 Active Directory 环境  
 Windows Server Essentials 提供了一种方法，使你可以快速设置 Active Directory 环境和相关的服务器功能。  
  
###  <a name="BKMK_WSEDeploy"></a> 部署 Windows Server Essentials  
 如果使用的 Windows Server Essentials，已启用 Windows Server Essentials 体验。 但是，必须完成一些步骤才能配置你的服务器。  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>若要在物理服务器上配置 Windows Server Essentials  
  
1. 在 Windows“欢迎使用”  页之后，“配置 Windows Server Essentials 向导”  将在桌面上可见。  
  
2. 按照说明完成向导，如下所示：  
  
   1.  在“配置 Windows Server Essentials”  页上，单击“下一步”  。  
  
   2.  在“时间设置”  中，确保日期、时间和时区均正确，然后单击“下一步”  。  
  
   3.  在中**公司信息**，键入你的公司名称，如**Contoso，Ltd.** ，然后单击**下一步**。 （可选）可以更改内部域名和服务器名称。  
  
   4.  在“创建网络管理员”  中，键入新的管理员帐户名称和密码。  
  
       > [!NOTE]
       >  不要使用默认“管理员”  帐户名称和密码。  
  
   5.  单击 **“配置”** 。  
  
3. 服务器在配置过程中将多次重新启动，并且在完成配置前，将会自动进行登录。 此过程需要大约 20 分钟的时间。  
  
4. 在桌面上，单击仪表板图标以启动服务器仪表板。 在“主页”  上，完成“安装”  选项卡上列出的“入门”  任务。  
  
   完成服务器配置后，运行 Windows Server Essentials 的服务器将被设置为域控制器。  
  
###  <a name="BKMK_DeployWSERole"></a> 部署 Windows Server 2012 R2 Standard 和 Datacenter 中的 Windows Server Essentials 体验角色  
 可以使用服务器管理器来启用和配置 Windows Server Essentials 体验角色在 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 中使用以下过程。  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>部署 Windows Server 2012 R2 中的 Windows Server Essentials 体验角色  
  
1.  以本地管理员身份登录到服务器。  
  
2.  打开“服务器管理器”  ，然后单击“添加角色和功能”  。  
  
3.  在“选择服务器角色”  中，选择“Windows Server Essentials Experience”  角色。 在对话框中，单击“添加功能”  ，然后单击“下一步”  。  
  
4.  在“功能”  中，单击“下一步”  。  
  
5.  查看“Windows Server Essentials Experience”  角色说明，然后单击“下一步”  。  
  
6.  在后续页面中，单击“下一步”  ，然后在配置页上，单击“安装”  。  
  
7.  安装完成后，应作为服务器角色在服务器管理器列出 Windows Server Essentials 体验。  
  
8.  在服务器管理器中的标志通知区域内，单击标志，然后单击“配置 Windows Server Essentials”  。  
  
9. （可选）更改服务器名称（如果需要）。  
  
    > [!IMPORTANT]
    >  配置 Windows Server Essentials 之后，无法再更改服务器名称。  
  

10. 按照向导中所述配置 Windows Server Essentials[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)部分。  

10. 按照向导中所述配置 Windows Server Essentials[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)部分。  

  
##  <a name="BKMK_ExistingAD"></a> 在现有 Active Directory 环境中部署 Windows Server Essentials  
 如果组织已经具有现有 Active Directory 环境，也可以部署 Windows Server Essentials。 此外，可以选择是否要将 Windows Server Essentials 部署为域控制器。  
  
> [!IMPORTANT]
>  此选项才可用部署 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 中的 Windows Server Essentials 体验角色。  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>在现有 Active Directory 环境中部署 Windows Server Essentials  
  
1.  （可选）更改服务器名称（如果需要）。  
  
    > [!IMPORTANT]
    >  配置 Windows Server Essentials 之后，无法再更改服务器名称。  
  
2.  将运行 Windows Server Essentials 的服务器加入到现有域中，如下所示：  
  
    1.  如果希望将服务器成为域控制器，则将该服务器设置为副本域控制器。  
  
    2.  如果不希望此服务器成为域控制器，则通过使用 Windows 本机工具将服务器加入域。  
  
3.  重新启动服务器并以域管理员身份登录到服务器。  
  
4.  打开服务器管理器，然后单击“添加角色和功能”  。  
  
5.  在后续页面中，单击“下一步”  。  
  
6.  在“选择服务器角色”  中，选择“Windows Server Essentials Experience”  。 在对话框中，单击“添加功能”  ，然后单击“下一步”  。  
  
7.  在“功能”  中，单击“下一步”  。  
  
8.  查看“Windows Server Essentials Experience”  说明，然后单击“下一步”  。  
  
9. 在后续页面中，单击“下一步”  ，然后在配置页上，单击“安装”  。  
  
10. 安装完成后，将作为服务器角色在服务器管理器列出 Windows Server Essentials 体验。  
  
11. 在“服务器管理器”  中的标志通知区域内，单击标志，然后单击“配置 Windows Server Essentials”  。  
  
12. 按照向导来配置 Windows Server Essentials。 根据 Active Directory 配置，将会通知你是否要将 Windows Server Essentials 配置在域控制器上或配置为域成员。 单击“配置”  以开始进行配置。 完成配置过程需要大约 10 分钟的时间。  
  
##  <a name="BKMK_VirtualWSE"></a> 虚拟化环境  
  可以作为虚拟机运行 Windows Server Essentials、 Windows Server 2012 R2 Standard 和 Windows Server 2012 R2 Datacenter。 可通过使用 Hyper-V 管理工具在运行 Hyper-V 的服务器上运行虚拟机。 从授权的角度来看，Windows Server Essentials 允许你设置 HYPER-V 角色和虚拟化您的环境。 该许可证允许你设置另一个来宾操作系统运行 Windows Server Essentials。 具体取决于系统提供程序"，配置的不同，Windows Server Essentials 可以无缝地设置虚拟化环境。  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>将 Windows Server Essentials 部署为虚拟机  
  
1.  在 Windows 欢迎页面 （具体取决于您的系统提供程序"，的配置） 之后,**在开始之前**页提供了一个选项来设置 Windows Server Essentials 作为虚拟实例或物理硬件上。 这些选项的可用性由系统提供程序进行预定义，并且这两个选项可能不是始终都可用的。 若要在作为虚拟机，安装 Windows Server Essentials**安装 Windows Server Essentials**，选择**作为虚拟实例安装**，然后单击**配置**。  
  
2.  该向导将自动设置虚拟机，大约需要五分钟。  
  

3.  接下来，配置 Windows Server Essentials 中所述[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)部分。  

3.  接下来，配置 Windows Server Essentials 中所述[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)部分。  

  
##  <a name="BKMK_PowerShell"></a> 安装和配置使用 Windows PowerShell 的 Windows Server Essentials  
 可以使用 Windows PowerShell cmdlets 自动安装 Windows Server Essentials。  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>使用 Windows PowerShell 安装 Windows Server Essentials  
  
1.  从提升的命令提示符中打开 Windows PowerShell 控制台。  
  
2.  使用以下命令安装 Windows Server Essentials 体验角色：  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  运行 `Get-Help Start-WssConfigurationService`。  
  
    1.  运行以下命令以启动配置，从而将 Windows Server Essentials 设置为域控制器：  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  运行以下命令以启动配置，从而将 Windows Server Essentials 设置为现有域成员。 你必须是 Active Directory 中的 Enterprise Admin 组和 Domain Admin 组的成员，才能执行此任务：  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  通过使用以下命令来监视安装进度：  
  
    -   若要在进度栏上显示安装状态，请运行 `Get-WssConfigurationStatus  œShowProgress`。  
  
    -   若要在没有进度栏的情况下获取即时进度，请运行 `Get-WssConfigurationStatus`。  
  
## <a name="see-also"></a>请参阅  
  
-   [什么是 Windows Server Essentials 中的新增功能](../get-started/what-s-new.md)  
  
-   [安装 Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [开始使用 Windows Server Essentials](../get-started/get-started.md)
