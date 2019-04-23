---
title: 第 2 代虚拟机的 HYPER-V 安全设置
description: 介绍了第 2 代虚拟机的 HYPER-V 管理器中提供的安全设置
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 90a2b7234ee55d8469b6e02ba3de3a0efc080a3e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889498"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>第 2 代虚拟机的 HYPER-V 安全设置

>适用于：Windows Server 2016 中，Microsoft 的 HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

使用在 Hyper-v 管理器中的虚拟机安全设置来帮助保护数据和虚拟机的状态。 您可以从检查、 盗用和篡改从主机和数据中心管理员可以运行这两个恶意软件保护虚拟机。 您获取的安全级别取决于运行虚拟机代次，主机硬件和是否设置调用主机保护者服务的服务的授权以启动受防护的虚拟机的主机。  

主机保护者服务是 Windows Server 2016 中的新角色。 它标识合法的 HYPER-V 主机，并允许其运行给定的虚拟机。 最常见设置主机保护者服务为数据中心。 但是，您可以创建受防护的虚拟机，而无需设置主机保护者服务本地运行。 您可以更高版本分发到的主机保护者构造中受防护的虚拟机。  

如果你尚未设置主机保护者服务或其本地模式下运行的 HYPER-V 主机上，并且主机具有虚拟机所有者的保护者密钥，可以更改此主题中所述的设置。   保护者密钥的所有者是创建组织和共享使用该密钥创建专用或公用密钥拥有的所有虚拟机。  

若要了解如何使你的虚拟机使用主机保护者服务更安全，请参阅以下资源。  

- [增强构造：保护 HYPER-V (Ignite 视频) 中的租户密钥](https://go.microsoft.com/fwlink/?LinkId=746379)
- [受保护的构造和受防护的 Vm](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>保护的 HYPER-V 管理器中的启动设置  

安全启动是与第 2 代虚拟机提供可帮助防止未经授权的固件、 操作系统或统一可扩展固件接口 (UEFI) 驱动程序 （也称为选项 Rom） 在启动时运行的功能。 默认情况下启用安全启动。 运行 Windows 或 Linux 分发操作系统的第 2 代虚拟机，可以使用安全启动。  

下表中描述的模板是指你需要验证引导过程的完整性的证书。  

|模板名称|描述|  
|-----------------|---------------|  
|Microsoft Windows|选择到安全启动的 Windows 操作系统的虚拟机。|  
|Microsoft UEFI 证书颁发机构|选择安全启动虚拟机的 Linux 分发操作系统。|  
|开放源代码受防护的 VM|此模板利用的安全启动[基于 Linux 的受防护的 Vm](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template)。|

有关详细信息，请参阅以下主题。  

- [Windows 10 安全概述](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [应在 HYPER-V 中创建第 1 或 2 代虚拟机？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Linux 和 FreeBSD 虚拟机上的 HYPER-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>在 Hyper-v 管理器中的加密支持设置

您可以帮助保护数据和虚拟机的状态通过选择以下加密的支持选项。  

- **启用受信任的平台模块**-此设置会使虚拟化的受信任的平台模块 (TPM) 芯片提供给你的虚拟机。 这允许来宾使用 BitLocker 加密虚拟机磁盘。
  - 如果你的 HYPER-V 主机在运行 Windows 10 1511年，你必须启用隔离用户模式。 
- **加密状态和 VM 迁移流量**-进行加密的虚拟机已保存状态和实时迁移流量。

### <a name="enable-isolated-user-mode"></a>启用隔离的用户模式

如果选择**启用受信任的平台模块**上运行早于 Windows 10 周年更新的 Windows 版本的 HYPER-V 主机，必须启用隔离用户模式。 无需执行此操作的 HYPER-V 主机的运行的 Windows Server 2016 或 Windows 10 周年更新或更高版本。

隔离的用户模式是承载虚拟安全模式中的 HYPER-V 主机上的安全应用程序的运行时环境。 虚拟安全模式下用于安全和保护的虚拟 TPM 芯片状态。  

若要在运行早期版本的 Windows 10 的 HYPER-V 主机上启用隔离用户模式  

1.  以管理员身份打开 Windows PowerShell。  

2.  运行以下命令：  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

你可以启用到运行 Windows Server 2016 的任何主机的虚拟 TPM 使用迁移虚拟机、 Windows 10 内部版本 10586 或更高版本。 但如果您将其迁移到另一台主机时，你可能无法启动它。 必须更新密钥保护程序为该虚拟机，以授权新主机以运行虚拟机。 有关详细信息，请参阅[受保护的构造和受防护的 Vm](https://go.microsoft.com/fwlink/?LinkId=746381)并[System requirements for Windows Server 上的 HYPER-V 要求](../System-requirements-for-Hyper-V-on-Windows.md)。  

## <a name="security-policy-in-hyper-v-manager"></a>在 HYPER-V 管理器中的安全策略  
对于多个虚拟机的安全性，请使用**启用防护**选项来禁用管理功能，如控制台连接、 PowerShell Direct 和某些集成组件。 如果选择此选项，**安全引导**，**启用受信任的平台模块**，并**加密状态和 VM 迁移流量**选项是选择和强制执行。   

你可以本地运行受防护的虚拟机，而无需设置主机保护者服务。 但如果您将其迁移到另一台主机时，你可能无法启动它。 必须更新密钥保护程序为该虚拟机，以授权新主机以运行虚拟机。 有关详细信息，请参阅[受保护的构造和受防护的 VM](https://go.microsoft.com/fwlink/?LinkId=746381)。  

有关 Windows Server 中的安全性的详细信息，请参阅[安全和保障](../../../security/Security-and-Assurance.md)。  
