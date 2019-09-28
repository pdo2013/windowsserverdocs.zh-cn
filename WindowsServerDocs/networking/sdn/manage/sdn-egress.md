---
title: 虚拟网络中的出口计量
description: 云网络盈利的一个基本方面是网络带宽出口。 例如，Microsoft Azure 业务模型中的出站数据传输。 根据在给定计费周期内通过 Internet 从 Azure 数据中心移出的数据总量对出站数据收费。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: e68a3889867b75152ea941ac1d8eb113b9acd3cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406006"
---
# <a name="egress-metering-in-a-virtual-network"></a>虚拟网络中的出口计量

>适用于：Windows Server 2019


云网络盈利的一个重要方面是能够按网络带宽的使用情况进行计费。 根据在给定计费周期内通过 Internet 移出数据中心的数据总量对出站数据收费。

Windows Server 2019 中 SDN 网络流量的出口计量功能可以提供出站数据传输的使用情况计量。 离开每个虚拟网络的网络流量可以单独进行跟踪，使其可以从计费计算中排除。 对于未包含在其中一个未开票地址范围内的目标 IP 地址绑定的数据包，将作为计费的出站数据传输进行跟踪。

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>虚拟网络未开票地址范围（IP 范围的白名单）

可以在现有虚拟网络的**UnbilledAddressRanges**属性下找到未开票地址范围。 默认情况下，不会添加任何地址范围。

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

输出如下所示：
   ```
    AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
    DhcpOptions            :
    UnbilledAddressRanges  :
    ConfigurationState     :
    ProvisioningState      : Succeeded
    Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                         29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
    VirtualNetworkPeerings :
    EncryptionCredential   :
    LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>例如：管理虚拟网络的未开票地址范围

你可以通过设置虚拟网络的**UnbilledAddressRange**属性来管理从计费的出口计量中排除的 IP 子网前缀集。  虚拟网络上的网络接口发送的任何流量与其中一个前缀相匹配的目标 IP 地址都不会包含在 BilledEgressBytes 属性中。

1.  更新**UnbilledAddressRanges**属性，使其包含不会进行访问计费的子网。

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >如果要添加多个 IP 子网，请在每个 IP 子网之间使用逗号。  不要包含逗号前后的任何空格。

2.  用修改后的**UnbilledAddressRanges**属性更新虚拟网络资源。

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    输出如下所示：
      ```
         Confirm
         Performing the operation 'New-NetworkControllerVirtualNetwork' on entities of type
         'Microsoft.Windows.NetworkController.VirtualNetwork' via
         'https://sdn.contoso.com/networking/v3/virtualNetworks/VNet1'. Are you sure you want to continue?
         [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y


         Tags             :
         ResourceRef      : /virtualNetworks/VNet1
         InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
         Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
         ResourceMetadata :
         ResourceId       : VNet1
         Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
      ```


3. 检查虚拟网络以查看配置的**UnbilledAddressRanges**。

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   你的输出现在将如下所示：
   ```
   AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
   DhcpOptions            :
   UnbilledAddressRanges  : 10.10.2.0/24,192.168.2.0/24
   ConfigurationState     :
   ProvisioningState      : Succeeded
   Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                        29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
   VirtualNetworkPeerings :
   EncryptionCredential   :
   LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>检查虚拟网络的未开票出口使用情况

配置**UnbilledAddressRanges**属性后，可以检查虚拟网络中每个子网的计费和未开票出口使用情况。 出口流量将每四分钟更新一次，并提供计费和未开票范围的总字节数。

以下属性可用于每个虚拟子网：

-   **UnbilledEgressBytes**显示连接到此虚拟子网的网络接口发送的未开票字节数。 未开票 bytes 是发送到作为父虚拟网络**UnbilledAddressRanges**属性一部分的地址范围的字节数。

-   **BilledEgressBytes**显示连接到此虚拟子网的网络接口发送的计费字节数。 计费字节数是发送到不属于父虚拟网络的**UnbilledAddressRanges**属性的地址范围的字节数。

使用以下示例查询出口使用：

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

输出如下所示：
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---
