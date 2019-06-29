---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: 租户-创建新的受防护的 Vm 受防护的 VM 在本地并将其移到受保护的构造
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9601145048b8798cfb102757384da49bed16a538
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469628"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>租户-创建新的受防护的 Vm 受防护的 VM 在本地并将其移到受保护的构造

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本主题介绍的步骤来创建受防护的 VM 使用仅 HYPER-V;也就是说，无需 Virtual Machine Manager、 模板磁盘或防护数据文件。 这是大多数公共云托管环境中，一个常见方案但测试受保护的构造时可能有用或企业中方案从部门结构中移动 VM 在其中共享 IT 基础结构和必须在迁移之前进行加密。

若要了解本主题如何适应总体部署受防护的 Vm 的过程，请参阅[托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>导入租户的 HYPER-V 服务器上的保护者配置

1.  开始过程之前，请确保您具有以下角色和功能安装运行 Windows Server 2016 的 HYPER-V 主机上：

    - Role

        - Hyper-V

    - 功能

        - 远程服务器管理工具\\的功能管理工具\\受防护的 VM 工具

    > [!NOTE]
    > 此处使用的主机应*不*是在受保护的构造中的主机。 这是单独的主机移动到受保护的构造之前，准备好 Vm。

2.  在此计算机上运行受防护的 VM 之前，需要确保虚拟化基于安全已启用且正在运行。 若要安装的主机保护者 HYPER-V 支持功能提升的 Windows PowerShell 窗口中运行以下命令。 这会在计算机可以运行受防护的 Vm 上配置所有所需的设置。

        Install-WindowsFeature HostGuardian

3.  你将需要为受保护的构造运行你的 VM 将获取保护者元数据。 此元数据用于授权该构造运行受防护的 VM。 如何获取此信息将为每个托管服务提供商或企业版不同。 托管商 （或，如果您有权访问受保护的构造网络） 可以通过运行以下 Windows PowerShell 命令来获取此信息：

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    在上面的示例中，"hgs"HGS 群集的分布式的网络名称，"bastion.local"HGS 域的名称。

4.  若要导入保护者密钥，你将需要更高版本的过程中，运行以下命令。

    有关&lt;路径&gt;&lt;Filename&gt;、 替换的路径和文件名的 xml 文件保存在上一步骤中，例如：**C:\\temp\\GuardianKey.xml**

    有关&lt;GuardianName&gt;，例如，指定你的托管提供商或企业数据中心的名称**HostingProvider1**。 记录下一过程的名称。

    包括 **-AllowUntrustedRoot**仅当 HGS 服务器设置的自签名证书。 （这些证书是在 HGS 密钥保护服务的一部分。）

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>在主机上创建新的受防护的虚拟机

在此过程中，将在 HYPER-V 主机上创建虚拟机，并准备将其导出到托管提供商或数据中心管理员，管理员可以在受保护的主机上运行它。

作为该过程的一部分，您将创建包含两个重要元素密钥保护程序：

-   **所有者**:中密钥保护程序中，您-或更有可能，共享证书等安全元素的组中，工作-被标识为"所有者"的 VM。 如果您运行的命令所示，生成自签名证书为证书所代表你作为所有者的身份。 （可选） 可以改为使用证书由 PKI 基础结构提供支持，并省略 **-AllowUntrustedRoot**命令中的参数。

-   **Guardians**:此外在密钥保护程序中，你的托管提供商或企业数据中心 （它运行 HGS 和受保护的主机） 被标识为"监护人。" 保护者为由您在上一过程中，导入的保护者密钥[导入租户的 HYPER-V 服务器上的保护者配置](#import-the-guardian-configuration-on-the-tenant-hyper-v-server)。

说明显示密钥保护程序，这是屏蔽数据文件中的元素，请参阅[什么屏蔽数据和为什么是必要？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

1. 租户的 HYPER-V 主机上要创建新的第 2 代虚拟机，请运行以下命令。

   有关&lt;ShieldedVMname&gt;，指定 VM 的名称，例如：**ShieldVM1**
    
   有关&lt;VHDPath&gt;，指定的位置来存储的 VHDX 的 VM，例如：**C:\\Vm\\ShieldVM1\\ShieldVM1.vhdx**
    
   有关&lt;nnGB&gt;，将大小指定为 VHDX，例如：**60GB**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. 安装受支持的操作系统 (Windows Server 2012 或更高版本，Windows 8 客户端或更高版本) 在 VM 上启用远程桌面连接和相应的防火墙规则。 记录 VM 的 IP 地址和/或 DNS 名称;你将需要它来远程连接到它。

3. 使用 RDP 远程连接到 VM，并验证正确配置 RDP 和防火墙。 作为屏蔽过程的一部分，将禁用通过 HYPER-V 虚拟机的控制台访问，因此务必要确保你能够进行远程管理在网络上的系统。

4. 若要创建新的密钥保护程序 （在本部分开头所述），请运行以下命令。

   有关&lt;GuardianName&gt;，使用在上一过程中，例如指定的名称：**HostingProvider1**

   包括 **-AllowUntrustedRoot**以允许自签名证书。

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   如果你想为多个数据中心要在运行受防护的 VM （例如，灾难恢复站点和公有云提供商），可以提供一系列到 guardians **-保护者**参数。 有关详细信息，请参阅 [新建 HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps 。

5. 若要启用 vTPM 使用密钥保护程序，请运行以下命令。 有关&lt;ShieldedVMname&gt;，使用前面步骤中使用的相同 VM 名称。

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. 若要启动 VM 后，若要验证用本地所有者的证书的密钥保护程序正常工作，请运行以下命令。

       Start-VM -Name $VMName

7. 验证 VM 在 HYPER-V 控制台中开始。

8. 使用 RDP 远程连接到 VM 并附加到受防护的 VM 的所有 Vhdx 上的所有分区上启用 BitLocker。

   > [!IMPORTANT]
   > 在前进到下一步前, 等待的 BitLocker 加密，若要完成在其中启用它的所有分区。

9. 关闭 VM，当你准备好将其移到受保护的构造。

10. 在租户的 HYPER-V 服务器上，使用工具 （Windows PowerShell 或 Hyper-v 管理器） 所选的 VM 的导出。 然后排列要复制到受保护的主机由主机托管提供商或企业数据中心维护的文件。

11. **适用于托管提供商或企业数据中心**:

    导入受防护的 VM 使用的 HYPER-V 管理器或 Windows PowerShell。 若要启动 VM，必须从 VM 所有者导 VM 配置文件。 这是因为密钥保护程序和虚拟机的虚拟 TPM 存储在配置文件。 如果 VM 配置为在受保护的构造上运行，它应能够成功启动。

## <a name="see-also"></a>请参阅

- [托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
