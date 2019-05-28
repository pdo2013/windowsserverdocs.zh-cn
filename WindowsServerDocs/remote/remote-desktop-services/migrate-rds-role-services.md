---
title: 远程桌面服务部署迁移到 Windows Server 2016
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
ms.openlocfilehash: 63037dd7e32320b6e640396e20344e5678ed91dd
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034438"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>远程桌面服务部署迁移到 Windows Server 2016

如果当前正在 Windows Server 2012 R2 中远程桌面服务，你可以转到 Windows Server 2016 以利用新功能，如对 Azure SQL 和存储空间直通的支持。

从运行 Windows Server 2016 的目标服务器到运行 Windows Server 2016 的源服务器支持远程桌面服务部署的迁移。 换而言之，是 Windows Server 2012 R2 中的 RDS 没有直接的就地迁移到 Windows Server 2016。 相反，对于大多数 RDS 组件，您首先升级到 Windows Server 2016，然后将迁移数据和许可证。 直接转换迁移的唯一组件是 RD Web、 RD 网关和授权服务器。

有关升级过程和要求的详细信息，请参阅[远程桌面服务部署升级到 Windows Server 2016](upgrade-to-rds-2016.md)。

使用以下步骤迁移远程桌面服务部署：
- [迁移 RD 连接代理服务器](#migrate-rd-connection-broker-servers)
- [迁移会话集合](#migrate-session-collections)
- [迁移虚拟机集合](#migrate-virtual-desktop-collections)
- [迁移 RD Web 访问服务器](#migrate-rd-web-access-servers)
- [迁移 RD 网关服务器](#migrate-rd-gateway-servers)
- [迁移 RD 授权服务器](#migrate-rd-licensing-servers)
- [迁移证书](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>迁移 RD 连接代理服务器

这是有关迁移的第一个也是最重要步骤： 迁移到目标服务器运行 Windows Server 2016 的 RD 连接代理。
> [!IMPORTANT] 
> 远程桌面连接代理 （RD 连接代理） 的源服务器必须配置以实现高可用性以支持迁移。 有关详细信息，请参阅[部署远程桌面连接代理群集](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)。

1. 如果高可用性设置中包含多个 RD 连接代理服务器，请只保留当前处于活动状态的 RD 连接代理服务器，删除所有其他 RD 连接代理服务器。
2. [升级](upgrade-to-rds-2016.md)到 Windows Server 2016 部署中的剩余 RD 连接代理服务器。
3. 将 Windows Server 2016 RD 连接代理服务器添加到高可用性部署。

> [!NOTE] 
> RD 连接代理服务器不支持使用 Windows Server 2016 和 Windows Server 2012 R2 的混合式高可用性配置。 运行 Windows Server 2016 RD 连接代理可以使用 RD 会话主机服务器运行 Windows Server 2012 R2，提供会话集合，它可以向 RD 虚拟化主机服务器运行 Windows Server 2012 R2 提供虚拟桌面集合。

## <a name="migrate-session-collections"></a>迁移会话集合

请按照这些步骤在 Windows Server 2012 R2 中的会话集合迁移到 Windows Server 2016 中的会话集合。
> [!IMPORTANT] 
> 只有成功完成上一步骤之后迁移会话集合[迁移 RD 连接代理服务器](#migrate-rd-connection-broker-servers)。

1. [升级会话集合](Upgrade-to-RDSH-2016.md)从 Windows Server 2012 R2 到 Windows Server 2016。
2. 将添加到会话集合中运行 Windows Server 2016 的新 RD 会话主机服务器。
3. 在 RD 会话主机服务器中注销所有会话，然后从会话集合中删除需要迁移的服务器。 
> [!NOTE]
> 如果在会话集合中启用 UVHD 模板 (uvhd-template.vhdx)，并且文件服务器已迁移到新的服务器，则更新用户配置文件磁盘：使用新路径位置集合属性。 用户配置文件磁盘在新位置中的相对路径必须与它们在源服务器上的相对路径相同。
>
> 不支持 RD 会话主机服务器的会话集合中混合运行 Windows Server 2012 R2 和 Windows Server 2016 的服务器。

## <a name="migrate-virtual-desktop-collections"></a>迁移虚拟机集合

请按照以下步骤来迁移虚拟机集合从运行 Windows Server 2016 的目标服务器到运行 Windows Server 2012 R2 的源服务器。

> [!IMPORTANT] 
> 仅在成功完成上一步骤后迁移虚拟机集合[迁移 RD 连接代理服务器](#migrate-rd-connection-broker-servers)。

1. [升级虚拟机集合](Upgrade-to-RDVH-2016.md)从服务器运行 Windows Server 2012 R2 到 Windows Server 2016。
2. 将新的 Windows Server 2016 RD 虚拟化主机服务器添加到虚拟桌面集合。
3. 将当前虚拟机集合中正在 RD 虚拟化主机服务器上运行的所有虚拟机迁移到新服务器。 
4. 从源服务器中的虚拟机集合内删除需要迁移的所有 RD 虚拟化主机服务器。

> [!NOTE] 
> 如果在会话集合中启用 UVHD 模板 (uvhd-template.vhdx)，并且文件服务器已迁移到新的服务器，则更新用户配置文件磁盘：使用新路径位置集合属性。 用户配置文件磁盘在新位置中的相对路径必须与它们在源服务器上的相对路径相同。
>
> 不支持虚拟桌面集合中 RD 虚拟化主机服务器的混合运行 Windows Server 2012 R2 和 Windows Server 2016 的服务器。

## <a name="migrate-rdweb-access-servers"></a>迁移 RD Web 访问服务器
请执行以下步骤来迁移 RD Web 访问服务器：
- 加入远程桌面服务部署到运行 Windows Server 2016 的目标服务器并安装 RD Web 角色
- 使用[IIS Web 部署工具](https://www.iis.net/)RD Web 网站设置从当前 RD Web 访问服务器迁移到运行 Windows Server 2016 的目标服务器。
- [迁移证书](#migrate-certificates)到目标服务器运行 Windows Server 2016。
- 从远程桌面服务部署中删除源服务器  

## <a name="migrate-rdgateway-servers"></a>迁移 RD 网关服务器
请执行以下步骤来迁移 RD 网关服务器：
- 加入远程桌面服务部署到运行 Windows Server 2016 的目标服务器并安装 RD 网关角色
- 使用[IIS Web 部署工具](https://www.iis.net/)将 RD 网关终结点设置从当前的 RD 网关服务器迁移到运行 Windows Server 2016 的目标服务器。
- [迁移证书](#migrate-certificates)到目标服务器运行 Windows Server 2016。
- 从远程桌面服务部署中删除源服务器  

## <a name="migrate-rdlicensing-servers"></a>迁移 RD 授权服务器

按照以下步骤从运行 Windows Server 2016 的目标服务器到运行 Windows Server 2012 或 Windows Server 2012 R2 的源服务器迁移 RD 授权服务器。

1. [迁移远程桌面服务客户端访问许可证 (RDS Cal)](migrate-rds-cals.md)从源服务器到目标服务器。
2. 编辑**部署属性**中**服务器管理器**（这通常第一个 RD 连接代理服务器上运行） 的远程桌面管理服务器上以包括新的 RD 授权运行 Windows Server 2016 的服务器。
3. 停用源 RD 授权服务器：在中**远程桌面授权管理器**，右键单击相应的服务器，将鼠标悬停**高级**若要选择**停用服务器**，然后按照向导中的步骤进行操作.
4. 从部署中删除源 RD 授权服务器**服务器管理器**远程桌面管理服务器上。

## <a name="migrate-certificates"></a>迁移证书

证书成功迁移需要这两个实际的迁移证书和更新远程桌面服务部署属性中的证书信息过程。

典型证书迁移包括以下步骤：
- 证书使用私钥导出到 PFX 文件。
- 从 PFX 文件导入证书。

迁移后的适当证书，更新在服务器管理器或 PowerShell 中远程桌面服务部署所需的以下证书： 
- RD 连接代理-单一登录
- RD 连接代理-RDP 文件发布
- RD 网关的 HTTPS 连接
- RD Web 访问的 HTTPS 连接，并且 RemoteApp/桌面连接订阅
