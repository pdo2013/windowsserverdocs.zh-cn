---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: 租户的受防护的 Vm-在本地创建新的受防护的 VM 并将其移动到受保护的结构
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a4b5ff2942c8485a4c10770a4374d56734f7f3c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402386"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>租户的受防护的 Vm-在本地创建新的受防护的 VM 并将其移动到受保护的结构

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本主题介绍了使用仅 Hyper-v 创建受防护的 VM 的步骤;也就是说，不 Virtual Machine Manager、模板磁盘或屏蔽数据文件。 对于大多数公有云托管环境而言，这是一种不常见的方案，但在以下情况下可能非常有用：测试受保护的构造，或在将 VM 从部门构造迁移到共享 IT 基础结构的企业方案中，在迁移之前必须加密。

若要了解本主题如何适应部署受防护的 Vm 的整个过程，请参阅[为受保护的主机和受防护的 Vm 托管服务提供商配置步骤](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>在租户 Hyper-v 服务器上导入监护人配置

1.  在开始此过程之前，请确保你在运行 Windows Server 2016 的 Hyper-v 主机上，并安装以下角色和功能：

    - Role

        - Hyper-V

    - 功能

        - 远程服务器管理工具 @ no__t-0Feature 管理工具 @ no__t-1Shielded VM 工具

    > [!NOTE]
    > 此处使用的主机*不*应是受保护的构造中的主机。 这是一个独立的主机，其中的 Vm 在被移动到受保护的构造之前已准备就绪。

2.  在此计算机上运行受防护的 VM 之前，需要确保启用并运行基于虚拟化的安全性。 在提升的 Windows PowerShell 窗口中运行以下命令，以安装主机保护者 Hyper-v 支持功能。 这会在计算机上配置所有必需的设置，以便能够运行受防护的 Vm。

        Install-WindowsFeature HostGuardian

3.  你将需要为 VM 将在其中运行的受保护构造获取保护者元数据。 此元数据用于授权该构造运行受防护的 VM。 对于每个托管服务提供商或企业，您获取此信息的方式将有所不同。 宿主（或者，如果你有权访问受保护的 fabric 网络）可以通过运行以下 Windows PowerShell 命令来获取此信息：

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    在上面的示例中，"hgs" 是 HGS 群集的分布式网络名称，而 "堡垒" 是 HGS 域的名称。

4.  若要导入在后面的过程中将需要的监护人密钥，请运行以下命令。

    对于 &lt;Path @ no__t-1 @ no__t-2Filename @ no__t-3，请替换在上一步中保存的 XML 文件的路径和文件名，例如：**C： \\temp\\GuardianKey.xml**

    对于 &lt;GuardianName @ no__t，为托管提供程序或企业数据中心指定一个名称，例如**HostingProvider1**。 记录下一个过程的名称。

    仅当已设置自签名证书的 HGS 服务器时，才包含 **-AllowUntrustedRoot** 。 （这些证书是 HGS 中密钥保护服务的一部分。）

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>在主机上创建新的受防护的虚拟机

在此过程中，你将在 Hyper-v 主机上创建一个虚拟机，并准备将其导出到托管提供程序或数据中心管理员，他们可以在受保护的主机上运行该虚拟机。

作为此过程的一部分，你将创建包含两个重要元素的密钥保护程序：

-   **所有者**：在密钥保护程序中，你所使用的（或更多）共享安全元素（如证书）的组将标识为 VM 的 "所有者"。 作为所有者的标识以证书表示，如果运行如下所示的命令，则会生成自签名证书。 您可以选择使用由 PKI 基础结构支持的证书，并在命令中省略 **-AllowUntrustedRoot**参数。

-   **监护人**：此外，在密钥保护程序中，托管提供商或企业数据中心（运行 HGS 和受保护的主机）被标识为 "监护人"。 监护人由您在前面过程中导入的监护人密钥表示，在[租户 hyper-v 服务器上导入监护人配置](#import-the-guardian-configuration-on-the-tenant-hyper-v-server)。

有关显示密钥保护程序（即屏蔽数据文件中的元素）的插图，请参阅[什么是防护数据？什么是必需的？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

1. 在租户 Hyper-v 主机上，若要创建新的第2代虚拟机，请运行以下命令。

   对于 &lt;ShieldedVMname @ no__t，为 VM 指定一个名称，例如：**ShieldVM1**
    
   对于 &lt;VHDPath @ no__t-1，请指定存储 VM VHDX 的位置，例如：**C： \\VMs @ no__t-2ShieldVM1\\ShieldVM1.vhdx**
    
   对于 &lt;nnGB @ no__t，为 VHDX 指定大小，例如：**60GB**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. 在 VM 上安装受支持的操作系统（Windows Server 2012 或更高版本、Windows 8 客户端或更高版本），并启用远程桌面连接和相应的防火墙规则。 记录 VM 的 IP 地址和/或 DNS 名称;你将需要它来远程连接到它。

3. 使用 RDP 远程连接到 VM，并验证是否正确配置了 RDP 和防火墙。 作为屏蔽过程的一部分，将禁用通过 Hyper-v 对虚拟机的控制台访问，因此确保能够通过网络远程管理系统非常重要。

4. 若要创建新的密钥保护程序（本部分开头部分所述），请运行以下命令。

   对于 &lt;GuardianName @ no__t-1，请使用在前面的过程中指定的名称，例如：**HostingProvider1**

   包含 **-AllowUntrustedRoot**以允许自签名证书。

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   如果希望多个数据中心能够运行受防护的 VM （例如，灾难恢复站点和公有云提供商），可以向 **-监护人**参数提供监护人列表。 有关详细信息，请参阅 [HgsKeyProtector] （@no__t。

5. 若要使用密钥保护程序启用 vTPM，请运行以下命令。 对于 &lt;ShieldedVMname @ no__t-1，请使用在前面的步骤中使用的相同 VM 名称。

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. 若要启动 VM 以验证密钥保护程序是否正在使用本地所有者证书，请运行以下命令。

       Start-VM -Name $VMName

7. 验证 VM 是否已在 Hyper-v 控制台中启动。

8. 使用 RDP 远程连接到 VM，并在附加到受防护 VM 的所有 Vhdx 上的所有分区上启用 BitLocker。

   > [!IMPORTANT]
   > 在继续下一步之前，请等待 BitLocker 加密在启用了它的所有分区上完成。

9. 准备好将 VM 移到受保护的结构中时，关闭 VM。

10. 在租户 Hyper-v 服务器上，使用所选的工具（Windows PowerShell 或 Hyper-v 管理器）导出 VM。 然后，将文件复制到托管提供商或企业数据中心维护的受保护的主机。

11. **对于宿主提供程序或企业数据中心**：

    使用 Hyper-v 管理器或 Windows PowerShell 导入受防护的 VM。 若要启动 VM，你必须从 VM 所有者导入 VM 配置文件。 这是因为密钥保护程序和 VM 的虚拟 TPM 存储在配置文件中。 如果 VM 配置为在受保护的构造上运行，则它应能够成功启动。

## <a name="see-also"></a>请参阅

- [受保护的主机和受防护的 Vm 的托管服务提供商配置步骤](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
