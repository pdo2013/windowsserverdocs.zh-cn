---
title: 租户-创建防护数据来定义受防护的 VM 的受防护的 Vm
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 1245b88a42b80218b5557dc89f2b97b5d0059d44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852538"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>租户-创建防护数据来定义受防护的 VM 的受防护的 Vm

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

屏蔽数据文件（也称为预配数据文件或 PDK 文件）是加密文件，由租户或 VM 所有者创建，用于保护重要的 VM 配置信息，例如管理员密码、RDP 和其他标识相关的证书，域加入凭据等。 本主题提供有关如何创建防护数据文件的信息。 创建该文件之前，必须从你托管服务提供商获取模板磁盘或创建模板磁盘，如中所述[租户-创建模板磁盘 （可选） 为受防护的 Vm](guarded-fabric-tenant-creates-template-disk.md)。

列表和关系图的防护数据文件的内容，请参阅[什么屏蔽数据和为什么是必要？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

> [!IMPORTANT]
> 应运行 Windows Server 2016 的租户计算机上完成此部分中的步骤。 该计算机必须不是受保护的构造的一部分 （即，不应将配置为使用 HGS 群集）。

若要准备创建防护数据文件，请执行以下步骤：

- [获取远程桌面连接的证书](#obtain-a-certificate-for-remote-desktop-connection)
- [创建答案文件](#create-an-answer-file)
- [获取卷签名目录文件](#get-the-volume-signature-catalog-file)
- [选择受信任的构造](#select-trusted-fabrics)

然后可以创建防护数据文件：

- [创建防护数据文件并添加 guardians](#create-a-shielding-data-file-and-add-guardians)


## <a name="obtain-a-certificate-for-remote-desktop-connection"></a>获取远程桌面连接的证书

由于租户均能够连接到其受防护的 Vm 使用远程桌面连接或其他远程管理工具，务必确保租户可以验证它们是否连接到的相应终结点 （即，没有"中间人"截获连接）。

若要验证连接到目标服务器的一种方法是安装和配置用于远程桌面服务来启动连接时提供的证书。 连接到服务器的客户端计算机将检查是否它信任的证书和显示一条警告是否不是。 通常情况下，若要确保连接的客户端信任的证书，RDP 颁发的证书从租户的 PKI。 有关详细信息[在远程桌面服务中使用证书](https://technet.microsoft.com/library/dn781533.aspx)TechNet 上可以找到。

<!-- The previous link comes from Windows 2012 R2 content, but as of Sept 2016, there isn't a more recent link that covers the same information. -->

> [!NOTE]
> 当选择要包括在你的防护数据文件中的 RDP 证书，请务必使用通配符证书。 一个屏蔽数据文件可能用于创建无限的数量的 Vm。 因为每个 VM 将共享相同的证书，可确保通配符证书，证书将是有效的而不考虑 VM 的主机名。

如果您正在评估受防护的 Vm，而不尚未准备好从证书颁发机构请求证书，你可以创建自签名的证书在租户计算机上通过运行以下 Windows PowerShell 命令 (其中*contoso.com*是租户的域):

``` powershell
$rdpCertificate = New-SelfSignedCertificate -DnsName '\*.contoso.com'
$password = ConvertTo-SecureString -AsPlainText 'Password1' -Force
Export-PfxCertificate -Cert $RdpCertificate -FilePath .\rdpCert.pfx -Password $password
```

## <a name="create-an-answer-file"></a>创建应答文件

因为在 VMM 中的已签名的模板磁盘已通用化，租户将要求提供应答文件来预配过程中具体指定其受防护的 Vm。 答案文件 （通常称为无人参与文件） 可以配置其预期角色的 VM-也就是说，它可以安装 Windows 功能，注册在上一步中创建的 RDP 证书并执行其他自定义操作。 它还将提供所需的 Windows 安装程序，包括默认管理员密码和产品密钥信息。

有关获取和使用信息**新建 ShieldingDataAnswerFile**函数生成答案文件 （Unattend.xml 文件） 用于创建受防护的 Vm，请参阅[通过生成一个答案文件新 ShieldingDataAnswerFile 函数](guarded-fabric-sample-unattend-xml-file.md)。 使用函数时，可以更轻松地生成答案文件，反映选择如下所示：

- VM 旨在作为初始化过程的末尾已加入域？
- 您是否要使用的批量许可证或每个 VM 的特定的产品密钥？
- 您正在使用 DHCP 还是静态 IP？
- 将使用远程桌面协议 (RDP) 证书将用于证明 VM 属于你的组织？
- 若要初始化的结束时运行脚本吗？
- 您使用 Desired State Configuration (DSC) 服务器进行进一步配置？

将使用该防护数据文件创建的每个 VM 上使用的防护数据文件中使用的应答文件。 因此，应确保不要硬编码到答案文件的任何特定于 VM 的信息。 VMM 支持在要处理的任务将从 VM 到 VM 的专用化值的无人参与文件中 （请参阅下表） 一些替换字符串。 您不需要使用这些;但是，如果它们存在 VMM 将充分利用它们。

当创建受防护的 Vm 的 unattend.xml 文件，请记住以下限制：

-   配置完之后关闭 VM 中的结果必须无人参与文件。 这是为了允许 VMM 知道它应报表时向租户 VM 完成预配，并可供使用。 检测到它在预配期间已禁用后，VMM 会自动将重新打开电源 VM。

-   强烈建议你配置 RDP 证书以确保要连接到正确的 VM 和不另一台计算机配置为拦截的攻击。

-   请务必启用 RDP 和相应的防火墙规则，以便在配置完之后，可以访问 VM。 因此您将需要 RDP 连接到 VM，不能使用 VMM 控制台到访问受防护的 Vm。 如果想要管理通过 Windows PowerShell 远程处理系统，请确保启用 WinRM，过。

-   在受防护的 VM 无人参与文件中受支持的唯一的替换字符串如下所示：

| 可替换的元素 | 替换字符串 |
|-----------|-----------|
| ComputerName        | @ComputerName@      |
| TimeZone            | @TimeZone@          |
| ProductKey          | @ProductKey@        |
| IPAddr4-1           | @IP4Addr-1@         |
| IPAddr6-1           | @IP6Addr-1@         |
| MACAddr-1           | @MACAddr-1@         |
| Prefix-1-1          | @Prefix-1-1@        |
| NextHop-1-1         | @NextHop-1-1@       |
| Prefix-1-2          | @Prefix-1-2@        |
| NextHop-1-2         | @NextHop-1-2@       |

使用替换字符串时，务必确保将 VM 预配过程期间填充字符串。 如果是字符串，如@ProductKey@ 未提供在部署时，离开&lt;ProductKey&gt;空白的无人参与文件中的节点，则专用化过程将失败，您将无法连接到 VM。

另请注意是否您将充分利用 VMM 静态 IP 地址池，仅使用表的末尾的与网络相关的替换字符串。 托管服务提供商应能够告知你是否需要这些替换字符串。 有关 VMM 模板中的静态 IP 地址的详细信息，请参阅 VMM 文档中的以下：

- [IP 地址池的准则](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools) 
- [设置 VMM 构造中的静态 IP 地址池](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

最后，务必要注意的受防护的 VM 部署过程将只加密操作系统驱动器。 如果部署受防护的 VM 与一个或多个数据驱动器，强烈建议在租户域对租户进行自动加密数据驱动器中，将你添加的无人参与命令或组策略设置。

## <a name="get-the-volume-signature-catalog-file"></a>获取卷签名目录文件

防护数据文件还包含一个租户信任信息的模板磁盘。 租户获取从受信任的模板磁盘的卷签名目录 (VSC) 文件形式的磁盘签名。 部署新的 VM 时，然后会验证这些签名。 如果没有防护数据文件中的签名匹配的模板磁盘尝试部署的 VM （即，它已修改或与不同、 潜在的恶意磁盘交换） 与预配过程将失败。

> [!IMPORTANT]
> VSC 可确保磁盘未被篡改，而是租户第一个位置中信任磁盘仍然很重要。 如果你是租户，并测试使用该模板磁盘的 VM 部署和运行你自己的工具 （如防病毒软件、 漏洞扫描程序等） 来验证磁盘，由托管方，提供的模板磁盘实际上是，在您信任的状态。

有两种方法来获取模板磁盘的 VSC:

-  主机 （或租户，如果租户在有权访问 VMM） 使用 VMM PowerShell cmdlet 来保存 VSC 并为其提供给租户。 使用 VMM 控制台安装和配置为管理托管构造的 VMM 环境，可以在任何计算机上执行此。 若要保存 VSC 的 PowerShell cmdlet 为：

        $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"
    
        $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk
    
        $vsc.WriteToFile(".\templateDisk.vsc")

-  租户可以访问模板磁盘文件。 如果在租户创建模板磁盘上传到托管服务提供商或租户可以下载托管商的模板磁盘，这可能是这种情况。 在这种情况下，如果没有在图片中的 VMM，租户将运行以下 cmdlet （安装了受防护的 VM 工具功能，远程服务器管理工具的一部分）：

        Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc

## <a name="select-trusted-fabrics"></a>选择受信任的构造

与所有者和 guardians VM 防护数据文件中的最后一个组件相关。 Guardians 用于指定这两个受防护的 VM 和受保护的构造其有权在其运行的所有者。

若要授权托管构造运行受防护的 VM，必须从托管服务提供商的主机保护者服务获取保护者元数据。 通常情况下，托管服务提供商将为您提供此元数据通过其管理工具。 在企业方案中，您可以直接访问自己获取元数据。

你的托管服务提供商可以获取保护者元数据从 HGS 通过执行以下操作之一：

-  通过运行以下 Windows PowerShell 命令，或浏览到网站并将保存 XML 文件，会显示直接从 HGS 获取保护者元数据：

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

-  从 VMM 使用 VMM PowerShell cmdlet 获取保护者元数据：

        $relecloudmetadata = Get-SCGuardianConfiguration

        $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8

<!-- Note that the VMM PowerShell cmdlets aren't Windows PowerShell, so "VMM PowerShell" is the correct terminology for them. -->

为你想要授权受防护的 Vm 以继续操作之前，在运行每个受保护的构造获取保护者元数据文件。

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>创建防护数据文件并添加 guardians 使用屏蔽数据文件向导

运行屏蔽数据文件向导来创建屏蔽数据 (PDK) 文件。 在这里，您将添加 RDP 证书，无人参与文件，卷签名目录所有者保护者并使用上述步骤中获取的下载保护者元数据。

1.  安装**远程服务器管理工具&gt;功能管理工具&gt;受防护的 VM 工具**使用服务器管理器或以下 Windows PowerShell 命令在计算机上：

        Install-WindowsFeature RSAT-Shielded-VM-Tools

2.  从管理员工具部分在开始菜单上或通过运行以下可执行文件打开屏蔽数据文件向导**c:\\Windows\\System32\\ShieldingDataFileWizard.exe**。

3.  在第一页上，使用第二个文件选择框选择屏蔽数据文件的位置和文件名称。 通常情况下，将命名的防护数据文件后创建实体拥有的所有 Vm，与该屏蔽数据 (例如，HR，IT，Finance) 和工作负荷角色其正在运行 （例如文件服务器、 web 服务器或配置无人参与文件的任何其他内容）。 保持单选按钮设置为**屏蔽数据的受防护模板**。

    > [!NOTE]
    > 屏蔽数据文件向导中您将注意到以下两个选项：
    >- **受防护模板的防护数据**
    >- **现有 Vm 和非受防护的模板的防护数据**<br>
    > 第一个选项用于从受防护的模板创建新受防护的 Vm。 第二个选项可以创建只能在转换现有的 Vm 或创建受防护的 Vm 从非受防护的模板时使用的屏蔽数据。

    ![屏蔽数据文件向导中，选择文件](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

       此外，您必须选择是否创建的 Vm 使用此屏蔽数据文件将真正受防护的或在"支持加密"模式下配置。 有关这两个选项的详细信息，请参阅[受保护的构造可以运行的虚拟机的类型有哪些？](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run)。

    > [!IMPORTANT]
    > 特别注意下一步，因为它定义受防护的 Vm 和它们的所有者将授权受防护的 Vm 上运行的构造。<br>拥有**所有者监护人**需要更高版本将更改从现有的受防护的 VM**防护**到**支持加密**或进行相反转换。
    
4.  你在此步骤中的目标是两个方面：

    - 创建或选择表示您作为 VM 所有者所有者保护者

    - 导入从宿主提供程序的下载保护者 （或你自己） 在上述步骤中的主机保护者服务

    若要指定现有的所有者保护者，请从下拉列表菜单中选择适当的保护者。 仅在本地计算机上安装带有私钥保持不变的 guardians 将显示在此列表中。 此外可以通过选择创建你自己的所有者监护人**管理本地 Guardians**在右上角，单击**创建**并完成向导。

    接下来，我们导入之前下载保护者元数据使用再次**所有者和 Guardians**页。 选择**管理本地 Guardians**从右下角。 使用**导入**功能保护者元数据文件导入。 单击**确定**后已导入或添加所有必要监护人。 最佳做法是，命名后的托管服务提供商或企业数据中心它们表示监护人。 最后，选择表示你受防护的 VM 有权在其中运行的数据中心的所有监护人。 不需要再次选择所有者保护者。 单击**下一步**一次完成。

    ![屏蔽数据文件向导、 所有者和 guardians](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5.  在卷 ID 限定符页上，单击**添加**授权你的防护数据文件中的已签名的模板磁盘。 当在对话框中选择 VSC 时，它将显示有关该磁盘的名称、 版本和用于对其进行签名的证书的信息。 为每个你想要授权的模板磁盘重复此过程。

6.  上**专用化值**页上，单击**浏览**以选择你将用于特殊化 Vm 的 unattend.xml 文件。

    使用**添加**底部将任何其他文件添加到在专用化过程中需要 PDK 按钮。 例如，如果无人参与文件安装到 VM 上的 RDP 证书 (如中所述[通过使用新建 ShieldingDataAnswerFile 函数生成一个答案文件](guarded-fabric-sample-unattend-xml-file.md))，应添加 RDPCert.pfx 文件中引用无人参与文件此处。 请注意，此处指定的任何文件将自动复制到 c:\\temp\\创建的虚拟机上。 无人参与文件应按路径中引用它们时为该文件夹中的文件。

7.  查看下一步的页上，您的选择，然后单击**生成**。

8.  完成后，请关闭向导。

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>创建防护数据文件并添加 guardians 使用 PowerShell

作为屏蔽数据文件向导的替代方法，你可以运行[新建 ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps)创建防护数据文件。

所有的防护数据文件需要使用正确的所有者和保护者证书来授权对受保护的构造运行受防护的 Vm 配置。
您可以检查您是否具有通过运行在本地安装任何监护人[Get HgsGuardian](https://docs.microsoft.com/en-us/powershell/module/hgsclient/get-hgsguardian?view=win10-ps)。 所有者 guardians 具有私钥，而你的数据中心的 guardians 通常不能。

如果需要创建所有者保护者，运行以下命令：

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

此命令在本地计算机的证书存储区的"受防护的 VM 本地证书"文件夹下创建一对签名和加密证书。
你将需要所有者证书和相关私钥来 unshield 虚拟机，因此请确保这些证书中备份和保护免遭盗窃。
攻击者有权访问所有者证书可以使用它们启动受防护的虚拟机或更改其安全配置。

如果你需要从你想要运行你的虚拟机 （在主数据中心、 备份数据中心等），运行以下命令为每个受保护的构造导入保护者信息[从受保护的构造中检索元数据文件](#Select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> 如果使用自签名的证书或证书注册的 HGS 已过期，可能需要使用`-AllowUntrustedRoot`和/或`-AllowExpired`导入 HgsGuardian 命令以绕过安全检查的标志。

您还需要[获取卷签名目录](#Get-the-volume-signature-catalog-file)为每个你想要使用此屏蔽数据文件的模板磁盘和一个[屏蔽数据答案文件](#Create-an-answer-file)以允许操作系统以完成其专用化将自动任务。
最后，决定是否要将完全受保护或只是 vTPM 已启用的 VM。
使用`-Policy Shielded`为完全受防护的 VM 或`-Policy EncryptionSupported`vTPM 已启用允许基本的控制台连接的 VM 和 PowerShell Direct。

一切准备就绪后，运行以下命令以创建你的防护数据文件：

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

在上述命令中，名为"所有者"（从 Get HgsGuardian 获取的） 保护者将能够更改 VM 的安全配置在将来，而美国东部数据中心可以运行 VM，但不是更改其设置。
如果你有多个保护者，单独的用逗号 guardians 名称类似于`'EAST-US Datacenter', 'EMEA Datacenter'`。
卷 ID 限定符指定您是否信任确切版本的模板磁盘 （等于） 或将来的版本 (GreaterThanOrEquals)。
磁盘名称和签名证书对于必须完全匹配要在部署时考虑的版本比较。
可以通过提供逗号分隔的卷列表 ID 限定符到信任多个模板磁盘`-VolumeIDQualifier`参数。
最后，如果您有其他文件需要附带有 VM，使用在答案文件的`-OtherFile`参数，并提供以逗号分隔的文件路径的列表。

请参阅的 cmdlet 文档[新建 ShieldingDataFile](https://docs.microsoft.com/en-us/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps)并[新建 VolumeIDQualifier](https://docs.microsoft.com/en-us/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps)若要了解如何配置你的防护数据文件的其他方法。

## <a name="see-also"></a>请参阅

- [部署受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的构造和受防护的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
