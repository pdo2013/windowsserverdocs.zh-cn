---
title: 创建 Windows 受防护的 VM 模板磁盘
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: d9f07d2e6e93d4f8d198c2fc3b62c28c940bdefb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447510"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>创建 Windows 受防护的 VM 模板磁盘

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

正如与常规 Vm，可以创建 VM 模板 (例如， [Virtual Machine Manager (VMM) 中的 VM 模板](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) 以使部署新 Vm 使用的模板磁盘在结构上的租户和管理员可以更轻松。 受防护的 Vm 都是安全敏感资产，因为有其他步骤来创建支持防护的 VM 模板。 本主题介绍在 VMM 中创建受防护的模板磁盘和 VM 模板的步骤。

若要了解本主题如何适应总体部署受防护的 Vm 的过程，请参阅[托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="prepare-an-operating-system-vhdx"></a>准备操作系统 VHDX

首先准备 OS 磁盘，然后，将运行受防护的模板磁盘创建向导。 此磁盘将用作你的租户的 Vm 中的 OS 磁盘。 可以使用任何现有的工具来创建该磁盘，如 Microsoft 桌面映像服务管理器 (DISM)，或手动设置具有空白 VHDX 的 VM 并安装到该磁盘上的操作系统。 时设置该磁盘，它必须遵循特定于第 2 代和/或受防护的 Vm 的以下要求： 

| 有关 VHDX 的要求 | Reason |
|-----------|----|
|必须为 GUID 分区表 (GPT) 磁盘 | 所需的第 2 代虚拟机，从而支持 UEFI|
|磁盘类型必须是**基本**而不是**动态**。 <br>注意：这是指逻辑磁盘类型，不"动态扩展"VHDX 支持的功能的 HYPER-V。 | BitLocker 不支持动态磁盘。|
|磁盘具有至少两个分区。 一个分区必须包含在其安装 Windows 的驱动器。 这是 BitLocker 将加密的驱动器。 另一个分区是活动分区，其中包含引导加载程序并保持未加密状态，以便可以启动计算机。|所需的 BitLocker|
|NTFS 是文件系统 | 所需的 BitLocker|
|安装在 VHDX 上的操作系统是以下值之一：<br>Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 <br>Windows 10，Windows 8.1，Windows 8| 支持第 2 代虚拟机和 Microsoft 安全启动模板所需|
|操作系统必须通用化 (运行的 sysprep.exe) | 模板预配涉及到专门针对特定租户的工作负荷的 Vm| 

> [!NOTE]
> 如果使用 VMM，不要复制模板磁盘到 VMM 库在此阶段。 

## <a name="run-windows-update-on-the-template-operating-system"></a>模板操作系统上运行 Windows 更新

模板磁盘上验证操作系统具有所有最新的 Windows 更新安装。 最近发布的更新来提高端到端屏蔽过程的可靠性-可能会失败时的完整模板操作系统的进程不是最新。

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>准备和保护的模板磁盘向导使用的 VHDX

若要使用受防护的 Vm 的模板磁盘，磁盘，必须准备并使用 BitLocker 加密的使用受防护的模板磁盘创建向导。 此向导将生成该磁盘的哈希值并将其添加到卷签名目录 (VSC)。 VSC 使用指定和预配过程中用于确保将磁盘正在部署的租户尚未更改或替换为租户不信任的磁盘的证书进行签名。 最后，BitLocker 安装磁盘的操作系统上 （如果尚不存在） 来加密 VM 预配过程准备磁盘。

> [!NOTE]
> 模板磁盘向导将修改指定位置中的模板磁盘。 您可能想要在运行向导后，若要在更高版本时对磁盘进行更新之前制作一份未受保护的 VHDX。 你将不能修改受保护的模板磁盘向导使用的磁盘。

运行 Windows Server 2016 （不需要为受保护的主机或 VMM 服务器） 的计算机上执行以下步骤：

1. 复制中创建的通用的 VHDX[准备操作系统 VHDX](#prepare-an-operating-system-vhdx)到服务器，如果尚不存在。

2. 若要管理本地服务器，安装**受防护的 VM 工具**功能从**远程服务器管理工具**在服务器上。

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    您还可以管理已安装的客户端计算机的服务器[远程服务器管理工具功能的 Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=45520)。

3. 获取或创建的证书进行签名，VHDX 可将成为新的受防护 Vm 的模板磁盘 VSC。 在创建其防护数据文件和磁盘他们相信这将授权时，将向租户显示有关此证书的详细信息。 因此，务必要从您和您的租户相互信任的证书颁发机构获取此证书。 在企业方案中你所在的主机和租户，可以考虑从你的 PKI 颁发此证书。

    如果您正在设置测试环境，并且只是想要使用自签名的证书准备你的模板磁盘，运行类似于下面的命令：

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. 启动**模板磁盘向导**从**管理工具**在开始菜单上或通过键入文件夹**TemplateDiskWizard.exe**到命令提示符。

5. 上**证书**页上，单击**浏览**以显示证书的列表。 选择用来准备磁盘模板的证书。 单击 **“确定”** ，然后单击 **“下一步”** 。

6. 在虚拟磁盘页上，单击**浏览**若要选择已准备好的 VHDX，然后单击**下一步**。

7. 在签名目录页上，提供一个友好**磁盘名称**和**版本。** 这些字段用来帮助你确定磁盘后已准备好。

    例如，对于**磁盘名称**您可以键入_WS2016_对于**版本**， _1.0.0.0_

8. 检查在向导的查看设置页上所做选择。 当您单击**生成**，向导将模板磁盘上启用 BitLocker、 计算的哈希的磁盘，并创建卷签名目录，它存储在 VHDX 元数据。

    等待，直到准备过程已完成之前尝试装载或移动模板磁盘。 此过程可能需要一段时间才能完成，具体取决于磁盘的大小。

    > [!IMPORTANT]
    > 模板磁盘仅用于安全的受防护 VM 预配过程。
    > 尝试启动一个常规 （非屏蔽） 使用的模板磁盘的 VM 将可能会导致停止错误 （蓝屏） 和不受支持。

9. 上**摘要**页上，有关磁盘模板，用于对 VSC，进行签名的证书信息和证书颁发者所示。 单击**关闭**以退出向导。

如果使用 VMM，请按照本主题，以将模板磁盘合并到在 VMM 中受防护的 VM 模板中的剩余部分中的步骤。 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>将模板磁盘复制到 VMM 库

如果您使用 VMM 中，在创建模板磁盘后，需要将其复制到 VMM 库共享，以便主机可以下载并预配新 Vm 时使用该磁盘。 使用以下过程将模板磁盘复制到 VMM 库，然后刷新库。

1. 将 VHDX 文件复制到 VMM 库共享文件夹。 如果你使用默认 VMM 配置，请将复制的模板磁盘。  _\\ <vmmserver>\MSSCVMMLibrary\VHDs_。

2. 刷新库服务器。 打开**库**工作区中，展开**库服务器**，右键单击你想要刷新，请在库服务器上，单击**刷新**。

3. 接下来，向 VMM 提供有关安装的模板磁盘上的操作系统的信息：

    a. 在您的库服务器上找到新导入的模板磁盘**库**工作区。

    b. 右键单击该磁盘，然后单击**属性**。

    c. 有关**操作系统**，展开列表并选择安装在磁盘上的操作系统。 选择操作系统向 VMM 指示 VHDX 不为空。

    d. 更新了属性之后，单击“确定”  。

磁盘的名称旁边的小盾牌图标表示该磁盘为准备好的模板磁盘为受防护的 Vm。 您还可以右键单击列标题和切换**防护**列可以看到，该值指示是否磁盘适用于常规或受防护的 VM 部署的文本表示形式。

![受防护的 vm 模板磁盘](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>在使用准备好的模板磁盘的 VMM 中创建受防护的 VM 模板

VMM 库中包含准备好的模板磁盘，已准备好为受防护的 Vm 创建 VM 模板。 受防护的 Vm 的虚拟机模板略有不同传统虚拟机模板，因为某些设置固定的 （生成启用虚拟机、 UEFI 和安全启动，等等），而有些不可用 （租户自定义仅限于少数，选择 VM 的属性）. 若要创建 VM 模板，请执行以下步骤：

1. 在中**库**工作区中，单击**创建 VM 模板**顶部的主页选项卡上。

2. 在“选择源”  页上，单击“使用现有 VM 模板或库中存储的虚拟硬盘”  ，然后单击“浏览”  。

3. 在窗口中显示，从 VMM 库中选择准备好的模板磁盘。 若要更轻松地识别哪些磁盘准备，右键单击列标题，并启用**防护**列。 单击**确定**然后**下一步**。

4. 指定 VM 模板名称和可选说明，然后依次**下一步**。

5. 上**配置硬件**页上，指定的此模板创建的 Vm 的功能。 请确保至少一个 NIC 可用并且配置上的 VM 模板。 若要连接到受防护的 VM 的租户的唯一方法是通过远程桌面连接、 Windows 远程管理或通过网络协议适用其他预配置远程管理工具。

    如果您选择而不是租户网络上运行的 DHCP 服务器利用在 VMM 中的静态 IP 池，将需要提醒您对此配置的租户。 租户提供 vmm 包含无人参与文件，其防护数据文件时，他们需要为静态 IP 池信息提供特殊的占位符值。 有关租户无人参与文件中的 VMM 占位符的详细信息，请参阅[创建一个答案文件](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)。 

6. 上**配置操作系统**页上，VMM 将仅显示受防护的 vm，包括产品密钥、 时区，以及计算机名称的几个选项。 通过屏蔽数据文件的一些安全信息，例如管理员密码和域的名称、 指定租户 (。PDK 文件）。 

    > [!NOTE]
    > 如果选择此页上指定产品密钥，请确保它是有效的模板磁盘上的操作系统。 如果使用了不正确的产品密钥，则创建 VM 的操作将失败。

创建模板后，租户可以使用它来创建新虚拟机。 将需要确保虚拟机模板是租户管理员用户角色可用的资源 (在 VMM 中，用户角色是在**设置**工作区)。

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>准备和保护使用 PowerShell 的 VHDX

作为运行模板磁盘向导的替代方法，你可以将你的模板磁盘和证书复制到运行 RSAT 的计算机并运行[保护 TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
)启动签名过程。
下面的示例使用由指定的名称和版本信息_TemplateName_并_版本_参数。
您向的 VHDX`-Path`参数将被覆盖具有已更新的模板磁盘，因此请确保在运行该命令之前创建一个副本。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

模板磁盘现在已准备好用于预配受防护的 Vm。
如果使用 System Center Virtual Machine Manager 来部署 VM，可以现在将 VHDX 复制到 VMM 库中。

您可能想要从 VHDX 提取卷签名目录。
此文件用于向希望使用您的模板 VM 所有者提供的签名证书、 磁盘名称和版本信息。
他们需要此文件导入屏蔽数据文件向导来将你拥有签名证书，若要创建此模板作者授权和未来的模板磁盘为它们。

若要提取卷签名目录，请在 PowerShell 中运行以下命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [创建防护数据文件](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>请参阅

- [托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
