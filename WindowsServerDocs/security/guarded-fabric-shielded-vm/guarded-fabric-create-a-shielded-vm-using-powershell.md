---
title: 创建受防护的 VM 使用 PowerShell
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0086edb7781a604cc90b9e76d34e5a3dc2725547
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447522"
---
# <a name="create-a-shielded-vm-using-powershell"></a>创建受防护的 VM 使用 PowerShell

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

在生产中，您通常会使用结构管理软件 (例如 VMM) 部署受防护的 Vm。 但是，如下图所示的步骤，可部署并验证整个方案而无需构造管理器。

简单地说，将在任何计算机上创建模板磁盘、 防护数据文件、 无人参与的安装答案文件和其他安全项目，然后将这些文件复制到受保护的主机和预配受防护的 VM。

## <a name="create-a-signed-template-disk"></a>创建已签名的模板磁盘

若要创建新的受防护的 VM，首先需要使用签名其 OS 卷 （或 Linux 上的启动和根分区） 预先加密的受防护的 VM 模板磁盘。
如何创建模板磁盘，请按照下面链接的详细信息。

- [准备 Windows 模板磁盘](guarded-fabric-create-a-shielded-vm-template.md)
- [准备 Linux 模板磁盘](guarded-fabric-create-a-linux-shielded-vm-template.md)

您还需要磁盘的卷签名目录创建防护数据文件的副本。
若要保存此文件，请运行以下命令在计算机上你在其中创建模板磁盘：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>下载保护者元数据

对于每个想要运行受防护的 VM 虚拟化结构，您将需要获取构造的 HGS 群集的保护者元数据。
托管提供商应能够提供此信息。

如果您是在企业环境中，并且可以与 HGS 服务器进行通信，保护者元数据将位于*http://\<HGSCLUSTERNAME\>/KeyProtection/service/metadata/2014-07/metadata.xml*

## <a name="create-shielding-data-pdk-file"></a>创建屏蔽数据 (PDK) 文件

屏蔽数据创建并由租户 VM 所有者拥有并包含创建受防护的 Vm，必须防止构造管理员，例如受防护的 VM 管理员密码所需的机密。
屏蔽数据进行加密，以便仅 HGS 服务器和租户能够对其解密。
后由租户/VM 所有者创建的则必须将生成的 PDK 文件复制到受保护的构造。
有关详细信息，请参阅[什么屏蔽数据和为什么是必要？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

此外，您将需要无人参与的安装答案文件 （适用于 Linux 的 Windows，unattend.xml 而异）。 请参阅[创建一个答案文件](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)有关要在答案文件中包含的内容的指导。

适用于安装的受防护的 Vm 具有远程服务器管理工具的计算机上运行以下 cmdlet。
如果要创建 PDK 适用于 Linux VM，必须执行此操作运行 Windows Server 版本 1709年或更高版本的服务器上。

 
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
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>在受保护的主机上预配受防护 VM
将模板磁盘文件 (ServerOS.vhdx) 和 PDK 文件 (contoso.pdk) 复制到受保护的主机，若要为部署做好准备。

在受保护的主机上安装的受保护的构造工具 PowerShell 模块，其中包含新建 ShieldedVM cmdlet 来简化预配过程。 如果在受保护的主机有权访问 Internet，运行以下命令：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

此外可以下载具有 Internet 访问，并将复制到生成的模块的另一台计算机上的模块`C:\Program Files\WindowsPowerShell\Modules`上受保护的主机。

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

安装该模块后，你准备好预配受防护的 VM。

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

如果您的模板磁盘包含基于 Linux 的操作系统，包括`-Linux`标志运行命令时：

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

检查帮助内容使用`Get-Help New-ShieldedVM -Full`若要了解有关其他选项，可以传递给 cmdlet 的详细信息。

完成 VM 预配后，它将进入后将其准备好使用的特定于操作系统的专用化阶段。
请务必将 VM 连接到有效的网络，以便运行 （使用 RDP、 PowerShell、 SSH 或你偏好的管理工具） 后，可以连接到它。

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>在 HYPER-V 群集上运行受防护的 Vm

如果想要部署受防护的 Vm 群集受保护的主机 （使用 Windows 故障转移群集） 上，可以配置受防护的 VM 具有高可用性，使用以下 cmdlet:

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

受防护的 VM 现在可以是实时迁移在群集中。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [部署受防护使用 VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)