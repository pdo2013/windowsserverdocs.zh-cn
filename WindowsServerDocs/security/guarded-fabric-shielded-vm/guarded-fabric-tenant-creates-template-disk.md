---
title: 租户的受防护的 Vm-创建模板磁盘-可选
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8e5080dd74506e86687dddb7be0fd35af92f5b56
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403438"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>租户的受防护的 Vm-创建模板磁盘（可选）

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

若要创建新的受防护 VM，你将需要使用经过特别准备的已签名模板磁盘。 已签名模板磁盘中的元数据有助于确保磁盘在创建后不会被修改，并允许你作为租户来限制可用于创建受防护 Vm 的磁盘。 提供此磁盘的一种方法是创建它的租户，如本主题中所述。 

> [!IMPORTANT]
> 如果愿意，可以改为使用托管服务提供商提供的模板磁盘。 如果执行此操作，则必须使用该模板磁盘部署测试 VM，并运行自己的工具（防病毒、漏洞扫描程序等）来验证磁盘是否确实处于你信任的状态。

## <a name="prepare-an-operating-system-vhdx"></a>准备操作系统 VHDX

若要创建受防护的模板磁盘，需要首先准备将通过模板磁盘向导运行的 OS 磁盘。 此磁盘将用作受防护的 Vm 中的 OS 磁盘。 你可以使用任何现有工具创建此磁盘（如 Microsoft Desktop Image Service Manager （DISM）），或者使用空白 VHDX 手动设置 VM，并将操作系统安装到该磁盘上。 设置磁盘时，必须遵守特定于第2代和/或受防护 Vm 的以下要求： 

| VHDX 的要求 | Reason |
|-----------|----|
|必须是 GUID 分区表（GPT）磁盘 | 需要用于第2代虚拟机以支持 UEFI|
|磁盘类型必须是**基本**磁盘，而不是**动态**磁盘。 <br>注意:这是指逻辑磁盘类型，而不是 Hyper-v 支持的 "动态扩展" VHDX 功能。 | BitLocker 不支持动态磁盘。|
|磁盘至少有两个分区。 一个分区必须包含安装 Windows 的驱动器。 这是 BitLocker 将加密的驱动器。 其他分区是活动分区，其中包含引导程序并保持未加密状态，以便可以启动计算机。|BitLocker 需要|
|文件系统为 NTFS | BitLocker 需要|
|在 VHDX 上安装的操作系统是以下项之一：<br>-Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 <br>-Windows 10、Windows 8.1、Windows 8| 需要支持第2代虚拟机和 Microsoft 安全启动模板|
|操作系统必须通用化（运行 sysprep.inf） | 模板预配涉及特定租户工作负荷的专用 Vm| 

> [!NOTE]
> 请勿在此阶段将模板磁盘复制到 VMM 库中。 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>创建 Nano Server 模板磁盘所需的包

如果计划在受防护的 Vm 中将 Nano Server 作为来宾操作系统运行，则必须确保 Nano Server 映像包含以下包：

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>在模板操作系统上运行 Windows 更新

在模板磁盘上，验证操作系统是否安装了所有最新的 Windows 更新。 最近发布的更新会提高端到端防护过程的可靠性，如果模板操作系统不是最新版本，则可能无法完成此过程。

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>通过模板磁盘向导签署和保护 VHDX

若要将模板磁盘与受防护的 Vm 一起使用，必须使用 BitLocker 对该磁盘进行签名和加密。 为此，你将使用受防护的模板磁盘创建向导。 此向导将为磁盘生成哈希，并将其添加到卷签名目录（VSC）。 使用指定的证书对 VSC 进行签名，并在预配过程中使用该证书，以确保为租户部署的磁盘未被更改或替换为租户不信任的磁盘。 最后，BitLocker 安装在磁盘的操作系统上（如果尚未安装），以便在 VM 预配期间为加密准备磁盘。

> [!NOTE]
> 模板磁盘向导将修改就地指定的模板磁盘。 在运行该向导之前，您可能希望复制不受保护的 VHDX，以便以后更新该磁盘。 你将不能修改使用模板磁盘向导保护的磁盘。

在运行 Windows Server 2016 的计算机（不需要是受保护的主机或 VMM 服务器）上执行以下步骤：

1. 将在[准备操作系统 VHDX](#prepare-an-operating-system-vhdx)中创建的通用 VHDX 复制到服务器（如果尚未存在）。

2. 从计算机上的**远程服务器管理工具**安装**受防护的 VM 工具**功能。

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. 获取或创建证书，以对将成为新的受防护 Vm 的模板磁盘的 VHDX 进行签名。 此证书的详细信息将合并到一个防护数据文件中，该文件将磁盘授权为受信任的磁盘。 因此，从你和托管服务提供商信任的证书颁发机构获取此证书非常重要。 在既是宿主又是租户的企业方案中，可能会考虑从 PKI 颁发此证书。

    如果要设置测试环境，而只是想要使用自签名证书对模板磁盘进行签名，请在计算机上运行与以下命令类似的命令：

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. 从 "开始" 菜单上的 "**管理工具**" 文件夹中或在命令提示符下键入**TemplateDiskWizard** ，启动**模板磁盘向导**。

5. 在 "**证书**" 页上，单击 "**浏览**" 以显示证书列表。 选择用于对磁盘模板进行签名的证书。 单击 **“确定”** ，然后单击 **“下一步”** 。

6. 在 "虚拟磁盘" 页上，单击 "**浏览**" 选择已准备的 VHDX，并单击 "**下一步**"。

7. 在 "签名目录" 页上，提供友好**磁盘名称**和**版本。** 提供这些字段是为了帮助你在对磁盘签名后对其进行标识。

    例如，对于**磁盘名称**，可以为**版本**键入_WS2016_ ， _1.0.0.0_

8. 在向导的 "查看设置" 页上查看您的选择。 单击 "**生成**" 后，向导将在模板磁盘上启用 BitLocker，计算磁盘的哈希值，并创建卷签名目录，该目录存储在 VHDX 元数据中。

    等待签名过程完成，然后再尝试装载或移动模板磁盘。 完成此过程可能需要一段时间，具体取决于磁盘的大小。 

9. 在 "**摘要**" 页上，将显示有关磁盘模板、用于签署模板的证书和证书颁发者的信息。 单击 "**关闭**" 退出向导。


如[创建用于定义受防护的 VM 的防护数据](guarded-fabric-tenant-creates-shielding-data.md)中所述，向托管服务提供商和你创建的防护数据文件提供屏蔽磁盘模板。

## <a name="see-also"></a>请参阅

- [部署受防护的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
