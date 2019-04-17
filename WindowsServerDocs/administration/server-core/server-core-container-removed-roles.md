---
title: 角色、 角色服务和功能不在服务器核心容器的 Windows Server 版本 1803
description: 了解角色和功能，我们已删除的服务器核心容器图像从 Windows server。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1859892"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>角色、 角色服务和功能不在服务器核心容器的 Windows Server 版本 1803

> 适用于： Windows Server 版本 1803年

Windows server 版本 1803，我们已经[降低到**1.58 GB**的服务器核心容器图像的总体大小](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/)。 我们已进行此设置的方法是优化体系结构和[服务器核心容器](https://docs.microsoft.com/virtualization/windowscontainers/about/)中删除不需要在您的操作。 一些容器中的内容的不起作用，一些已角色和无人已使用的功能。 

> [!IMPORTANT]
> 我们已删除这些从服务器核心**容器**图像，不[服务器核心本身](server-core-roles-and-services.md)。 

下面是功能和从服务器核心容器映像的角色的完整列表：

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Bitlocker 实用程序
<br>BitLocker
<br>BITS
<br>BITSExtensions 上载
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>ClientForNFS 基础结构
<br>容器
<br>CoreFileServer
<br>DataCenterBridging-LLDP 工具
<br>DataCenterBridging
<br>Dedup 核
<br>DeviceHealthAttestationService
<br>DFSN 服务器
<br>服务器-版基础结构 DFSR
<br>DirectoryServices ADAM
<br>DirectoryServices 域控制器
<br>DiskIo QoS
<br>EnhancedStorage
<br>FailoverCluster AdminPak
<br>FailoverCluster AutomationServer
<br>FailoverCluster CmdInterface
<br>FailoverCluster FullServer
<br>FailoverCluster PowerShell
<br>文件服务
<br>FileServerVSSAgent
<br>FRS 基础结构
<br>FSRM 基础结构服务
<br>FSRM 基础结构
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService 包
<br>IdentityServer SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>授权
<br>LightweightServer
<br>Microsoft-超-V-管理-客户端
<br>Microsoft-超-V-脱机
<br>Microsoft-超-V-在线
<br>Microsoft-Hyper-v
<br>Microsoft Windows-FCI-客户端-程序包
<br>Microsoft Windows 的组策略-ServerAdminTools-更新
<br>Microsoft Windows 子系统 Linux
<br>MSRDC 基础结构
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P PnrpOnly
<br>PeerDist
<br>打印-客户端-Gui
<br>打印 LPDPrintService
<br>打印服务器 Foundation 功能
<br>打印服务器角色
<br>QWAVE
<br>RasRoutingProtocols
<br>远程桌面服务
<br>远程访问
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices 角色
<br>RightsManagementServices
<br>RMS 联合身份验证
<br>SBMgr UI
<br>ServerCore 驱动程序的常规 WOW64
<br>ServerCore-驱动程序-常规
<br>ServerForNFS 基础结构
<br>ServerManager-核心-RSAT-功能的工具
<br>ServerMediaFoundation
<br>ServerMigration
<br>SessionDirectory
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol 服务器
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>存储-副本 AdminPack
<br>存储副本
<br>Tpm-PSH Cmdlet
<br>UpdateServices 数据库
<br>UpdateServices 服务
<br>UpdateServices WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation 完全角色
<br>Web 应用程序代理
<br>Web 访问
<br>WebEnrollmentServices
<br>Windows Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders 服务器
<br>WSS 产品程序包

</div>