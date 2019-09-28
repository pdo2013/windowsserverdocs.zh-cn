---
title: 远程桌面服务 VDI 支持的 Windows 10 安全配置
description: 提供有关 Windows Server 2016 中 RDS 支持的 Windows 10 VDI 配置的信息。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/27/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: 08941c49469dcf9b9e3e42c7ab799186380bab35
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387026"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>远程桌面服务 VDI 支持的 Windows 10 安全配置

> 适用于：Windows Server 2016

Windows 10 和 Windows Server 2016 提供操作系统中内置的保护层来进一步防止出现安全漏洞，帮助阻止恶意攻击并提高虚拟机、应用程序和数据的安全性。

> [!NOTE]
> 请务必查看[远程桌面服务支持的配置信息](rds-supported-config.md)。

下表概述了使用 RDS 的 VDI 部署支持其中的哪些新功能。

|  VDI 集合类型               |  托管共用 |  托管个人 |  非托管共用                                     |  非托管个人                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | 是              | 是                | 是                                                    | 是                                                    |
| [Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | 是              | 是                | 是                                                    | 是                                                    |
| [Remote Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | 否               | 否                 | 否                                                     | 否                                                     |
| [受防护的 VM 和加密支持的 VM](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | 否               | 否                 | 加密支持的采用其他配置的 VM | 加密支持的采用其他配置的 VM |

## <a name="remote-credential-guard"></a>Remote Credential Guard：

仅支持直接将 Remote Credential Guard 连接到目标计算机，而不支持通过远程桌面连接代理和远程桌面网关进行连接。
> [!NOTE]
> 如果在单实例环境中使用连接代理，并且 DNS 名称与计算机名称相匹配，则你也许可以使用 Remote Credential Guard，不过它不受支持。

## <a name="shielded-vms-and-encryption-supported-vms"></a>受防护的 VM 和加密支持的 VM： 

- 远程桌面服务 VDI 不支持受防护的 VM 

若要利用加密支持的 VM：
- 请在远程桌面服务集合创建过程以外使用非托管集合和预配技术来预配虚拟机。 
- 不支持用户配置文件磁盘，因为它们依赖于差异磁盘 

