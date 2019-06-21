---
title: 将域控制器升级到 Windows Server 2016
description: 本文档介绍如何从 Windows Server 2012 R2 升级到 Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 572f923c33739b854808372a826e9c9bbc6aaca3
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280854"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>将域控制器升级到 Windows Server 2016

适用于：Windows Server 2016

本主题提供有关 Windows Server 2016 中的 Active Directory 域服务的背景信息，并解释了从 Windows Server 2012 或 Windows Server 2012 R2 升级域控制器的过程。 

## <a name="pre-requisites"></a>先决条件
升级域的推荐的方法是将提升运行较新版本的 Windows Server 并降级较旧的域控制器根据域控制器。 该方法优于升级现有域控制器的操作系统。 此列表包括要执行之前将提升运行较新版本的 Windows Server 的域控制器的常规步骤： 

1. 验证目标服务器是否满足系统要求。 
2. 验证应用程序兼容性。 
3. 查看移动到 Windows Server 2016 的建议 
4. 验证安全设置。 有关详细信息，请参阅[与 Windows Server 2016 中的 AD DS 有关的弃用功能及行为变化](https://docs.microsoft.com/windows-server/get-started/deprecated-features)。 
5. 从计划运行安装的计算机上检查与目标服务器的连接性。 
6. 检查所需操作主机角色的可用性： 
   - 若要安装在现有域和林中运行 Windows Server 2016 的第一个 DC，运行安装的计算机需要连接到**架构主机**才能运行 adprep/forestprep 和基础结构主机若要运行 adprep /domainprep。 
   - 若要在已扩展林架构的域中安装第一个 DC，则只需连接至结构主机即可。 
   - 若要安装或在现有林中删除域，则需要连接至**域命名主机**。 
   - 任何域控制器安装也需要连接到**RID 主机。** 
   - 如果正在现有林中安装第一个只读域控制器，则需要为每一个应用程序目录分区（也称作非域命名上下文或 NDNC）连接至结构主机。 

### <a name="installation-steps-and-required-administrative-levels"></a>安装步骤和所需的管理级别
下表提供升级步骤以及完成这些步骤的权限要求的摘要

|安装操作|凭据要求|
| ----- | ----- |
|安装一个新林|目标服务器上的本地管理员|
|在现有林中安装一个新域|Enterprise Admins|
|在现有域中安装另一个 DC|Domain Admins|
|运行 adprep /forestprep|Schema Admins、Enterprise Admins 和 Domain Admins|
|运行 adprep /domainprep|Domain Admins|
|运行 adprep /domainprep /gpprep|Domain Admins|
|运行 adprep /rodcprep|Enterprise Admins|

Windows Server 2016 中的新增功能的其他信息，请参阅[什么是 Windows Server 2016 中的新增功能](../../../get-started/what-s-new-in-windows-server-2016.md)。



## <a name="supported-in-place-upgrade-paths"></a>支持的就地升级路径
运行 64 位版本的 Windows Server 2012 或 Windows Server 2012 R2 的域控制器可以升级到 Windows Server 2016。 因为 Windows Server 2016 仅出现在 64 位版本中支持的仅 64 位版本升级。

|如果运行此版本：|可以升级到这些版本：|
| ----- | ----- |   
|Windows Server 2012 Standard|Windows Server 2016 Standard 或 Datacenter|   
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard 或 Datacenter|    
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard| 
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|   
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|  
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|    

有关支持的升级路径的详细信息，请参阅[支持的升级路径](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep 和 Domainprep
如果正在执行的现有域控制器到 Windows Server 2016 操作系统的就地升级，您将需要手动运行 adprep/forestprep 和 adprep /domainprep。  需要在林中只有一次运行 Adprep /forestprep。  需要在其中可以升级到 Windows Server 2016 的域控制器的每个域中一次运行 Adprep /domainprep。

如果你要升级不需要手动运行这些新的 Windows Server 2016 服务器。  这些集成到 PowerShell 和服务器管理器体验。

运行 adprep 的详细信息请参阅[运行 Adprep](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>功能级别的功能和要求
Windows Server 2016 需要 Windows Server 2003 林功能级别。 也就是说，可以添加到现有的 Active Directory 林中运行 Windows Server 2016 的域控制器之前，林功能级别必须是 Windows Server 2003 或更高版本。 如果林包含运行 Windows Server 2003 或更高版本的域控制器，但是域功能级别仍是 Windows 2000，则安装也会被阻止。 

将 Windows Server 2016 域控制器添加到你的林之前，必须删除 Windows 2000 域控制器。 对于这种情况，请考虑下列工作流： 


1. 安装运行 Windows Server 2003 或更高版本的域控制器。 这些域控制器可以部署在 Windows Server 的评估版上。 此步骤还要求运行该操作系统版本的 adprep.exe 的必备组件。 
2.  删除 Windows 2000 域控制器。 具体来说，从域中适当降级或强制性删除 Windows Server 2000 域控制器，并且使用“Active Directory 用户和计算机”，将所有已删除的域控制器的域控制器帐户删除。 
3.  将林功能级别提升到 Windows Server 2003 或更高版本。 
4.  安装运行 Windows Server 2016 的域控制器。 
5.  删除运行 Windows Server 早期版本的域控制器。 

### <a name="rolling-back-functional-levels"></a>回滚功能级别

将林功能级别 (FFL) 设置为某个值后，不能回滚或降低林功能级别，但存在以下例外： 

- 如果要从 Windows Server 2012 R2 FFL 升级，可以将它降级回 Windows Server 2012 R2。 
- 如果要从 Windows Server 2008 R2 FFL 升级，可以将它降级回 Windows Server 2008 R2。

域功能级别设置为某个值之后，不能回滚或降低域功能级别，但存在以下例外： 

- 选择回滚域功能级别提升到 Windows Server 2016 域功能级别和林功能级别为 Windows Server 2012 或更低版本，你必须时回 Windows Server 2012 或 Windows Server 2012 R2 

有关较低功能级别提供的功能的详细信息，请参阅 [了解 Active Directory 域服务 (AD DS) 功能级别](../active-directory-functional-levels.md)。 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>AD DS 与其他服务器角色及 Windows 操作系统的互操作性
下列 Windows 操作系统不支持 AD DS： 


- Windows MultiPoint Server 
- Windows Server 2016 Essentials 

无法将 AD DS 安装在同时运行下列服务器角色或角色服务的服务器上： 

- Microsoft Hyper-V Server 2016
- 远程桌面连接代理 

## <a name="administration-of-windows-server-2016-servers"></a>管理 Windows Server 2016 服务器
使用远程服务器管理工具适用于 Windows 10 的管理域控制器和运行 Windows Server 2016 的其他服务器。 您可以在运行 Windows 10 的计算机上运行 Windows Server 2016 远程服务器管理工具。 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>升级到 Windows Server 2016 的循序渐进指南
以下是从 Windows Server 2012 R2 的 Contoso 林升级到 Windows Server 2016 的一个简单示例。

![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. 将新的 Windows Server 2016 加入到你的林。 出现提示时，请重新启动。 
   ![Upgrade](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2. 登录到新的 Windows Server 2016 使用域管理员帐户。
3. 在中**服务器管理器**下**添加角色和功能**，安装**Active Directory 域服务**新的 Windows Server 2016 上。 在 2012 R2 林和域，这将自动运行 adprep。
   ![Upgrade](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4. 在中**服务器管理器**，单击黄色三角形，然后从下拉列表中，单击**将服务器提升为域控制器**。 
   ![Upgrade](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5. 上**部署配置**屏幕上，选择**的域控制器添加到现有林**和单击下一步。 
   ![Upgrade](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6. 上**域控制器选项**屏幕中，输入**目录服务还原模式 (DSRM)** 密码，然后单击下一步。 
7. 有关屏幕的其余部分，请单击**下一步**。 
8. 上**先决条件检查**屏幕上，单击**安装**。 在重新启动完成后你可以重新登录。
9. 在 Windows Server 2012 R2 服务器上，在**服务器管理器**，在工具，选择**Active Directory 的 Windows PowerShell 模块**。 
   ![Upgrade](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. 在 PowerShell 窗口中使用移动 ADDirectoryServerOperationMasterRole 移动 FSMO 角色。 可以键入的每个-OperationMasterRole 名称或编号，用于指定角色。 有关详细信息请参阅[移动 ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. 验证是否已通过转到 Windows Server 2016 的服务器，在移动角色**服务器管理器**下**工具**，选择**Active Directory 的 Windows PowerShell 模块**。 使用`Get-ADDomain`和`Get-ADForest`cmdlet 查看对 FSMO 角色持有者。
    ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
    ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. 降级和删除 Windows Server 2012 R2 域控制器。 降级的 dc 的信息，请参阅[降级域控制器和域](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. 降级和删除服务器后可以引发的林功能和到 Windows Server 2016 域功能级别。


## <a name="next-steps"></a>后续步骤
-   [Active Directory 域服务安装和删除的新功能](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [安装 Active Directory 域服务&#40;级别 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Windows Server 2016 功能级别](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
