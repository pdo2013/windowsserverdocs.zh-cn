---
title: 创建受防护的 Windows VM 模板磁盘
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: 686fd2ed5969d191240bbd726f1d759e9974f08a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386678"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>创建受防护的 Windows VM 模板磁盘

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

与常规 Vm 一样，你可以[在 Virtual Machine Manager （VMM）中](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)创建 vm 模板（例如，vm 模板），使租户和管理员可以轻松地使用模板磁盘在构造上部署新 vm。 由于受防护的 Vm 是安全敏感资产，因此还需要执行其他步骤来创建支持防护的 VM 模板。 本主题介绍在 VMM 中创建受防护的模板磁盘和 VM 模板的步骤。

若要了解本主题如何适应部署受防护的 Vm 的整个过程，请参阅[为受保护的主机和受防护的 Vm 托管服务提供商配置步骤](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="prepare-an-operating-system-vhdx"></a>准备操作系统 VHDX

首先准备要通过受防护的模板磁盘创建向导运行的 OS 磁盘。 此磁盘将用作租户的 Vm 中的 OS 磁盘。 你可以使用任何现有工具创建此磁盘（如 Microsoft Desktop Image Service Manager （DISM）），或者使用空白 VHDX 手动设置 VM，并将操作系统安装到该磁盘上。 设置磁盘时，必须遵守特定于第2代和/或受防护 Vm 的以下要求： 

| VHDX 的要求 | Reason |
|-----------|----|
|必须是 GUID 分区表（GPT）磁盘 | 需要用于第2代虚拟机以支持 UEFI|
|磁盘类型必须是**基本**磁盘，而不是**动态**磁盘。 <br>注意:这是指逻辑磁盘类型，而不是 Hyper-v 支持的 "动态扩展" VHDX 功能。 | BitLocker 不支持动态磁盘。|
|磁盘至少有两个分区。 一个分区必须包含安装 Windows 的驱动器。 这是 BitLocker 将加密的驱动器。 其他分区是活动分区，其中包含引导程序并保持未加密状态，以便可以启动计算机。|BitLocker 需要|
|文件系统为 NTFS | BitLocker 需要|
|在 VHDX 上安装的操作系统是以下项之一：<br>-Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 <br>-Windows 10、Windows 8.1、Windows 8| 需要支持第2代虚拟机和 Microsoft 安全启动模板|
|操作系统必须通用化（运行 sysprep.inf） | 模板预配涉及特定租户工作负荷的专用 Vm| 

> [!NOTE]
> 如果使用 VMM，请不要在此阶段将模板磁盘复制到 VMM 库中。 

## <a name="run-windows-update-on-the-template-operating-system"></a>在模板操作系统上运行 Windows 更新

在模板磁盘上，验证操作系统是否安装了所有最新的 Windows 更新。 最近发布的更新会提高端到端防护过程的可靠性，如果模板操作系统不是最新版本，则可能无法完成此过程。

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>利用模板磁盘向导准备和保护 VHDX

若要将模板磁盘与受防护的 Vm 一起使用，必须使用受防护的模板磁盘创建向导来准备磁盘并使用 BitLocker 对其进行加密。 此向导将为磁盘生成哈希，并将其添加到卷签名目录（VSC）。 使用指定的证书对 VSC 进行签名，并在预配过程中使用该证书，以确保为租户部署的磁盘未被更改或替换为租户不信任的磁盘。 最后，BitLocker 安装在磁盘的操作系统上（如果尚未安装），以便在 VM 预配期间为加密准备磁盘。

> [!NOTE]
> 模板磁盘向导将修改就地指定的模板磁盘。 在运行该向导之前，您可能希望复制不受保护的 VHDX，以便以后更新该磁盘。 你将不能修改使用模板磁盘向导保护的磁盘。

在运行 Windows Server 2016 的计算机（不需要是受保护的主机或 VMM 服务器）上执行以下步骤：

1. 将在[准备操作系统 VHDX](#prepare-an-operating-system-vhdx)中创建的通用 VHDX 复制到服务器（如果尚未存在）。

2. 若要在本地管理服务器，请从服务器上的**远程服务器管理工具**安装**受防护的 VM 工具**功能。

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    你还可以从已安装[Windows 10 远程服务器管理工具](https://www.microsoft.com/en-us/download/details.aspx?id=45520)的客户端计算机管理服务器。

3. 获取或创建一个证书，以便为要成为新的受防护 Vm 的模板磁盘的 VHDX 签名 VSC。 此证书的详细信息将在租户创建其防护数据文件并授权它们信任的磁盘时显示给租户。 因此，请务必从你和你的租户信任的证书颁发机构获取此证书。 在既是宿主又是租户的企业方案中，可能会考虑从 PKI 颁发此证书。

    如果要设置测试环境，而只是想要使用自签名证书来准备模板磁盘，请运行类似于以下内容的命令：

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. 从 "开始" 菜单上的 "**管理工具**" 文件夹中或在命令提示符下键入**TemplateDiskWizard** ，启动**模板磁盘向导**。

5. 在 "**证书**" 页上，单击 "**浏览**" 以显示证书列表。 选择要用于准备磁盘模板的证书。 单击 **“确定”** ，然后单击 **“下一步”** 。

6. 在 "虚拟磁盘" 页上，单击 "**浏览**" 选择已准备的 VHDX，并单击 "**下一步**"。

7. 在 "签名目录" 页上，提供友好**磁盘名称**和**版本。** 提供这些字段是为了帮助你在准备好磁盘后对其进行标识。

    例如，对于**磁盘名称**，可以为**版本**键入_WS2016_ ， _1.0.0.0_

8. 在向导的 "查看设置" 页上查看您的选择。 单击 "**生成**" 后，向导将在模板磁盘上启用 BitLocker，计算磁盘的哈希值，并创建卷签名目录，该目录存储在 VHDX 元数据中。

    等待准备过程完成，然后再尝试装载或移动模板磁盘。 完成此过程可能需要一段时间，具体取决于磁盘的大小。

    > [!IMPORTANT]
    > 模板磁盘只能用于安全防护的 VM 预配过程。
    > 尝试使用模板磁盘启动常规（非屏蔽） VM 可能会导致停止错误（蓝屏）并且不受支持。

9. 在 "**摘要**" 页上，将显示有关磁盘模板、用于对 VSC 进行签名的证书和证书颁发者的信息。 单击 "**关闭**" 退出向导。

如果使用 VMM，请按照本主题的其余部分中的步骤将模板磁盘合并到 VMM 中的受防护的 VM 模板。 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>将模板磁盘复制到 VMM 库

如果使用 VMM，则在创建模板磁盘后，需要将其复制到 VMM 库共享中，以便主机可以在预配新 Vm 时下载并使用该磁盘。 使用以下过程将模板磁盘复制到 VMM 库中，然后刷新库。

1. 将 VHDX 文件复制到 VMM 库共享文件夹。 如果使用了默认的 VMM 配置，请将模板磁盘复制到 _\\ @ no__t-2\MSSCVMMLibrary\VHDs_。

2. 刷新库服务器。 打开 "**库**" 工作区，展开 "**库服务器**"，右键单击要刷新的库服务器，然后单击 "**刷新**"。

3. 接下来，为 VMM 提供有关模板磁盘上所安装操作系统的信息：

    a. 在库服务器上的 "**库**" 工作区中找到新导入的模板磁盘。

    b. 右键单击该磁盘，然后单击 "**属性**"。

    c. 对于 "**操作系统**"，请展开列表，并选择磁盘上安装的操作系统。 选择操作系统将向 VMM 指示 VHDX 不为空。

    d. 更新了属性之后，单击“确定”。

磁盘名称旁边的小盾牌图标将磁盘表示为受防护的 Vm 的准备好的模板磁盘。 也可以右键单击列标题，并切换**受防护**的列，以查看指示磁盘是用于常规 VM 部署还是受防护 VM 部署的文本表示形式。

![受防护的 vm 模板磁盘](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>使用准备好的模板磁盘在 VMM 中创建受防护的 VM 模板

使用 VMM 库中的已准备好的模板磁盘，你可以为受防护的 Vm 创建 VM 模板。 受防护的 vm 的 VM 模板略有不同于传统的 VM 模板，因为某些设置是固定的（已启用第2代 VM、UEFI 和安全启动等），而其他设置不可用（租户自定义仅限于少数几个选择 VM 的属性）. 若要创建 VM 模板，请执行以下步骤：

1. 在 "**库**" 工作区中，单击顶部 "主文件夹" 选项卡上的 "**创建 VM 模板**"。

2. 在“选择源”页上，单击“使用现有 VM 模板或库中存储的虚拟硬盘”，然后单击“浏览”。

3. 在出现的窗口中，从 VMM 库中选择一个准备好的模板磁盘。 若要更轻松地识别哪些磁盘已准备就绪，请右键单击列标题，并启用**受防护**的列。 单击 **"确定"** **，然后**单击 "确定"。

4. 指定 VM 模板名称和说明（可选），然后单击 "**下一步**"。

5. 在 "**配置硬件**" 页上，指定从此模板创建的 vm 的功能。 确保 VM 模板上至少有一个可用的 NIC。 租户连接到受防护的 VM 的唯一方式是通过远程桌面连接、Windows 远程管理或其他通过网络协议工作的预配置远程管理工具。

    如果选择在 VMM 中利用静态 IP 池，而不是在租户网络上运行 DHCP 服务器，则需要向此配置发出警报。 当租户提供其防护数据文件（其中包含 VMM 的无人参与文件）时，他们将需要为静态 IP 池信息提供特殊的占位符值。 有关租户无人参与文件中的 VMM 占位符的详细信息，请参阅[创建应答文件](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)。 

6. 在 "**配置操作系统**" 页上，VMM 将仅为受防护的 vm 显示几个选项，包括产品密钥、时区和计算机名称。 某些安全信息（如管理员密码和域名）由租户通过防护数据文件（。PDK 文件）。 

    > [!NOTE]
    > 如果选择在此页上指定产品密钥，请确保其对于模板磁盘上的操作系统有效。 如果使用了不正确的产品密钥，则 VM 创建将失败。

创建模板后，租户可以使用它来创建新的虚拟机。 需要验证 VM 模板是否为租户管理员用户角色可用的资源之一（在 VMM 中，用户角色位于 "**设置**" 工作区中）。

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>使用 PowerShell 准备和保护 VHDX

作为运行模板磁盘向导的替代方法，可以将模板磁盘和证书复制到运行 RSAT 的计算机，并 @no__t 运行 0Protect-TemplateDisk @ no__t 来启动签名过程。
下面的示例使用_TemplateName_和_version_参数指定的名称和版本信息。
你向 `-Path` 参数提供的 VHDX 会被更新的模板磁盘覆盖，因此请确保在运行该命令之前进行复制。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

你的模板磁盘现在可以用于预配受防护的 Vm。
如果使用 System Center Virtual Machine Manager 部署 VM，现在可以将 VHDX 复制到 VMM 库中。

你可能还希望从 VHDX 提取卷签名目录。
此文件用于向要使用模板的 VM 所有者提供有关签名证书、磁盘名称和版本的信息。
他们需要将此文件导入到防护数据文件向导中，以授权你（拥有签名证书的模板作者）为其创建此类和将来的模板磁盘。

若要提取卷签名目录，请在 PowerShell 中运行以下命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [创建防护数据文件](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>请参阅

- [受保护的主机和受防护的 Vm 的托管服务提供商配置步骤](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
