---
title: 租户的受防护的 Vm-创建屏蔽数据来定义受防护的 VM
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 86047420cb4b1095d5715739d76daa3dba3ff5d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403456"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>租户的受防护的 Vm-创建屏蔽数据来定义受防护的 VM

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

屏蔽数据文件（也称为预配数据文件或 PDK 文件）是加密文件，由租户或 VM 所有者创建，用于保护重要的 VM 配置信息，例如管理员密码、RDP 和其他标识相关的证书，域加入凭据等。 本主题提供有关如何创建防护数据文件的信息。 创建文件之前，必须从托管服务提供商处获取模板磁盘，或者根据[租户的受防护 vm 创建模板磁盘（可选）](guarded-fabric-tenant-creates-template-disk.md)创建模板磁盘。

有关防护数据文件内容的列表和关系图，请参阅[什么是防护数据？什么是必需的？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

> [!IMPORTANT]
> 本部分中的步骤应在运行 Windows Server 2016 的租户计算机上完成。 该计算机不得是受保护的构造的一部分（即，不应将其配置为使用 HGS 群集）。

若要准备创建防护数据文件，请执行以下步骤：

- [获取远程桌面连接的证书](#obtain-a-certificate-for-remote-desktop-connection)
- [创建应答文件](#create-an-answer-file)
- [获取卷签名目录文件](#get-the-volume-signature-catalog-file)
- [选择受信任的构造](#select-trusted-fabrics)

然后，可以创建防护数据文件：

- [创建防护数据文件并添加监护人](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)


## <a name="obtain-a-certificate-for-remote-desktop-connection"></a>获取远程桌面连接的证书

由于租户只能使用远程桌面连接或其他远程管理工具连接到受防护的 Vm，因此请务必确保租户可以验证它们是否连接到正确的终结点（即，没有 "中间人"截获连接）。

验证连接到目标服务器的一种方法是安装和配置一个证书，以便在启动连接时提供远程桌面服务。 连接到服务器的客户端计算机将检查其是否信任证书，并在不显示警告的情况。 通常，若要确保连接客户端信任证书，则会从租户的 PKI 颁发 RDP 证书。 有关[在远程桌面服务中使用证书的](https://technet.microsoft.com/library/dn781533.aspx)详细信息，请参阅 TechNet。

> [!NOTE]
> 选择要包含在防护数据文件中的 RDP 证书时，请确保使用通配符证书。 一个防护数据文件可用于创建不限数量的 Vm。 由于每个 VM 共享同一个证书，因此无论 VM 的主机名如何，通配符证书都可确保证书有效。

如果你正在评估受防护的 Vm，但尚未准备好从证书颁发机构请求证书，则可以通过运行以下 Windows PowerShell 命令在租户计算机上创建自签名证书（其中， *contoso.com*是租户的域）：

``` powershell
$rdpCertificate = New-SelfSignedCertificate -DnsName '\*.contoso.com'
$password = ConvertTo-SecureString -AsPlainText 'Password1' -Force
Export-PfxCertificate -Cert $RdpCertificate -FilePath .\rdpCert.pfx -Password $password
```

## <a name="create-an-answer-file"></a>创建应答文件

由于 VMM 中的已签名模板磁盘是通用化的，因此在预配过程中，租户需要提供应答文件来专用化其受防护的 Vm。 答案文件（通常称为无人参与文件）可以将 VM 配置为其预期的角色，即它可以安装 Windows 功能，注册在上一步中创建的 RDP 证书，并执行其他自定义操作。 它还将提供 Windows 安装程序所需的信息，包括默认管理员的密码和产品密钥。

有关获取并使用**ShieldingDataAnswerFile**函数生成用于创建受防护 vm 的应答文件（unattend.xml 文件）的信息，请参阅[使用 ShieldingDataAnswerFile 函数生成应答文件](guarded-fabric-sample-unattend-xml-file.md)。 使用函数，你可以更轻松地生成一个应答文件，用于反映如下选择：

- 在初始化过程结束时，VM 是否应加入域？
- 是否会对每个 VM 使用批量许可或特定的产品密钥？
- 你使用的是 DHCP 还是静态 IP？
- 是否会使用远程桌面协议（RDP）证书来证明 VM 属于你的组织？
- 是否要在初始化结束时运行脚本？
- 是否使用了所需状态配置（DSC）服务器进行进一步配置？

屏蔽数据文件中使用的应答文件将在使用该防护数据文件创建的每个 VM 上使用。 因此，应确保不会将任何特定于 VM 的信息硬编码到答案文件中。 VMM 在无人参与文件中支持一些替代字符串（请参阅下表），以处理可能会从 VM 更改为 VM 的专用化值。 不需要使用这些值;但是，如果它们存在，VMM 将利用它们。

为受防护的 Vm 创建 unattend.xml 文件时，请记住以下限制：

-   无人参与文件必须导致 VM 在配置后关闭。 这是为了让 VMM 知道何时应向租户报告 VM 完成预配并可供使用的时间。 当 VM 检测到它在预配期间已关闭时，它将自动开机。

-   强烈建议配置 RDP 证书，确保连接到正确的 VM，而不是配置为中间人攻击的另一台计算机。

-   确保启用 RDP 和相应的防火墙规则，以便可以在配置 VM 后对其进行访问。 你不能使用 VMM 控制台来访问受防护的 Vm，因此你将需要 RDP 来连接到 VM。 如果希望使用 Windows PowerShell 远程处理来管理系统，请确保还启用了 WinRM。

-   受防护的 VM 无人参与文件中支持的唯一替换字符串如下：

| 可替换元素 | 替换字符串 |
|-----------|-----------|
| ComputerName        | @ComputerName@      |
| TimeZone            | @TimeZone@          |
| ProductKey          | @ProductKey@        |
| IPAddr4-1           | @IP4Addr-1@         |
| IPAddr6-1           | @IP6Addr-1@         |
| MACAddr-1           | @MACAddr-1@         |
| 前缀-1-1          | @Prefix-1-1@        |
| NextHop-1-1         | @NextHop-1-1@       |
| 前缀-1-2          | @Prefix-1-2@        |
| NextHop-1-2         | @NextHop-1-2@       |

使用替换字符串时，务必确保在 VM 预配过程中填充字符串。 如果部署时未提供 @ProductKey @ 之类的字符串，则将无人参与文件中的 &lt;ProductKey @ no__t 节点保留为空白，则专用化过程将失败，并且你将无法连接到 VM。

另请注意，仅当使用的是 VMM 静态 IP 地址池时，才使用与表末尾相关的网络相关的替换字符串。 托管服务提供商应能够告诉你是否需要这些替换字符串。 有关 VMM 模板中静态 IP 地址的详细信息，请参阅 VMM 文档中的以下内容：

- [IP 地址池的准则](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools) 
- [在 VMM 构造中设置静态 IP 地址池](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

最后，请务必注意，受防护的 VM 部署过程只会加密 OS 驱动器。 如果使用一个或多个数据驱动器部署受防护的 VM，强烈建议你在租户域中添加无人参与命令或组策略设置，以自动加密数据驱动器。

## <a name="get-the-volume-signature-catalog-file"></a>获取卷签名目录文件

屏蔽数据文件还包含有关租户信任的模板磁盘的信息。 租户以卷签名目录（VSC）文件的形式从受信任的模板磁盘获取磁盘签名。 然后，在部署新 VM 时验证这些签名。 如果防护数据文件中的任何一个签名与尝试与 VM 一起部署的模板磁盘都不匹配（即，它已被修改或交换为其他可能的恶意磁盘），则设置过程将失败。

> [!IMPORTANT]
> 尽管 VSC 可确保磁盘未被篡改，但在第一次信任该磁盘时，租户仍然非常重要。 如果你是租户，并且模板磁盘是由你的主机提供的，则使用该模板磁盘部署测试 VM，并运行你自己的工具（防病毒、漏洞扫描程序等）来验证该磁盘事实上是否处于你信任的状态。

可通过两种方式获取模板磁盘的 VSC：

-  宿主（或者租户，如果租户有权访问 VMM）使用 VMM PowerShell cmdlet 来保存 VSC，并将其提供给租户。 这可以在安装了 VMM 控制台的任何计算机上执行，并配置为管理托管构造的 VMM 环境。 用于保存 VSC 的 PowerShell cmdlet 是：

        $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"
    
        $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk
    
        $vsc.WriteToFile(".\templateDisk.vsc")

-  租户有权访问模板磁盘文件。 如果租户创建要上传到托管服务提供商的模板磁盘，或者如果租户可以下载宿主的模板磁盘，则可能会出现这种情况。 在这种情况下，如果没有 VMM，租户将运行以下 cmdlet （与受防护的 VM 工具功能一起安装，远程服务器管理工具中的一部分）：

        Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc

## <a name="select-trusted-fabrics"></a>选择受信任的构造

防护数据文件中的最后一个组件与 VM 的所有者和监护人相关。 保护者用于指定受防护的 VM 的所有者以及授权其运行的受保护的结构。

若要授权托管构造运行受防护的 VM，必须从托管服务提供商的主机保护者服务获取保护者元数据。 通常，托管服务提供商会通过其管理工具为你提供此元数据。 在企业方案中，您可以直接访问自己获取元数据。

你或你的托管服务提供商可以通过执行下列操作之一，从 HGS 获取保护者元数据：

-  通过运行以下 Windows PowerShell 命令，或浏览到网站并保存显示的 XML 文件，直接从 HGS 获取保护者元数据：

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

-  使用 VMM PowerShell cmdlet 从 VMM 中获取保护者元数据：

        $relecloudmetadata = Get-SCGuardianConfiguration

        $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8

在继续操作之前，为你希望授权受防护的 Vm 运行的每个受保护的构造获取保护者元数据文件。

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>使用防护数据文件向导创建防护数据文件并添加监护人

运行防护数据文件向导创建防护数据（PDK）文件。 在此，你将添加 RDP 证书、无人参与文件、卷签名目录、所有者保护者和在上一步骤中获取的下载的保护者元数据。

1.  使用服务器管理器或以下 Windows PowerShell 命令，在计算机上安装**远程服务器管理工具 @no__t 功能管理工具 &gt; 受防护的 VM 工具**：

        Install-WindowsFeature RSAT-Shielded-VM-Tools

2.  从 "开始" 菜单上的 "管理员工具" 部分中打开 "防护数据文件向导"，或通过运行以下可执行文件**C： \\Windows @ no__t-2System32\\ShieldingDataFileWizard.exe**。

3.  在第一页上，使用第二个文件选择框为防护数据文件选择位置和文件名。 通常，在拥有任何使用该防护数据创建的 Vm （例如，HR、IT、财务）及其正在运行的工作负荷角色（例如，文件服务器、web 服务器或任何其他无人参与文件配置的其他内容）的实体之后，会将屏蔽数据文件命名为。 将单选按钮设置为**屏蔽模板的防护数据**。

    > [!NOTE]
    > 在防护数据文件向导中，你会注意到以下两个选项：
    >- **屏蔽模板的防护数据**
    >- **现有 Vm 和未屏蔽模板的防护数据**<br>
    > 从受防护的模板创建新的受防护的 Vm 时，会使用第一个选项。 第二个选项允许你创建只能在转换现有 Vm 或从非屏蔽模板创建受防护的 Vm 时使用的防护数据。

    ![屏蔽数据文件向导，文件选择](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

       此外，还必须选择是使用此防护数据文件创建的 Vm 在 "受支持的加密" 模式下是否被真正屏蔽或配置。 有关这两个选项的详细信息，请参阅[受保护的构造可以运行哪些类型的虚拟机？](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run)。

    > [!IMPORTANT]
    > 请注意下一步，因为它定义了受防护的 Vm 的所有者，以及受防护的 Vm 有权在其上运行的结构。<br>若要将现有受防护的 VM 从受**防护的防护**版本更改为**受支持的加密**，需要拥有**所有者保护者**，反之亦然。
    
4.  此步骤中的目标为两折：

    - 创建或选择将你表示为 VM 所有者的所有者监护人

    - 导入在上一步骤中从托管提供程序的（或你自己的）主机保护者服务下载的保护者

    若要指定现有的所有者监护人，请从下拉菜单中选择相应的保护者。 此列表中仅显示安装在本地计算机上的私钥的保护者。 你还可以通过选择右下角的 "**管理本地监护人**" 并单击 "**创建**" 并完成向导来创建自己的所有者保护者。

    接下来，我们将使用 "**所有者" 和**"保护者" 页导入先前下载的保护者元数据。 选择右下角的 "**管理本地监护人**"。 使用 "**导入**" 功能导入监护人元数据文件。 导入或添加了所有必要的保护者后，单击 **"确定"** 。 作为最佳方案，请在托管服务提供商或企业数据中心提供服务后，为其命名。 最后，选择表示受防护的 VM 有权运行的数据中心的所有监护人。 不需要再次选择所有者保护者。 完成后，单击 "**下一步**"。

    ![防护数据文件向导，所有者和监护人](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5.  在 "卷 ID 限定符" 页上，单击 "**添加**" 以授权你的防护数据文件中的已签名模板磁盘。 当你在对话框中选择 VSC 时，它会向你显示该磁盘的名称、版本以及用于对其进行签名的证书的相关信息。 为要授权的每个模板磁盘重复此过程。

6.  在 "**专用化值**" 页上，单击 "**浏览**" 以选择将用于专用化 vm 的 unattend.xml 文件。

    使用底部的 "**添加**" 按钮可将任何其他文件添加到专用化过程中所需的 PDK。 例如，如果无人参与文件正在将 RDP 证书安装到 VM 上（如[通过使用 ShieldingDataAnswerFile 函数生成答案文件](guarded-fabric-sample-unattend-xml-file.md)中所述），则应在此处添加无人参与文件中引用的 RDPCert 文件。 请注意，在此处指定的任何文件将自动复制到创建的 VM 上的 C @no__t： 0temp @ no__t-1。 在按路径引用文件时，无人参与文件应将这些文件放在该文件夹中。

7.  在下一页上查看您的选择，然后单击 "**生成**"。

8.  完成后关闭向导。

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>使用 PowerShell 创建防护数据文件并添加监护人

作为防护数据文件向导的替代方法，你可以运行[ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps)来创建防护数据文件。

所有屏蔽数据文件都需要使用正确的所有者和监护人证书进行配置，以便授权受防护的 Vm 在受保护的构造中运行。
可以通过运行[HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps)来检查是否在本地安装了任何监护人。 所有者保护者具有私钥，而你的数据中心的保护者通常不会这样做。

如果需要创建所有者监护人，请运行以下命令：

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

此命令在本地计算机的证书存储区中的 "受防护的 VM 本地证书" 文件夹下创建一对签名和加密证书。
你将需要所有者证书及其相应的私钥来 unshield 虚拟机，因此请确保这些证书已备份，并防止被盗。
有权访问所有者证书的攻击者可以使用这些证书启动受防护的虚拟机或更改其安全配置。

如果需要在要运行虚拟机的受保护构造（主数据中心、备份数据中心等）中导入监护人信息，请对[从受保护的构造中检索的每个元数据文件](#select-trusted-fabrics)运行以下命令。

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> 如果你使用的是自签名证书或已向 HGS 注册的证书已过期，你可能需要使用 HgsGuardian 命令的 @no__t 0 和/或 @no__t 标志来绕过安全检查。

还需要为要与此防护数据文件一起使用的每个模板磁盘[获取卷签名目录](#get-the-volume-signature-catalog-file)，并获取[防护数据答案文件](#create-an-answer-file)，以允许操作系统自动完成其专用化任务。
最后，决定是要将 VM 完全屏蔽还是只是启用了 vTPM。
对于完全受防护的 VM，请使用 `-Policy Shielded`; 对于允许基本控制台连接和 PowerShell Direct 的启用 vTPM 的 VM，请使用 `-Policy EncryptionSupported`。

一切准备就绪后，运行以下命令以创建防护数据文件：

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

在上述命令中，名为 "Owner" 的监护人（从 HgsGuardian 获取）将能够在未来更改 VM 的安全配置，而 "美国 Datacenter" 则可以运行 VM，但不会更改其设置。
如果有多个监护人，请用逗号分隔监护人的名称，如 `'EAST-US Datacenter', 'EMEA Datacenter'`。
卷 ID 限定符指定你是仅信任模板磁盘还是未来版本（GreaterThanOrEquals）的确切版本（等于）。
磁盘名称和签名证书必须与部署时要考虑的版本比较完全匹配。
你可以通过向 `-VolumeIDQualifier` 参数提供一个以逗号分隔的卷 ID 限定符列表来信任多个模板磁盘。
最后，如果有其他文件需要随 VM 一起附带答案文件，请使用 @no__t 参数，并提供以逗号分隔的文件路径列表。

请参阅[ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps)和[VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps)的 cmdlet 文档，了解配置防护数据文件的其他方法。

## <a name="see-also"></a>请参阅

- [部署受防护的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
