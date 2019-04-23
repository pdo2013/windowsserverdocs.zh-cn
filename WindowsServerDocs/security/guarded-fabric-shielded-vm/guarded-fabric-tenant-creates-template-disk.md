---
title: 租户-创建模板磁盘-可选的受防护的 Vm
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2709c84a16dadc2211af4a6f5b43c13266ede155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874628"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>租户-创建模板磁盘 （可选） 为受防护的 Vm

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

若要创建新的受防护的 VM，你将需要使用专门已准备的、 已签名模板磁盘。 从已签名的模板磁盘的元数据有助于确保磁盘不会修改已创建后，可以如租户来限制哪些磁盘可用于创建受防护的 Vm。 提供此磁盘的一种方法是为你的租户，来创建它，如本主题中所述。 

> [!IMPORTANT]
> 如果您愿意，可以改为使用托管服务提供商提供的模板磁盘。 如果这样做，务必要部署 VM 使用该模板磁盘，并运行你自己的工具 （如防病毒软件、 漏洞扫描程序等），以验证磁盘实际上是，在您信任的状态的测试。

## <a name="prepare-an-operating-system-vhdx"></a>准备操作系统 VHDX

若要创建受防护的模板磁盘，需要先准备将通过模板磁盘向导运行的 OS 磁盘。 此磁盘将用作 OS 磁盘中受防护的 Vm。 可以使用任何现有的工具来创建该磁盘，如 Microsoft 桌面映像服务管理器 (DISM)，或手动设置具有空白 VHDX 的 VM 并安装到该磁盘上的操作系统。 时设置该磁盘，它必须遵循特定于第 2 代和/或受防护的 Vm 的以下要求： 

| 有关 VHDX 的要求 | 原因 |
|-----------|----|
|必须为 GUID 分区表 (GPT) 磁盘 | 所需的第 2 代虚拟机，从而支持 UEFI|
|磁盘类型必须是**基本**而不是**动态**。 <br>注意：这是指逻辑磁盘类型，不"动态扩展"VHDX 支持的功能的 HYPER-V。 | BitLocker 不支持动态磁盘。|
|磁盘具有至少两个分区。 一个分区必须包含在其安装 Windows 的驱动器。 这是 BitLocker 将加密的驱动器。 另一个分区是活动分区，其中包含引导加载程序并保持未加密状态，以便可以启动计算机。|所需的 BitLocker|
|NTFS 是文件系统 | 所需的 BitLocker|
|安装在 VHDX 上的操作系统是以下值之一：<br>Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 <br>Windows 10，Windows 8.1，Windows 8| 支持第 2 代虚拟机和 Microsoft 安全启动模板所需|
|操作系统必须通用化 (运行的 sysprep.exe) | 模板预配涉及到专门针对特定租户的工作负荷的 Vm| 

> [!NOTE]
> 不要复制模板磁盘到 VMM 库在此阶段。 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>所需的包创建 Nano Server 的模板磁盘

如果想要运行 Nano Server 作为来宾操作系统中受防护的 Vm，则必须确保 Nano Server 映像包括以下包：

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>模板操作系统上运行 Windows 更新

模板磁盘上验证操作系统具有所有最新的 Windows 更新安装。 最近发布的更新来提高端到端屏蔽过程的可靠性-可能会失败时的完整模板操作系统的进程不是最新。

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>签名和保护的模板磁盘向导使用的 VHDX

若要使用受防护的 Vm 的模板磁盘，必须签名并使用 BitLocker 加密磁盘。 若要执行此操作，将使用受防护的模板磁盘创建向导。 此向导将生成该磁盘的哈希值并将其添加到卷签名目录 (VSC)。 VSC 使用指定和预配过程中用于确保将磁盘正在部署的租户尚未更改或替换为租户不信任的磁盘的证书进行签名。 最后，BitLocker 安装磁盘的操作系统上 （如果尚不存在） 来加密 VM 预配过程准备磁盘。

> [!NOTE]
> 模板磁盘向导将修改指定位置中的模板磁盘。 您可能想要在运行向导后，若要在更高版本时对磁盘进行更新之前制作一份未受保护的 VHDX。 你将不能修改受保护的模板磁盘向导使用的磁盘。

运行 Windows Server 2016 （不需要为受保护的主机或 VMM 服务器） 的计算机上执行以下步骤：

1. 复制中创建的通用的 VHDX[准备操作系统 VHDX](#prepare-an-operating-system-vhdx)到服务器，如果尚不存在。

2. 安装**受防护的 VM 工具**功能从**远程服务器管理工具**在计算机上。

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. 获取或创建证书进行签名，VHDX 可将成为新的受防护 Vm 的模板磁盘。 有关此证书的详细信息将合并到防护数据文件，该帐户作为受信任的磁盘的磁盘。 因此，务必要获取你和你托管服务提供方信任此证书从证书颁发机构。 在企业方案中你所在的主机和租户，可以考虑从你的 PKI 颁发此证书。

    如果您正在设置测试环境，并且只是想要使用自签名的证书进行签名模板磁盘，在计算机上运行的命令如下所示：

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. 启动**模板磁盘向导**从**管理工具**在开始菜单上或通过键入文件夹**TemplateDiskWizard.exe**到命令提示符。

5. 上**证书**页上，单击**浏览**以显示证书的列表。 选择用于对磁盘模板的证书。 单击 **“确定”**，然后单击 **“下一步”**。

6. 在虚拟磁盘页上，单击**浏览**若要选择已准备好的 VHDX，然后单击**下一步**。

7. 在签名目录页上，提供一个友好**磁盘名称**和**版本。** 这些字段用来帮助你确定磁盘后已签名。

    例如，对于**磁盘名称**您可以键入_WS2016_对于**版本**， _1.0.0.0_

8. 检查在向导的查看设置页上所做选择。 当您单击**生成**，向导将模板磁盘上启用 BitLocker、 计算的哈希的磁盘，并创建卷签名目录，它存储在 VHDX 元数据。

    等待，直到然后再尝试装载或移动模板磁盘签名过程已完成。 此过程可能需要一段时间才能完成，具体取决于磁盘的大小。 

9. 上**摘要**页上，有关磁盘模板，使用模板进行签名的证书信息和证书颁发者所示。 单击**关闭**以退出向导。


受防护的磁盘模板向托管服务提供商，以及你所创建的防护数据文件中所述[创建防护数据来定义受防护的 VM](guarded-fabric-tenant-creates-shielding-data.md)。

## <a name="see-also"></a>请参阅

- [部署受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的构造和受防护的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
