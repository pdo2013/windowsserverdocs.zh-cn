---
title: 为域部署配置组策略
description: 了解如何在 MultiPoint Services 中设置了组策略
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f661fbdc40fd7dd2562d51756bc7642c8e9a4a82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888038"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>为域部署配置组策略
若要确保你的 MultiPoint 服务的域部署可以正常运行，应用到 MultiPoint 服务系统上的 WMSshell 用户帐户的以下组策略设置。  
  
> [!IMPORTANT]  
> 某些组策略设置可以防止应用到 MultiPoint 服务所需的配置设置。 请务必了解并定义你的组策略设置，以便它们在 MultiPoint 服务上正常工作。 例如，一组策略设置来阻止自动登录可能会存在问题的 MultiPoint 服务登录行为。  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>更新 WMSshell 用户帐户的组策略 
WMSshell 用户帐户是系统帐户的 MultiPoint services 用于登录到控制台 actuall 工作站的创建位置。 此帐户不是解释由 MultiPoint 管理器。
  
> [!NOTE]  
> 若要了解如何更新组策略，请参阅[本地组策略编辑器](https://technet.microsoft.com/library/dn265982.aspx)。  
  
**策略：** 用户配置 > 管理模板 > 控制面板 >**个性化设置**  
  
将分配以下值：  
  
|设置|值|  
|-----------|----------|  
|启用屏幕保护程序|Disabled|  
|屏幕保护程序超时|Disabled<br /><br />秒： xxx|  
|密码保护的屏幕保护程序|Disabled|  
  
**策略：** 计算机配置 > Windows 设置 > 安全设置 > 本地策略 > 用户权限分配 >**允许本地登录**  
  
|设置|值|  
|-----------|----------|  
|允许本地登录|请确保帐户的列表包括 WMSshell 帐户。<br /><br />**注意：** 默认情况下，WMSshell 帐户是用户组的成员。 如果用户组在列表中，且 WMSshell 是用户组的成员，您不需要将 WMSshell 帐户添加到列表。|  
  
> [!IMPORTANT]  
> 当你设置的任何组策略时，请确保策略不会影响使用自动更新和错误的 Windows 错误报告的多点服务器上。 设置这些**自动安装更新**并**自动 Windows 错误报告**MultiPoint 中配置的 Windows MultiPoint Server 安装过程中所选择的设置使用管理器**编辑服务器设置**，或在计划的更新为磁盘保护配置。  
  
## <a name="update-the-registry"></a>更新注册表  
对于域部署的 MultiPoint 服务时，应更新以下注册表子项。  
  
> [!IMPORTANT]  
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>若要更新域部署 MultiPoint 服务的注册表子项  
  
1.  打开注册表编辑器。 (在命令提示符下键入**regedit.exe**，然后按 ENTER。)  
  
2.  在左窗格中，找到并选择以下注册表子项：  
  
    HKEY_USERS\<SIDofWMSshell>\Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    其中<SIDofWMSshell>是 WMSshell 帐户的安全标识符 (SID)。 若要了解如何识别 SID，请参阅[如何将用户名相关联的安全标识符 (SID)](https://support.microsoft.com/kb/154599)。  
  
3.  在右侧列表中，更新下列子项。  
  
    |子项|值名称|“数值数据”|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 （零）|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 （零）|  
  
    若要更新注册表子项：  
  
    1.  通过在左窗格中选择的注册表项，右键单击该子项在右窗格中，然后依次**修改**。  
  
    2.  在编辑字符串对话框中，键入中的新值**数值数据**，然后单击**确定**。  
  
4.  在完成更新注册表子项后，重新启动计算机以使更改生效。 