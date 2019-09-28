---
title: 创建受防护的 Linux VM 模板磁盘
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 66d5f70f747a6209f2856afde58b6f486ea597f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386715"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>创建受防护的 Linux VM 模板磁盘

> 适用于：Windows Server 2019、Windows Server （半年频道）、 

本主题介绍如何为可用于实例化一个或多个租户 Vm 的 Linux 受防护 Vm 准备模板磁盘。

## <a name="prerequisites"></a>先决条件

若要准备和测试受防护的 Linux VM，你将需要以下资源：

- 运行 Windows Server 1709 或更高版本的虚拟化 capababilities 的服务器
- 能够运行 Hyper-v 管理器连接到正在运行的 VM 控制台的另一台计算机（Windows 10 或 Windows Server 2016）
- 受支持的 Linux 防护 VM 操作系统之一的 ISO 映像：
    - 带有4.4 内核的 Ubuntu 16.04 LTS
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- 下载 lsvmtools 之前包和 OS 更新的 Internet 访问权限

> [!IMPORTANT]
> 以前的 Linux 操作系统的较新版本可能包含已知的 TPM 驱动程序 bug，该 bug 会阻止将其成功设置为受防护的 Vm。
> 不建议将模板或受防护的 Vm 更新到较新的版本，直到提供修补程序。
> 当更新被公开后，会更新上面受支持的操作系统列表。

## <a name="prepare-a-linux-vm"></a>准备 Linux VM

受防护的 Vm 是从安全模板磁盘创建的。
模板磁盘包含 VM 和元数据的操作系统，包括/boot 和/root 分区的数字签名，以确保在部署之前不修改核心操作系统组件。

若要创建模板磁盘，必须首先创建一个常规（非屏蔽） VM，该 VM 将作为未来受防护 Vm 的基础映像准备就绪。
你安装的软件和对此 VM 所做的配置更改将应用于从此模板磁盘创建的所有受防护的 Vm。
这些步骤将指导你完成获取 Linux VM templatization 的最低要求。

> [!NOTE]
> 对磁盘进行分区时，将配置 Linux 磁盘加密。
> 这意味着，你必须使用 dm-crypt 创建预加密的新 VM，以创建受防护的 Linux VM 模板磁盘。


1.  在虚拟化服务器上，通过在权限提升的 PowerShell 控制台中运行以下命令，确保 Hyper-v 和主机保护者 Hyper-v 支持功能已安装：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  从可信的来源下载 ISO 映像，并将其存储在虚拟化服务器上，或存储在虚拟化服务器可访问的文件共享上。

3.  在运行 Windows Server 版本1709的管理计算机上，通过运行以下命令安装受防护的 VM 远程服务器管理工具：

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  打开管理计算机上的**Hyper-v 管理器**并连接到虚拟化服务器。
    为此，可以单击 "连接到服务器 ..."在 "操作" 窗格中，或通过右键单击 "Hyper-v 管理器" 并选择 "连接到服务器 ..."为 Hyper-v 服务器提供 DNS 名称，并在必要时提供连接到该服务器所需的凭据。

5.  使用 Hyper-v 管理器配置虚拟化服务器上的[外部交换机](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines)，以便 Linux VM 可以访问 Internet 以获取更新。

6.  接下来，创建新的虚拟机以在上安装 Linux 操作系统。
    在 "操作" 窗格中，单击 "**新建** > "**虚拟机**以打开向导。
    为 VM 提供一个友好名称，如 "模板化 Linux"，然后单击 "**下一步**"。

7.  在向导的第二页上，选择 "**第2代**"，以确保使用基于 UEFI 的固件配置文件设置 VM。

8.  根据你的喜好完成向导的剩余部分。
    不要将差异磁盘用于此 VM;受防护的 VM 模板磁盘不能使用差异磁盘。
    最后，将之前下载的 ISO 映像连接到此 VM 的虚拟 DVD 驱动器，以便可以安装操作系统。

9.  在 "Hyper-v 管理器" 中，选择新创建的 VM，并单击 "操作" 窗格中的 "**连接 ...** "，以附加到 VM 的虚拟控制台。
    在出现的窗口中，单击 "**启动**" 以打开虚拟机。

10. 继续执行所选 Linux 分发的设置过程。
    虽然每个 Linux 分发版都使用不同的安装向导，但对于将成为 Linux 受防护 VM 模板磁盘的 Vm，必须满足以下要求：

    - 磁盘必须使用 GUID 分区表（GPT）布局进行分区
    - 根分区必须用 dm dm-crypt。 密码应设置为**密码**（所有小写字母）。 此通行短语是随机的，在设置受防护的 VM 时分区会重新加密。
    - 启动分区必须使用**ext2**文件系统

11. 在 Linux 操作系统完全启动并登录后，建议安装 linux 虚拟内核和关联的 Hyper-v integration services 包。
    此外，你还需要安装 SSH 服务器或其他远程管理工具，以在受防护的情况下访问 VM。

    在 Ubuntu 上，运行以下命令以安装这些组件：

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    在 RHEL 上，改为运行以下命令：

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    在 SLES 上，运行以下命令：

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. 根据需要配置 Linux OS。
    所安装的任何软件、添加的用户帐户以及所做的系统范围的配置更改都将应用于将来通过此模板磁盘创建的所有 Vm。
    应避免将任何机密或不必要的包保存到磁盘。

13. 如果你计划使用 System Center Virtual Machine Manager 来部署 Vm，请在 VM 预配过程中安装 VMM 来宾代理以使 VMM 专用化你的操作系统。
    专用化允许每个 VM 安全地设置不同的用户和 SSH 密钥、网络配置和自定义设置步骤。
    了解如何在 VMM 文档中[获取和安装 vmm 来宾代理](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent)。

14. 接下来，[将 Microsoft Linux 软件存储库添加到你的包管理器](../../administration/linux-package-repository-for-microsoft-software.md)。

15. 使用包管理器安装 lsvmtools 之前包，其中包含 Linux 受防护的 VM 引导程序、预配组件和磁盘准备工具。

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. 自定义 Linux OS 完成后，找到系统上的 lsvmprep 安装程序并运行它。
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. 关闭 VM。

16. 如果你使用了 VM 的任何检查点（包括通过 Windows 10 秋季创意者更新由 Hyper-v 创建的自动检查点），请确保在继续操作之前删除它们。
    检查点创建了模板磁盘向导不支持的差异磁盘（. .avhdx）。
    
    若要删除检查点，请打开**Hyper-v 管理器**，选择你的 VM，右键单击 "检查点" 窗格中最顶层的检查点，然后单击 "**删除检查点" 子树**。

    ![在 Hyper-v 管理器中删除模板 VM 的所有检查点](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>保护模板磁盘

你在上一部分中准备的 VM 几乎可以用作受防护的 Linux VM 模板磁盘。
最后一步是通过模板磁盘向导运行磁盘，该向导将对根和启动分区的当前状态进行哈希和数字签名。
预配受防护的 VM 时，将验证哈希和数字签名，以确保在创建和部署模板之间没有对两个分区进行未经授权的更改。

### <a name="obtain-a-certificate-to-sign-the-disk"></a>获取用于对磁盘进行签名的证书

若要对磁盘度量进行数字签名，需要在将运行模板磁盘向导的计算机上获取证书。
证书必须满足以下要求：

证书属性 | 必需的值
---------------------|---------------
密钥算法 | RSA
最小密钥大小 | 2048位
签名算法 | SHA256 （建议）
密钥用法 | 数字签名

此证书的详细信息将在租户创建其防护数据文件并授权它们信任的磁盘时显示给租户。
因此，请务必从你和你的租户信任的证书颁发机构获取此证书。
在既是宿主又是租户的企业方案中，可能会考虑从企业证书颁发机构颁发此证书。
小心保护此证书，因为拥有此证书的任何人都可以创建与可信磁盘相同的新模板磁盘。

在测试实验室环境中，可以使用以下 PowerShell 命令创建自签名证书：

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>用模板磁盘向导 cmdlet 处理磁盘

将模板磁盘和证书复制到运行 Windows Server 版本1709的计算机，然后运行以下命令以启动签名过程。
你向 `-Path` 参数提供的 VHDX 会被更新的模板磁盘覆盖，因此请确保在运行该命令之前进行复制。

> [!IMPORTANT]
> Windows Server 2016 或 Windows 10 上的可用远程服务器管理工具无法用于准备 Linux 受防护的 VM 模板磁盘。
> 仅使用 Windows Server 2019 版本1709上提供的[TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) cmdlet，或 windows server 上提供的远程服务器管理工具来准备 Linux 受防护的 VM 模板磁盘。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

你的模板磁盘现在可以用于预配受防护的 Linux Vm。
如果使用 System Center Virtual Machine Manager 部署 VM，现在可以将 VHDX 复制到 VMM 库中。

你可能还希望从 VHDX 提取卷签名目录。
此文件用于向要使用模板的 VM 所有者提供有关签名证书、磁盘名称和版本的信息。
他们需要将此文件导入到防护数据文件向导中，以授权你（拥有签名证书的模板作者）为其创建此类和将来的模板磁盘。

若要提取卷签名目录，请在 PowerShell 中运行以下命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
