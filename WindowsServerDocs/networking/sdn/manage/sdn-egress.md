---
title: 虚拟网络中的出口计量
description: 云网络货币化一个基本方面是网络带宽出口。 例如-出站数据传输在 Microsoft Azure 中的业务模型。 出站数据的基础上的总移出 Azure 数据中心通过 Internet 在给定计费周期中的数据量收费。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: ad1bed11308420e271b8e06410d5a4548181314a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876418"
---
# <a name="egress-metering-in-a-virtual-network"></a>虚拟网络中的出口计量

>适用于：Windows Server 2019


云网络货币化一个基本方面能够通过网络带宽使用量计费。 出站数据的基础上的总移出数据中心通过 Internet 在给定计费周期中的数据量收费。

在 Windows Server 2019 SDN 网络流量的计量的出口，可为出站数据传输提供了用量计量表。 保留每个虚拟网络，但保留在数据中心的网络流量可以通过分别跟踪以便可以从计费计算中排除。 绑定中的某个开票的地址范围不包括的目标 IP 地址的数据包将跟踪出站数据传输计费。

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>未出单虚拟网络地址范围 （的 IP 范围加入允许列表）

您可以找到下的开票的地址范围**UnbilledAddressRanges**的现有虚拟网络的属性。 默认情况下，没有添加任何地址范围。

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

你的输出将类似于此：
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


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>例如：管理虚拟网络的开票的地址范围

你可以管理组的 IP 子网前缀从计费的出口计数通过设置排除**UnbilledAddressRange**虚拟网络的属性。  发送具有相匹配的前缀之一的目标 IP 地址的虚拟网络上的网络接口的任何流量不会包含 BilledEgressBytes 属性中。

1.  更新**UnbilledAddressRanges**属性以包含将不计费的访问权限的子网。

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```
    
    >[!TIP]
    >如果添加多个 IP 子网，请使用逗号分隔每个 IP 子网。  之前或之后逗号，则不包含任何空格。

2.  使用修改后更新的虚拟网络资源**UnbilledAddressRanges**属性。

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    你的输出将类似于此：
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


3.  检查以查看已配置的虚拟网络**UnbilledAddressRanges**。

    ```PowerShell
    (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
    ```

    你的输出将类似于此：
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

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>检查计费的虚拟网络未出单的出口使用情况

在配置之后**UnbilledAddressRanges**属性，可以检查虚拟网络中的每个子网的计费和未出单出口使用情况。 传出流量分钟更新一次每四个与计费和未出单范围的总字节数。

以下属性是可用于每个虚拟子网：

-   **UnbilledEgressBytes**显示发送的网络接口连接到此虚拟子网未出单字节数。 未出单的字节是发送到一部分的地址范围的字节数**UnbilledAddressRanges**父虚拟网络的属性。

-   **BilledEgressBytes**显示计费的网络接口连接到此虚拟子网发送的字节数。 计费的字节是发送到不是地址范围的字节数的一部分**UnbilledAddressRanges**父虚拟网络的属性。

使用下面的示例查询出口使用情况：

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

你的输出将类似于此：
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```
    

---