---
title: 准备要部署的映像
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838528"
---
# <a name="preparing-the-image-for-deployment"></a>准备要部署的映像

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

用于准备映像的一个典型工具是 sysprep.exe。 运行此工具可通用化映像并关闭服务器，从而当包含此映像的服务器重新启动时将运行初始配置。 运行 sysprep.exe 之前，必须完成所有的映像修改。  
  
> [!NOTE]
>  你最多可使用 sysprep.exe 命令对 Windows 产品激活重设三次。  
  
#### <a name="to-prepare-the-image"></a>准备映像  
  
1.  删除添加的 SkipIC.txt。  
  
2.  打开提升的命令提示符窗口。 单击 **“开始”**，右键单击 **“命令提示符”**，然后选择 **“以管理员身份运行”**。  
  
3.  运行以下命令重设注册表项，以使用户在服务器出现故障之前拥有足够的宽限期。  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  运行以下命令来添加注册表项以显示密钥、语言页面、区域设置页面和 EULA 页面。 默认情况下，在初始配置过程中不会显示这些页面。 因此，如果发布预安装框，则需要添加此注册表项。 然而，如果发布 DVD，由于将在 WinPE 及初始配置过程中显示这些页面，则不应添加此项。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  如果你的框是预先输入密钥的，则会禁用初始配置密钥页面。 仅当 ShowPreinstallPages = true 且 KeyPreInstalled != true 时，才会显示密钥页面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  如果要禁用硬件要求检查，请运行以下命令来添加注册表项。 这仅用于不满足硬件要求的预安装框。 如果发布 DVD 或是你的框满足硬件要求，则建议不添加此项。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  （可选）删除 **%programdata%\Microsoft\Windows Server\Logs** 下的日志。  
  
8.  为以下模板中显示的 sysprep 准备无人参与的 xml 文件。  
  
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
  
9. 为 sysprep 运行以下命令。  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  你也可以在 %systemdrive% 下添加 unattend.xml，而不是将其作为 sysprep 的参数。 如果该文件位于 c:\将介绍由用户 s 的设置，但如果使用的 sysprep 参数，它将不会转换由用户 s 的设置。 服务器每次重新启动时，将会删除 %systemdrive% 下的 unattend.xml。 因此，请确保在 %systemdrive% 下创建 unattend.xml 后，服务器没有重新启动。  
  
10. 运行以下命令添加注册表项以跳过 Windows OOBE 项页面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. 运行以下命令添加注册表项以跳过 Windows 语言选择页面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  必须执行最后 2 个步骤，否则 Windows OOBE 页面会出现，这是初始配置页面所带来的，会远程破坏管理的服务器方案。  
  
12. 在 sysprep 之后关闭框，可从客户端计算机捕获映像或重新启动服务器以继续执行初始配置。  
  
> [!IMPORTANT]
>  对于计划创建服务器恢复介质的合作伙伴，必须在继续进行下一步之前捕获映像并创建恢复介质。