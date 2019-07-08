---
title: 将远程桌面服务部署迁移到 Windows Server 2016
description: 本文介绍如何将远程桌面服务部署迁移到新的 Windows Server 2016 服务器。
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: 0e4736f753fc0ad2ece6135de84d481eecb8b7a1
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812585"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>将远程桌面服务部署迁移到 Windows Server 2016

如果你当前正在 Windows Server 2012 R2 中运行远程桌面服务，可以过渡到 Windows Server 2016 以利用新的功能，例如，对 Azure SQL 和存储空间直通的支持。

支持将远程桌面服务部署从运行 Windows Server 2016 的源服务器迁移到运行 Windows Server 2016 的目标服务器。 换而言之，无法直接就地将 Windows Server 2012 R2 中的 RDS 迁移到 Windows Server 2016。 对于大多数 RDS 组件，首先需要升级到 Windows Server 2016，然后再迁移数据和许可证。 只有 RD Web、RD 网关和授权服务器组件支持直接迁移。

有关升级过程和要求的详细信息，请参阅[将远程桌面服务部署升级到 Windows Server 2016](upgrade-to-rds-2016.md)。

使用以下步骤迁移远程桌面服务部署：

- [迁移 RD 连接代理服务器](#migrate-rdconnection-broker-servers)

- [迁移会话集合](#migrate-session-collections)

- [迁移虚拟机集合](#migrate-virtual-desktop-collections)

- [迁移 RD Web 访问服务器](#migrate-rdweb-access-servers)

- [迁移 RD 网关服务器](#migrate-rdgateway-servers)

- [迁移 RD 授权服务器](#migrate-rdgateway-servers)

- [迁移证书](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>迁移 RD 连接代理服务器

这是第一个，也是最重要的迁移步骤：将 RD 连接代理迁移到运行 Windows Server 2016 的目标服务器。

> [!IMPORTANT]
> 必须配置远程桌面连接代理（RD 连接代理）源服务器，使之具有进行迁移所需的高可用性。 有关详细信息，请参阅[部署远程桌面连接代理群集](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)。

1. 如果高可用性设置中包含多个 RD 连接代理服务器，请只保留当前处于活动状态的 RD 连接代理服务器，删除所有其他 RD 连接代理服务器。

2. 将部署中余下的 RD 连接代理服务器[升级](upgrade-to-rds-2016.md)到 Windows Server 2016。

3. 将 Windows Server 2016 RD 连接代理服务器添加到高可用性部署中。

> [!NOTE] 
> RD 连接代理服务器不支持包含 Windows Server 2016 和 Windows Server 2012 R2 的混合式高可用性配置。 运行 Windows Server 2016 的 RD 连接代理能够为包含运行 Windows Server 2012 R2 的 RD 会话主机服务器的会话集合提供服务，并且能够为包含运行 Windows Server 2012 R2 的 RD 虚拟化主机服务器的虚拟机集合提供服务。

## <a name="migrate-session-collections"></a>迁移会话集合

执行以下步骤可将 Windows Server 2012 R2 中的会话集合迁移到 Windows Server 2016 中的会话集合。

> [!IMPORTANT]
> 只能在成功完成前一步骤[迁移 RD 连接服务器代理](#migrate-rdconnection-broker-servers)之后迁移会话集合。

1. 将 Windows Server 2012 R2 中的[会话集合升级](Upgrade-to-RDSH-2016.md)到 Windows Server 2016。

2. 将运行 Windows Server 2016 的新 RD 会话主机服务器添加到该会话集合中。

3. 在 RD 会话主机服务器中注销所有会话，然后从会话集合中删除需要迁移的服务器。

   > [!NOTE]
   > 如果已在会话集合中启用 UVHD 模板 (UVHD-template.vhdx)，并且文件服务器已迁移到新服务器，请使用新路径更新“用户配置文件磁盘:位置”集合属性。 用户配置文件磁盘在新位置中的相对路径必须与它们在源服务器上的相对路径相同。
   >
   > 不支持在 RD 会话主机服务器的会话集合中结合使用运行 Windows Server 2012 R2 和 Windows Server 2016 的服务器。

## <a name="migrate-virtual-desktop-collections"></a>迁移虚拟机集合

执行以下步骤可将虚拟机集合从运行 Windows Server 2012 R2 的源服务器迁移到运行 Windows Server 2016 的目标服务器。

> [!IMPORTANT]
> 只能在成功完成前一步骤[迁移 RD 连接代理服务器](#migrate-rdconnection-broker-servers)之后迁移虚拟机集合。

1. 将运行 Windows Server 2012 R2 的服务器中的[虚拟桌面集合升级](Upgrade-to-RDVH-2016.md)到 Windows Server 2016。

2. 将新的 Windows Server 2016 RD 虚拟化主机服务器添加到虚拟桌面集合中。

3. 将当前虚拟机集合中正在 RD 虚拟化主机服务器上运行的所有虚拟机迁移到新服务器。

4. 从源服务器中的虚拟机集合内删除需要迁移的所有 RD 虚拟化主机服务器。

> [!NOTE]
> 如果已在会话集合中启用 UVHD 模板 (UVHD-template.vhdx)，并且文件服务器已迁移到新服务器，请使用新路径更新“用户配置文件磁盘:位置”集合属性。 用户配置文件磁盘在新位置中的相对路径必须与它们在源服务器上的相对路径相同。
>
> 不支持在 RD 虚拟化主机服务器的虚拟桌面集合中结合使用运行 Windows Server 2012 R2 和 Windows Server 2016 的服务器。

## <a name="migrate-rdweb-access-servers"></a>迁移 RD Web 访问服务器

执行以下步骤可迁移 RD Web 访问服务器：

- 将运行 Windows Server 2016 的目标服务器加入远程桌面服务部署，并安装 RD Web 角色

- 使用 [IIS Web 部署工具](https://www.iis.net/)将当前 RD Web 访问服务器中的 RD Web 网站设置迁移到运行 Windows Server 2016 的目标服务器。

- 将[证书迁移](#migrate-certificates)到运行 Windows Server 2016 的目标服务器。

- 从远程桌面服务部署中删除源服务器。

## <a name="migrate-rdgateway-servers"></a>迁移 RD 网关服务器

执行以下步骤可迁移 RD 网关服务器：

- 将运行 Windows Server 2016 的目标服务器加入远程桌面服务部署，并安装 RD 网关角色

- 使用 [IIS Web 部署工具](https://www.iis.net/)将当前 RD 网关服务器中的 RD 网关终结点设置迁移到运行 Windows Server 2016 的目标服务器。

- 将[证书迁移](#migrate-certificates)到运行 Windows Server 2016 的目标服务器。

- 从远程桌面服务部署中删除源服务器。

## <a name="migrate-rdlicensing-servers"></a>迁移 RD 授权服务器

执行以下步骤可将 RD 授权服务器从运行 Windows Server 2012 或 Windows Server 2012 R2 的源服务器迁移到运行 Windows Server 2016 的目标服务器。

1. 将[远程桌面服务客户端访问许可证 (RDS CAL)](migrate-rds-cals.md) 从源服务器迁移到目标服务器。

2. 在远程桌面管理服务器（通常在第一个 RD 连接代理服务器上运行）上的“服务器管理器”中编辑“部署属性”，以便仅包含运行 Windows Server 2016 的新 RD 授权服务器。  

3. 停用源 RD 授权服务器：在“远程桌面授权管理器”中右键单击相应的服务器，将鼠标悬停在“高级”上并选择“停用服务器”，然后遵循向导中的步骤操作。   

4. 在远程桌面管理服务器上的“服务器管理器”中，从部署中删除源 RD 授权服务器。 

## <a name="migrate-certificates"></a>迁移证书

成功迁移证书需要实际完成迁移证书，并在远程桌面服务的“部署属性”中更新证书信息的过程。

典型的证书迁移过程包括以下步骤：

- 将证书导出到包含私钥的 PFX 文件。

- 从 PFX 文件导入证书。

迁移相应的证书后，在服务器管理器或 PowerShell 中更新远程桌面服务部署所需的以下证书：

- RD 连接代理 - 单一登录

- RD 连接代理 - RDP 文件发布

- RD 网关 - HTTPS 连接

- RD Web 访问 - HTTPS 连接和 RemoteApp/桌面连接订阅
