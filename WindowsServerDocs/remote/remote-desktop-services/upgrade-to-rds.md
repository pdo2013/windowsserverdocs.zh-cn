---
title: 将远程桌面服务部署升级到 Windows Server 2016
description: 本文介绍如何将现有的远程桌面服务部署升级到 Windows Server 2016。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 03/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7b1f1f6-57c8-40ab-a235-e36240dcc1f8
author: spatnaik
manager: scottman
notes: https://social.technet.microsoft.com/wiki/contents/articles/22069.remote-desktop-services-upgrade-guidelines-for-windows-server-2012-r2.aspx
ms.openlocfilehash: d39361c62d71f9a804087e62936d2cf8c646917a
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63711802"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>将远程桌面服务部署升级到 Windows Server 2016

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>支持的装有 RDS 角色的 OS 升级
仅支持从 Windows Server 2012 R2 和 Windows Server 2016 升级到 Windows Server 2016。

## <a name="flow-for-deployment-upgrades"></a>部署升级的流程
为了尽量减少停机时间，最好是遵循以下步骤：

1. 应该先升级 **RD 连接代理服务器**。 如果部署中采用了主动/主动设置，请在部署中保留一个服务器并删除所有其他服务器，然后执行就地升级。 针对剩余的 RD 连接代理服务器执行脱机升级，然后将其重新添加到部署中。 在升级 RD 连接代理服务器期间，部署将不可用。

   > [!NOTE] 
   > 必须升级 RD 连接代理服务器。 不支持在包含 Windows Server 2016 服务器的混合部署中使用 Windows Server 2012 R2 RD 连接代理服务器。 RD 连接代理服务器在运行 Windows Server 2016 后，部署将会正常运行，即使部署中的剩余服务器仍在运行 Windows Server 2012 R2。

2. 应该先升级 **RD 许可证服务器**，然后再升级 RD 会话主机服务器。
   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD 许可证服务器可与 Windows Server 2016 部署配合工作，但它们只能处理来自 Windows Server 2012 R2 和更低版本的 CAL。 它们不能使用 Windows Server 2016 CAL。 有关 RD 许可证服务器的详细信息，请参阅[使用客户端访问许可证 (CAL) 为 RDS 部署授权](rds-client-access-license.md)。

3. 接下来可以升级 **RD 会话主机服务器**。 为了避免升级过程中停机，管理员可按下面的详述，以 2 个步骤分批升级服务器。 升级后，所有服务器可正常运行。 若要升级，请使用[将远程桌面会话主机服务器升级到 Windows Server 2016](upgrade-to-rdsh.md) 中所述的步骤。

4. 接下来可以升级 **RD 虚拟化主机服务器**。 若要升级，请使用[将远程桌面虚拟化主机服务器升级到 Windows Server 2016](upgrade-to-rdvh.md) 中所述的步骤。

5. 随时可以升级 **RD Web 访问服务器**。
   > [!NOTE]
   > 升级 RD Web 可能会重置 IIS 属性（例如任何配置文件）。 为确保更改不会丢失，请记下或复制对 IIS 中的 RD Web 站点所做的自定义设置。

   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD Web 访问服务器可与 Windows Server 2016 部署配合工作。

6. 随时可以升级 **RD 网关服务器**。
   > [!NOTE]
   > Windows Server 2016 不包含网络访问保护 (NAP) 策略 - 必须删除这些策略。 删除适当策略的最简单方法是运行升级向导。 如果必须删除某些 NAP 策略，升级过程将会阻塞，并在桌面上创建一个包含特定策略的文本文件。 若要管理 NAP 策略，请打开网络策略服务器工具。 删除策略后，单击安装程序工具中的“刷新”以继续升级。  

   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD 网关服务器可与 Windows Server 2016 部署配合工作。

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>VDI 部署 – 支持的来宾 OS 升级
管理员可使用以下选项升级 VM 集合：

### <a name="upgrade-managed-shared-vm-collections"></a>升级托管的共享 VM 集合 
管理员需要使用所需的 OS 版本创建 VM 模板，并使用它来修补池中的所有 VM。 

支持以下修补方案：
- Windows 7 SP1 可修补为 Windows 8 或 Windows 8.1
- Windows 8 可修补为 Windows 8.1
- Windows 8.1 可修补为 Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>升级非托管的共享 VM 集合 
最终用户无法升级其个人桌面。 应该由管理员执行这种升级。 确切的步骤有待确定。