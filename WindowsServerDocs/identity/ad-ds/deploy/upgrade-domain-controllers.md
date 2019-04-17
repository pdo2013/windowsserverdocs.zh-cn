---
title: "升级到 Windows Server 2016 的域控制器"
description: "本文介绍了如何从 Windows Server 2012 R2 升级到 Windows Server 2016"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 187972c2f44a4d7f91b1b3ac1c905529564cfa6d
ms.sourcegitcommit: f748c6c4ce700b0787ffdd1fca620c21c4331fd2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>升级到 Windows Server 2016 的域控制器

适用于：Windows Server 2016

本主题提供了有关在 Windows Server 2016 的 Active Directory 域服务的背景信息并介绍升级从 Windows Server 2012 或 Windows Server 2012 R2 域控制器的过程。 

## <a name="pre-requisites"></a>先决条件
推荐的方法，升级域是提升运行较新版本的 Windows Server 和降级较旧的域控制器根据需要的域控制器。 方法是适用于升级了现有的域控制器在操作系统。 此列表涉及之前推广运行较新版本的 Windows Server 的域控制器的常规步骤： 

1.  确认目标服务器满足系统要求。 
2.  验证应用程序兼容性。 
3.  即将移至 Windows Server 2016 的评价建议 
4.  检查安全设置。 有关详细信息，请参阅[建议不再使用功能和行为更改与 Windows Server 2016 中的广告 DS](../../../get-started\deprecated-features.md)。 
5.  从计算机计划运行安装到目标服务器检查连接。 
6.  检查可用性必要操作主机的角色： 
    - 若要安装现有域和森林中运行 Windows Server 2016 的第一个 DC，你在运行安装该计算机需要连接到**架构主机**以便以运行 adprep /domainprep 运行 adprep /forestprep 成功和基础结构母版。 
    - 若要安装已被扩展森林架构某个域中的第一个域控制器，只需连接到的基础结构母版。 
    - 若要安装或在现有的树林中删除某个域，你需要连接到**域名主机**。 
    - 任何域控制器安装还需要连接到**RID 主机。** 
    - 如果您的第一个只读域控制器安装现有的树林中，你将需要为每个应用目录分区，也称为非域命名上下文或 NDNC 连接到主服务器基础结构。 

### <a name="installation-steps-and-required-administrative-levels"></a>安装步骤和需要管理员级别
下表提供的升级的步骤，来完成以下步骤操作的权限要求摘要

|安装操作|凭据要求|
| ----- | ----- |
|安装新森林|目标服务器上的本地管理员|
|在现有的树林中安装新域|企业管理员|
|现有的域中安装其他直流|域管理员|
|运行 adprep /forestprep 成功|方案管理员企业管理员，域管理员|
|运行 adprep /domainprep|域管理员|
|运行 adprep /domainprep /gpprep|域管理员|
|运行 adprep /rodcprep|企业管理员|

在 Windows Server 2016 的新增功能的其他信息，请参阅[在 Windows Server 2016 的新增功能](../../../get-started/what-s-new-in-windows-server-2016.md)。



## <a name="supported-in-place-upgrade-paths"></a>受支持的就地升级路径
运行 64 位版本的 Windows Server 2012 或 Windows Server 2012 R2 域控制器可以升级到 Windows Server 2016。 支持是仅 64 位版本的升级，因为 Windows Server 2016 仅随附 64 位版本。

|如果你运行的此版本：|你可以升级到这些版本：|
| ----- | ----- |   
|Windows Server 2012 标准|Windows Server 2016 标准或 Datacenter|   
|Windows Server 2012 Datacenter|Windows Server 2016 数据中心| 
|Windows Server 2012 R2 Standard|Windows Server 2016 标准或 Datacenter|    
|Windows Server 2012 R2 Datacenter|Windows Server 2016 数据中心|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 软件包|  
|Windows 存储 Server 2012 标准|Windows 存储 Server 2016 标准| 
|Windows 存储 Server 2012 工作组中|Windows 存储 Server 2016 工作组中|   
|Windows 存储 Server 2012 R2 Standard|Windows 存储 Server 2016 标准|  
|Windows 存储 Server 2012 R2 工作组中|Windows 存储 Server 2016 工作组中|    

有关支持的升级路径的详细信息，请参阅[受支持的升级路径](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep 和域
如果你正在执行的现有的 Windows Server 2016 操作系统域控制器就地升级，你将需要手动运行 adprep /forestprep 成功 adprep /domainprep。  需要运行一次树林中 Adprep /forestprep 成功。  Adprep /domainprep 需要必须要升级到 Windows Server 2016 的域控制器在其中的每个域中运行一次。

如果你要升级你不需要手动运行这些新的 Windows Server 2016 服务器。  这些已集成到 PowerShell 和服务器管理器体验。

运行 adprep 的详细信息，请参阅[运行 Adprep](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>功能功能和给要求
Windows Server 2016 要求 Windows Server 2003 森林功能级别。 也就是说，可以添加到现有的 Active Directory 林运行 Windows Server 2016 的域控制器之前，森林功能级别必须是 Windows Server 2003 或更高版本。 林中包含域控制器在运行 Windows Server 2003 或更高版本功能森林但级别仍是 Windows 2000，还可以阻止安装。 

Windows 2000 域控制器必须之前添加到你的森林 Windows Server 2016 域控制器中删除。 在此情况下，请考虑以下工作流程： 


1. 将运行 Windows Server 2003 或更高版本的域控制器安装。 可以将这些域控制器部署 Windows Server 评估版上。 此步骤还需要运行该操作系统发布的 adprep.exe 的先决条件。 
2.  删除 Windows 2000 域控制器。 具体来说，完美降级或强制地域和使用 Active Directory 用户和计算机中删除所有删除的域控制器域控制器帐户中删除 Windows Server 2000 域控制器。 
3.  提升森林功能级别对 Windows Server 2003 或更高版本。 
4.  安装在运行 Windows Server 2016 的域控制器。 
5.  删除运行较早版本的 Windows Server 的域控制器。 

### <a name="rolling-back-functional-levels"></a>回退功能级别

你为某个特定值设置森林功能级别 (FFL) 后，你不能回退或降低森林功能级别，有以下例外： 

- 如果你从 Windows Server 2012 R2 FFL 升级，你可以返回到 Windows Server 2012 R2 降低它。 
- 如果你从 Windows Server 2008 R2 FFL 升级，你可以返回到 Windows Server 2008 R2 降低它。

你为某个特定值设置域功能级别后，不能回退或降低有以下例外域功能级别： 

- 对 Windows Server 2012 或 Windows Server 2012 R2 时筹集域对 Windows Server 2016 的功能级别森林功能级别是 Windows Server 2012 或更低，如果你有重新推出域功能级别的选项 

有关可以在较低的功能级别的功能的详细信息，请参阅[了解 Active Directory 域服务 (广告 DS) 功能级别](../active-directory-functional-levels.md)。 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>与其他服务器的角色以及 Windows 操作系统的广告 DS 互操作性
广告 DS 不支持在以下 Windows 操作系统上： 


- Windows 多点 Server 
- Windows Server 2016 软件包 

广告 DS 无法安装以下服务器角色或角色服务也运行的服务器上： 

- Microsoft Hyper V Server 2016
- 远程桌面连接经纪人 

## <a name="administration-of-windows-server-2016-servers"></a>Windows Server 2016 服务器管理
使用远程服务器管理工具适用于 Windows 10 管理域控制器和其他运行 Windows Server 2016 的服务器。 你可以运行 Windows 10 的计算机上运行 Windows Server 2016 远程服务器管理工具。 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>升级到 Windows Server 2016 的分步
以下是从 Windows Server 2012 R2 的 Contoso 森林升级到 Windows Server 2016 的一个简单的示例。

![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1.  加入你的森林新的 Windows Server 2016。 在出现提示时，请重新启动。 
![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2.  登录到新的 Windows Server 2016 的域的管理员帐户。
3.  在**服务器管理器**下**添加角色和功能**，安装**Active Directory 域服务**在新的 Windows Server 2016。 在 2012 R2 森林和域，这将自动运行 adprep。
![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4.  在**服务器管理器**单击黄色三角形，，从下拉列表中，单击**提升到某个域控制器服务器**。 
![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5.  在**部署配置**屏幕上，选择**添加到现有的森林域控制器**单击下一步。 
![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6.  在**域控制器选项**屏幕上，输入**目录服务还原模式 (DSRM)**密码，然后单击下一步。 
7.  用于单击屏幕的其余部分**下一步**。 
8.  在**先决条件检查**屏幕上，单击**安装**。 当你完成重启后可以登录重新。
9.  在 Windows Server 2012 R2 服务器上，在**服务器管理器**，在工具中，选择**Active Directory 模块的 Windows PowerShell**。 
![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. 在 PowerShell windows 中使用移动 ADDirectoryServerOperationMasterRole 移动域控制器。 你可以键入每个-OperationMasterRole 的名称，或使用数字指定的角色。 数字。 有关详细信息，请参阅[移动 ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

``` powershell
Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
```

![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. 验证已通过转到 Windows Server 2016 服务器，在移动角色**服务器管理器**下**工具**、 选择**Active Directory 模块的 Windows PowerShell**。 使用`Get-ADDomain`和`Get-ADForest`cmdlet 以查看 FSMO 角色持有者。
![升级] (media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
！[升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. 降级和删除 Windows Server 2012 R2 域控制器。 降级 dc 的信息，请参阅[降级域控制器和域](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. 降级并删除服务器后，您可以提升森林功能和 Windows Server 2016 的域功能级别。


## <a name="next-steps"></a>后续步骤
-   [安装和卸载最近更新 Active Directory 域中服务](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [安装 Active Directory 域服务 & #40;级别 100 & #41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Windows Server 2016 功能级别](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
