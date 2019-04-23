---
title: 创建受防护的 Linux VM 的模板磁盘
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e791d18e027e5e3e1c5b9c52659641ff588a96a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858668"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>创建受防护的 Linux VM 的模板磁盘

> 适用于：Windows Server 2019，Windows Server （半年频道） 

本主题介绍如何进行准备的模板磁盘的 Linux 受防护的 Vm，可以用于实例化一个或多个租户 Vm。

## <a name="prerequisites"></a>先决条件

若要准备和测试受防护的 Linux VM，你将需要以下资源：

- 具有运行 Windows Server 版本 1709年或更高版本的虚拟化 capababilities 的服务器
- 另一台计算机 （Windows 10 或 Windows Server 2016） 能够运行的 HYPER-V 管理器连接到正在运行的 VM 控制台
- ISO 映像的其中一个受支持的 Linux 受防护的 VM 的操作系统：
    - 4.4 内核的 Ubuntu 16.04 LTS
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- 访问 Internet 以下载 lsvmtools 包和操作系统更新

> [!IMPORTANT]
> 较新版本的 Linux 操作系统可能包括一个已知的 TPM 驱动程序 bug，将阻止其成功预配为上述受防护的 Vm。
> 不建议你更新你的模板或可用修补程序之前，受防护到较新版本的 Vm。
> 公共进行更新时，将更新的更高版本的受支持操作系统列表。

## <a name="prepare-a-linux-vm"></a>准备 Linux VM

从安全模板磁盘创建受防护的 Vm。
模板磁盘包含操作系统的 VM 和元数据，包括数字签名 /boot 和 /root 分区上，以确保在部署前不会修改核心操作系统组件。

若要创建模板磁盘，必须首先创建将在未来受防护的 Vm 为基础的映像准备的常规 （非屏蔽） VM。
软件安装和配置对此 VM 所做的更改将应用于所有从此模板磁盘创建的受防护的 Vm。
这些步骤将引导您完成裸机的最低要求若要获取 Linux VM 准备好进行操作。

> [!NOTE]
> 该磁盘进行分区时，配置 Linux 磁盘加密。
> 这意味着，必须创建新的 VM 使用 dm-crypt 若要创建 Linux 的预先加密的受防护的 VM 模板磁盘。


1.  在虚拟化服务器上，请确保通过在提升的 PowerShell 控制台中运行以下命令安装 HYPER-V 和主机保护者 HYPER-V 支持功能：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  来自可信来源下载该 ISO 映像，并将其存储在虚拟化服务器，或文件共享上的虚拟化服务器可以访问。

3.  在管理计算机上运行 Windows Server 版本 1709年，安装受防护的 VM 远程服务器管理工具通过运行以下命令：

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  打开**Hyper-v 管理器**管理计算机上并连接到虚拟化服务器。
    您可以执行此操作通过单击"连接到服务器..."在操作窗格中或通过右键单击 Hyper-v 管理器并选择"连接到服务器..."提供你的 HYPER-V 的服务器的 DNS 名称，并如有必要，为所需的凭据连接到它。

5.  使用 HYPER-V 管理器[配置外部交换机](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines)上虚拟化服务器以便 Linux VM 可以访问 Internet 以获取更新。

6.  接下来，创建新的虚拟机安装到 Linux OS。
    在操作窗格中单击**新建** > **虚拟机**以便启动该向导。
    提供你的 VM，如"Pre-templatized Linux"的友好名称，然后单击**下一步**。

7.  在向导的第二个页面上，选择**第 2 代**以确保与基于 UEFI 的固件配置文件预配 VM。

8.  完成向导根据您的首选项的其他部分。
    为此 VM; 不使用差异磁盘受防护的 VM 模板磁盘不能使用差异磁盘。
    最后，连接之前下载到虚拟 DVD 驱动器为此虚拟机，以便可以安装操作系统的 ISO 映像。

9.  在 HYPER-V 管理器中，选择你新创建的 VM，然后单击**连接...** 将附加到 VM 的虚拟控制台操作窗格中。
    在出现的窗口，单击**启动**开启虚拟机。

10. 通过所选的 Linux 分发的安装过程继续执行。
    每个 Linux 分发版使用不同的安装向导，而对于虚拟机将成为 Linux 受防护的 VM 模板磁盘必须满足以下要求：

    - 必须使用 GUID 分区表 (GPT) 布局对磁盘进行分区
    - 根分区必须使用 dm 加密进行加密。 通行短语应设置为**通行短语**（全部小写）。 此密码将随机和分区重新加密时预配受防护的 VM。
    - 启动分区必须使用**ext2**文件系统

11. 一旦 Linux OS 已完全启动，并且已登录，则建议您安装的 linux 虚拟内核和关联的 HYPER-V integration services 包。
    此外，想要安装 SSH 服务器或其他远程管理工具来访问受防护的 VM。

    在 Ubuntu 上，运行以下命令以安装这些组件：

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    在 RHEL、 改为运行以下命令：

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    和上 SLES，运行以下命令：

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. 根据需要配置你的 Linux 操作系统。
    任何软件安装时，你添加的用户帐户和系统范围内所做的配置更改将应用于从该模板磁盘创建的所有将来的 Vm。
    应避免将任何机密或不必要的包保存到磁盘。

13. 如果想要使用 System Center Virtual Machine Manager 来部署 Vm，安装 VMM 来宾代理，以使 VMM 能够使您的操作系统专用化 VM 预配期间。
    专用化允许使用不同的用户和 SSH 密钥、 网络配置和自定义安装程序步骤安全地设置每个 VM。
    了解如何[获取并安装 VMM 来宾代理](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent)VMM 文档中。

14. 下一步，[将 Microsoft Linux 软件存储库添加到包管理器](../../administration/linux-package-repository-for-microsoft-software.md)。

15. 使用包管理器，安装 lsvmtools 之前包，其中包含 Linux 受防护的 VM 引导加载程序填充程序，预配组件和磁盘准备工具。

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. 完成后自定义 Linux OS，你的系统上找到 lsvmprep 安装程序并运行它。
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. 关闭 VM。

16. 如果花费了 VM （包括创建的 HYPER-V 与 Windows 10 Fall Creators Update 的自动检查点） 的任何检查点，请确保将其删除然后再继续。
    检查点创建差异磁盘 (.avhdx) 不支持的模板磁盘向导。
    
    若要删除检查点，请打开**Hyper-v 管理器**，选择你的 VM，右键单击检查点窗格中，在最顶层的检查点，然后单击**删除检查点的子树**。

    ![删除模板 VM 的 HYPER-V 管理器中的所有检查的点](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>保护模板磁盘

在上一节中准备的 VM 是几乎可供使用和 Linux 受防护的 VM 模板磁盘。
最后一步是运行磁盘通过模板磁盘向导，它将哈希和数字签名的根和启动分区的当前状态。
预配受防护的 VM 以确保任何未经授权的更改已对模板的创建和部署之间的两个分区进行时验证哈希和数字签名。

### <a name="obtain-a-certificate-to-sign-the-disk"></a>获取要对磁盘签名的证书

若要进行数字签名磁盘度量，需要获取计算机上的证书模板磁盘向导在运行。
证书必须满足以下要求：

证书属性 | 所需的值
---------------------|---------------
密钥算法 | RSA
最小密钥大小 | 2048 位
签名算法 | SHA256 （推荐）
密钥用法 | 数字签名

在创建其防护数据文件和磁盘他们相信这将授权时，将向租户显示有关此证书的详细信息。
因此，务必要从您和您的租户相互信任的证书颁发机构获取此证书。
在企业方案中你所在的主机和租户，可以考虑颁发此证书从企业证书颁发机构。
小心地保护此证书，因为此证书的所有权的任何人都可以创建新的模板磁盘的受信任的可信磁盘相同。

在测试实验室环境中，可以使用以下 PowerShell 命令来创建自签名的证书：

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>过程模板磁盘向导 cmdlet 的磁盘

将你的模板磁盘和证书复制到运行 Windows Server 版本 1709 的计算机，然后运行以下命令来启动签名过程。
您向的 VHDX`-Path`参数将被覆盖具有已更新的模板磁盘，因此请确保在运行该命令之前创建一个副本。

> [!IMPORTANT]
> Windows Server 2016 或 Windows 10 上提供的远程服务器管理工具不能用于准备受防护的 Linux VM 的模板磁盘。
> 只能使用[保护 TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) cmdlet 可在上找到 Windows Server、 1709年版或 Windows Server 2019 上可用的远程服务器管理工具来准备 Linux 受防护的 VM 模板磁盘。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

模板磁盘现在已准备好预配 Linux 受防护的 Vm 使用。
如果使用 System Center Virtual Machine Manager 来部署 VM，可以现在将 VHDX 复制到 VMM 库中。

您可能想要从 VHDX 提取卷签名目录。
此文件用于向希望使用您的模板 VM 所有者提供的签名证书、 磁盘名称和版本信息。
他们需要此文件导入屏蔽数据文件向导来将你拥有签名证书，若要创建此模板作者授权和未来的模板磁盘为它们。

若要提取卷签名目录，请在 PowerShell 中运行以下命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
