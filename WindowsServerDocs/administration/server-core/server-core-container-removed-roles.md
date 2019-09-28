---
title: 不在服务器核心容器中的角色、角色服务和功能-Windows Server，版本1803
description: 了解我们从 Windows Server 的 Server Core 容器映像中删除的角色和功能。
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 41b5a9ac32066f1b2a41de84f66b9be79252c336
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383408"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>不在服务器核心容器中的角色、角色服务和功能-Windows Server，版本1803

> 适用于：Windows Server 版本 1803

在 Windows Server 版本1803中，我们已将[服务器核心容器映像的总大小缩小到了**1.58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/)。 完成此操作的方法是优化体系结构，并删除[服务器核心容器](https://docs.microsoft.com/virtualization/windowscontainers/about/)中不需要的项目。 有些功能在容器中不起作用，有些角色和功能没有被使用。 

> [!IMPORTANT]
> 我们从服务器核心**容器**映像（而非[服务器核心本身](server-core-roles-and-services.md)）中删除了这些映像。 

下面是从服务器核心容器映像中删除的功能和角色的完整列表：

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Bitlocker-实用工具
<br>BitLocker
<br>BITS
<br>BITSExtensions-上传
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>ClientForNFS-基础结构
<br>容器
<br>CoreFileServer
<br>DataCenterBridging-LLDP 工具
<br>DataCenterBridging
<br>重复数据删除-核心
<br>DeviceHealthAttestationService
<br>DFSN-服务器
<br>DFSR-基础结构-ServerEdition
<br>Microsoft.directoryservices-ADAM
<br>Microsoft.directoryservices-控制器
<br>DiskIo-QoS
<br>EnhancedStorage
<br>Failovercluster-cmdinterface 该-AdminPak
<br>Failovercluster-cmdinterface 该-AutomationServer
<br>Failovercluster-cmdinterface 该-Failovercluster-cmdinterface
<br>Failovercluster-cmdinterface 该-FullServer
<br>Failovercluster-cmdinterface 该-PowerShell
<br>文件-服务
<br>FileServerVSSAgent
<br>FRS-基础结构
<br>FSRM-基础结构-服务
<br>FSRM-基础结构
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService 包
<br>IdentityServer
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>Add-windowsfeature fs-iscsitargetserver-PowerShell
<br>Add-windowsfeature fs-iscsitargetserver
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>授权
<br>LightweightServer
<br>Microsoft-Hyper-v 管理-客户端
<br>Microsoft-Hyper-v-脱机
<br>Microsoft-Hyper-v-联机
<br>Microsoft-Hyper-v
<br>Microsoft FCI-程序包
<br>Microsoft-windows-grouppolicy-ServerAdminTools-Update
<br>Microsoft-子系统-Linux
<br>MSRDC-基础结构
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>打印-客户端-Gui
<br>打印-LPDPrintService
<br>打印-服务器-基础-功能
<br>打印-服务器-角色
<br>QWAVE
<br>RasRoutingProtocols
<br>远程桌面服务
<br>RemoteAccess
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-角色
<br>RightsManagementServices
<br>RMS-联合身份验证
<br>SBMgr-UI
<br>ServerCore-常规-WOW64
<br>ServerCore-驱动程序-常规
<br>ServerForNFS-基础结构
<br>ServerManager-功能-工具
<br>ServerMediaFoundation
<br>Servermigration.log
<br>SessionDirectory
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol-服务器
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>存储-AdminPack
<br>存储-副本
<br>PSH Cmdlet
<br>Updateservices-api-数据库
<br>Updateservices-api-服务
<br>Updateservices-api-WidDatabase
<br>Updateservices-api
<br>VmHostAgent
<br>VolumeActivation-完全角色
<br>Web 应用程序-代理
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-服务器
<br>WSS-产品包

</div>