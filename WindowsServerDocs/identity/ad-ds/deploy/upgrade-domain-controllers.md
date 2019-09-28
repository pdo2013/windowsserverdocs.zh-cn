---
title: 将域控制器升级到 Windows Server 2016
description: 本文档介绍如何从 Windows Server 2012 R2 升级到 Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cbb947c17219d4fe2f6694f0e44e379fc8671e76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401939"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>将域控制器升级到 Windows Server 2016

适用于：Windows Server

本主题提供有关 Windows Server 2016 中的 Active Directory 域服务的背景信息, 并说明了从 Windows Server 2012 或 Windows Server 2012 R2 升级域控制器的过程。

## <a name="pre-requisites"></a>先决条件

升级域的推荐方法是升级运行较新版本 Windows Server 的域控制器, 并根据需要降级较旧的域控制器。 该方法优于升级现有域控制器的操作系统。 此列表涵盖在提升运行较新版本的 Windows Server 的域控制器之前要遵循的一般步骤:

1. 验证目标服务器是否满足系统要求。
1. 验证应用程序兼容性。
1. 查看有关迁移到 Windows Server 2016 的建议
1. 验证安全设置。 有关详细信息, 请参阅[Windows Server 2016 中与 AD DS 相关的弃用的功能和行为更改](../../../get-started/deprecated-features.md)。
1. 从计划运行安装的计算机上检查与目标服务器的连接性。
1. 检查所需操作主机角色的可用性：
   - 若要在现有域和林中安装运行 Windows Server 2016 的第一个 DC, 运行安装的计算机需要连接到**架构主机**才能运行 adprep/forestprep 和基础结构主机以便运行 adprep/域.
   - 若要在已扩展林架构的域中安装第一个 DC, 则只需连接到**基础结构主机**。
   - 若要在现有林中安装或删除域, 需要连接到**域命名主机**。
   - 任何域控制器安装也要求连接到**RID 主机。**
   - 如果要在现有林中安装第一个只读域控制器, 则需要连接到每个应用程序目录分区 (也称为非域命名上下文或 NDNC) 的**基础结构主机**。

### <a name="installation-steps-and-required-administrative-levels"></a>安装步骤和所需的管理级别

下表提供了用于完成这些步骤的升级步骤和权限要求摘要

|安装操作|凭据要求|
| ----- | ----- |
|安装一个新林|目标服务器上的本地管理员|
|在现有林中安装一个新域|Enterprise Admins|
|在现有域中安装另一个 DC|Domain Admins|
|运行 adprep /forestprep|Schema Admins、Enterprise Admins 和 Domain Admins|
|运行 adprep /domainprep|Domain Admins|
|运行 adprep /domainprep /gpprep|Domain Admins|
|运行 adprep /rodcprep|Enterprise Admins|

有关 Windows Server 2016 中的新功能的其他信息, 请参阅[Windows server 2016 中的新增](../../../get-started/what-s-new-in-windows-server-2016.md)功能。

## <a name="supported-in-place-upgrade-paths"></a>支持的就地升级路径

运行64位版本的 Windows Server 2012 或 Windows Server 2012 R2 的域控制器可以升级到 Windows Server 2016。 仅支持64位版本的升级, 因为 Windows Server 2016 仅提供64位版本。

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

有关支持的升级路径的详细信息, 请参阅[支持的升级路径](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep 和域

如果要将现有域控制器就地升级到 Windows Server 2016 操作系统, 则需要手动运行 adprep/forestprep 和 adprep/domainprep。  Adprep/forestprep 只需在林中运行一次。  Adprep/domainprep 需要在要升级到 Windows Server 2016 的域控制器所在的每个域中运行一次。

如果要升级新的 Windows Server 2016 服务器, 则无需手动运行。  这些集成到了 PowerShell 和服务器管理器体验。

有关运行 adprep 的详细信息, 请参阅[运行 adprep](https://technet.microsoft.com/library/dd464018.aspx)

## <a name="functional-level-features-and-requirements"></a>功能级别的功能和要求

Windows Server 2016 需要 Windows Server 2003 林功能级别。 也就是说, 在将运行 Windows Server 2016 的域控制器添加到现有 Active Directory 林之前, 林功能级别必须是 Windows Server 2003 或更高版本。 如果林包含运行 Windows Server 2003 或更高版本的域控制器，但是域功能级别仍是 Windows 2000，则安装也会被阻止。

在将 Windows Server 2016 域控制器添加到林之前, 必须先删除 windows 2000 域控制器。 对于这种情况，请考虑下列工作流：

1. 安装运行 Windows Server 2003 或更高版本的域控制器。 这些域控制器可以部署在 Windows Server 的评估版上。 此步骤还需要运行适用于该操作系统版本的 adprep.log 作为必备组件。
1. 删除 Windows 2000 域控制器。 具体来说，从域中适当降级或强制性删除 Windows Server 2000 域控制器，并且使用“Active Directory 用户和计算机”，将所有已删除的域控制器的域控制器帐户删除。
1. 将林功能级别提升到 Windows Server 2003 或更高版本。
1. 安装运行 Windows Server 2016 的域控制器。
1. 删除运行 Windows Server 早期版本的域控制器。

### <a name="rolling-back-functional-levels"></a>回退功能级别

将林功能级别 (FFL) 设置为某个值之后, 就不能回滚或降低林功能级别, 但以下情况例外:

- 如果是从 Windows Server 2012 R2 FFL 升级, 则可以将其降低回 Windows Server 2012 R2。
- 如果是从 Windows Server 2008 R2 FFL 升级, 则可以将其降低回 Windows Server 2008 R2。

将域功能级别设置为某个值之后, 就不能回滚或降低域功能级别, 但以下情况例外:

- 当你将域功能级别提升到 Windows Server 2016 并且林功能级别为 Windows Server 2012 或更低时, 可以选择将域功能级别回滚到 Windows Server 2012 或 Windows Server 2012 R2。

有关较低功能级别提供的功能的详细信息，请参阅 [了解 Active Directory 域服务 (AD DS) 功能级别](../active-directory-functional-levels.md)。

## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>AD DS 与其他服务器角色及 Windows 操作系统的互操作性

下列 Windows 操作系统不支持 AD DS：

- Windows MultiPoint Server
- Windows Server 2016 Essentials

无法将 AD DS 安装在同时运行下列服务器角色或角色服务的服务器上：

- Microsoft Hyper-V Server 2016
- 远程桌面连接代理

## <a name="administration-of-windows-server-2016-servers"></a>Windows Server 2016 服务器的管理

使用适用于 Windows 10 的远程服务器管理工具来管理运行 Windows Server 2016 的域控制器和其他服务器。 可以在运行 Windows 10 的计算机上运行 Windows Server 2016 远程服务器管理工具。

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>升级到 Windows Server 2016 的循序渐进步骤

以下简单示例将 Contoso 林从 Windows Server 2012 R2 升级到 Windows Server 2016。

![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. 将新的 Windows Server 2016 加入你的林。 在出现提示时重新启动。

   ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)

1. 使用域管理员帐户登录到新的 Windows Server 2016。
1. 在**服务器管理器**的 "**添加角色和功能**" 下, 在新的 Windows Server 2016 上安装**Active Directory 域服务**。 这会在 2012 R2 林和域上自动运行 adprep。

   ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png)

1. 在**服务器管理器**中, 单击黄色三角形, 并从下拉菜单中单击 "将**服务器提升为域控制器**"。

   ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)

1. 在 "**部署配置**" 屏幕上, 选择 "向**现有林添加域控制器**", 并单击 "下一步"。

   ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)

1. 在 "**域控制器选项**" 屏幕上, 输入**目录服务还原模式 (DSRM)** 密码, 然后单击 "下一步"。
1. 对于其余屏幕, 请单击 "**下一步**"。
1. 在 "**先决条件检查**" 屏幕上, 单击 "**安装**"。 重新启动完成后, 可以重新登录。
1. 在 Windows Server 2012 R2 服务器上的 "工具" 下的 "**服务器管理器**" 中, 选择 " **Active Directory Module for Windows PowerShell**"。

   ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)

1. 在 PowerShell 窗口中, 使用 ADDirectoryServerOperationMasterRole 移动 FSMO 角色。 你可以键入 OperationMasterRole 的名称, 或使用数字来指定角色。 有关详细信息, 请参阅[ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)

1. 通过转到 Windows Server 2016 服务器验证角色是否已移动, 在**服务器管理器**中的 "**工具**" 下, 选择 " **Active Directory Module for Windows PowerShell**"。 `Get-ADDomain`使用和`Get-ADForest` cmdlet 查看 FSMO 角色持有者。

    ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)

    ![升级](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)

1. 降级并删除 Windows Server 2012 R2 域控制器。 有关降级 dc 的信息, 请参阅[降级域控制器和域](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
1. 降级并删除服务器后, 可以将林功能级别和域功能级别提升到 Windows Server 2016。

## <a name="next-steps"></a>后续步骤

- [Active Directory 域服务安装和删除的新功能](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)
- [安装 Active Directory 域服务&#40;级别100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)
- [Windows Server 2016 功能级别](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
