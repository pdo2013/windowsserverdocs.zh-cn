---
title: 受防护的 Vm-准备 VM 防护帮助程序 VHD
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7984d1c965c15f7d8c3f3abfdc99f01e3adc215f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403427"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>受防护的 Vm-准备 VM 防护帮助程序 VHD

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

> [!IMPORTANT]
> 在开始这些过程之前，请确保已安装最新的 Windows 2016 Server 累积更新，或者使用最新的 Windows 10[远程服务器管理工具](https://www.microsoft.com/en-us/download/details.aspx?id=45520)。 否则，这些过程将不起作用。 

本部分概述了托管服务提供商执行的步骤，支持将现有的 Vm 转换为受防护的 Vm。

若要了解本主题如何适应部署受防护的 Vm 的整个过程，请参阅[为受保护的主机和受防护的 Vm 托管服务提供商配置步骤](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="which-vms-can-be-shielded"></a>哪些 Vm 可以受防护？

现有 Vm 的防护过程仅适用于满足以下先决条件的 Vm：

- 来宾操作系统是 Windows Server 2012、2012 R2、2016或半年频道发布。 无法将现有 Linux Vm 转换为受防护的 Vm。
- VM 是第2代 VM （UEFI 固件）
- VM 不会对其 OS 卷使用差异磁盘。

## <a name="prepare-helper-vhd"></a>准备 Helper VHD

1.  在安装了 Hyper-v 的计算机和已安装远程服务器管理工具功能**防护的 Vm 工具**上，使用空白 VHDX 创建新的第2代 VM，并使用 WINDOWS server ISO 安装媒体在其上安装 windows server 2016。 不应屏蔽此 VM，必须运行具有桌面体验的服务器核心或服务器。

    > [!IMPORTANT]
    > VM 防护帮助程序 VHD**不**能与你在托管服务提供程序中创建的模板磁盘相关联。[创建受防护的 VM 模板](guarded-fabric-create-a-shielded-vm-template.md)。 如果重新使用模板磁盘，则在防护过程中会发生磁盘签名冲突，因为这两个磁盘将具有相同的 GPT 磁盘标识符。 为此，可以创建新的（空白） VHD，并使用 ISO 安装媒体将 Windows Server 2016 安装到它上面。

2.  启动 VM，完成任何安装步骤，然后登录到桌面。 验证 VM 处于工作状态后，关闭 VM。

3.  在提升的 Windows PowerShell 窗口中运行以下命令，以便准备之前创建的 VHDX 成为 VM 防护帮助程序磁盘。 用您的环境的正确路径更新路径。

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  成功完成该命令后，将 VHDX 复制到 VMM 库共享。 **不要**再从步骤1启动 VM。 这样做会损坏帮助程序磁盘。

5.  你现在可以从 Hyper-v 中的步骤1中删除 VM。

## <a name="configure-vmm-host-guardian-server-settings"></a>配置 VMM 主机保护者服务器设置

在 VMM 控制台中，打开 "设置" 窗格，然后在 "**常规**" 下**主机保护者服务设置**。 在此窗口的底部，有一个字段可用于配置帮助程序 VHD 的位置。 使用 "浏览" 按钮从库共享中选择 VHD。 如果在共享中看不到你的磁盘，则可能需要手动刷新 VMM 中的库以使其显示。

![VMM-主机保护者服务设置](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>请参阅

- [受保护的主机和受防护的 Vm 的托管服务提供商配置步骤](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
