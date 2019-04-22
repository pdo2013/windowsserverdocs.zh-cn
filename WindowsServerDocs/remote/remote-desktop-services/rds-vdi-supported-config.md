---
title: 支持远程桌面服务 VDI 的 Windows 10 安全配置
description: 提供有关使用 Windows Server 2016 中的 RDS 的 Windows 10 VDI 的支持的配置信息。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ff890150dcea30c425267dcaae9b1bdbc6d78b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820078"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>支持远程桌面服务 VDI 的 Windows 10 安全配置

> 适用于：Windows Server 2016

Windows 10 和 Windows Server 2016 具有新内置于操作系统进一步防止出现安全漏洞，帮助阻止恶意攻击并提高了虚拟机、 应用程序和数据安全性的保护的层。

> [!NOTE]
> 请确保查看[远程桌面服务支持的配置信息](rds-supported-config.md)。

下表概述了哪一项新功能支持在 VDI 部署中使用 rds。

|  VDI 集合类型               |  托管共用 |  管理个人 |  非托管共用                                     |  非托管的个人                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | 是              | 是                | 是                                                    | 是                                                    |
| [设备保护](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | 是              | 是                | 是                                                    | 是                                                    |
| [远程 Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | 否               | 否                 | 否                                                     | 否                                                     |
| [受防护的和支持加密的 Vm](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | 否               | 否                 | 加密支持与其他配置的 Vm | 加密支持与其他配置的 Vm |

## <a name="remote-credential-guard"></a>远程 Credential Guard:

直接连接到的目标计算机而不通过远程桌面连接代理和远程桌面网关的仅支持远程 Credential Guard。
> [!NOTE]
> 如果在单实例环境中，已连接代理和 DNS 名称与计算机名称匹配，您可能能够使用远程 Credential Guard，尽管不支持此功能。

## <a name="shielded-vms-and-encryption-supported-vms"></a>受防护的 Vm 和加密支持的 Vm: 

- 远程桌面服务 VDI 中不支持受防护的 Vm 

有关利用加密支持的 Vm:
- 使用非托管的集合和预配技术外部远程桌面服务集合创建过程来设置虚拟机。 
- 不支持用户配置文件磁盘，因为它们依赖于差异磁盘 

