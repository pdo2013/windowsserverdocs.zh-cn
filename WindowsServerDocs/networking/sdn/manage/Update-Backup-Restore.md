---
title: 升级、 备份和还原 SDN 基础结构
description: 在本主题中，您将学习如何更新、 备份和还原 SDN 基础结构。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7916377f58261d0ccaa3fa24f135fccca3d5e79b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446333"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>升级、 备份和还原 SDN 基础结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，您将学习如何更新、 备份和还原 SDN 基础结构。 

## <a name="upgrade-the-sdn-infrastructure"></a>SDN 基础结构升级
SDN 基础结构可以从 Windows Server 2016 升级到 Windows Server 2019。 对于升级排序，按照"更新 SDN 基础结构"一节中提到的相同的步骤序列。 在升级之前，建议执行网络控制器数据库的备份。

对于网络控制器的计算机，使用 Get NetworkControllerNode cmdlet 升级完成后检查节点的状态。 请确保，返回来自节点状态更改为"Up"然后再升级其他节点。 升级后的所有网络控制器节点后，网络控制器都更新网络控制器群集中运行一小时内的微服务。 可以触发使用更新 networkcontroller cmdlet 立即更新。 

所有软件定义网络 (SDN) 系统，其中包括操作系统组件上都安装相同的 Windows 更新：

- SDN 启用的 HYPER-V 主机
- 网络控制器 Vm
- 软件负载均衡器 Mux Vm
- RAS 网关 Vm 

>[!IMPORTANT]
>如果使用 System Center 虚拟管理器，则必须与最新的更新汇总来更新它。

更新每个组件时，您可以使用任何一种标准方法安装 Windows 更新。 但是，若要确保最短停机时间的工作负荷和网络控制器数据库的完整性，请按照下列步骤：

1. 更新管理控制台。<p>每个位置使用的网络控制器 Powershell 模块的计算机上安装更新。  任意位置包括具有单独安装的 RSAT NetworkController 角色。 不包括网络控制器 Vm 本身;在下一步中更新它们。

2. 第一个网络控制器 VM 上安装所有更新并重新启动。

3. 在前进到下一个网络控制器 VM 前, 使用`get-networkcontrollernode`cmdlet 来检查更新并重新启动的节点的状态。

4. 在重新启动周期中，等待要关闭然后再回来再次的网络控制器节点。<p>后重新启动 VM，可能需要几分钟时间，它将重新进入 **_向上_** 状态。 输出的示例，请参阅 

5. 每个 SLB Mux VM 之一上安装更新，以确保持续可用性的负载均衡器基础结构的一次。

6. 更新 HYPER-V 主机和 RAS 网关，从包含在 RAS 网关的主机开始**待机**模式。<p>不能实时迁移 RAS 网关 Vm，而不会丢失租户连接。 在更新周期中，您必须注意最大程度减少的次数租户连接故障转移到新的 RAS 网关。 通过协调更新的主机和 RAS 网关，每个租户进行故障转移一次，最多。

    a. 撤走主机能够进行实时迁移的 Vm。<p>RAS 网关 Vm 应保留在主机上。

    b. 在此主机上的每个网关 VM 上安装更新。

    c. 如果更新要求网关 VM 重启然后重启 VM。  

    d. 包含网关刚刚更新的 VM 在主机上安装更新。

    e. 如果的更新需要重启主机。

    f. 每个其他主机重复包含备用网关。<p>如果仍未备用网关，然后为剩余的所有主机遵循相同的步骤。


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>例如：使用 get networkcontrollernode cmdlet 

在此示例中，您看到的输出`get-networkcontrollernode`cmdlet 从运行在网络控制器 Vm 之一中。  

示例输出中看到节点的状态是：

- NCNode1.contoso.com = Down
- NCNode2.contoso.com = Up
- NCNode3.contoso.com = Up

>[!IMPORTANT]
>对更改的节点，必须等待几分钟时间，直到状态 _**向上**_ 更新任何其他节点，一次一个地之前。

在更新后的所有网络控制器节点，网络控制器都更新网络控制器群集中运行一小时内的微服务。 

>[!TIP]
>可触发立即更新使用`update-networkcontroller`cmdlet。


```Powershell
PS C:\> get-networkcontrollernode
Name            : NCNode1.contoso.com
Server          : NCNode1.Contoso.com
FaultDomain     : fd:/NCNode1.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Down

Name            : NCNode2.Contoso.com
Server          : NCNode2.contoso.com
FaultDomain     : fd:/ NCNode2.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up

Name            : NCNode3.Contoso.com
Server          : NCNode3.Contoso.com
FaultDomain     : fd:/ NCNode3.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up
```

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>例如：使用更新 networkcontroller cmdlet
在此示例中，您看到的输出`update-networkcontroller`cmdlet 来强制更新网络控制器。 

>[!IMPORTANT]
>当有更多的更新安装时，请运行此 cmdlet。


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>备份 SDN 基础结构

网络控制器数据库的定期备份可确保在发生灾难或数据丢失时的业务连续性。  因为它不能确保该会话将继续在多个网络控制器节点，网络控制器 Vm 备份不足够。

**要求：**
* SMB 共享和使用读/写权限的共享和文件系统的凭据。
* 如果使用 GMSA 也安装网络控制器，可以选择使用组托管服务帐户 (GMSA)。

**过程：**

1. 使用所选的 VM 备份方法或使用 HYPER-V 导出每个网络控制器 VM 的副本。<p>备份网络控制器 VM 可确保必要证书用于解密该数据库存在。  

2. 如果使用 System Center Virtual Machine Manager (SCVMM)，停止 SCVMM 服务，并通过 SQL Server 备份它。<p>此处的目标是确保在此期间，这可能会产生不一致造成的网络控制器备份和 SCVMM 获取进行到 SCVMM 没有更新。  

   >[!IMPORTANT]
   >不要重新启动 SCVMM 服务网络控制器备份完成之前。

3. 备份与网络控制器数据库`new-networkcontrollerbackup`cmdlet。

4. 检查完成和成功与备份`get-networkcontrollerbackup`cmdlet。

5. 如果使用的 SCVMM，启动 SCVMM 服务。



### <a name="example-backing-up-the-network-controller-database"></a>例如：网络控制器数据库备份

```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

# Get or Create Credential object for File share user

$ShareUserResourceId = "BackupUser"

$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }
If ($ShareCredential -eq $null) {
    $CredentialProperties = New-Object Microsoft.Windows.NetworkController.CredentialProperties
    $CredentialProperties.Type = "usernamePassword"
    $CredentialProperties.UserName = "contoso\alyoung"
    $CredentialProperties.Value = "<Password>"

    $ShareCredential = New-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential -Properties $CredentialProperties -ResourceId $ShareUserResourceId -Force
}

# Create backup

$BackupTime = (get-date).ToString("s").Replace(":", "_")

$BackupProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerBackupProperties
$BackupProperties.BackupPath = "\\fileshare\backups\NetworkController\$BackupTime"
$BackupProperties.Credential = $ShareCredential

$Backup = New-NetworkControllerBackup -ConnectionURI $URI -Credential $Credential -Properties $BackupProperties -ResourceId $BackupTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>例如：正在检查网络控制器备份操作的状态

```Powershell
PS C:\ > Get-NetworkControllerBackup -ConnectionUri $URI -Credential $Credential -ResourceId $Backup.ResourceId
| ConvertTo-JSON -Depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerBackup/2017-04-25T16_53_13",
    "InstanceId":  "c3ea75ae-2892-4e10-b26c-a2243b755dc8",
    "Etag":  "W/\"0dafea6c-39db-401b-bda5-d2885ded470e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-25T16_53_13",
    "Properties":  {
                    "BackupPath":  "\\\\fileshare\backups\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  "",
                    "FailedResourcesList":  [

                                            ],
                    "SuccessfulResourcesList":  [
                                                    "/networking/v1/credentials/11ebfc10-438c-4a96-a1ee-8a048ce675be",
                                                    "/networking/v1/credentials/41229069-85d4-4352-be85-034d0c5f4658",
                                                    "/networking/v1/credentials/b2a82c93-2583-4a1f-91f8-232b801e11bb",
                                                    "/networking/v1/credentials/BackupUser",
                                                    "/networking/v1/credentials/fd5b1b96-b302-4395-b6cd-ed9703435dd1",
                                                    "/networking/v1/virtualNetworkManager/configuration",
                                                    "/networking/v1/virtualSwitchManager/configuration",
                                                    "/networking/v1/accessControlLists/f8b97a4c-4419-481d-b757-a58483512640",
                                                    "/networking/v1/logicalnetworks/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8",
                                                    "/networking/v1/logicalnetworks/48610528-f40b-4718-938e-99c2be76f1e0",
                                                    "/networking/v1/logicalnetworks/89035b49-1ee3-438a-8d7a-f93cbae40619",
                                                    "/networking/v1/logicalnetworks/a9c8eaa0-519c-4988-acd6-11723e9efae5",
                                                    "/networking/v1/logicalnetworks/d4ea002c-c926-4c57-a178-461d5768c31f",
                                                    "/networking/v1/macPools/11111111-1111-1111-1111-111111111111",
                                                    "/networking/v1/loadBalancerManager/config",
                                                    "/networking/v1/publicIPAddresses/2c502b2d-b39a-4be1-a85a-55ef6a3a9a1d",
                                                    "/networking/v1/GatewayPools/Default",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8056-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8057-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5910-8056-b4c04f395931",
                                                    "/networking/v1/networkInterfaces/058430d3-af43-4328-a440-56540f41da50",
                                                    "/networking/v1/networkInterfaces/08756090-6d55-4dec-98d5-80c4c5a47db8",
                                                    "/networking/v1/networkInterfaces/2175d74a-aacd-44e2-80d3-03f39ea3bc5d",
                                                    "/networking/v1/networkInterfaces/2400c2c3-2291-4b0b-929c-9bb8da55851a",
                                                    "/networking/v1/networkInterfaces/4c695570-6faa-4e4d-a552-0b36ed3e0962",
                                                    "/networking/v1/networkInterfaces/7e317638-2914-42a8-a2dd-3a6d966028d6",
                                                    "/networking/v1/networkInterfaces/834e3937-f43b-4d3c-88be-d79b04e63bce",
                                                    "/networking/v1/networkInterfaces/9d668fe6-b1c6-48fc-b8b1-b3f98f47d508",
                                                    "/networking/v1/networkInterfaces/ac4650ac-c3ef-4366-96e7-d9488fb661ba",
                                                    "/networking/v1/networkInterfaces/b9f23e35-d79e-495f-a1c9-fa626b85ae13",
                                                    "/networking/v1/networkInterfaces/fdd929f1-f64f-4463-949a-77b67fe6d048",
                                                    "/networking/v1/virtualServers/15a891ee-7509-4e1d-878d-de0cb4fa35fd",
                                                    "/networking/v1/virtualServers/57416993-b410-44fd-9675-727cd4e98930",
                                                    "/networking/v1/virtualServers/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a",
                                                    "/networking/v1/virtualServers/6c812217-5931-43dc-92a8-1da3238da893",
                                                    "/networking/v1/virtualServers/d78b7fa3-812d-4011-9997-aeb5ded2b431",
                                                    "/networking/v1/virtualServers/d90820a5-635b-4016-9d6f-bf3f1e18971d",
                                                    "/networking/v1/loadBalancerMuxes/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d78b7fa3-812d-4011-9997-aeb5ded2b431_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d90820a5-635b-4016-9d6f-bf3f1e18971d_suffix",
                                                    "/networking/v1/Gateways/15a891ee-7509-4e1d-878d-de0cb4fa35fd_suffix",
                                                    "/networking/v1/Gateways/57416993-b410-44fd-9675-727cd4e98930_suffix",
                                                    "/networking/v1/Gateways/6c812217-5931-43dc-92a8-1da3238da893_suffix",
                                                    "/networking/v1/virtualNetworks/b3dbafb9-2655-433d-b47d-a0e0bbac867a",
                                                    "/networking/v1/virtualNetworks/d705968e-2dc2-48f2-a263-76c7892fb143",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.2",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.3",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.4"
                                                ],
                    "InProgressResourcesList":  [

                                                ],
                    "ProvisioningState":  "Succeeded",
                    "Credential":  {
                                        "Tags":  null,
                                        "ResourceRef":  "/credentials/BackupUser",
                                        "InstanceId":  "00000000-0000-0000-0000-000000000000",
                                        "Etag":  null,
                                        "ResourceMetadata":  null,
                                        "ResourceId":  null,
                                        "Properties":  null
                                    }
                }
}
```

## <a name="restore-the-sdn-infrastructure-from-a-backup"></a>从备份中还原 SDN 基础结构

当从备份中还原所有必要的组件时，SDN 环境将返回到操作状态。  

>[!IMPORTANT]
>步骤会有所不同，具体取决于还原的组件数量。


1. 如有必要，重新部署的 HYPER-V 主机和必要的存储。

2. 如有必要，请从备份还原网络控制器 Vm、 RAS 网关 Vm 和 Mux Vm。 

3. 停止 NC 主机代理和所有 HYPER-V 主机上的 SLB 主机代理：

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. 停止 RAS 网关 Vm。

5. 停止 SLB Mux Vm。

6. 还原与网络控制器`new-networkcontrollerrestore`cmdlet。

7. 检查还原**ProvisioningState**知道当还原已成功完成。

8. 如果使用的 SCVMM，还原 SCVMM 数据库使用网络控制器备份时创建的备份。

9. 如果你想要从备份中还原工作负荷的 Vm，请立即。

10. 检查您的系统使用调试 networkcontrollerconfigurationstate cmdlet 的运行状况。

```Powershell
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController "https://NC.contoso.com" -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

### <a name="example-restoring-a-network-controller-database"></a>例如：网络控制器将数据库还原
 
```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

$ShareUserResourceId = "BackupUser"
$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }

$RestoreProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerRestoreProperties
$RestoreProperties.RestorePath = "\\fileshare\backups\NetworkController\2017-04-25T16_53_13"
$RestoreProperties.Credential = $ShareCredential

$RestoreTime = (Get-Date).ToString("s").Replace(":", "_")
New-NetworkControllerRestore -ConnectionURI $URI -Credential $Credential -Properties $RestoreProperties -ResourceId $RestoreTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>例如：检查网络控制器数据库还原状态

```PowerShell
PS C:\ > get-networkcontrollerrestore -connectionuri $uri -credential $cred -ResourceId $restoreTime | convertto-json -depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerRestore/2017-04-26T15_04_44",
    "InstanceId":  "22edecc8-a613-48ce-a74f-0418789f04f6",
    "Etag":  "W/\"f14f6b84-80a7-4b73-93b5-59a9c4b5d98e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-26T15_04_44",
    "Properties":  {
                    "RestorePath":  "\\\\sa18fs\\sa18n22\\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  null,
                    "FailedResourcesList":  null,
                    "SuccessfulResourcesList":  null,
                    "ProvisioningState":  "Succeeded",
                    "Credential":  null
                }
}
```


有关可能出现的配置状态消息的信息，请参阅[Windows Server 2016 软件定义网络堆栈疑难解答](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)。