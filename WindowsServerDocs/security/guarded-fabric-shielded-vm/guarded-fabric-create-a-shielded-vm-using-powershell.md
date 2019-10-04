---
title: 使用 PowerShell 创建受防护的 VM
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 317da0ae3c41d142db6f5a076fd3004d9970b815
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940746"
---
# <a name="create-a-shielded-vm-using-powershell"></a>使用 PowerShell 创建受防护的 VM

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

在生产环境中，通常会使用结构管理器（例如 VMM）来部署受防护的 Vm。 但是，下面所示的步骤可让你在不使用结构管理器的情况下部署和验证整个方案。

简而言之，你将在任何计算机上创建一个模板磁盘、一个防护数据文件、一个无人参与的安装答案文件和其他安全项目，然后将这些文件复制到受保护的主机，并设置受防护的 VM。

## <a name="create-a-signed-template-disk"></a>创建签名的模板磁盘

若要创建新的受防护的 VM，首先需要一个使用其 OS 卷（或 Linux 上的启动和根分区）预先加密的受防护的 VM 模板磁盘。
请参阅下面的链接，了解有关如何创建模板磁盘的详细信息。

- [准备 Windows 模板磁盘](guarded-fabric-create-a-shielded-vm-template.md)
- [准备 Linux 模板磁盘](guarded-fabric-create-a-linux-shielded-vm-template.md)

还需要磁盘的卷签名目录的副本来创建防护数据文件。
若要保存此文件，请在创建模板磁盘的计算机上运行以下命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>下载保护者元数据

对于要在其中运行受防护 VM 的每个虚拟化结构，你将需要获取构造的 HGS 群集的监护人元数据。
你的托管提供商应能够为你提供此信息。

如果你在企业环境中并且可以与 HGS 服务器通信，则可以在*http://\<HGSCLUSTERNAME @ no__t/KeyProtection//service/metadata/2014-07/metadata*中获取保护者元数据

## <a name="create-shielding-data-pdk-file"></a>创建屏蔽数据（PDK）文件

屏蔽数据由租户 VM 所有者创建和拥有，其中包含创建受防护的 Vm 所需的机密，这些 Vm 必须从结构管理员处保护，例如受防护的 VM 的管理员密码。
屏蔽数据已加密，因此只有 HGS 服务器和租户才能对其进行解密。
完成后，必须将生成的 PDK 文件复制到受保护的构造。
有关详细信息，请参阅[什么是防护数据，为什么需要这样做？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

此外，你将需要一个无人参与安装答案文件（适用于 Windows 的 unattend.xml）。 有关要包括在答案文件中的内容的指导，请参阅[创建应答文件](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)。

在安装了受防护的 Vm 的远程服务器管理工具的计算机上运行以下 cmdlet。
如果要为 Linux VM 创建 PDK，则必须在运行 Windows Server 1709 或更高版本的服务器上执行此操作。

 
```powershell
# Create owner certificate, don't lose this!
# The certificate is stored at Cert:\LocalMachine\Shielded VM Local Certificates
$Owner = New-HgsGuardian –Name 'Owner' –GenerateCertificates
 
# Import the HGS guardian for each fabric you want to run your shielded VM
$Guardian = Import-HgsGuardian -Path C:\HGSGuardian.xml -Name 'TestFabric'
 
# Create the PDK file
# The "Policy" parameter describes whether the admin can see the VM's console or not
# Use "EncryptionSupported" if you are testing out shielded VMs and want to debug any issues during the specialization process
New-ShieldingDataFile -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Owner $Owner –Guardian $guardian –VolumeIDQualifier (New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\MyTemplateDiskCatalog.vsc' -VersionRule Equals) -WindowsUnattendFile 'C:\unattend.xml' -Policy Shielded
```
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>在受保护的主机上预配受防护的 VM
将模板磁盘文件（ServerOS）和 PDK 文件（contoso）复制到受保护的主机，以便准备好进行部署。

在受保护的主机上，安装受保护的结构工具 PowerShell 模块，其中包含用于简化预配过程的 ShieldedVM cmdlet。 如果受保护的主机可以访问 Internet，请运行以下命令：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

你还可以在具有 Internet 访问权限的另一台计算机上下载该模块，并将生成的模块复制到受保护的主机上 `C:\Program Files\WindowsPowerShell\Modules`。

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

安装该模块后，便可以预配受防护的 VM。

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

如果防护数据应答文件包含专用化值，则可以将替换值提供给 ShieldedVM。 在此示例中，应答文件配置了静态 IPv4 地址的占位符值。

```powershell
$specializationValues = @{
    "@IP4Addr-1@" = "192.168.1.10"
    "@MacAddr-1@" = "Ethernet"
    "@Prefix-1-1@" = "192.168.1.0/24"
    "@NextHop-1-1@" = "192.168.1.254"
}
New-ShieldedVM -Name 'MyStaticIPVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -SpecializationValues $specializationValues -Wait

```

如果模板磁盘包含基于 Linux 的操作系统，请在运行命令时包含 `-Linux` 标志：

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

使用 `Get-Help New-ShieldedVM -Full` 检查帮助内容，了解可传递给 cmdlet 的其他选项的详细信息。

VM 完成预配后，将进入特定于 OS 的专用化阶段，之后它将可供使用。
请确保将 VM 连接到有效的网络，以便可以在运行它（使用 RDP、PowerShell、SSH 或首选管理工具）后连接到该网络。

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>在 Hyper-v 群集上运行受防护的 Vm

如果尝试在群集受保护的主机（使用 Windows 故障转移群集）上部署受防护的 Vm，则可以使用以下 cmdlet 将受防护的 VM 配置为高度可用：

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

受防护的 VM 现在可以在群集中进行实时迁移。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [使用 VMM 部署防护](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)