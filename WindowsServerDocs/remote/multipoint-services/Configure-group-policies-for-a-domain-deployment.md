---
title: 为域部署配置组策略
description: 了解如何在 MultiPoint Services 中设置组策略
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 5ac6524289d231d152e366d2ba750a59d27ce14f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395519"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>为域部署配置组策略
若要确保 MultiPoint 服务的域部署正常工作, 请对 MultiPoint 服务系统上的 WMSshell 用户帐户应用以下组策略设置。  
  
> [!IMPORTANT]  
> 某些组策略设置可能会阻止所需的配置设置应用到 MultiPoint 服务。 请确保了解和定义组策略设置, 使其能够在 MultiPoint 服务上正常工作。 例如, 防止自动登录的组策略设置可能会出现 MultiPoint Services 登录行为的问题。  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>更新 WMSshell 用户帐户的组策略 
WMSshell 用户帐户是 MultiPoint 服务用来登录到控制台的系统帐户, 在该帐户中创建了实际的工作站。 此帐户不应由 MultiPoint 管理器管理。
  
> [!NOTE]  
> 若要了解如何更新组策略, 请参阅[本地组策略编辑器](https://technet.microsoft.com/library/dn265982.aspx)。  
  
**政策**用户配置 > 管理模板 > 控制面板 >**个性化**  
  
分配下列值:  
  
|设置|值|  
|-----------|----------|  
|启用屏幕保护程序|Disabled|  
|屏幕保护程序超时|Disabled<br /><br />秒: xxx|  
|密码保护屏幕保护程序|Disabled|  
  
**政策**计算机配置 > Windows 设置 > 安全设置 > 本地策略 > 用户权限分配 >**允许本地登录**  
  
|设置|值|  
|-----------|----------|  
|允许本地登录|确保帐户列表包含 WMSshell 帐户。<br /><br />**注意：** 默认情况下, WMSshell 帐户是用户组的成员。 如果用户组在列表中, 并且 WMSshell 是用户组的成员, 则无需将 WMSshell 帐户添加到列表。|  
  
> [!IMPORTANT]  
> 设置任何组策略时, 请确保这些策略不会影响在 MultiPoint 服务器上的自动更新和错误 Windows 错误报告。 这些设置由 "**自动安装更新**" 和 "**自动 Windows 错误报告**设置, 这些设置是在使用 Windows multipoint server 安装期间选择的, 在 multipoint 管理器中使用"**编辑服务器设置**"进行配置, 或在磁盘保护的计划更新中进行配置。  
  
## <a name="update-the-registry"></a>更新注册表  
对于 MultiPoint 服务的域部署, 你应该更新以下注册表子项。  
  
> [!IMPORTANT]  
> 不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>更新 MultiPoint 服务的域部署的注册表子项  
  
1.  打开注册表编辑器。 (在命令提示符下, 键入**regedit.exe**, 然后按 enter。)  
  
2.  在左窗格中, 找到并选择以下注册表子项:  
  
    HKEY_USERS @ no__t-0SIDofWMSshell > \Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    其中 "<SIDofWMSshell>" 是 WMSshell 帐户的安全标识符 (SID)。 若要了解如何识别 SID, 请参阅[如何将用户名与安全标识符 (SID) 相关联](https://support.microsoft.com/kb/154599)。  
  
3.  在右侧的列表中, 更新以下子项。  
  
    |子项|值名称|“数值数据”|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (零)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (零)|  
  
    更新注册表子项:  
  
    1.  在左窗格中选择注册表项后, 在右窗格中右键单击子项, 然后单击 "**修改**"。  
  
    2.  在 "编辑字符串" 对话框中, 在 "**数值数据**" 中键入新值, 然后单击 **"确定"** 。  
  
4.  完成更新注册表子项后, 请重新启动计算机以激活更改。 
