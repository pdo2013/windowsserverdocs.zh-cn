---
title: Windows Server 中的运行状况服务
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: df455dfb0d2936192a3c2d7825e2d6d031cfe892
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361069"
---
# <a name="health-service-in-windows-server"></a>Windows Server 中的运行状况服务

> 适用于：Windows Server 2019、Windows Server 2016

运行状况服务是 Windows Server 2016 中的一项新功能，可改进运行存储空间直通的群集的日常监视和操作体验。

## <a name="prerequisites"></a>先决条件  

默认情况下，存储空间直通启用运行状况服务。 设置或启动它时无需执行任何其他操作。 若要了解有关存储空间直通的详细信息，请参阅[Windows Server 2016 中的存储空间直通](../storage/storage-spaces/storage-spaces-direct-overview.md)。  

## <a name="reports"></a>报告

请参阅[运行状况服务报表](health-service-reports.md)。

## <a name="faults"></a>故障

请参阅[运行状况服务错误](health-service-faults.md)。

## <a name="actions"></a>操作

请参阅[运行状况服务操作](health-service-actions.md)。

## <a name="automation"></a>自动化  

下一部分介绍磁盘生命周期中运行状况服务自动化的工作流。  

### <a name="disk-lifecycle"></a>磁盘生命周期   

运行状况服务自动执行物理磁盘生命周期的大多数阶段。 假设部署的初始状态处于最佳运行状况 - 也就是说，所有物理磁盘正常运行。  

#### <a name="retirement"></a>停用  

物理磁盘不再可用且引发相应故障时，将自动停用。 有以下几种情况：  

-   介质故障：物理磁盘彻底失败或损坏，并且必须进行替换。  

-   通信中断：物理磁盘连接断开的持续时间超过 15 分钟。  

-   无响应：物理磁盘在一小时内出现三次或更多次时间超过 5.0 秒的延迟。  

>[!NOTE]
> 如果许多物理磁盘一次性断开连接或整个节点或存储机箱断开连接，运行状况服务将*不*停用这些磁盘，因为它们不太可能是根本问题。  

如果已停用的磁盘曾用作许多其他物理磁盘的缓存，则将自动重新分配到另一个缓存磁盘（如果存在）。 无需特定的用户操作。  

#### <a name="restoring-resiliency"></a>还原复原能力  

一旦停用物理磁盘，运行状况服务会立即开始将其数据复制到其余物理磁盘来还原完全复原能力。 完成后，数据是完全安全的并重新具有容错能力。  

>[!NOTE]
> 此立即还原操作要求剩余的物理磁盘之间具有足够的可用容量。  

#### <a name="blinking-the-indicator-light"></a>闪烁的指示灯  

如果可能，运行状况服务将开始在停用的物理磁盘或其插槽上闪烁指示灯。 这将无限期继续下去，直到更换已停用的磁盘。  

>[!NOTE]
> 在某些情况下，磁盘甚至可能出现阻止指示灯正常运行的故障 - 例如，完全断电。  

#### <a name="physical-replacement"></a>物理替换  

应尽可能替换已停用的物理磁盘。 最常见的情况是，这种情况下，不需要关闭节点或存储机箱。 查看故障了解有用的位置和部件信息。  

#### <a name="verification"></a>验证

插入替换磁盘后，将根据支持的组件文档对其进行验证（请参阅下一节）。

#### <a name="pooling"></a>池  

如果允许，替代磁盘将被自动替换到其前身池中以进行使用。 此时，系统会恢复到处于最佳运行状况的初始状态，故障消失。  

## <a name="supported-components-document"></a>支持的组件文档  

运行状况服务提供了一种强制机制，用于将存储空间直通所使用的组件限制到管理员或解决方案供应商提供的支持的组件文档中。 这可用来防止你或其他人误用不受支持的硬件，可能会帮助保证或支持合同的合规性。 此功能当前仅限于物理磁盘设备，包括 Ssd、Hdd 和 NVMe 驱动器。 支持的组件文档可以限制模型、制造商（可选）和固件版本（可选）。

### <a name="usage"></a>用法  

支持的组件文档使用了 XML 灵感的语法。 建议使用最喜欢的文本编辑器，如免费[Visual Studio Code](http://code.visualstudio.com/)或记事本，创建一个可以保存并重复使用的 XML 文档。

#### <a name="sections"></a>部分

该文档有两个独立的部分： `Disks` 和 `Cache`。

如果提供了 @no__t 0 部分，则只允许列出列出的驱动器（如 `Disk`）来加入池。 将阻止所有未列出的驱动器加入池，从而有效地阻止它们在生产中的使用。 如果此部分为空，则允许任何驱动器加入池。

如果提供了 @no__t 0 部分，则仅将列出的驱动器（如 `CacheDisk`）用于缓存。 如果此部分为空，则存储空间直通会[根据媒体类型和总线类型](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically)尝试进行猜测。 此处列出的驱动器还应列在 `Disks`。

>[!IMPORTANT]
> 支持的组件文档不会将以追溯方式应用到已在使用中的驱动器。  

#### <a name="example"></a>示例

```XML
<Components>

  <Disks>
    <Disk>
      <Manufacturer>Contoso</Manufacturer>
      <Model>XYZ9000</Model>
      <AllowedFirmware>
        <Version>2.0</Version>
        <Version>2.1</Version>
        <Version>2.2</Version>
      </AllowedFirmware>
      <TargetFirmware>
        <Version>2.1</Version>
        <BinaryPath>C:\ClusterStorage\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Disks>

  <Cache>
    <CacheDisk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </CacheDisk>
  </Cache>

</Components>

```

若要列出多个驱动器，只需添加额外的 @no__t 0 或 @no__t 的标记即可。

若要在部署存储空间直通时插入此 XML，请使用 @no__t 参数：

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Enable-ClusterS2D -XML $MyXML
```

部署存储空间直通后，若要设置或修改支持的组件文档：

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>型号、制造商和固件版本属性应完全匹配使用 **Get-physicaldisk** cmdlet 获取的值。 这可能不同于“常识”期望，具体取决于供应商的实施。 例如，制造商不是“Contoso”，而可能是“CONTOSO-LTD”，或者在型号为“Contoso-XZY9000”时它可能保留为空。  

你可以使用以下 PowerShell cmdlet 进行验证：  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>设置

请参阅[运行状况服务设置](health-service-settings.md)。

## <a name="see-also"></a>请参阅

- [运行状况服务报表](health-service-reports.md)
- [运行状况服务故障](health-service-faults.md)
- [运行状况服务操作](health-service-actions.md)
- [运行状况服务设置](health-service-settings.md)
- [Windows Server 2016 中的存储空间直通](../storage/storage-spaces/storage-spaces-direct-overview.md)
