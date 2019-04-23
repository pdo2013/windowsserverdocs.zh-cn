---
title: 升级远程桌面服务部署到 Windows Server 2016
description: 本文介绍如何升级现有的远程桌面服务部署到 Windows Server 2016。
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
ms.openlocfilehash: f683a7d9346494e7f1fb6faf716ca9c90cfef8d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875748"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>升级远程桌面服务部署到 Windows Server 2016

>适用于：Windows 服务器 （半年频道），Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安装 RDS 角色支持 OS 升级
仅从 Windows Server 2012 R2 和 Windows Server 2016 支持的升级到 Windows Server 2016。

## <a name="flow-for-deployment-upgrades"></a>部署升级的流
为了保持在最低限度的停机时间，最好遵循以下步骤：

1. **RD 连接代理服务器**应为要升级的第一个。 如果没有主动/主动设置在部署中，从部署中删除所有但一个服务器并且执行就地升级。 剩余的 RD 连接代理服务器脱机上执行升级以及然后重新将其添加到部署。 部署 RD 连接代理服务器升级过程将不可用。

   > [!NOTE] 
   > 它是必须升级 RD 连接代理服务器。 我们不支持在具有 Windows Server 2016 的服务器的混合部署中的 Windows Server 2012 R2 RD 连接代理服务器。 RD 连接代理服务器运行 Windows Server 2016 后的部署将会正常运行，即使在部署中的服务器的其余部分仍在运行 Windows Server 2012 R2。

2. **远程桌面许可证服务器**之前升级 RD 会话主机服务器应升级。
   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 远程桌面许可证服务器将使用 Windows Server 2016 的部署，但它们只能处理从 Windows Server 2012 R2 和较旧的 Cal。 它们不能使用 Windows Server 2016 Cal。 请参阅[与客户端访问许可证 (Cal) 许可 RDS 部署](rds-client-access-license.md)的远程桌面许可证服务器的详细信息。

3. **RD 会话主机服务器**接下来可以升级。 若要避免故障时间在升级过程中管理员可以拆分如下所示的 2 个步骤中要升级的服务器。 在升级后，所有将能正常运行。 若要升级，请使用中所述的步骤[到 Windows Server 2016 的升级远程桌面会话主机服务器](upgrade-to-rdsh.md)。

4. **RD 虚拟化主机服务器**接下来可以升级。 若要升级，请使用中所述的步骤[到 Windows Server 2016 的升级远程桌面虚拟化主机服务器](upgrade-to-rdvh.md)。

5. **RD Web 访问服务器**可以随时升级。
   > [!NOTE]
   > 升级 RD Web 可能会重置 IIS 属性 （如任何配置文件）。 若要不会丢失所做的更改，请说明或对 IIS 中的 RD Web 站点的自定义项的副本。

   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD Web 访问服务器将使用 Windows Server 2016 部署。

6. **RD 网关服务器**可以随时升级。
   > [!NOTE]
   > Windows Server 2016 不包括网络访问保护 (NAP) 策略-它们将需要删除。 若要删除的正确策略的最简单方法是通过运行升级向导。 如果必须删除任何 NAP 策略，升级将阻止，并创建一个文本文件，其中包括特定的策略在桌面上。 若要管理 NAP 策略，请打开网络策略服务器工具。 删除后它们，单击**刷新**安装工具以继续升级过程中。 

   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 远程桌面网关服务器会使用 Windows Server 2016 部署。

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>VDI 部署 – 受支持的来宾 OS 升级
管理员将具有以下选项来升级的虚拟机集合：

### <a name="upgrade-managed-shared-vm-collections"></a>升级托管共享 VM 集合 
管理员需要创建具有所需的操作系统版本的虚拟机模板并使用它来修补在池中的所有 Vm。 

我们支持以下修补方案：
- Windows 7 SP1 可以修补的 Windows 8 或 Windows 8.1
- Windows 8 可以修补到 Windows 8.1
- Windows 8.1 可修补到 Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>升级共享的非托管的 VM 集合 
最终用户不能升级其个人桌面。 管理员应执行升级。 确切步骤仍是确定。