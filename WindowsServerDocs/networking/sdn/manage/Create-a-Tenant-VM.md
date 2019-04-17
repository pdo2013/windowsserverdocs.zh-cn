---
title: 创建 VM 和网络或 VLAN 连接到虚拟租户
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 001eb3efa073e4ffbdfad41f88949303159a7274
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>创建 VM 和网络或 VLAN 连接到虚拟租户

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以创建一个租户虚拟计算机 \(VM\)，然后将 VM 连接到你使用 HYPER-V 网络虚拟化创建的任一虚拟网络或虚拟的本地网络区域 \(VLAN\)。 

本主题包含以下部分。

- [创建 VM 和使用的 Windows PowerShell 网络控制器 cmdlet 连接到虚拟网络](#bkmk_vn)
- [创建 VM 和使用 NetworkControllerRESTWrappers 连接到 VLAN](#bkmk_vlan)

## <a name="requirements"></a>要求
在以下部分中执行这些过程之前, 注意以下要求。

1. 必须与静态媒体访问控制 \(MAC\) 地址，以便 VM 的 MAC 地址不会更改虚拟机生命周期内创建 VM 网络适配器。 
>[!NOTE]
>如果 VM MAC 地址更改虚拟机生命周期内，网络控制器无法配置的网络适配器的必要策略。 没有配置的网络适配器的策略，如果该网络适配器无法从处理网络通信和网络的所有通信无法正常都工作。 

2. 如果 VM 需要在启动时的网络访问权限，请务必不启动后的最终配置步-在虚拟机上网络适配器端口设置接口 ID 直到 VM。 如果你将开始 VM 才能完成此步骤，VM 无法进行通信网络上直到网络控制器中创建网络接口和控制器的所有适用的策略虚拟网络策略应用访问控制列出 \(ACLs\) 和服务 \(QoS\) 质量。

你还可以使用用于部署虚拟装置本主题中所述的过程。 几个步骤，使用中，您可以配置装置进程，或检查 flow 或其他 Vm 虚拟网络上的数据包。

>[!IMPORTANT]
>以下部分包括包含许多参数示例值的示例 Windows PowerShell 命令。 确保你在运行以下命令之前就提供适用于你的部署的值与替换示例值在这些命令。  

## <a name="bkmk_vn"></a>创建 VM 和使用的 Windows PowerShell 网络控制器 cmdlet 连接到虚拟网络

此部分中包括以下主题。

1.  [VM 网络适配器拥有静态的 MAC 地址创建 VM](#bkmk_create)
2.  [获取包含你想要连接的网络适配器的子网虚拟网络](#bkmk_getvn)
3.  [在网络控制器中创建网络接口对象](#bkmk_object)
4.  [获取网络接口从网络控制器实例的 Id](#bkmk_getinstance)
5.  [网络适配器端口 HYPER-V 虚拟机上设置接口 ID](#bkmk_setinstance)
6.  [开始 VM](#bkmk_start)

### <a name="bkmk_create"></a>VM 网络适配器拥有静态的 MAC 地址创建 VM

若要创建网络适配器拥有静态的 MAC 地址 VM 中，使用下面的示例命令。

    
    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
    Set-VM -Name "MyVM" -ProcessorCount 4
    
    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
    

### <a name="bkmk_getvn"></a>获取包含你想要连接的网络适配器的子网虚拟网络
确保，已在使用此命令之前创建了虚拟网络。 有关详细信息，请参阅[创建、 删除或更新租户虚拟网络](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks)。

若要获取虚拟网络，请使用下面的示例命令。

    
    $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
    

>[!NOTE]
>如果你针对此网络接口需要自定义 Acl，然后创建 ACL 现在使用主题中的说明进行操作[使用访问控制列出 (Acl) 管理数据中心网络交通 Flow 到](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)

### <a name="bkmk_object"></a>在网络控制器中创建网络接口对象

若要在网络控制器中创建网络接口对象，请使用下面的示例命令。

>[!NOTE]
>如果你之前的步骤后创建自定义 ACL，你可以立即使用它。

    
    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "001122334455" 
    $vmnicproperties.PrivateMacAllocationMethod = "Static" 
    $vmnicproperties.IsPrimary = $true 

    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = @("24.30.1.11", "24.30.1.12")
    
    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_IP1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = “24.30.1.101”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"
    
    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $vnet.Properties.Subnets[0].ResourceRef
    
    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri
    

### <a name="bkmk_getinstance"></a>获取网络接口从网络控制器实例的 Id
若要为网络接口从网络控制器实例 Id，请使用下面的示例命令。

    
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
    

### <a name="bkmk_setinstance"></a>网络适配器端口 HYPER-V 虚拟机上设置接口 ID
要设置 HYPER-V VM 网络适配器端口界面 ID，请使用下面的示例命令。

>[!NOTE]
>你必须运行以下命令 HYPER-V 主机上安装 VM 的位置。

    
    #Do not change the hardcoded IDs in this section, because they are fixed values and must not change.
    
    $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"
    
    $vmNics = Get-VMNetworkAdapter -VMName “MyVM”
    
    $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNics
    
    if ($CurrentFeature -eq $null)
    {
    $Feature = Get-VMSystemSwitchExtensionPortFeature -FeatureId $FeatureId
    
    $Feature.SettingData.ProfileId = "{$($nic.InstanceId)}"
    $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
    $Feature.SettingData.CdnLabelString = "TestCdn"
    $Feature.SettingData.CdnLabelId = 1111
    $Feature.SettingData.ProfileName = "Testprofile"
    $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
    $Feature.SettingData.VendorName = "NetworkController"
    $Feature.SettingData.ProfileData = 1
    
    Add-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNics
    }
    else
    {
    $CurrentFeature.SettingData.ProfileId = "{$($nic.InstanceId)}"
    $CurrentFeature.SettingData.ProfileData = 1
    
    Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
    }
    

### <a name="bkmk_start"></a>开始 VM
若要开始 VM，请使用下面的示例命令。

    
    Get-VM -Name “MyVM” | Start-VM 
    
你现在成功创建 VM、 VM 连租户虚拟网络，并开始 VM，以便它可以处理租户工作负载。

## <a name="bkmk_vlan"></a>创建 VM 和使用 NetworkControllerRESTWrappers 连接到 VLAN

此部分中包括以下主题。

1. [创建 VM 和分配静态的 MAC 地址](#bkmk_mac)
2. [上 VM 网络适配器设置 VLAN ID](#bkmk_vid)
3. [获取逻辑网并创建网络接口](#bkmk_subnet)
4. [设置 HYPER-V 端口上的实例 Id](#bkmk_instance)
5. [开始 VM](#bkmk_startvlan)


###<a name="bkmk_mac"></a>创建 VM 和分配静态的 MAC 地址
若要创建 VM 并将静态媒体访问控制 \(MAC\) 地址分配给 VM，可以使用下面的示例命令。

    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

    Set-VM -Name "MyVM" -ProcessorCount 4

    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 

###<a name="bkmk_vid"></a>上 VM 网络适配器设置 VLAN ID
若要在网络适配器设置 VLAN ID，你可以使用下面的示例命令。


    Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123


###<a name="bkmk_subnet"></a>获取逻辑网并创建网络接口

若要获取逻辑网并使用逻辑网网络接口，可以使用下面的示例命令。


    $logicalnet = get-networkcontrollerLogicalNetwork -connectionuri $uri -ResourceId "00000000-2222-1111-9999-000000000002"

    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "00-1D-C8-B7-01-02"
    $vmnicproperties.PrivateMacAllocationMethod = "Static"
    $vmnicproperties.IsPrimary = $true 
    
    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = $logicalnet.Properties.Subnets[0].DNSServers

    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_Ip1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = “10.127.132.177”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"

    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $logicalnet.Properties.Subnets[0].ResourceRef

    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    $vnic = New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri

    $vnic.InstanceId
    

###<a name="bkmk_instance"></a>设置 HYPER-V 端口上的实例 Id
若要设置的 HYPER-V 端口实例 Id，你可以使用下面的示例命令 HYPER-V 主机上 VM 所在的位置。

  
    #The hardcoded Ids in this section are fixed values and must not change.
    $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"

    $vmNics = Get-VMNetworkAdapter -VMName “MyVM”

    $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNic
        
    if ($CurrentFeature -eq $null)
    {
        $Feature = Get-VMSystemSwitchExtensionFeature -FeatureId $FeatureId
        
        $Feature.SettingData.ProfileId = "{$InstanceId}"
        $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
        $Feature.SettingData.CdnLabelString = "TestCdn"
        $Feature.SettingData.CdnLabelId = 1111
        $Feature.SettingData.ProfileName = "Testprofile"
        $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
        $Feature.SettingData.VendorName = "NetworkController"
        $Feature.SettingData.ProfileData = 1
                
        Add-VMSwitchExtensionFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNic
    }        
    else
    {
        $CurrentFeature.SettingData.ProfileId = "{$InstanceId}"
        $CurrentFeature.SettingData.ProfileData = 1

        Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
    }


###<a name="bkmk_startvlan"></a>开始 VM
若要开始 VM，你可以使用下面的示例命令。


    Get-VM -Name “MyVM” | Start-VM 

你现在成功创建 VM、 VM 连 VLAN，并开始 VM，以便它可以处理租户工作负载。

  

