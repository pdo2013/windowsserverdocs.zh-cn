---
title: "Windows Server 2016 中的运行状况服务"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 834fcfb749e89e4768dce3f229564feea550a432
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-in-windows-server-2016"></a>Windows Server 2016 中的运行状况服务
> 适用于 Windows Server 2016

运行状况服务是 Windows Server 2016，以便改进的日常监视和运营群集运行存储空间直通体验中的新增功能。

## <a name="prerequisites"></a>先决条件  

默认情况下使用存储空间直通启用运行状况服务。 不需要对其进行设置，或开始它执行任何其他操作。 若要了解有关存储空间直通的详细信息，请参阅[存储空间直通在 Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)。  

## <a name="reports"></a>报告

请参阅[运行状况服务报告](health-service-reports.md)。

## <a name="faults"></a>错误

请参阅[运行状况服务错误](health-service-faults.md)。

## <a name="actions"></a>操作

请参阅[运行状况服务操作](health-service-actions.md)。

## <a name="automation"></a>自动化  

本部分介绍工作流，这自动磁盘生命周期的运行状况服务。  

### <a name="disk-lifecycle"></a>磁盘生命周期   

运行状况服务自动大多数阶段物理磁盘生命周期。 假设你的部署的初始状态处于完美健康-是说一句所有物理磁盘正常工作。  

#### <a name="retirement"></a>停用  

当可以不再使用它们，并引发相应故障，物理磁盘自动已停用。 有几种情况：  

-   媒体失败： 物理磁盘明确失败，或折断，必须取代。  

-   丢失通信： 物理磁盘具有连续的超过 15 分钟失去连接。  

-   响应： 物理磁盘具有表现超过 5.0 秒三或一小时内更多时间的延迟。  

>[!NOTE]
> 如果连接到多物理磁盘丢失次，或运行状况服务将给整个节点或设置箱存储，*不*停用这些磁盘，因为它们很可能无法根问题。  

如果磁盘停用充当的缓存多物理磁盘，这些将自动重新分配到另一个缓存磁盘如果可用。 不不需要任何特殊的用户操作。  

#### <a name="restoring-resiliency"></a>还原复原  

一旦物理磁盘已停用，运行状况服务将立即开始复制到剩余物理磁盘，若要还原完整复原及其数据。 完成后的数据是完全安全，并重新出现故障能力。  

>[!NOTE]
> 此即时还原需要剩余物理磁盘足够可用的容量。  

#### <a name="blinking-the-indicator-light"></a>闪烁的指示灯  

如果可能，请运行状况服务将开始闪烁停用的物理磁盘或其插槽指示灯。 这将继续无限期，直到更换停用的磁盘。  

>[!NOTE]
> 在某些情况下，磁盘可能失败甚至其指示灯，不能无法正常工作的方式等总体电源胡子。  

#### <a name="physical-replacement"></a>物理更换  

你应当替换停用的物理磁盘时可能。 大多数情况下，这包含热交换-即 不需要关闭节点或存储外壳接通电源。 请参阅有关有用的位置和一部分信息错误。  

#### <a name="verification"></a>验证

替换磁盘插入时，它将验证针对支持组件文档 （请参阅下一步部分）。

#### <a name="pooling"></a>池  

如果允许，到其前置池输入使用自动替换替换磁盘。 在此情况下，系统会返回为其初始状态完美的运行状况，并故障消失。  

## <a name="supported-components-document"></a>受支持的组件文档  

运行状况服务提供强制机制限制使用存储空间直通到受支持组件文档管理员或解决方案供应商提供的这些组件。 这可用于防止错误的使用不受支持的硬件由你或其他人，它可帮助的保修或支持合同合规性。 此功能当前仅限于物理磁盘设备，包括 ssd 的系统，硬盘，而且 NVMe 驱动器。 在型号、 （可选）、 制造商和固件版本 （可选） 上，可以限制支持组件文档。

### <a name="usage"></a>使用情况  

受支持组件文档使用 XML 灵感语法。 我们建议使用你最喜欢的文本编辑器，如 Visual Studio 代码 (可用免费[此处](http://code.visualstudio.com/)) 或记事本，创建一个 XML 文档，你可以保存并重新使用该值。

#### <a name="sections"></a>部分

文档有两个独立的各部分：**磁盘**和**缓存**。

如果**磁盘**提供部分时，列出的驱动器允许加入池。 任何未列出的驱动器不能加入池，它可以有效地消除他们使用生产中。 如果本部分留空，将允许任何驱动器加入池。

如果**缓存**提供部分、 列出的驱动器将用于缓存。 如果本部分留空，存储空间直通将尝试自行也猜不到根据的媒体类型和总线类型。 例如，如果你的部署使用固态驱动器 (SSD) 与硬盘驱动器 （硬盘），前者自动为选择缓存;但是，如果你的部署使用所有 flash，你可能需要指定你希望用于下面缓存的更高版本耐受设备。

>[!IMPORTANT]
> 已汇集在一起的驱动器，并且在使用支持组件文档不会应用。  

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
        <BinaryPath>\\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
  </Disks>

  <Cache>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Cache>

</Components>

```

若要在列表中的多个驱动器，只需添加其他**&lt;磁盘&gt;**两部分内的标签。

以便此 XML，插入部署存储空间直通时，使用**XML**标志：

```PowerShell
Enable-ClusterS2D -XML <MyXML>
```

若要设置或对其进行修改支持组件存储空间直通部署后 （即后已经在运行状况服务），使用以下 PowerShell cmdlet:

```PowerShell
$MyXML = Get-Content <\\path\to\file.xml> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>模型、 制造商和固件版本属性完全应匹配的值你获得使用**获取物理磁盘**cmdlet。 这可能不同于你的"常识"期望，具体取决于你的供应商实现。 例如，而不是"Contoso，"制造商可能"CONTOSO-LTD"或者"Contoso XZY9000"模型时可能空白。  

你可以使用以下 PowerShell cmdlet，验证：  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>设置

请参阅[运行状况服务设置](health-service-settings.md)。

## <a name="see-also"></a>请参阅

- [运行状况服务报告](health-service-reports.md)
- [运行状况服务的错误](health-service-faults.md)
- [运行状况服务操作](health-service-actions.md)
- [运行状况服务设置](health-service-settings.md)
- [直接在 Windows Server 2016 的存储空间](../storage/storage-spaces/storage-spaces-direct-overview.md)
