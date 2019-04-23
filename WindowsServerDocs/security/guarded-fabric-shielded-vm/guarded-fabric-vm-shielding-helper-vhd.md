---
title: 受防护的 Vm-准备 VM 防护帮助程序 VHD
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8e14cdeed435f23f28ca514e232fbcfa6220fc74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887718"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>受防护的 Vm-准备 VM 防护帮助程序 VHD

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

<!-- This comment creates a break between the Applies To above and the Important note below. -->

> [!IMPORTANT]
> 在开始这些过程之前, 确保你已安装 Windows Server 2016 的最新累积更新，或使用最新的 Windows 10[远程服务器管理工具](https://www.microsoft.com/en-us/download/details.aspx?id=45520)。 否则，过程将无法工作。 

本部分概述了由托管的服务提供商以支持将现有 Vm 转换为受防护的 Vm 执行的步骤。

若要了解本主题如何适应总体部署受防护的 Vm 的过程，请参阅[托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="which-vms-can-be-shielded"></a>可以受防护的 Vm？

现有的 Vm 的防护过程功能仅适用于满足以下先决条件的 Vm:

- 来宾操作系统是 Windows Server 2012、 2012 R2 和 2016 中或半年频道发布。 现有 Linux 虚拟机不能转换为受防护的 Vm。
- VM 是第 2 代 VM （UEFI 固件）
- VM 不为其 OS 卷使用差异磁盘。

## <a name="prepare-helper-vhd"></a>准备帮助程序 VHD

1.  具有 HYPER-V 和远程服务器管理工具功能的计算机上**受防护的 VM 工具**安装，创建新第 2 代 VM 具有空白的 VHDX 和安装 Windows Server 2016 上使用 Windows Server ISO 安装介质。 此 VM 不应要求，并且必须带有桌面体验中运行服务器核心或服务器。

    > [!IMPORTANT]
    > VM 防护帮助程序 VHD**不得**中创建的模板磁盘与相关[托管服务提供商创建受防护的 VM 模板](guarded-fabric-create-a-shielded-vm-template.md)。 如果重复使用的模板磁盘，将有磁盘签名冲突发生在屏蔽过程中由于这两个磁盘将具有相同的 GPT 磁盘标识符。 您可以避免此问题创建新的 （空白） VHD 和到它使用 ISO 安装介质上安装 Windows Server 2016。

2.  启动 VM，完成任何安装程序的步骤，并登录到桌面。 验证 VM 处于工作状态后，请关闭 VM。

3.  在提升的 Windows PowerShell 窗口，运行以下命令以准备成为 VM 防护帮助程序磁盘之前创建的 VHDX。 将路径更新为您的环境的正确路径。

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  该命令已成功完成后，将 VHDX 复制到 VMM 库共享。 **不这样做**再次启动步骤 1 中的 VM。 否则将破坏帮助程序磁盘。

5.  现在可以在 HYPER-V 中的步骤 1 中删除 VM。

## <a name="configure-vmm-host-guardian-server-settings"></a>配置 VMM 主机保护者服务器设置

在 VMM 控制台中，打开设置窗格，然后**主机保护者服务设置**下**常规**。 在此窗口的底部，是要配置你的帮助程序 VHD 的位置的字段。 使用浏览按钮来选择库共享中的 VHD。 如果没有看到您在共享中的磁盘，可能需要手动刷新 VMM 中为它的显示中的库。

![VMM 的主机保护者服务设置](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>请参阅

- [托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的构造和受防护的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
