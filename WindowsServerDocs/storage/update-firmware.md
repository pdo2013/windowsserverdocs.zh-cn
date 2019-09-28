---
ms.assetid: e5945bae-4a33-487c-a019-92a69db8cf6c
title: 更新驱动器固件
ms.prod: windows-server
ms.author: toklima
ms.manager: dmoss
ms.technology: storage-spaces
ms.topic: article
author: toklima
ms.date: 10/04/2016
ms.openlocfilehash: 2f0530101bb7d597d2d95c26648aad65d62b69ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365870"
---
# <a name="updating-drive-firmware"></a>更新驱动器固件
>适用于：Windows Server 2019、Windows Server 2016、Windows 10

更新驱动器固件历来是一项可能需要停机的繁重任务，这正是我们要对存储空间、Windows Server 以及 Windows 10 版本 1703 和更高版本进行改进的原因。 如果你有支持 Windows 中包含的新固件更新机制的驱动器，则可以在无需停机的情况下更新生产中驱动器的驱动器固件。 但是，如果你打算更新生产驱动器的固件，请确保阅读我们的关于如何在使用此强大的新功能时最大限度减少风险的提示。

  > [!Warning]
  > 固件更新是潜在风险的维护操作，应仅在全面测试新固件映像后应用这些更新。 不受支持的硬件上的新固件可能对可靠性和稳定性产生负面影响，或者甚至会导致数据丢失。 管理员应阅读给定更新附带的发行说明，确定其影响和适用性。

## <a name="drive-compatibility"></a>驱动器兼容性

要使用 Windows Server 更新驱动器固件，必须具有受支持的驱动器。 若要确保常见设备行为，则可以通过为 SAS、SATA 和 NVMe 设备定义新的（对于 Windows 10 和 Windows Server 2016 - 可选 Hardware Lab Kit (HLK)）要求开始入手。 这些要求列出了 SAS、SATA 和 NVMe 设备必须支持才可使用这些新的 Windows 本机 PowerShell cmdlet 更新固件的命令。 为了支持这些要求，有一个新的 HLK 测试可以验证供应商的产品是否支持正确的命令并在将来的修订版中实施。 

有关你的硬件是否支持 Windows 更新驱动器固件的信息，请联系你的解决方案供应商。
以下是指向各种要求的链接：

-   SATANo__t [-在](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragehdsata) **[If 实现 @ 固件下载 & 激活**部分
    
-   块No__t [-在](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragehdsas) **[If 实现 @ 固件下载 & 激活**部分

-   NVMe[ControllerDrive](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragecontrollerdrivenvme) -在**5.7**和**5.8**节中。

## <a name="powershell-cmdlets"></a>PowerShell cmdlet

添加到 Windows 的两个 cmdlet 为：

-   Get-StorageFirmwareInformation
-   Update-StorageFirmware

第一个 cmdlet 为你提供有关设备的功能、固件映像和修订的详细信息。 在这种情况下，计算机仅包含单个带 1 个固件插槽的 SATA SSD。 以下是一个示例：

   ```powershell
   Get-PhysicalDisk | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E16101}
   ```

请注意，SAS 设备始终报告“SupportsUpdate”为“True”，因为没有任何办法来显式查询设备是否支持这些命令。

第二个 cmdlet，Update-StorageFirmware，在驱动器支持新的固件更新机制的情况下，可使管理员用映像文件更新驱动器固件。 你应直接从 OEM 或驱动器供应商处获取该映像文件。

  > [!Note]
  > 在更新任何生产硬件之前，请在实验室环境中测试相同硬件上的特定固件映像。

驱动器会首先将新的固件映像加载到内部暂存区域。 在此情况下，I/O 通常继续进行。 在下载后激活该映像。 在此期间，由于内部重置，驱动器将不能响应 I/O 命令。 这意味着此驱动器在激活过程中不提供任何数据。 访问此驱动器上数据的应用程序需要等待响应直至固件激活完成。 下面是运行中的 cmdlet 的示例：

   ```powershell 
   $pd | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
   $pd | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E160@3}
   ```

驱动器在激活新的固件映像时通常不会完成 I/O 请求。 驱动器的激活时间取决于它的设计和更新的固件类型。 我们观察到的更新时间从不到 5 秒到超过 30 秒不等。

此驱动器在大约 5.8 秒内执行固件更新，如下所示：

```powershell 
Measure-Command {$pd | Update-StorageFirmware -ImagePath C:\\Firmware\\J3E16101.enc -SlotNumber 0}

 Days : 0
 Hours : 0
 Minutes : 0
 Seconds : 5
 Milliseconds : 791
 Ticks : 57913910
 TotalDays : 6.70299884259259E-05
 TotalHours : 0.00160871972222222
 TotalMinutes : 0.0965231833333333
 TotalSeconds : 5.791391
 TotalMilliseconds : 5791.391
 ```

## <a name="updating-drives-in-production"></a>更新生产中的驱动器

在将服务器投入生产之前，强烈建议将你的驱动器固件更新为硬件供应商或销售解决方案和提供支持的 OEM 建议的固件（存储机箱、驱动器和服务器）。

服务器投入生产后，在保证切实可行的前提下尽量少更改服务器。 但是有时解决方案供应商会告知你的驱动器需要一个至关重要的固件更新。 如果发生这种情况，以下是在应用任何驱动器固件更新前应遵循的几个不错做法：

1. 查看固件发行说明并确认更新是否解决了可能会影响环境的问题，而且固件不包含任何对你造成负面影响的已知问题。

2. 在实验室中具有相同驱动器（包括驱动器的修订版本，如果同一驱动器有多个修订版本）的服务器上安装固件，并使用新固件测试负载下的驱动器。 有关执行综合负载测试的信息，请参阅[使用综合工作负载测试存储空间性能](https://technet.microsoft.com/library/dn894707.aspx)。

## <a name="automated-firmware-updates-with-storage-spaces-direct"></a>使用存储空间直通自动更新固件

Windows Server 2016 包括存储空间直通的运行状况服务部署（包括 Microsoft Azure 堆栈解决方案）。 运行状况服务的主要目的是为了方便监视和管理你的硬件部署。 作为其管理功能的一部分，它具有无需使工作负载离线或导致故障时间便可跨整个群集推出驱动器固件的能力。 此功能由策略驱动，受管理员控制。

使用运行状况服务跨群集推出固件非常简单，步骤如下：

-   确定你希望哪些 HDD 和 SSD 驱动器成为存储空间直通群集的一部分，以及驱动器是否支持 Windows 执行固件更新
-   在受支持的组件 xml 文件中列出那些驱动器
-   确定希望这些驱动器在受支持的组件 xml（包括固件映像的位置路径）中拥有的固件版本
-   将 xml 文件上载到群集 DB

此时，运行状况服务将检查和分析 xml 并标识出未部署所需固件版本的驱动器。 然后它将继续从受影响的驱动器重定向 I/O – 逐节点进行 – 并更新它们上面的固件。 存储空间直通群集通过跨多个服务器节点分布数据来实现复原；运行状况服务可以隔离值得驱动器更新的整个节点。 更新节点后，将开始修复存储空间，使所有跨群集数据副本恢复彼此同步，然后再移到下一个节点。 在推出固件时，存储空间过渡到“降级”操作模式是可以预期的，这也是正常的。

为了确保新固件映像稳定推出和验证时间充足，多个服务器的更新之间存在较长的延迟。 默认情况下，运行状况服务会在更新第 <sup>2</sup> 个服务器之前先等待 7 天。 任何后续服务器（第 <sup>3</sup> 个，第 <sup>4</sup> 个...）更新都有 1 天的延迟。 如果管理员发现固件不稳定或不理想，即可随时让运行状况服务停止进一步推广。 如果固件在之前已经过验证并且需要更快推出，则可以将这些默认值从天更改为小时或分钟。

下面是通用存储空间直通群集的受支持组件 xml 的示例：

```xml
 <Components>
     <Disks>
        <Disk>
            <Manufacturer>Contoso</Manufacturer>
            <Model>XYZ9000</Model>
            <AllowedFirmware>
              <Version>2.0</Version>
              <Version>2.1>/Version>
              <Version>2.2</Version>
            </AllowedFirmware>
            <TargetFirmware>
              <Version>2.2</Version>
              <BinaryPath>\\path\to\image.bin</BinaryPath>
            </TargetFirmware>
        </Disk>
        ...
        ...
    </Disks>
 </Components>
```

若要在此存储空间直通群集中开始推出新固件，只需将.xml 上载到群集数据库：

```powershell
$SpacesDirect = Get-StorageSubSystem Clus*

$CurrentDoc = $SpacesDirect | Get-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document"

$CurrentDoc.Value | Out-File <Path>
```

在你最喜爱的编辑器中编辑文件，比如 Visual Studio Code 或 Notepad，然后保存。

```powershell
$NewDoc = Get-Content <Path> | Out-String

$SpacesDirect | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $NewDoc
```

若要查看操作中的运行状况服务并详细了解其推出机制，请参阅此视频： https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct

## <a name="frequently-asked-questions"></a>常见问题解答

另请参阅[驱动器固件更新疑难解答](troubleshoot-firmware-update.md)。

### <a name="will-this-work-on-any-storage-device"></a>这是否适用于任何存储设备

这适用于在其固件中实施正确命令的存储设备。 Get-StorageFirmwareInformation cmdlet 将显示驱动器固件是否真地支持正确命令（对于 SATA/NVMe），以及 HLK 测试是否允许供应商和 OEM 测试此行为。

### <a name="after-i-update-a-sata-drive-it-reports-to-no-longer-support-the-update-mechanism-is-something-wrong-with-the-drive"></a>我更新 SATA 驱动器后，它报告不再支持更新机制。 驱动器是否出现了问题

否，此驱动器没有什么问题，除非新固件不再允许更新。 你遇到的是一个已知问题，其中缓存的驱动器版本的功能不正确。 运行“Update-StorageProviderCache -DiscoveryLevel Full”将重新枚举驱动器功能并更新缓存的副本。 作为一个解决方法，建议在启动固件更新或空间直通群集上的完整推出前运行一次上面的命令。

### <a name="can-i-update-firmware-on-my-san-through-this-mechanism"></a>是否可以通过此机制在我的 SAN 上更新固件
否，对于此类维护操作，SAN 通常有自己的实用程序和接口。 此新机制用于直接附加的存储设备，例如 SATA、SAS 或 NVMe 设备。

### <a name="from-where-do-i-get-the-firmware-image"></a>从何处获取固件映像

应始终直接从你的 OEM、解决方案供应商或驱动器供应商获取固件，而不是从其他参与方下载。 Windows 提供将映像安装到驱动器的机制，但无法验证其完整性。

### <a name="will-this-work-on-clustered-drives"></a>这是否适用于群集驱动器

cmdlet 也可以在群集驱动器上执行其功能，但请记住，运行状况服务业务流程可缓解对在运行工作负载的 I/O 影响。 如果直接在群集驱动器上使用 cmdlet，则 I/O 很可能停止。 通常最好在基础驱动器上无工作负载或只有最少的工作负载时执行驱动器固件更新。

### <a name="what-happens-when-i-update-firmware-on-storage-spaces"></a>在存储空间上更新固件时会怎样

在存储空间直通上部署了运行状况服务的 Windows Server 2016 上，无需使工作负载离线，便可执行此操作，前提是驱动器支持 Windows Server 更新固件。

### <a name="what-happens-if-the-update-fails"></a>更新失败怎么办

更新可能因多种原因而失败，其中一些原因如下：1）驱动器不支持 Windows 更新其固件的正确命令。 在这种情况下，新的固件映像永远不会激活并且驱动器继续使用旧映像运行。 2) 无法将映像下载到或应用于此驱动器（版本不匹配、映像损坏...）。 在这种情况下，激活命令无法激活驱动器。 同样地，旧的固件映像将继续运行。

如果固件更新后驱动器根本没有响应，你很可能是遇到了驱动器固件本身的 bug。 在投入生产之前，请在实验室环境中测试所有固件更新。 唯一的补救措施可能是更换驱动器。

有关详细信息，请参阅[驱动器固件更新疑难解答](troubleshoot-firmware-update.md)。

### <a name="how-do-i-stop-an-in-progress-firmware-roll-out"></a>如何停止正在进行中的固件推出

通过以下命令在 PowerShell 中禁用推出：
```powershell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled" -Value false
```

### <a name="i-am-seeing-an-access-denied-or-path-not-found-error-during-roll-out-how-do-i-fix-this"></a>推出期间我看到了拒绝访问或未找到路径错误。如何解决此问题

确保用于更新的固件映像可被所有群集节点访问。 确保这一点的最简单方法是将其放在群集共享卷上。
