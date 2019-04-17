---
title: "更新、备份和还原软件定义的网络的基础结构。"
description: "本主题介绍的软件定义网络指南如何在 Windows Server 2016 的更新、 备份和还原 SDN 基础结构的一部分。"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: grcusanz
ms.openlocfilehash: bb7194ec865db980962853b87d68a84a5446269e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="update-backup-and-restore-software-defined-networking-infrastructure"></a>更新、备份和还原软件定义的网络的基础结构。

>适用于：Windows Server（半年通道），Windows Server 2016

本主题包含以下部分。

- [更新 SDN 基础结构](#bkmk_Updating)
- [备份 SDN 基础结构](#bkmk_backup)
- [从备份中还原 SDN 基础结构](#bkmk_restore)

## <a name="bkmk_Updating"></a>更新 SDN 基础结构

更新是所有操作系统组件的软件定义网络 (SDN) 系统上都安装的 Windows 更新的过程。  这包括 SDN 启用 HYPER-V 主机，网络控制器虚拟机的功能、 软件负载平衡输入混合虚拟机的功能和 RAS 网关虚拟机的功能。  这些组件的所有已安装的更新的确切的同一组至关重要。  如果使用 System Center 虚拟机 Manager 还建议，您也更新的新更新汇总以及。

每个组件的更新使用来执行的任何标准方法以安装 windows 更新，但是步骤如下所述必须以确保降至最低下时间工作负载，并确保网络控制器数据库完整性遵循。

### <a name="step-1-update-the-management-consoles"></a>第 1 步： 更新管理控制台
在每个在你使用的网络控制器 Powershell 模块的计算机上安装必需更新。  这包括任何你已自行安装的 RSAT NetworkController 角色。  这不包括网络控制器 Vm 本身它们将在更新在第 2 步。

### <a name="step-2-update-the-network-controllers"></a>第 2 步： 网络控制器的更新
这在更新过程中是最重要的步骤，因为每个网络控制器 VM 必须先更新，并且要完全还原到下一个网络控制器群集之前中联机。

将启动带有一网络控制器 VM 并安装所有必需更新。  如有必要，请重启 VM。

继续下一步之前网络控制器 VM 使用获取 networkcontrollernode 来检查更新时的节点状态，并且重新启动。  等待网络控制器节点以重新启动周期期间转，然后再返回再次。  VM 已经后重新启动后，它仍然可能需要几分钟时间才能回退向上状态。

#### <a name="example-using-get-networkcontrollernode-to-check-the-status-of-network-controller-nodes"></a>示例： 使用获取 networkcontrollernode 检查网络控制器节点的状态

此示例显示的输出运行获取-networkcontrollernode 从内网络控制器虚拟机的功能之一。  你只要显示 NCNode1.contoso.com 已关闭，虽然其他两个节点正常运行。  你必须等待几分钟，直到状态该节点变为向上前继续使用更新的任何其他节点。

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

仅所有网络控制器节点中向上状态后可以你重复这些步骤其他每个网络控制器节点。  继续更新一次一个的每个节点。

网络控制器节点的所有更新后网络控制器将更新 microservices 在一小时内网络控制器群集运行。  你可以触发使用更新 networkcontroller cmdlet 立即更新。

#### <a name="example-using-update-networkcontroller-to-force-network-controller-to-update"></a>示例： 使用更新 networkcontroller 强制网络控制器更新

当没有其余要安装的更新，此命令显示更新 networkcontroller 结果。

```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

### <a name="step-3-update-slb-muxes"></a>第 3 步： 更新 SLB Muxes

在每个 SLB 输入混合 VM 一个上安装更新，以确保负载平衡基础结构连续发布一次。

### <a name="step-4-update-hyper-v-hosts-and-ras-gateways"></a>第 4 步： 更新 Hyper V 主机和 RAS 网关

因为无法实时迁移，而不会丢失租户连接 RAS 网关虚拟机的功能，必须格外小心以最小化次数连接将进行故障转移到新的 RAS 网关更新周期期间该租户。  按协调主机和 RAS 网关更新每个租户将仅故障转移最一次。  

对于每个主机，通过主机包含 RAS 网关待机模式中启动，请按照以下步骤：

1.  撤离实时迁移能够虚拟机的功能主机。  在主机上，应保持 RAS 网关虚拟机的功能。
2.  在该主机上的每个网关虚拟机上安装的更新。
3.  如果更新要求执行网关 VM 重新启动，然后重新启动 VM。  
4.  包含网关只是更新的 VM 在主机上安装的更新。
5.  如果所需的更新，重新启动主机。
6.  每个其他主机重复包含待机网关。  如果没有待机网关保持，然后按照以下相同的所有剩余主机。

## <a name="bkmk_backup"></a>备份 SDN 基础结构

网络控制器数据库的定期备份至关重要以确保业务连续性一旦灾难或数据丢失。  备份网络控制器虚拟机的功能是不足，因为它不能保证该仲裁保留跨多个网络控制器节点。
要求：
 * SMB 共享和凭据读取/写入到共享和文件系统的权限。
 * 如果使用 GMSA 以及安装网络控制器，你可以选择使用组托管服务帐户 (GMSA)。

请按照以下步骤进行备份：

1. 备份网络控制器 Vm 使用您的选择，VM 备份方法或使用 HYPER-V 导出的每个网络控制器 VM 副本。  这将确保执行完全重新生成包括还原基础结构虚拟机的功能时，如果进行解密数据库必要的证书存在。
2. 如果你使用的系统中心虚拟机管理器 (SCVMM)，停止 SCVMM service 和备份通过 SQL Server，以确保在此期间，这可以创建的备份网络控制器和 SCVMM 一致，对 SCVMM 进行任何更新。  不要重新启动 SCVMM 服务网络控制器备份完成前。
3. 备份使用新 networkcontrollerbackup 网络控制器数据库。

 #### <a name="example-backing-up-the-network-controller-database"></a>示例： 网络控制器数据库备份
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

4. 使用获取 networkcontrollerbackup 检查完成和成功的备份。

 #### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>示例： 检查网络控制器备份操作的状态

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

5.  如果使用的 SCVMM 你现在可以开始 SCVMM 服务。

## <a name="bkmk_restore"></a>从备份中还原 SDN 基础结构

还原是从备份返回到运行状态 SDN 环境中还原所有必需的组件的进程。  这些步骤因略有组件被恢复量。

1. 如有必要，然后重新部署 HYPER-V 主机和必要的存储。

2. 如有必要，请从备份还原网络控制器虚拟机的功能、 RAS 网关虚拟机的功能和输入混合虚拟机的功能。 

3. 停止 HYPER-V 的所有主机上的 NC 主机代理和 SLB 主机代理

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. 停止 RAS 网关虚拟机的功能

5. 停止 SLB 输入混合虚拟机的功能

6. 还原使用新 networkcontrollerrestore cmdlet 网络控制器。

 #### <a name="example-restoring-a-network-controller-database"></a>示例： 还原网络控制器数据库
 
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

7. 检查还原 ProvisioningState 了解当还原已成功完成。

 #### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>示例： 检查状态的网络控制器数据库还原

    ```
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

8. 如果使用 SCVMM，还原 SCVMM 数据库使用网络控制器备份次创建的备份。

9. 如果从备份还原工作负载虚拟机的功能，你可以立即执行该操作。

10. 使用调试 networkcontrollerconfigurationstate cmdlet 检查你的系统运行状况。

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

配置的状态消息可能显示的信息，请参阅[解决 Windows Server 2016 软件定义网络堆栈](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)。