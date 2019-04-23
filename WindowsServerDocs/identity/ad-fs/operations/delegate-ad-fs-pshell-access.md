---
title: 委派给非管理员用户的 AD FS Powershell Commandlet 访问权限
description: 此文档 descirbes 如何委托对 AD FS PowerShell cmdlt 的非管理员的权限。
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 265d22b045011e34192e56bdb970955b601cda56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883558"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>委派给非管理员用户的 AD FS Powershell Commandlet 访问权限 
默认情况下，只能通过 AD FS 管理员来通过 PowerShell 的 AD FS 管理。 对于许多大型组织，这可能与其他角色，例如，技术支持人员在处理时不是一种可行的操作模式。  

使用 Just Enough Administration (JEA)，客户现在可以委派特定 commandlet 到另一个人组。  
此用例的一个很好示例允许查询 AD FS 的技术支持人员的帐户锁定状态和用户具有已审查，所以会重置帐户锁定状态，在 AD FS。 在这种情况下，将需要委派的 commandlet 是： 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

本文档其余部分中，我们使用此示例。 但是，一个可以自定义此选项还允许委派设置的信赖方的属性以及将关闭这给组织内的应用程序所有者。  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>创建所需的组授予用户权限所需 
1. 创建[组托管的服务帐户](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。 GMSA 帐户用于允许 JEA 用户作为其他计算机或 web 服务访问网络资源。 它提供了用于对域中任何计算机上的资源进行身份验证的域标识。 GMSA 帐户被授予更高版本中安装必要的管理权限。 对于此示例中，我们调用帐户**gMSAContoso**。 
2. 创建 Active Directory 组可以填入需要授予对委派命令的权限的用户。 在此示例中，技术支持人员授予权限以读取、 更新和重置 ADFS 锁定状态。 我们引用此组在该示例作为整个**JEAContoso**。 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>在 ADFS 服务器上安装的 gMSA 帐户： 
创建具有 ADFS 服务器的管理权限的服务帐户。 这可以是域控制器上或远程执行为 long 类型的值为已安装 AD RSAT 程序包。  必须在同一个林的 ADFS 服务器中创建的服务帐户。 修改您的服务器场的配置的示例值。 

```powershell
 # This command should only be run if this is the first time gMSA accounts are enabled in the forest 
Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))  
 
# Run this on every node that you want to have JEA configured on  
$adfsServer = Get-ADComputer server01.contoso.com  
 
# Run targeted at domain controller  
$serviceaccount = New-ADServiceAccount gMSAcontoso -DNSHostName <FQDN of the domain containing the KDS key> - PrincipalsAllowedToRetrieveManagedPassword $adfsServer –passthru 
 
# Run this on every node 
Add-ADComputerServiceAccount -Identity server01.contoso.com -ServiceAccount $ServiceAccount 
```

在 ADFS 服务器上安装的 gMSA 帐户。  这需要在 ADFS 服务器场中的每个节点上运行。 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>授予 gMSA 帐户管理员权限 
如果服务器场正在使用委派的管理，授予 gMSA 帐户管理员权限将其添加到具有委派的管理员访问权限的现有组。  
 
如果服务器场不使用委派的管理，授予 gMSA 帐户管理员权限通过使其在所有 ADFS 服务器上的本地管理组。 
 
 
### <a name="create-the-jea-role-file"></a>创建 JEA 角色文件 
 
在记事本文件中创建 JEA 角色。 在提供说明以创建角色[JEA 角色功能](https://docs.microsoft.com/powershell/jea/role-capabilities)。 
 
在此示例中委派的 commandlet 将`Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`。 

示例 JEA 角色委派访问权限的重置 ADFSAccountLockout、 Get-ADFSAccountActivity 和组 ADFSAccountActivity commandlet:

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>创建 JEA 会话配置文件 
按照说明进行操作来创建[JEA 会话配置](https://docs.microsoft.com/powershell/jea/session-configurations)文件。 配置文件决定谁可以使用 JEA 终结点，并且他们有权访问哪些功能。 

角色功能由角色功能文件的平面名称 （不带扩展名的文件名） 引用。 如果多个角色功能具有相同的平面名称的系统上可用，PowerShell 将使用其隐式搜索顺序选择有效的角色功能文件。 它并不意味着赋予访问具有相同名称的所有角色功能文件。 

若要指定角色功能文件的路径，请使用`RoleCapabilityFiles`参数。 为子文件夹，JEA 将查找包含的有效 Powershell 模块`RoleCapabilities`子文件夹，其中`RoleCapabilityFiles`应修改参数为`RoleCapabilities`。 

示例会话配置文件： 

```powershell
@{
SchemaVersion = '2.0.0.0'
GUID = '54c8d41b-6425-46ac-a2eb-8c0184d9c6e6'
SessionType = 'RestrictedRemoteServer'
GroupManagedServiceAccount =  'CONTOSO\gMSAcontoso'
RoleDefinitions = @{ JEAcontoso = @{ RoleCapabilityFiles = 'C:\Program Files\WindowsPowershell\Modules\AccountActivityJEA\RoleCapabilities\JEAAccountActivityResetRole.psrc' } }
}
```

保存会话配置文件。 
 
我们强烈建议[测试会话配置文件](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1)如果已手动编辑 pssc 文件使用文本编辑器以确保语法正确无误。 如果会话配置文件未通过此测试，它是不成功的系统上注册。  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>安装 AD FS 服务器上的 JEA 会话配置 

在 AD FS 服务器上安装 JEA 会话配置 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>操作说明 
设置完成后，日志记录和审核 JEA 可用于确定 if 合适的用户有权访问 JEA 终结点。 

若要使用委派的命令： 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 
```
