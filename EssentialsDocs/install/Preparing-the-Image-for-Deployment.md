---
title: "准备部署该映像"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 16411ab073e9417c52592aa9a6b13707dd461537
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="preparing-the-image-for-deployment"></a>准备部署该映像

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

准备图像典型工具是 sysprep.exe。 运行此工具使一般化图像，并关闭服务器，以便重新启动该映像所在的服务器时，将运行初始配置。 运行 sysprep.exe 之前，必须完成到映像中的所有修改。  
  
> [!NOTE]
>  你可以通过使用 sysprep.exe 重置 Windows 产品激活最多三次。  
  
#### <a name="to-prepare-the-image"></a>准备图像  
  
1.  删除你已添加你的 SkipIC.txt？  
  
2.  打开提升了权限的 command prompt 窗口。 单击**开始**，右键单击**权限的命令提示符**，然后选择**以管理员身份运行**。  
  
3.  运行以下命令以重置注册表项，以便用户将具有完整的宽限期内之前服务器变得不兼容。  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  运行以下命令以添加注册表项以显示键、页面语言、区域设置页和 EULA 页面。 默认情况下浏览以下页面不会显示在初始配置过程。 因此，如果你将发布预安装的框中，你需要添加此注册表项。 但是，如果你发布的 DVD，你应该不添加此项，因为这些页面将显示在 WinPE 和初始配置过程。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  如果您的框预键，禁用初始配置关键页面。 主要的页面将仅显示时 ShowPreinstallPages = 真和 KeyPreInstalled！= true。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  运行以下命令以添加注册表项，如果你想要禁用的硬件要求检查。 这是仅针对不符合硬件要求你预装框。 如果将发布 DVD，，或者你框达到硬件要求，建议未添加该键。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  （可选）删除下的日志**%programdata%\ Microsoft \ Windows Server \Logs**。  
  
8.  准备 sysprep 以下模板中所示的无人照看的 xml 文件。  
  
    ```  
    <unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:ms="urn:schemas-microsoft-com:asm.v3">  
  
      <settings pass="oobeSystem">  
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
          <!-- Must have -->  
          <OOBE>  
             <HideEULAPage>true</HideEULAPage>  
          </OOBE>  
          <!-- Must have -->  
          <AutoLogon>   
            <Enabled>true</Enabled>   
            <Username>Administrator</Username>   
            <Domain>.</Domain>   
            <Password>   
              <!--You can set any password you like, but keep it consistent with password settings -->       
              <Value>Admin@123</Value>   
              <PlainText>true</PlainText>   
            </Password>   
          </AutoLogon>   
          <UserAccounts>   
           <AdministratorPassword>   
             <!--You can set any password you like, but keep it consistent with auto logon settings -->       
             <Value>Admin@123</Value>   
             <PlainText>true</PlainText>   
           </AdministratorPassword>   
          </UserAccounts>  
  
          <!-- Optional -->  
          <OEMInformation>  
             <HelpCustomized>true</HelpCustomized>  
             <Manufacturer>OEM name</Manufacturer>  
             <Model>model name</Model>  
             <SupportHours>hours</SupportHours>  
             <SupportPhone>123-456-7890</SupportPhone>  
             <SupportURL>http://www.contoso.com</SupportURL>  
          </OEMInformation>  
  
        </component>  
  
      </settings>  
  
      <settings pass="specialize">  
        <component name="Microsoft-Windows-Shell-Setup" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">  
          <!-- Must have -->  
          <ComputerName>Server</ComputerName>          
          <!-- Optional -->  
          <ProductKey>XXXXX-XXXXX-XXXXX-XXXXX-XXXXX</ProductKey>  
        </component>  
      </settings>  
    </unattend>  
    ```  
  
9. 运行以下命令以获得 sysprep。  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  你还可以作为 sysprep 的参数添加而不是 %系统驱动器 %下 unattend.xml。 如果文件位于下 c:\ 它将涵盖用户的设置，但如果用作的 sysprep 参数，它将不会覆盖范围内用户的设置。 下 %系统驱动器 %unattend.xml 将被删除每次服务器重启。 因此，请确保，创建 unattend.xml %系统驱动器 %下后，该服务器未重新启动。  
  
10. 运行以下命令以添加注册表项，可跳过 Windows OOBE 关键页面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. 运行以下命令以添加注册表项，可跳过 Windows 语言中选择页面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  你必须执行最后两个步骤，否则 Windows OOBE 页面将提出这是由于与初始配置网页和 break 远程管理的服务器方案。  
  
12. 在 sysprep 后关闭框中，您可以捕获映像或重新启动才能继续从客户端计算机初始配置服务器。  
  
> [!IMPORTANT]
>  打算创建服务器恢复媒体的合作伙伴必须捕获映像并创建继续下一步之前恢复媒体。