---
title: Hyper-v 的第2代虚拟机安全设置
description: 描述适用于第2代虚拟机的 Hyper-v 管理器中可用的安全设置
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 82544a58a8d46b3063605557be3c63cfa799e4fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364237"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Hyper-v 的第2代虚拟机安全设置

>适用于：Windows Server 2016，Microsoft Hyper-V Server 2016，Windows Server 2019，Microsoft Hyper-V 服务器2019

使用 Hyper-v 管理器中的虚拟机安全设置来帮助保护虚拟机的数据和状态。 你可以保护虚拟机不受检查、盗窃和篡改，以防受到主机上可能运行的两个恶意软件和数据中心管理员。 获取的安全级别取决于所运行的主机硬件、虚拟机生成以及是否设置用于授权主机启动受防护虚拟机的服务（称为主机保护者服务）。  

主机保护者服务是 Windows Server 2016 中的新角色。 它标识合法的 Hyper-v 主机，并允许它们运行给定的虚拟机。 最常见的情况是为数据中心设置主机保护者服务。 但你可以创建一个受防护的虚拟机，以便在本地运行它，而无需设置主机保护者服务。 稍后可将受防护的虚拟机分发到主机保护者构造。  

如果尚未设置主机保护者服务或在 Hyper-v 主机上以本地模式运行它，并且主机具有虚拟机所有者的监护人密钥，则可以更改本主题中描述的设置。   监护人密钥的所有者是一个组织，它创建并共享私钥或公钥，以拥有使用该密钥创建的所有虚拟机。  

若要了解如何利用主机保护者服务更安全地保护虚拟机，请参阅以下资源。  

- [增强构造：在 Hyper-v 中保护租户机密（Ignite 视频） ](https://go.microsoft.com/fwlink/?LinkId=746379)
- [受保护的构造和受防护的 Vm](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Hyper-v 管理器中的安全启动设置  

安全启动是第2代虚拟机提供的一项功能，可帮助防止未经授权的固件、操作系统或统一可扩展固件接口（UEFI）驱动程序（也称为选项 Rom）在启动时运行。 默认情况下启用安全启动。 你可以对运行 Windows 或 Linux 分发操作系统的第2代虚拟机使用安全启动。  

下表中所述的模板指的是验证启动过程的完整性所需的证书。  

|模板名称|描述|  
|-----------------|---------------|  
|Microsoft Windows|选择此为 Windows 操作系统的虚拟机启动安全。|  
|Microsoft UEFI 证书颁发机构|选择此为 Linux 分发操作系统的虚拟机启动。|  
|开放源受防护的 VM|此模板可用于安全启动[基于 Linux 的防护 vm](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template)。|

有关详细信息，请参阅以下主题。  

- [Windows 10 安全概述](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [是否应在 Hyper-v 中创建第1代或第2代虚拟机？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Hyper-v 上的 Linux 和 FreeBSD 虚拟机](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Hyper-v 管理器中的加密支持设置

可以通过选择以下加密支持选项来帮助保护虚拟机的数据和状态。  

- **启用受信任的平台模块**-此设置使虚拟化受信任的平台模块（TPM）芯片可用于虚拟机。 这允许来宾使用 BitLocker 加密虚拟机磁盘。
  - 如果 Hyper-v 主机运行的是 Windows 10 1511，则必须启用隔离用户模式。 
- **加密状态和 VM 迁移流量**-对虚拟机已保存状态和实时迁移通信进行加密。

### <a name="enable-isolated-user-mode"></a>启用隔离用户模式

如果在运行早于 Windows 10 周年更新的 Windows 版本的 Hyper-v 主机上选择 "**启用受信任的平台模块**"，则必须启用隔离用户模式。 对于运行 Windows Server 2016 或 Windows 10 周年更新或更高版本的 Hyper-v 主机，无需执行此操作。

独立用户模式是在 Hyper-v 主机上托管安全应用程序的运行时环境。 虚拟安全模式用于保护和保护虚拟 TPM 芯片的状态。  

若要在运行早期版本的 Windows 10 的 Hyper-v 主机上启用隔离用户模式，  

1.  以管理员身份打开 Windows PowerShell。  

2.  运行以下命令：  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

可以将启用了虚拟 TPM 的虚拟机迁移到运行 Windows Server 2016、Windows 10 build 10586 或更高版本的任何主机。 但如果将其迁移到另一台主机，则可能无法启动。 必须更新该虚拟机的密钥保护程序，才能授权新主机运行该虚拟机。 有关详细信息，请参阅[受保护的构造和受防护的 vm](https://go.microsoft.com/fwlink/?LinkId=746381)和[Windows Server 上的 hyper-v 的系统要求](../System-requirements-for-Hyper-V-on-Windows.md)。  

## <a name="security-policy-in-hyper-v-manager"></a>Hyper-v 管理器中的安全策略  
有关更多虚拟机的安全性，请使用**启用防护**选项来禁用管理功能，例如控制台连接、PowerShell Direct 和某些集成组件。 如果选择此选项，则会选择 "**安全启动**"、"**启用受信任的平台模块**" 和 "**加密状态" 和 "VM 迁移流量**" 选项。   

你可以在本地运行受防护的虚拟机，而无需设置主机保护者服务。 但如果将其迁移到另一台主机，则可能无法启动。 必须更新该虚拟机的密钥保护程序，才能授权新主机运行该虚拟机。 有关详细信息，请参阅[受保护的构造和受防护的 VM](https://go.microsoft.com/fwlink/?LinkId=746381)。  

有关 Windows Server 中的安全性的详细信息，请参阅[安全性和保证](../../../security/Security-and-Assurance.md)。  
