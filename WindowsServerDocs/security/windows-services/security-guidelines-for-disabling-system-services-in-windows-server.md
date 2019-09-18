---
title: Windows Server 2016 中的系统服务的安全指导原则
description: 有关在带有桌面体验的 Windows Server 2016 中禁用服务的安全准则
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: fe2f82c373b0014a3f385dcfad77ec11a0b1e6c0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870208"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>有关在带有桌面体验的 Windows Server 2016 中禁用系统服务的指南

适用于：Windows Server 2016

Windows 操作系统包含许多提供重要功能的系统服务。 不同的服务具有不同的默认启动策略：有些服务默认会自动启动，有些服务按需启动（手动），还有一些服务默认已禁用，只有在显式启用之后才能运行。 这些默认设置是为每个服务精心选择的，目的是为典型的客户找到性能、功能与安全性之间的平衡。

但是，某些企业客户可能偏向于对其 Windows 电脑和服务器采用注重于安全的配置，这种配置可将受攻击面减至绝对最小。因此，这些客户可能想要完全禁用其特定环境中不需要的所有服务。 对于这些客户，Microsoft® 将提供随附的指南，说明可以出于此目的安全禁用哪些服务。

本指南仅适用于带有桌面体验的 Windows Server 2016（除非用作最终用户的桌面替代版本）。 从 Windows Server 2019 开始，默认会配置这些指导原则。 系统上每个服务的分类如下：

-   **应该禁用：** 注重于安全的企业很可能想要禁用此服务并放弃其功能（请参阅下面的附加详细信息）。
- **可以禁用：** 此服务提供的功能对某些（但不是所有）企业有用，如果企业不使用此服务且注重安全，可以安全禁用它。
- **不要禁用：** 禁用此服务会影响基本功能，或导致特定的角色或功能无法正常运行。 因此不应禁用它。
-  **（无指导）：** 尚未完全评估禁用这些服务的影响。 因此，不应更改这些服务的默认配置。


客户可以使用其组策略中的安全模板或使用 PowerShell 自动化将其 Windows 电脑和服务器配置为禁用所选的服务。 在某些情况下，指南中包含用于直接禁用服务功能的特定组策略设置，作为禁用服务本身的替代方法。

Microsoft 建议客户在带有桌面体验的 Windows Server 2016 中禁用以下服务及其相应的计划任务：

服务： 
1. Xbox Live 身份验证管理器
2. Xbox Live 游戏保存

计划的任务： 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

（还可以通过查看附加的 Microsoft Excel 电子表格，来访问有关本文中详述的所有服务的信息：[有关在带有桌面体验的 Windows Server 2016 中禁用系统服务的指南](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx)）

<br />

### <a name="disabling-services-not-installed-by-default"></a>禁用非默认安装的服务

Microsoft 建议不要应用策略来禁用非默认安装的服务。
-  如果安装了相应的功能，则通常需要这些服务。 安装服务或功能需要管理权限。 禁止安装功能，但不要禁止启动服务。
-  阻止 Microsoft Windows 服务不会阻止管理员（在某些情况下为非管理员）安装类似的第三方服务（可能是安全风险更高的服务）。
-  禁用非默认 Windows 服务（例如 W3SVC）的基线或基准会给某些审核员带来这样一种错觉：该技术（例如 IIS）在本质上就是不安全的，永远不可使用它。
-  如果永远不安装该功能（和服务），只会不必要地增大基线测试和验证的工作量。

<br />
对于本文档中列出的所有系统服务，下面两份表格提供了列的说明，以及有关在带有桌面体验的 Windows Server 2016 中启用和禁用系统服务的 Microsoft 建议： 

<br />

### <a name="explanation-of-columns"></a>列的说明

| | |
|---|---|
|**服务说明**|   服务的说明，摘自 sc.exe 说明。|
|**名称** |服务的键（内部）名称|
|**安装** |始终安装：服务位于 Server Core 和带有桌面体验的 Server 上  <br /> 仅用于桌面体验：服务位于带有桌面体验的 Windows Server 2016 上，但不位于 Server Core 上 |
|**StartType**  |Windows Server 2016 上的服务启动类型|
|**建议** |有关妥善管理的典型企业部署中的、其服务器不是用作最终用户桌面替代版本的 Windows Server 2016 上禁用此服务的 Microsoft 建议。|
|**备注** |附加说明|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Microsoft 建议说明

| | |
|---|---|
|**不要禁用** |不应禁用此服务|
|**可以禁用**| 如果未使用此服务支持的功能，则可以禁用此服务。|
|**已禁用**|  此服务默认已禁用；无需使用策略强制禁用|
|**应该禁用** |在妥善管理的企业系统中永远不应该启用此服务。|

<br />

下表提供了有关在带有桌面体验的 Windows Server 2016 中禁用系统服务的 Microsoft 指导：

<br />

##  <a name="activex-installer-axinstsv"></a>ActiveX 安装程序 (AxInstSV)

| | |
|---|---|
|   **服务说明** |   针对从 Internet 安装的 ActiveX 控件提供用户帐户控制验证，并启用基于组策略设置的 ActiveX 控件安装管理。 此服务按需启动，如果已禁用，则 ActiveX 控件安装行为遵循默认的浏览器设置。    |
|   **服务名称**    |   AxInstSV    |
|   **安装**    |   仅用于桌面体验    |
|   **StartType**   |   Manual  |
|   **建议**  |   可以禁用   |
|   **备注**    |   如果不需要相关的功能，则可以禁用 |


<br />

## <a name="alljoyn-router-service"></a>AllJoyn 路由器服务   

| | |
|---|---|
|   **服务说明** |   路由本地 AllJoyn 客户端的 AllJoyn 消息。 如果此服务已停止，则没有自身捆绑路由器的 AllJoyn 客户端将无法运行。 |
|   **服务名称**    |   AJRouter    |
|   **安装**    |   仅用于桌面体验    |
|   **StartType**   |   Manual  |
|   **建议**  | 无指导       |
|   **备注**    |       |
| | |

<br />

## <a name="app-readiness"></a>应用就绪状态

| | |
|---|---|
**服务说明** |   使应用在用户首次登录此电脑以及添加新应用时可供使用。
**服务名称**    |   AppReadiness
**安装**    |   仅用于桌面体验
**StartType**   |   Manual
**建议**  |   不要禁用
**备注**    |   
| | |

<br />

##  <a name="application-identity"></a>应用程序标识

| | |       
|---|---|   
**服务说明** |   确定并验证应用程序的标识。 禁用此服务会阻止实施 AppLocker。
**服务名称**    |   AppIDSvc
**安装**    |   始终安装
**StartType**   |   Manual
**建议**  |无指导    
**备注**    |   
|||     

<br />

##  <a name="application-information"></a>应用程序信息 

| | |       
|---|---|   
|   **服务说明** |   方便使用附加的管理特权运行交互式应用程序。  如果此服务已停止，则用户无法使用执行所需用户任务而要提供的附加管理特权启动应用程序。
|   **服务名称**    |   Appinfo
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   支持 UAC 相同桌面提升
|||     

<br />

##  <a name="application-layer-gateway-service"></a>应用程序层网关服务       

| | |           
|---|---|           
|   **服务说明** |   提供用于 Internet 连接共享的第三方协议插件的支持
|   **服务名称**    |   ALG
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |无指导    
|   **备注**    |   
|||     

<br />

##  <a name="application-management"></a>应用程序管理      

| | |           
|---|---|       
|   **服务说明** |   处理通过组策略部署的软件的安装、删除和枚举请求。 如果禁用该服务，则用户无法安装、删除或枚举通过组策略部署的软件。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   AppMgmt
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="appx-deployment-service-appxsvc"></a>AppX 部署服务 (AppXSVC)       

| | |           
|---|---|
|   **服务说明** |   提供用于部署 Store 应用程序的基础结构支持。 此服务按需启动，如果已禁用，则 Store 应用程序将不会部署到系统，并且可能无法正常运行。
|   **服务名称**    |   AppXSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="auto-time-zone-updater"></a>自动时区更新程序           

| | |           
|---|---|           
|   **服务说明** |   自动设置系统时区。
|   **服务名称**    |   tzautoupdate
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   
|||         

<br />          

## <a name="background-intelligent-transfer-service"></a>后台智能传送服务          

| | |           
|---|---|   
|   **服务说明** |   使用空闲网络带宽在后台传输文件。 如果该服务已禁用，则依赖于 BITS 的任何应用程序（例如 Windows 更新或 MSN Explorer）将无法自动下载程序和其他信息。
|   **服务名称**    |   BITS
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          


## <a name="background-tasks-infrastructure-service"></a>后台任务基础结构服务      

| | |           
|---|---|   
|   **服务说明** |   控制哪些后台任务可在系统上运行的 Windows 基础结构服务。
|   **服务名称**    |   BrokerInfrastructure
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="base-filtering-engine"></a>基本筛选引擎            

| | |           
|---|---|       
|   **服务说明** |   基本筛选引擎 (BFE) 服务管理防火墙和 Internet 协议安全性 (IPsec) 策略，以及实现用户模式筛选。 停止或禁用 BFE 服务会明显降低系统的安全性。 此外，还会导致 IPsec 管理和防火墙应用程序中出现不可预测的行为。
|   **服务名称**    |   BFE
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="bluetooth-support-service"></a>蓝牙支持服务            

| | |           
|---|---|   
|   **服务说明** |   蓝牙服务支持远程蓝牙设备的发现和关联。  停止或禁用此服务可能会导致已安装的蓝牙设备无法正常运行，并阻止发现或关联新设备。
|   **服务名称**    |   bthserv
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   如果不使用，则可以禁用。 另一种禁用机制： https://technet.microsoft.com/library/dd252791.aspx
|||         

<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           

| | |           
|---|---|   
|   **服务说明** |   此用户服务用于互联设备平台方案
|   **服务名称**    |   CDPUserSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  |   可以禁用
|   **备注**    |   用户服务模板
|||         

<br />          


##  <a name="certificate-propagation"></a>证书传播     

| | |           
|---|---|
|   **服务说明** |   将智能卡中的用户证书和根证书复制到当前用户的证书存储，检测智能卡何时已插入到智能卡读卡器，并根据需要安装智能卡即插即用微型驱动程序。
|   **服务名称**    |   CertPropSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="client-license-service-clipsvc"></a>客户端许可证服务 (ClipSVC)        

| | |           
|---|---|   
|   **服务说明** |   提供 Microsoft Store 的基础结构支持。 此服务按需启动，如果已禁用，则通过 Microsoft Store 购买的应用程序将行为异常。
|   **服务名称**    |   ClipSVC
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="cng-key-isolation"></a>CNG 密钥隔离

| | |           
|---|---|   
|   **服务说明** |   CNG 密钥隔离服务托管在 LSA 进程中。 该服务根据通用准则的要求，针对私钥和关联的加密操作提供密钥进程隔离。 该服务在符合通用准则要求的安全进程中存储并使用长期生存的密钥。
|   **服务名称**    |   KeyIso
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="com-event-system"></a>COM+ 事件系统       

| | |           
|---|---|       
|   **服务说明** |   支持系统事件通知服务 (SENS)，该服务可自动将事件分发到组件对象模型 (COM) 订阅组件。 如果该服务已停止，则 SENS 将会关闭，无法提供登录和注销通知。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   EventSystem
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="com-system-application"></a>COM+ 系统应用程序     

| | |           
|---|---|       
|   **服务说明** |   管理基于组件对象模型 (COM)+ 的组件的配置和跟踪。 如果该服务已停止，大多数基于 COM+ 的组件将无法正常运行。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   COMSysApp
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="computer-browser"></a>计算机浏览器        

| | |           
|---|---|       
|   **服务说明** |   维护更新的网络中计算机列表，并将此列表提供给指定为浏览器的计算机。 如果此服务已停止，则不会更新或维护此列表。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   浏览器
|   **安装**    |   始终安装
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   
|||         

<br />          

## <a name="connected-devices-platform-service"></a>互联设备平台服务       

| | |           
|---|---|       
|   **服务说明** |   此服务用于互联设备和 Universal Glass 方案
|   **服务名称**    |   CDPSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="connected-user-experiences-and-telemetry"></a>已连接的用户体验和遥测     

| | |           
|---|---|       
|   **服务说明** |   已连接的用户体验和遥测服务启用支持应用程序内部体验和已连接用户体验的功能。 此外，如果在“反馈和诊断”中启用了诊断和使用情况隐私选项设置，则此服务会管理诊断和使用情况信息的事件驱动式收集与传输（用于改进 Windows 平台的体验和质量）。
|   **服务名称**    |   DiagTrack
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="contact-data"></a>联系人数据        

| | |           
|---|---|       
|   **服务说明** |   为联系人数据编制索引以加速联系人搜索。 如果停止或禁用此服务，搜索结果中可能会缺少联系人。
|   **服务名称**    |   PimIndexMaintenanceSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   用户服务模板
|||         

<br />          

## <a name="coremessaging"></a>CoreMessaging            

| | |           
|---|---|           
|   **服务说明** |   管理系统组件之间的通信。
|   **服务名称**    |   CoreMessagingRegistrar
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="credential-manager"></a>凭据管理器           

| | |           
|---|---|       
|   **服务说明** |   提供用户、应用程序和安全服务包凭据的安全存储和检索。
|   **服务名称**    |   VaultSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="cryptographic-services"></a>加密服务           

| | |           
|---|---|       
|   **服务说明** |   提供三个管理服务：目录数据库服务：确认 Windows 文件的签名，并允许安装新的程序；受保护的根服务：在此计算机中添加和删除受信任的根证书颁发机构证书；自动根证书更新服务：从 Windows 更新检索根证书，并实现 SSL 等方案。 如果此服务已停止，则这些管理服务将无法正常运行。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   CryptSvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="data-sharing-service"></a>数据共享服务         

| | |           
|---|---|       
|   **服务说明** |   在应用程序之间提供数据中转。
|   **服务名称**    |   DsSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          

| | |           
|---|---|       
|   **服务说明** |   DCP（数据收集和发布）服务支持第一方应用向云上传数据。
|   **服务名称**    |   DcpSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="dcom-server-process-launcher"></a>DCOM 服务器进程启动器         

| | |           
|---|---|       
|   **服务说明** |   DCOMLAUNCH 服务启动 COM 和 DCOM 服务器以响应对象激活请求。 如果此服务已停止或禁用，则使用 COM 或 DCOM 的程序将无法正常运行。 强烈建议使 DCOMLAUNCH 服务保持运行。
|   **服务名称**    |   DcomLaunch
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |无指导    
|   **备注**    |   
|||         

<br />

##  <a name="device-association-service"></a>设备关联服务      

| | |           
|---|---|       
|   **服务说明** |   在系统与有线或无线设备之间启用配对。
|   **服务名称**    |   DeviceAssociationService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />

##  <a name="device-install-service"></a>设备安装服务

| | |
|---|---|
|   **服务说明** |   使计算机能够识别和适应硬件更改，而且只需用户提供少量的输入或者根本不需要输入。 停止或禁用此服务会导致系统不稳定。
|   **服务名称**    |   DeviceInstall
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导
|   **备注**    |
|||

<br />          

##  <a name="device-management-enrollment-service"></a>设备管理注册服务        

| | |           
|---|---|       
|   **服务说明** |   执行设备管理的设备注册活动
|   **服务名称**    |   DmEnrollmentSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="device-setup-manager"></a>设备安装管理器         

| | |           
|---|---|       
|   **服务说明** |   启用设备相关软件的检测、下载和安装。 如果此服务已禁用，则可能会使用过期的软件配置设备，因此设备可能无法正常工作。
|   **服务名称**    |   DsmSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="devquery-background-discovery-broker"></a>DevQuery 后台发现代理         

| | |           
|---|---|           
|   **服务说明** |   使应用能够使用后台任务发现设备
|   **服务名称**    |   DevQueryBroker
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="dhcp-client"></a>DHCP 客户端          

| | |           
|---|---|       
|   **服务说明** |   注册和更新此计算机的 IP 地址与 DNS 记录。 如果此服务已停止，则此计算机不会接收动态 IP 地址和 DNS 更新。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   Dhcp
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="diagnostic-policy-service"></a>诊断策略服务            

| | |           
|---|---|       
|   **服务说明** |   诊断策略服务启用 Windows 组件的问题检测、故障排除和解决。  如果此服务已停止，则诊断将不再正常运行。
|   **服务名称**    |   DPS
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="diagnostic-service-host"></a>诊断服务主机     

| | |           
|---|---|       
|   **服务说明** |   诊断策略服务使用诊断服务主机来托管需要在本地服务上下文中运行的诊断。  如果此服务已停止，则依赖于它的任何诊断将不再正常运行。
|   **服务名称**    |   WdiServiceHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="diagnostic-system-host"></a>诊断系统主机           

| | |           
|---|---|       
|   **服务说明** |   诊断策略服务使用诊断系统主机来托管需要在本地系统上下文中运行的诊断。  如果此服务已停止，则依赖于它的任何诊断将不再正常运行。
|   **服务名称**    |   WdiSystemHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="distributed-link-tracking-client"></a>分布式链接跟踪客户端            

| | |           
|---|---|   
|   **服务说明** |   维护一台计算机中或者网络上不同计算机中 NTFS 文件之间的链接。
|   **服务名称**    |   TrkWks
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="distributed-transaction-coordinator"></a>分布式事务处理协调器     

| | |           
|---|---|   
|   **服务说明** |   协调跨多个资源管理器（例如数据库、消息队列和文件系统）的事务。 如果此服务已停止，这些事务将会失败。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   MSDTC
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        

| | |           
|---|---|       
|   **服务说明** |   WAP 推送消息路由服务
|   **服务名称**    |   dmwappushservice
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   Intune、MDM 和类似管理技术以及统一写入筛选器的客户端设备上所需的服务。 Windows Server 不需要此服务。
|||         

<br />      

##  <a name="dns-client"></a>DNS 客户端的运行情况      

| | |           
|---|---|       
|   **服务说明** |   DNS 客户端服务(dnscache)缓存域名系统(DNS)名称并注册此计算机的计算机全名。 如果该服务已停止，将继续解析 DNS 名称。 但是，将不会缓存 DNS 名称队列的结果，且不注册计算机名称。 如果该服务已禁用，将无法启动显式依赖它的服务。
|   **服务名称**    |   Dnscache
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="downloaded-maps-manager"></a>已下载的地图管理器     

| | |           
|---|---|   
|   **服务说明** |   可让应用程序访问已下载的地图的 Windows 服务。 应用程序在访问下载的地图时会按需启动此服务。 禁用此服务可防止应用访问地图。
|   **服务名称**    |   MapsBroker
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  |   可以禁用
|   **备注**    |   禁用该服务会中断依赖于它的应用；如果应用不依赖于它，则可将其禁用
|||         

<br />          

## <a name="embedded-mode"></a>嵌入模式            

| | |           
|---|---|       
|   **服务说明** |   嵌入模式服务实现后台应用程序相关的方案。  禁用此服务会阻止激活后台应用程序。
|   **服务名称**    |   embeddedmode
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="encrypting-file-system-efs"></a>加密文件系统 (EFS)

| | |                   
|---|---|           
|   **服务说明** | 提供用于在 NTFS 文件系统卷上存储加密文件的核心文件加密技术。 如果此服务已停止或禁用，应用程序将无法访问加密的文件。            
|   **服务名称**  |  EFS            
|   **安装**  |  始终安装           
|   **StartType**   |  Manual           
|   **建议**  | 无指导           
|   **备注**   |
|||                 

<br />  

## <a name="enterprise-app-management-service"></a>企业应用管理服务            

| | |           
|---|---|       
|   **服务说明** |   启用企业应用程序管理。
|   **服务名称**    |   EntAppSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="extensible-authentication-protocol"></a>可扩展身份验证协议           

| | |           
|---|---|   
|   **服务说明** |   可扩展身份验证协议 (EAP) 服务在 802.1x 有线和无线、VPN 和网络访问保护 (NAP) 等方案中提供网络身份验证。  EAP 还提供应用程序编程接口 (API)，在身份验证过程中，网络访问客户端（包括无线和 VPN 客户端）将使用这些 API。  如果禁用此服务，则会阻止此计算机访问需要 EAP 身份验证的网络。
|   **服务名称**    |   EapHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="function-discovery-provider-host"></a>功能发现提供程序主机服务         

| | |           
|---|---|       
|   **服务说明** |   FDPHOST 服务托管功能发现 (FD) 网络发现提供程序。 这些 FD 提供程序为简单服务发现协议 (SSDP) 和 Web 服务 - 发现 (WS-D) 协议提供网络发现服务。 停止或禁用 FDPHOST 服务会在使用 FD 时禁用这些协议的网络发现。 如果此服务不可用，使用 FD 并依赖于这些发现协议的网络服务将无法查找网络设备或资源。
|   **服务名称**    |   fdPHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="function-discovery-resource-publication"></a>功能发现资源发布服务      

| | |           
|---|---|       
|   **服务说明** |   发布此计算机以及附加到此计算机的资源，以便可以通过网络发现它们。  如果此服务已停止，则不再会发布网络资源，网络中的其他计算机不会发现它们。
|   **服务名称**    |   FDResPub
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="geolocation-service"></a>地理定位服务          

| | |           
|---|---|       
|   **服务说明** |   此服务监视系统的当前位置并管理地理围栏（具有关联事件的地理位置）。  如果关闭此服务，应用程序将无法使用或接收有关地理位置或地理围栏的通知。
|   **服务名称**    |   lfsvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   禁用该服务会中断依赖于它的应用；如果应用不依赖于它，则可将其禁用
|||         

<br />          

##  <a name="group-policy-client"></a>组策略客户端     

| | |           
|---|---|       
|   **服务说明** |   该服务负责应用管理员通过组策略组件为计算机和用户配置的设置。 如果该服务已禁用，则不会应用这些设置，并且无法通过组策略管理应用程序和组件。 如果该服务已禁用，依赖于组策略组件的组件或应用程序可能无法正常运行。
|   **服务名称**    |   gpsvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          


## <a name="human-interface-device-service"></a>人机接口设备服务           

| | |           
|---|---|       
|   **服务说明** |   激活并保持使用键盘、遥控器和其他多媒体设备上的热按钮。 建议使此服务保持运行。
|   **服务名称**    |   hidserv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="hv-host-service"></a>HV 主机服务     

| | |           
|---|---|   
|   **服务说明** |   提供 Hyper-V 虚拟机监控程序的接口，以便为主机操作系统提供每个分区的性能计数器。
|   **服务名称**    |   HvHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   来宾 VM 的性能增强器。 目前只在显式填充的 VM 上使用，但会在应用程序防护中使用
|||         

<br />          

## <a name="hyper-v-data-exchange-service"></a>Hyper-V 数据交换服务        

| | |           
|---|---|   
|   **服务说明** |   提供了一种在虚拟机和运行在物理计算机上的操作系统之间交换数据的机制。
|   **服务名称**    |   vmickvpexchange
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   请参阅 HvHost
|||         

<br />      

## <a name="hyper-v-guest-service-interface"></a>Hyper-V 来宾服务接口          

| | |           
|---|---|   
|   **服务说明** |   为 Hyper-V 主机提供接口，以便与虚拟机内运行的特定服务进行交互。
|   **服务名称**    |   vmicguestinterface
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   请参阅 HvHost
|||         

<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Hyper-V 来宾关闭服务           

| | |           
|---|---|       
|   **服务说明** |   提供一种机制用于从物理计算机上的管理界面关闭此虚拟机的操作系统。
|   **服务名称**    |   vmicshutdown
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   请参阅 HvHost
|||         

<br />

## <a name="hyper-v-heartbeat-service"></a>Hyper-V 检测信号服务
| | |
|---|---|
|   **服务说明** |   通过定期报告检测信号来监视此虚拟机的状态。 此服务可帮助识别正在运行的但已停止响应的虚拟机。
|   **服务名称**    |   vmicheartbeat
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   请参阅 HvHost
|||

<br />          

## <a name="hyper-v-powershell-direct-service"></a>Hyper-V PowerShell Direct 服务            

| | |           
|---|---|       
|   **服务说明** |   提供在没有虚拟网络的情况下，通过 VM 会话使用 PowerShell 管理虚拟机的机制。
|   **服务名称**    |   vmicvmsession
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   请参阅 HvHost
|||         

<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Hyper-V 远程桌面虚拟化服务            

| | |           
|---|---|       
|   **服务说明** |   提供一个平台用于在虚拟机与运行于物理计算机上的操作系统之间通信。
|   **服务名称**    |   vmicrdv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   请参阅 HvHost
|||         

<br />          

## <a name="hyper-v-time-synchronization-service"></a>Hyper-V 时间同步服务         

| | |           
|---|---|       
|   **服务说明** |   将此虚拟机的系统时间与物理计算机的系统时间同步。
|   **服务名称**    |   vmictimesync
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   请参阅 HvHost
|||         

<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Hyper-V 卷影复制请求程序         

| | |           
|---|---|           
|   **服务说明** |   协调需要使用卷影复制服务通过物理计算机上的操作系统备份此虚拟机上的应用程序和数据的通信。
|   **服务名称**    |   vmicvss
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   请参阅 HvHost
|||         

<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>IKE 和 AuthIP IPsec 密钥模块          

| | |           
|---|---|       
|   **服务说明** |   IKEEXT 服务托管 Internet 密钥交换 (IKE) 和身份验证 Internet 协议 (AuthIP) 密钥模块。 这些密钥模块用于 Internet 协议安全性 (IPsec) 中的身份验证和密钥交换。 停止或禁用 IKEEXT 服务会禁用对等计算机上的 IKE 和 AuthIP 密钥交换。 IPsec 通常配置为使用 IKE 或 AuthIP；因此，停止或禁用 IKEEXT 服务可能会导致 IPsec 失败，并损害系统的安全性。 强烈建议使 IKEEXT 服务保持运行。
|   **服务名称**    |   IKEEXT
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |    
|||         

<br />          

## <a name="interactive-services-detection"></a>交互服务检测           

| | |           
|---|---|   
|   **服务说明** |   为交互式服务启用用户输入通知，以便在显示交互式服务创建的对话框时可以访问这些对话框。 如果此服务已停止，新交互式服务对话框通知将不再运行，并且可能无法访问交互式服务对话框。 如果此服务已禁用，新交互式服务对话框的通知和访问将不再运行。
|   **服务名称**    |   UI0Detect
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />  

## <a name="internet-connection-sharing-ics"></a>Internet 连接共享(ICS)            

| | |           
|---|---|           
|   **服务说明** |   为家庭或小型办公网络提供网络地址转换、寻址、名称解析和/或入侵防护服务。
|   **服务名称**    |   SharedAccess
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   客户端需要将 ICS 用作 WiFi 热点；ICS 还会在 Miracast 投影的两端使用。 可能无法使用 GPO 设置“禁止在 DNS 域网络上使用 Internet 连接共享”来阻止 ICS
|||         

<br />          

## <a name="ip-helper"></a>IP 帮助程序            

| | |           
|---|---|       
|   **服务说明** |   使用 IPv6 转换技术（6to4、ISATAP、端口代理和 Teredo）和 IP-HTTPS 提供隧道连接。 如果此服务已停止，计算机将不会获得这些技术提供的增强型连接优势。
|   **服务名称**    |   iphlpsvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          


##  <a name="ipsec-policy-agent"></a>IPsec 策略代理      

| | |           
|---|---|       
|   **服务说明** |   Internet 协议安全性 (IPSec) 支持网络级对等身份验证、数据源身份验证、数据完整性、数据保密性（加密）和重播保护。  此服务实施通过“IP 安全策略”管理单元或命令行工具“netsh ipsec”创建的 IPsec 策略。  如果停止此服务，当策略要求连接使用 IPsec 时，你可能会遇到网络连接问题。  此外，停止此服务时，Windows 防火墙远程管理不可用。
|   **服务名称**    |   PolicyAgent
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />

##  <a name="kdc-proxy-server-service-kps"></a>KDC 代理服务器服务 (KPS)      

| | |           
|---|---|       
|   **服务说明** |   KDC 代理服务器服务在边缘服务器上运行，用于将 Kerberos 协议消息中转到企业网络上的域控制器。
|   **服务名称**    |   KPSSVC
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导    
|   **备注**    |   
|||         

<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>适用于分布式事务处理协调器的 KtmRm            

| | |           
|---|---|       
|   **服务说明** |   协调分布式事务处理协调器 (MSDTC) 与内核事务管理器 (KTM) 之间的事务。 如果不需要此服务，建议将其保留停止状态。 如果需要，MSDTC 和 KTM 会自动启动此服务。 如果此服务已禁用，与内核资源管理器交互的任何 MSDTC 事务都将失败，而显式依赖于此服务的任何服务将无法启动。
|   **服务名称**    |   KtmRm
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />

##  <a name="link-layer-topology-discovery-mapper"></a>链路层拓扑发现映射器        

| | |       
|---|---|       
|   **服务说明** |   创建网络映射，该映射由电脑和设备拓扑（连接）信息，以及描述每台电脑和设备的元数据组成。  如果此服务已禁用，网络映射将无法正常运行。
|   **服务名称**    |   lltdsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   如果不依赖于网络映射，则可以禁用
|||         

<br />

## <a name="local-session-manager"></a>本地会话管理器                    

| | |                   
|---|---|   
|   **服务说明** |   管理本地用户会话的核心 Windows 服务。 停止或禁用此服务会导致系统不稳定。    
|   **服务名称**    |   LSM |
|   **安装**    |   始终安装    |
|   **StartType**   |   自动   |
|   **建议**  | 无指导   
|   **备注**    |   
|||                 

<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Microsoft (R) 诊断中心标准收集器         

| | |           
|---|---|           
|   **服务说明** |   管理本地用户会话的核心 Windows 服务。 停止或禁用此服务会导致系统不稳定。
|   **服务名称**    |   LSM
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />

## <a name="microsoft-account-sign-in-assistant"></a>Microsoft 帐户登录助手
| | |
|---|---|
|   **服务说明** |   使用户能够通过 Microsoft 帐户标识服务登录。 如果此服务已停止，则用户无法使用其 Microsoft 帐户登录到计算机。
|   **服务名称**    |   wlidsvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   Microsoft 帐户在 Windows Server 上为 N/A
|||

<br />          

##  <a name="microsoft-app-v-client"></a>Microsoft App-V 客户端      

| | |           
|---|---|       
|   **服务说明** |   管理 App-V 用户和虚拟应用程序
|   **服务名称**    |   AppVClient
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   
|||         

<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Microsoft iSCSI 发起程序服务            

| | |           
|---|---|       
|   **服务说明** |   管理从此计算机与远程 iSCSI 目标设备建立的 Internet SCSI (iSCSI) 会话。 如果此服务已停止，则此计算机无法登录或访问 iSCSI 目标。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   MSiSCSI
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   我们的诊断数据表明，此服务会在客户端和服务器上使用。 禁用此服务并无好处。
|||         

<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           

| | |           
|---|---|   
|   **服务说明** |   为用于对用户关联的标识提供者进行身份验证的加密密钥提供进程隔离。 如果此服务已禁用，则无法使用和管理这些密钥，包括无法登录计算机以及单一登录到应用和网站。 此服务会自动启动和停止。 建议不要重新配置此服务。
|   **服务名称**    |   NgcSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   PIN/Hello 登录需要此服务，在 Windows Server 上不受支持
|||         

<br />          

## <a name="microsoft-passport-container"></a>Microsoft Passport 容器         

| | |           
|---|---|       
|   **服务说明** |   管理用于在标识提供者中对用户进行身份验证的本地用户标识密钥以及 TPM 虚拟智能卡。 如果此服务已禁用，将无法访问本地用户标识密钥和 TPM 虚拟智能卡。 建议不要重新配置此服务。
|   **服务名称**    |   NgcCtnrSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Microsoft 软件卷影复制提供程序          

| | |           
|---|---|       
|   **服务说明** |   管理卷影复制服务创建的基于软件的卷影副本。 如果此服务已停止，则无法管理基于软件的卷影副本。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   swprv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="microsoft-storage-spaces-smp"></a>Microsoft 存储空间 SMP         

| | |           
|---|---|       
|   **服务说明** |   Microsoft 存储空间管理提供程序的宿主服务。 如果此服务已停止或禁用，则无法管理存储空间。
|   **服务名称**    |   smphost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   如果没有此服务，存储管理 API 将会失败。 示例："Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage"。
|||         

<br />          

## <a name="nettcp-port-sharing-service"></a>Net.Tcp 端口共享服务         

| | |           
|---|---|       
|   **服务说明** |   提供通过 net.tcp 协议共享 TCP 端口的功能。
|   **服务名称**    |   NetTcpPortSharing
|   **安装**    |   始终安装
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   
|||         

<br />          

## <a name="netlogon"></a>Netlogon         

| | |           
|---|---|           
|   **服务说明** |   在此计算机与用来对用户和服务进行身份验证的域控制器之间维持一个安全通道。 如果此服务已停止，则该计算机将无法对用户和服务进行身份验证，并且该域控制器无法注册 DNS 记录。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   Netlogon
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="network-connection-broker"></a>网络连接代理            

| | |           
|---|---|       
|   **服务说明** |   可让 Microsoft Store 应用从 Internet 接收通知的代理连接。
|   **服务名称**    |   NcbService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

##  <a name="network-connections"></a>网络连接         

| | |           
|---|---|   
|   **服务说明** |   管理“网络和拨号连接”文件夹中的对象，在其中可以查看局域网和远程连接。
|   **服务名称**    |   Netman
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="network-connectivity-assistant"></a>网络连接助手      

| | |           
|---|---|       
|   **服务说明** |   提供 UI 组件的 DirectAccess 状态通知
|   **服务名称**    |   NcaSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />  

##  <a name="network-list-service"></a>网络列表服务        

| | |           
|---|---|   
|   **服务说明** |   标识计算机连接到的网络，收集和存储这些网络的属性，并在这些属性发生更改时通知应用程序。
|   **服务名称**    |   netprofm
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="network-location-awareness"></a>网络位置感知           

| | |           
|---|---|       
|   **服务说明** |   收集和存储网络的配置信息，并在这些信息发生更改时通知程序。 如果此服务已停止，则可能不会提供配置信息。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   NlaSvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="network-setup-service"></a>网络安装服务       

| | |           
|---|---|       
|   **服务说明** |   网络安装服务管理网络驱动程序的安装，并允许配置低级别的网络设置。  如果此服务已停止，正在进行的任何驱动程序安装可能会被取消。
|   **服务名称**    |   NetSetupSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="network-store-interface-service"></a>网络存储接口服务      

| | |           
|---|---|   
|   **服务说明** |   此服务向用户模式客户端传送网络通知（例如接口添加/删除等）。 停止此服务会导致网络连接断开。 如果此服务已禁用，显式依赖此服务的任何其他服务将无法启动。
|   **服务名称**    |   nsi
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="offline-files"></a>脱机文件            

| | |           
|---|---|       
|   **服务说明** |   脱机文件服务对脱机文件缓存执行维护活动，响应用户登录和注销事件，实现公共 API 的内部功能，并将所需的事件分发到想要了解脱机文件活动和缓存状态更改的用户。
|   **服务名称**    |   CscService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   
|||         

<br />          

## <a name="optimize-drives"></a>优化驱动器          

| | |           
|---|---|   
|   **服务说明** |   通过优化存储驱动器上的文件，帮助计算机更有效地运行。
|   **服务名称**    |   defragsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />

## <a name="performance-counter-dll-host"></a>性能计数器 DLL 宿主         

| | |           
|---|---|       
|   **服务说明** |   使远程用户和 64 位进程能够查询 32 位 DLL 提供的性能计数器。 如果此服务已停止，则只有本地用户和 32 位进程可以查询 32 位 DLL 提供的性能计数器。
|   **服务名称**    |   PerfHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导    
|   **备注**    |   
|||         

<br />          

## <a name="performance-logs--alerts"></a>性能日志和警报            

| | |           
|---|---|   
|   **服务说明** |   性能日志和警报根据预配置的计划参数从本地或远程计算机收集性能数据，然后将数据写入日志或触发警报。 如果此服务已停止，则不会收集性能信息。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   pla
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="phone-service"></a>电话服务       

| | |           
|---|---|   
|   **服务说明** |   管理设备上的电话状态
|   **服务名称**    |   PhoneSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   由新式 VoIP 应用使用
|||         

<br />          

##      <a name="plug-and-play"></a>即插即用       

| | |           
|---|---|   
|   **服务说明** |   使计算机能够识别和适应硬件更改，而且只需用户提供少量的输入或者根本不需要输入。 停止或禁用此服务会导致系统不稳定。
|   **服务名称**    |   PlugPlay
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="portable-device-enumerator-service"></a>便携设备枚举器服务           

| | |           
|---|---|       
|   **服务说明** |   对可移动大容量存储设备实施组策略。 使 Windows Media Player 和映像导入向导等应用程序能够使用可移动大容量存储设备传输与同步内容。
|   **服务名称**    |   WPDBusEnum
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="power"></a>电源            

| | |           
|---|---|       
|   **服务说明** |   管理电源策略和电源策略通知传送。
|   **服务名称**    |   电源
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="print-spooler"></a>打印后台处理程序            

| | |           
|---|---|   
|   **服务说明** |   此服务在后台处理打印作业，并处理与打印机之间的交互。  如果关闭此服务，将无法打印或查看打印机。
|   **服务名称**    |   后台处理程序
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   如果不是打印服务器或 DC，则可以禁用
|   **备注**    |   在域控制器上，安装 DC 角色会将一个线程添加到负责执行打印修剪（从 Active Directory 中删除过时的打印队列对象）的后台处理程序服务。  如果后台处理程序服务未在每个站点中的至少一个 DC 上运行，则 AD 无法删除不再存在的旧队列。 https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         

<br />          

##  <a name="printer-extensions-and-notifications"></a>打印机扩展和通知        

| | |           
|---|---|       
|   **服务说明** |   此服务打开自定义打印机对话框，并处理来自远程打印服务器或打印机的通知。 如果关闭此服务，将无法查看打印机扩展或通知。
|   **服务名称**    |   PrintNotify
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   如果不是打印服务器，则可以禁用
|   **备注**    |   
|||         

<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>问题报告和解决方法控件面板支持     

| | |           
|---|---|   
|   **服务说明** |   此服务为“问题报告和解决方法”控制面板提供查看、发送和删除系统级问题报告的支持。
|   **服务名称**    |   wercplsupport
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="program-compatibility-assistant-service"></a>程序兼容性助手服务     

| | |           
|---|---|       
|   **服务说明** |   此服务提供程序兼容性助手 (PCA) 的支持。  PCA 监视用户安装和运行的程序，并检测已知的兼容性问题。 如果此服务已停止，PCA 将无法正常运行。
|   **服务名称**    |   PcaSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

##  <a name="quality-windows-audio-video-experience"></a>高质量 Windows 音频视频体验      

| | |           
|---|---|   
|   **服务说明** |   高质量 Windows 音频视频体验 (qWave) 是适用于 IP 家庭网络中音频视频 (AV) 流应用程序的网络平台。 qWave 确保 AV 应用程序的网络服务质量 (QoS)，增强了 AV 流的性能和可靠性。 它提供许可控制、运行时监视和实施、应用程序反馈和流量优先级的机制。
|   **服务名称**    |   QWAVE
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   客户端 QoS 服务
|||         

<br />          

##      <a name="radio-management-service"></a>无线电管理服务        

| | |           
|---|---|   
|   **服务说明** |   无线电管理和飞行模式服务
|   **服务名称**    |   RmSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="remote-access-auto-connection-manager"></a>远程访问自动连接管理器            

| | |           
|---|---|   
|   **服务说明** |   每当程序引用远程 DNS 或者 NetBIOS 名称或地址时，都会与远程网络建立连接。
|   **服务名称**    |   RasAuto
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="remote-access-connection-manager"></a>远程访问连接管理器         

| | |           
|---|---|   
|   **服务说明** |   管理从此计算机与 Internet 或其他远程网络建立的拨号和虚拟专用网 (VPN) 连接。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   RasMan
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="remote-desktop-configuration"></a>远程桌面配置         

| | |           
|---|---|   
|   **服务说明** |   远程桌面配置服务 (RDCS) 负责需要系统上下文的所有远程桌面服务和远程桌面相关配置和会话维护活动。 这包括每个会话的临时文件夹、远程桌面主题和 RD 证书。
|   **服务名称**    |   SessionEnv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   
|||         

<br />          

## <a name="remote-desktop-services"></a>远程桌面服务          

| | |           
|---|---|   
|   **服务说明** |   允许用户以交互方式连接到远程计算机。 远程桌面和远程桌面会话主机服务器依赖于此服务。  若要防止远程使用此计算机，请清除“系统属性”控制面板项的“远程”选项卡中的复选框。
|   **服务名称**    |   TermService
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   
|||         

<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>远程桌面服务用户模式端口重定向程序        

| | |           
|---|---|   
|   **服务说明** |   允许重定向 RDP 连接的打印机/驱动器/端口
|   **服务名称**    |   UmRdpService
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   支持连接的服务器端上的重定向。
|||         

<br />          

## <a name="remote-procedure-call-rpc"></a>远程过程调用 (RPC)          

| | |           
|---|---|   
|   **服务说明** |   RPCSS 服务是 COM 和 DCOM 服务器的服务控制管理器。 它会执行 COM 和 DCOM 服务器的对象激活请求、对象导出程序解决方案和分布式垃圾回收。 如果此服务已停止或禁用，则使用 COM 或 DCOM 的程序将无法正常运行。 强烈建议使 RPCSS 服务保持运行。
|   **服务名称**    |   RpcSs
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>远程过程调用 (RPC) 定位器             

| | |               
|---|---|   
|   **服务说明** |   在 Windows 2003 和早期版本的 Windows 中，远程过程调用 (RPC) 定位器服务管理 RPC 名称服务数据库。 在 Windows Vista 和更高版本的 Windows 中，此服务不提供任何功能，只用于确保应用程序兼容性。   |
|   **服务名称**    |   RpcLocator  |
|   **安装**    |   仅用于桌面体验    |
|   **StartType**   |   Manual  |
|   **建议**  | 无指导   |
|   **备注**    |       |
|||             

<br />              

## <a name="remote-registry"></a>远程注册表          

| | |           
|---|---|   
|   **服务说明** |   使远程用户能够修改此计算机上的注册表设置。 如果此服务已停止，则注册表只能由此计算机上的用户修改。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   RemoteRegistry
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **备注**    |   
|||         

<br />          

##  <a name="resultant-set-of-policy-provider"></a>策略提供程序的结果集            

| | |           
|---|---|       
|   **服务说明** |   提供一个网络服务用于处理在各种情况下针对目标用户或计算机模拟应用组策略设置的请求，以及计算策略设置的结果集。
|   **服务名称**    |   RSoPProv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |无指导    
|   **备注**    |   
|||         

<br />          

## <a name="routing-and-remote-access"></a>路由和远程访问            

| | |           
|---|---|   
|   **服务说明** |   为局域网和广域网环境中的企业提供路由服务。
|   **服务名称**    |   RemoteAccess
|   **安装**    |   始终安装
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   已禁用
|||         

<br />          

## <a name="rpc-endpoint-mapper"></a>RPC 终结点映射程序          

| | |           
|---|---|   
|   **服务说明** |   将 RPC 接口标识符解析为传输终结点。 如果此服务已停止或禁用，则使用远程过程调用 (RPC) 的程序将无法正常运行。
|   **服务名称**    |   RpcEptMapper
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="secondary-logon"></a>辅助登录     

| | |           
|---|---|       
|   **服务说明** |   允许使用备用凭据启动进程。 如果此服务已停止，则无法使用此类登录访问。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   seclogon
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>安全套接字隧道协议服务            

| | |               
|---|---|       
|   **服务说明** |   提供使用 VPN 连接到远程计算机的安全套接字隧道协议 (SSTP) 支持。 如果此服务已禁用，则用户无法使用 SSTP 访问远程服务器。    |
|   **服务名称**    |   SstpSvc |
|   **安装**    |   始终安装    |
|   **StartType**   |   Manual  |
|   **建议**  |   不要禁用  |
|   **备注**    |   禁用此服务会中断 RRAS   |
|||             

<br />              

## <a name="security-accounts-manager"></a>安全帐户管理器            

| | |           
|---|---|       
|   **服务说明** |   启动此服务会告知其他服务，安全帐户管理器 (SAM) 已准备好接受请求。  如果禁用此服务，当 SAM 准备就绪时，系统中的其他服务将收不到通知，从而可能导致这些服务无法正常启动。 不应禁用此服务。
|   **服务名称**    |   SamSs
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不要禁用
|   **备注**    |   
|||         

<br />          

## <a name="sensor-data-service"></a>传感器数据服务  

| | |           
|---|---|   
|   **服务说明** |   传送各种传感器的数据
|   **服务名称**    |   SensorDataService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />  

## <a name="sensor-monitoring-service"></a>传感器监视服务            

| | |           
|---|---|       
|   **服务说明** |   监视各种传感器，以公开数据以及适应系统和用户状态。  如果此服务已停止或禁用，将不会根据光照条件调整显示器亮度。 停止此服务也可能会影响其他系统功能和特性。
|   **服务名称**    |   SensrSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br /><br/>
## <a name="sensor-servicebr--br------br---strongservice-descriptionstrong----a-service-for-sensors-that-manages-different-sensors39-functionality-manages-simple-device-orientation-sdo-and-history-for-sensors-loads-the-sdo-sensor-that-reports-device-orientation-changes--if-this-service-is-stopped-or-disabled-the-sdo-sensor-will-not-be-loaded-and-so-auto-rotation-will-not-occur-history-collection-from-sensors-will-also-be-stopped"></a>传感器服务<br/>| | |<br/>|---|---|<br/>|   <strong>服务说明</strong> |   用于管理不同传感器功能的传感器服务。 管理传感器的简单设备方向 (SDO) 和历史记录。 加载报告设备方向更改的 SDO 传感器。  如果此服务已停止或禁用，则不会加载 SDO 传感器，因此自动旋转不会发生。 此外还会停止从传感器收集历史记录。
|   <strong>服务名称</strong>    |   SensorService |   <strong>安装</strong>    |   仅用于桌面体验 |   <strong>StartType</strong>   |   手动 |   <strong>建议</strong>  |   可以禁用 |   <strong>备注</strong>    |<br/>|||<br/>
<br />          

## <a name="server"></a>Server           

| | |           
|---|---|   
|   **服务说明** |   支持通过此计算机的网络进行文件、打印和命名管道共享。 如果此服务已停止，则这些功能将不可用。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   LanmanServer
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **备注**    |   远程管理、IPC$ 和 SMB 文件共享需要此服务
|||         

<br />          

## <a name="shell-hardware-detection"></a>Shell 硬件检测             

| | |           
|---|---|       
|   **服务说明** |   提供自动播放硬件事件的通知。
|   **服务名称**    |   ShellHWDetection
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="smart-card"></a>智能卡           

| | |           
|---|---|   
|   **服务说明** |   管理对此计算机读取的智能卡的访问。 如果此服务已停止，则此计算机无法读取智能卡。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   SCardSvr
|   **安装**    |   始终安装
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   
|||         

<br />          

## <a name="smart-card-device-enumeration-service"></a>智能卡设备枚举服务                    

| | |               
|---|---|       
|   **服务说明** |   为给定会话可访问的所有智能卡读卡器创建软件设备节点。 如果此服务已禁用，则 WinRT API 无法枚举智能卡读卡器。   |
|   **服务名称**    |   ScDeviceEnum    |
|   **安装**    |   始终安装    |
|   **StartType**   |   Manual  |
|   **建议**  |   可以禁用   |
|   **备注**    |   基本上仅供 WinRT 应用使用    |
|||             

<br />              

## <a name="smart-card-removal-policy"></a>智能卡移除策略        

| | |           
|---|---|       
|   **服务说明** |   允许将系统配置为在移除智能卡后锁定用户桌面。
|   **服务名称**    |   SCPolicySvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="snmp-trap"></a>SNMP 陷阱            

| | |           
|---|---|       
|   **服务说明** |   接收本地或远程简单网络管理协议 (SNMP) 代理生成的陷阱消息，并将消息转发到此计算机上运行的 SNMP 管理程序。 如果此服务已停止，则此计算机上的基于 SNMP 的程序将不会接收 SNMP 陷阱消息。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   SNMPTRAP
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="software-protection"></a>软件保护             

| | |           
|---|---|       
|   **服务说明** |   启用对 Windows 和 Windows 应用程序下载、安装和实施数字许可证。 如果该服务已禁用，则操作系统和许可的应用程序可在通知模式下运行。 强烈建议不要禁用软件保护服务。
|   **服务名称**    |   sppsvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="special-administration-console-helper"></a>特殊管理控制台帮助程序        

| | |           
|---|---|   
|   **服务说明** |   允许管理员使用紧急管理服务远程访问命令提示符。
|   **服务名称**    |   sacsvr
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="spot-verifier"></a>错误校验程序            

| | |           
|---|---|   
|   **服务说明** |   验证潜在的文件系统损坏。
|   **服务名称**    |   svsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="ssdp-discovery"></a>SSDP 发现           

| | |           
|---|---|   
|   **服务说明** |   发现使用 SSDP 发现协议的联网设备和服务，例如 UPnP 设备。 此外声明本地计算机上运行的 SSDP 设备和服务。 如果此服务已停止，则不会发现基于 SSDP 的设备。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   SSDPSRV
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="state-repository-service"></a>状态存储库服务         

| | |           
|---|---|   
|   **服务说明** |   提供应用程序模型所需的基础结构支持。
|   **服务名称**    |   StateRepository
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="still-image-acquisition-events"></a>静止图像采集事件

| | |           
|---|---|   
|   **服务说明** |   启动与静止图像采集事件关联的应用程序。
|   **服务名称**    |   WiaRpc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />  

## <a name="storage-service"></a>存储服务          

| | |           
|---|---|       
|   **服务说明** |   提供用于存储设置和外部存储扩展的启用服务
|   **服务名称**    |   StorSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="storage-tiers-management"></a>存储层管理        

| | |           
|---|---|   
|   **服务说明** |   优化系统中所有分层存储空间上的存储层中的数据位置。
|   **服务名称**    |   TieringEngineService
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="superfetch"></a>Superfetch          

| | |           
|---|---|       
|   **服务说明** |   维持和不断改进系统性能。
|   **服务名称**    |   SysMain
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="sync-host"></a>同步主机            

| | |           
|---|---|       
|   **服务说明** |   此服务同步邮件、联系人、日历和其他各种用户数据。 如果此服务未运行，依赖于此功能的邮件和其他应用程序将无法正常工作。
|   **服务名称**    |   OneSyncSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  |   可以禁用
|   **备注**    |   用户服务模板
|||         

<br />          

## <a name="system-event-notification-service"></a>系统事件通知服务            

| | |           
|---|---|       
|   **服务说明** |   监视系统事件，并通知订阅者 COM+ 事件系统中发生了这些事件。
|   **服务名称**    |   SENS
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="system-events-broker"></a>系统事件代理             

| | |           
|---|---|       
|   **服务说明** |   协调 WinRT 应用程序的后台工作执行。 如果此服务已停止或禁用，则可能不会触发后台工作。
|   **服务名称**    |   SystemEventsBroker
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **备注**    |   虽然此服务的说明暗示着它仅适用于 WinRT 应用，但任务计划程序、代理基础结构服务和其他内部组件也需要此服务。
|||         

<br />          

## <a name="task-scheduler"></a>任务计划程序           

| | |           
|---|---|   
|   **服务说明** |   使用户能够在此计算机上配置和计划自动化任务。 该服务还托管多个 Windows 系统关键任务。 如果此服务已停止或禁用，则不会按计划时间运行这些任务。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   计划
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="tcpip-netbios-helper"></a>TCP/IP NetBIOS 帮助程序            

| | |           
|---|---|   
|   **服务说明** |   提供对网络中客户端的 TCP/IP 上的 NetBIOS (NetBT) 服务和 NetBIOS 名称解析的支持，因此可让用户共享文件、打印和登录到网络。 如果此服务已停止，则这些功能可能不可用。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   lmhosts
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="telephony"></a>Telephony           

| | |           
|---|---|       
|   **服务说明** |   为控制本地计算机上的电话设备，以及通过 LAN 控制同样运行该服务的服务器上的电话服务的程序提供电话 API (TAPI) 支持。
|   **服务名称**    |   TapiSrv
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   禁用此服务会中断 RRAS
|||         

<br />          

## <a name="themes"></a>主题           

| | |           
|---|---|
|   **服务说明** |   提供用户体验主题管理。
|   **服务名称**    |   主题
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **备注**    |   禁用此服务时，无法设置辅助功能主题
|||         

<br />  

## <a name="tile-data-model-server"></a>图块数据模型服务器           

| | |           
|---|---|   
|   **服务说明** |   用于图块更新的图块服务器。
|   **服务名称**    |   tiledatamodelsvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **备注**    |   如果此服务已禁用，“开始”菜单将会损坏
|||         

<br />          

##  <a name="time-broker"></a>计时代理     

| | |           
|---|---|       
|   **服务说明** |   协调 WinRT 应用程序的后台工作执行。 如果此服务已停止或禁用，则可能不会触发后台工作。
|   **服务名称**    |   TimeBrokerSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   虽然此服务的说明暗示着它仅适用于 WinRT 应用，但任务计划程序、代理基础结构服务和其他内部组件也需要此服务。
|||         

<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>触摸键盘和手写面板服务         

| | |           
|---|---|   
|   **服务说明** |   启用触摸键盘、手写面板笔和墨迹功能
|   **服务名称**    |   TabletInputService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Windows 更新的更新业务流程协调程序服务           

| | |           
|---|---|       
|   **服务说明** |   管理 Windows 更新。 如果停止该服务，则设备无法下载和安装最新的更新。
|   **服务名称**    |   UsoSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   v1607 中缺少服务说明；Windows 更新（包括 WSUS）依赖此服务。
|||         

<br />          

## <a name="upnp-device-host"></a>UPnP 设备主机         

| | |           
|---|---|   
|   **服务说明** |   允许在此计算机上托管 UPnP 设备。 如果此服务已停止，则任何托管的 UPnP 设备将停止运行，且无法添加其他托管设备。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   upnphost
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="user-access-logging-service"></a>用户访问日志记录服务          

| | |           
|---|---|   
|   **服务说明** |   此服务记录本地服务器上已安装的产品和角色的唯一客户端访问请求（采用 IP 地址和用户名格式）。 需要量化脱机客户端访问许可证 (CAL) 管理的服务器软件客户端需求的管理员可以通过 Powershell 查询此信息。 如果该服务已禁用，则不会记录客户端请求，且无法通过 Powershell 查询检索此类请求。 停止该服务不会影响历史数据的查询（有关删除历史数据的步骤，请参阅支持文档）。 本地系统管理员必须查阅其 Windows Server 许可条款来确定需要适当许可的服务器软件所需的 CAL 数目；使用 UAL 服务和数据不会改变此义务。
|   **服务名称**    |   UALSVC
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="user-data-access"></a>用户数据访问        

| | |           
|---|---|   
|   **服务说明** |   使应用能够访问结构化的用户数据，包括联系人信息、日历、邮件和其他内容。 如果停止或禁用此服务，则使用此数据的应用可能无法正常工作。
|   **服务名称**    |   UserDataSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   用户服务模板
|||         

<br />          

## <a name="user-data-storage"></a>用户数据存储            

| | |           
|---|---|       
|   **服务说明** |   处理结构化用户数据（包括联系人信息、日历、邮件和其他内容）的存储。 如果停止或禁用此服务，则使用此数据的应用可能无法正常工作。
|   **服务名称**    |   UnistoreSvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   用户服务模板
|||         

<br />          

## <a name="user-experience-virtualization-service"></a>用户体验虚拟化服务           

| | |           
|---|---|       
|   **服务说明** |   提供应用程序和 OS 设置漫游的支持
|   **服务名称**    |   UevAgentService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   
|||         

<br />          

##  <a name="user-manager"></a>用户管理器        

| | |           
|---|---|   
|   **服务说明** |   用户管理器提供多用户交互所需的运行时组件。  如果此服务已停止，某些应用程序可能无法正常运行。
|   **服务名称**    |   UserManager
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="user-profile-service"></a>用户配置文件服务         

| | |           
|---|---|   
|   **服务说明** |   此服务负责加载和卸载用户配置文件。 如果此服务已停止或禁用，用户将不再能够成功登录或注销，应用在获取用户数据时可能会遇到问题，注册为接收配置文件事件通知的组件将收不到这些通知。
|   **服务名称**    |   ProfSvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="virtual-disk"></a>虚拟磁盘             

| | |           
|---|---|   
|   **服务说明** |   提供磁盘、卷、文件系统和存储阵列的管理服务。
|   **服务名称**    |   vds
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   无指导
|   **备注**    |   
|||         

<br />          

## <a name="volume-shadow-copy"></a>卷影复制           

| | |           
|---|---|   
|   **服务说明** |   管理和实施用于备份和其他目的的卷影副本。 如果此服务已停止，卷影副本将不可用于备份，并且备份可能会失败。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   VSS
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   无指导
|   **备注**    |   
|||         

<br />          

##  <a name="walletservice"></a>WalletService           

| | |           
|---|---|   
|   **服务说明** |   托管 wallet 客户端使用的对象
|   **服务名称**    |   WalletService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="windows-audio"></a>Windows 音频            

| | |           
|---|---|       
|   **服务说明** |   管理基于 Windows 的程序的音频。  如果此服务已停止，音频设备和音效将无法正常运行。  如果此服务已禁用，显式依赖它的任何服务将无法启动
|   **服务名称**    |   Audiosrv
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="windows-audio-endpoint-builder"></a>Windows 音频终结点生成器           

| | |           
|---|---|
|   **服务说明** |   管理 Windows 音频服务的音频设备。  如果此服务已停止，音频设备和音效将无法正常运行。  如果此服务已禁用，显式依赖它的任何服务将无法启动
|   **服务名称**    |   AudioEndpointBuilder
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="windows-biometric-service"></a>Windows 生物识别服务            

| | |           
|---|---|   
|   **服务说明** |   Windows 生物识别服务为客户端应用程序提供无需获取对任何生物识别硬件或样本的直接访问权限就可捕获、比较、处理和存储生物识别数据的功能。 该服务托管在特权 SVCHOST 进程中。
|   **服务名称**    |   WbioSrvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="windows-camera-frame-server"></a>Windows 相机帧服务器         

| | |           
|---|---|       
|   **服务说明** |   使多个客户端能够从相机设备访问视频帧。
|   **服务名称**    |   FrameServer
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="windows-connection-manager"></a>Windows 连接管理器           

| | |           
|---|---|   
|   **服务说明** |   根据电脑当前可用的网络连接选项做出自动连接/断开连接决策，并支持根据组策略设置管理网络连接。
|   **服务名称**    |   Wcmsvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-defender-network-inspection-service"></a>Windows Defender 网络检查服务          

| | |           
|---|---|       
|   **服务说明** |   帮助抵御针对网络协议中已知和新发现的漏洞发起的入侵企图
|   **服务名称**    |   WdNisSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导    
|   **备注**    |   
|||         

<br />          

## <a name="windows-defender-service"></a>Windows Defender 服务         

| | |           
|---|---|       
|   **服务说明** |   帮助防范恶意软件和其他潜在有害软件对用户造成影响
|   **服务名称**    |   WinDefend
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation - 用户模式驱动程序框架           

| | |           
|---|---|   
|   **服务说明** |   创建和管理用户模式驱动程序进程。 不能停止此服务。
|   **服务名称**    |   wudfsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-encryption-provider-host-service"></a>Windows 加密提供程序主机服务     

| | |           
|---|---|   
|   **服务说明** |   Windows 加密提供程序主机服务将第三方加密提供程序中的加密相关功能中转到需要评估和应用 EAS 策略的进程。 停止此服务会破坏已连接邮件帐户建立的 EAS 合规性检查
|   **服务名称**    |   WEPHOSTSVC
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-error-reporting-service"></a>Windows 错误报告服务          

| | |           
|---|---|       
|   **服务说明** |   允许在程序停止运行或无响应时报告错误，并允许传送现有的解决方案。 此外，允许生成诊断和修复服务的日志。 如果此服务已停止，错误报告可能无法正常工作，并且诊断服务和修复结果可能不会显示。
|   **服务名称**    |   WerSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   收集并发送 MS 和第三方 ISV/IHV 使用的崩溃/挂起数据。 该数据用于诊断崩溃造成的 bug，其中可能包括安全 bug。 企业错误报告也需要此服务
|||         

<br />          

## <a name="windows-event-collector"></a>Windows 事件收集器          

| | |           
|---|---|   
|   **服务说明** |   此服务管理支持 WS-Management 协议的远程源发出的事件的持久订阅。 这包括 Windows Vista 事件日志、硬件和支持 IPMI 的事件源。 该服务在本地事件日志中存储转发的事件。 如果此服务已停止或禁用，则无法创建事件订阅，且无法接受转发的事件。
|   **服务名称**    |   Wecsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   收集 ETW 事件（包括安全事件）以方便管理和诊断。  有大量的功能和第三方工具依赖此服务，其中包括安全审核工具
|||         

<br />          

## <a name="windows-event-log"></a>Windows 事件日志            

| | |           
|---|---|       
|   **服务说明** |   此服务管理事件和事件日志。 它支持记录事件、查询事件、订阅事件、存档事件日志和管理事件元数据。 它可以 XML 和纯文本格式显示事件。 停止此服务可能会损害系统的安全性和可靠性。
|   **服务名称**    |   EventLog
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-firewall"></a>Windows 防火墙         

| | |           
|---|---|   
|   **服务说明** |   Windows 防火墙会阻止未经授权的用户通过互联网或网络访问你的计算机，因而可以帮助保护你的计算机。
|   **服务名称**    |   MpsSvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="windows-font-cache-service"></a>Windows 字体缓存服务      

| | |           
|---|---|   
|   **服务说明** |   通过缓存常用的字体数据来优化应用程序的性能。 如果此服务尚未运行，应用程序会将其启动。 可将其禁用，不过，这样做会降低应用程序的性能。
|   **服务名称**    |   FontCache
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-image-acquisition-wia"></a>Windows 图像采集 (WIA)          

| | |           
|---|---|   
|   **服务说明** |   为扫描仪和照相机提供图像采集服务
|   **服务名称**    |   stisvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

##  <a name="windows-insider-service"></a>Windows Insider 服务     

| | |           
|---|---|   
|   **服务说明** |   wisvc
|   **服务名称**    |   wisvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   Windows Server 不支持外部测试，因此，此服务在 Windows Server 不会运行。 也可以通过 GP 禁用功能。
|||         

<br />          

##  <a name="windows-installer"></a>Windows Installer       

| | |           
|---|---|
|   **服务说明** |   添加、修改和删除作为 Windows Installer（*.msi、*.msp）包提供的应用程序。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   msiserver
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-license-manager-service"></a>Windows 许可证管理器服务          

| | |           
|---|---|   
|   **服务说明** |   提供 Microsoft Store 的基础结构支持。  此服务按需启动，如果已禁用，则通过 Microsoft Store 获取的内容将无法正常运行。
|   **服务名称**    |   LicenseManager
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-management-instrumentation"></a>Windows Management Instrumentation       

| | |           
|---|---|       
|   **服务说明** |   提供一个通用接口和对象模型用于访问有关操作系统、设备、应用程序和服务的管理信息。 如果此服务已停止，则大多数基于 Windows 的软件将无法正常运行。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   Winmgmt
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

##  <a name="windows-mobile-hotspot-service"></a>Windows 移动热点服务          

| | |           
|---|---|       
|   **服务说明** |   提供与另一台设备共享手机网络数据连接的功能。
|   **服务名称**    |   icssvc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   
|||         

<br />          

## <a name="windows-modules-installer"></a>Windows 模块安装程序        

| | |           
|---|---|   
|   **服务说明** |   启用 Windows 更新和可选组件的安装、修改与删除。 如果此服务已禁用，在此计算机上安装或卸载 Windows 更新可能会失败。
|   **服务名称**    |   TrustedInstaller
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="windows-push-notifications-system-service"></a>Windows 推送通知系统服务            

| | |           
|---|---|
|   **服务说明** |   此服务在会话 0 中运行，托管通知平台以及用于处理设备与 WNS 服务器之间的连接的连接提供程序。
|   **服务名称**    |   WpnService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   自动
|   **建议**  |   可以禁用
|   **备注**    |   动态磁贴和其他功能需要此服务
|||         

<br />      

## <a name="windows-push-notifications-user-service"></a>Windows 推送通知用户服务          

| | |           
|---|---|   
|   **服务说明** |   此服务托管提供本地和推送通知支持的 Windows 通知平台。 支持的通知包括磁贴、toast 和原始。
|   **服务名称**    |   WpnUserService
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   可以禁用
|   **备注**    |   用户服务模板
|||         

<br />

## <a name="windows-remote-management-ws-management"></a>Windows 远程管理 (WS-Management)
| | |
|---|---|
|   **服务说明** |   Windows 远程管理 (WinRM) 服务为远程管理实施 WS-Management 协议。 WS-Management 是一个用于远程软件和硬件管理的标准 Web 服务协议。 WinRM 服务为 WS-Management 请求监听网络并处理它们。 WinRM 服务需要使用 winrm.cmd 命令行工具或通过组策略配置侦听器，以在网络上进行监听。 WinRM 服务提供对 WMI 数据的访问并启用事件收集。 事件收集和事件订阅要求服务正在运行。 WinRM 消息使用 HTTP 和 HTTPS 作为传输器。 WinRM 服务不依赖于 IIS 但预配置与同一台机器上的 IIS 共享一个端口。  WinRM 服务保留 /wsman URL 前缀。 为了防止与 IIS 冲突，管理员应确保 IIS 上托管的任何网站不使用 /wsman URL 前缀。
|   **服务名称**    |   WinRM
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **备注**    |   远程管理需要此服务
|||

<br />          

##  <a name="windows-search"></a>Windows 搜索      

| | |           
|---|---|       
|   **服务说明** |   提供文件、电子邮件和其他内容的内容索引、属性缓存和搜索结果。
|   **服务名称**    |   WSearch
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **备注**    |   
|||         

<br />          

##  <a name="windows-time"></a>Windows 时间        

| | |           
|---|---|   
|   **服务说明** |   在网络中的所有客户端和服务器上保持日期与时间同步。 如果此服务已停止，日期和时间同步将不可用。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   W32Time
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导
|   **备注**    |   
|||         

<br />          

## <a name="windows-update"></a>Windows 更新           

| | |           
|---|---|       
|   **服务说明** |   启用 Windows 和其他程序的更新检测、下载和安装。 如果此服务已禁用，则此计算机的用户将无法使用 Windows 更新或其自动更新功能，程序将无法使用 Windows 更新代理 (WUA) API。
|   **服务名称**    |   wuauserv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>WinHTTP Web 代理自动发现服务         

| | |           
|---|---|   
|   **服务说明** |   WinHTTP 实现客户端 HTTP 堆栈，为开发人员提供 Win32 API 和 COM 自动化组件用于发送 HTTP 请求及接收响应。 此外，WinHTTP 通过其 Web 代理自动发现 (WPAD) 协议的实现提供自动发现代理配置的支持。
|   **服务名称**    |   WinHttpAutoProxySvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **备注**    |   使用网络堆栈的任何组件都可能在功能上依赖于此服务。 许多组织依赖于此服务来配置其内部网络的 HTTP 代理路由。  如果没有此服务，从内部向 Internet 发起的 HTTP 连接都会失败。
|||         

<br />          

## <a name="wired-autoconfig"></a>有线自动配置         

| | |           
|---|---|       
|   **服务说明** |   有线自动配置 (DOT3SVC) 服务负责对以太网接口执行 IEEE 802.1X 身份验证。 如果当前有线网络部署实施 802.1X 身份验证，则应配置为运行 DOT3SVC 服务，以建立第 2 层连接和/或提供对网络资源的访问。 不实施 802.1X 身份验证的有线网络不会受到 DOT3SVC 服务的影响。
|   **服务名称**    |   dot3svc
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  | 无指导   
|   **备注**    |   
|||         

<br />          

## <a name="wmi-performance-adapter"></a>WMI 性能适配器          

| | |           
|---|---|   
|   **服务说明** |   提供从 Windows Management Instrumentation (WMI) 提供程序到网络中客户端的性能库信息。 仅当已激活性能数据帮助程序时，此服务才会运行。
|   **服务名称**    |   wmiApSrv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 无指导       
|   **备注**    |   
|||         

<br />          

## <a name="workstation"></a>工作站          

| | |           
|---|---|   
|   **服务说明** |   使用 SMB 协议来与远程服务器建立并保持客户端网络连接。 如果此服务已停止，则这些连接将不可用。 如果此服务已禁用，显式依赖它的任何服务将无法启动。
|   **服务名称**    |   LanmanWorkstation
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 无指导       
|   **备注**    |   
|||         

<br />

## <a name="xbox-live-auth-manager"></a>Xbox Live 身份验证管理器           

| | |           
|---|---|   
|   **服务说明** |   提供用来与 Xbox Live 交互的身份验证和授权服务。 如果此服务已停止，某些应用程序可能无法正常运行。
|   **服务名称**    |   XblAuthManager
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   应该禁用
|   **备注**    |   
|||         

<br />          

## <a name="xbox-live-game-save"></a>Xbox Live 游戏保存          

| | |           
|---|---|   
|   **服务说明** |   此服务用于同步支持 Xbox Live“保存”的游戏的保存数据。  如果此服务已停止，则不会向/从 Xbox Live 上传/下载游戏保存数据。
|   **服务名称**    |   XblGameSave
|   **安装**    |   仅用于桌面体验
|   **StartType**   |   Manual
|   **建议**  |   应该禁用
|   **备注**    |   此服务用于同步支持 Xbox Live“保存”的游戏的保存数据。  如果此服务已停止，则不会向/从 Xbox Live 上传/下载游戏保存数据。
|||         

<br /> 
<br /> 

