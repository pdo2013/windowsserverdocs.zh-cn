---
title: Windows Server 2016 中的系统服务的安全指导原则
description: 在 Windows Server 2016 中禁用服务的安全准则及桌面体验
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 1a9496a121fc45df0b788ea56d50db922fd24536
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749444"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>禁用带桌面体验的 Windows Server 2016 上的系统服务的指南

适用于：Windows Server 2016

Windows 操作系统包括许多系统服务提供重要功能。 不同的服务具有不同的默认启动策略： 一些默认情况下启动 （自动），一些时所需 （手动），以及一些默认情况下禁用和可以运行之前必须显式启用。 为每个服务，以平衡性能、 功能和安全的典型客户仔细选择这些默认值。

但是，一些企业客户可能会更专注于安全性的平衡倾向于使用其 Windows Pc 和服务器，一台可以减少其攻击面上绝对最小，并因此可能想要完全禁用所有不需要在其特定的服务环境。 对于那些客户，Microsoft® 提供哪些服务可以安全地禁用实现此目的的随附指南。

本指南是仅为带桌面体验的 Windows Server 2016 （除非用作最终用户的桌面替换）。 从 Windows Server 2019 开始，默认情况下配置这些指导原则。 在系统上的每个服务进行分类，如下所示：

-   **应该禁用：** 专注于安全性的企业将最可能愿意使用以禁用此服务并放弃其功能 （请参阅下面的其他详细信息）。
- **禁用确定:** 此服务提供的某些但并非所有的企业来说，非常有用的功能，并不使用它的专注于安全性的企业可以安全地禁用它。
- **不要禁用：** 禁用此服务将会影响基本功能，或阻止特定角色或功能正常。 因此它不应禁用。
-  **（没有的指南）：** 禁用这些服务的影响不完全评估。 因此，不应更改这些服务的默认配置。


客户可以配置其 Windows Pc 和服务器，若要禁用所选的服务在其组策略中使用安全模板，或使用 PowerShell 自动化。 在某些情况下，该指南包括特定组策略设置，以禁用该服务本身的一种替代方法直接，禁用该服务的功能。

Microsoft 建议客户禁用以下服务和带有桌面体验的 Windows Server 2016 上其各自的计划的任务：

服务： 
1. Xbox Live 的身份验证管理器
2. Xbox Live 游戏保存

计划的任务： 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(还可以访问通过查看附加的 Microsoft Excel 电子表格这篇文章中详细介绍的所有服务的信息：[禁用带桌面体验的 Windows Server 2016 上的系统服务的指南](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>禁用默认情况下不安装的服务

Microsoft 建议针对应用策略禁用默认情况下不安装的服务。
-  如果安装该功能，则通常需要该服务。 安装服务或功能需要管理权限。 不允许进行功能安装，不服务启动。
-  阻止 Microsoft Windows 服务不会阻止安装类似的第三方等效，可能是一个具有更高版本的安全风险的管理员 （或在某些情况下的非管理员）。
-  基线或禁用非默认 Windows 服务 (例如，W3SVC) 的基准将提供一些审核员的技术 (例如，IIS) 在本质上是不安全，应永远不会有错误的印象。
-  如果永远不会安装的功能 （和服务），这只会添加不必要的大容量与基线和验证工作。

<br />
对于本文档中列出的所有系统服务，下面的两个表提供了用于启用和禁用系统服务与桌面体验的 Windows Server 2016 中的列和 Microsoft 建议的说明： 

<br />

### <a name="explanation-of-columns"></a>列的说明

| | |
|---|---|
|**服务说明**|   Sc.exe qdescription 来自服务的说明。|
|**名称** |密钥 （内部） 的服务名称|
|**安装** |始终安装：服务是在服务器核心和带有桌面体验的服务器上  <br /> 仅带有桌面体验：服务是带桌面体验的 Windows Server 2016 上但是***不***在 Server Core 上 |
|**StartType**  |Windows Server 2016 上的服务启动类型|
|**建议** |Microsoft 建议/有关建议禁用 Windows Server 2016 上此服务在典型、 管理完善的企业部署，其中服务器未用作最终用户桌面替换。|
|**注释** |附加说明|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Microsoft 建议的说明

| | |
|---|---|
|**不要禁用** |不应禁用此服务|
|**确定以禁用**| 如果未使用它支持的功能，可以禁用此服务。|
|**已禁用**|  此服务禁用默认设置。无需与策略强制实施|
|**应禁用** |上一个妥善管理的企业系统应永远不会启用此服务。|

<br />

下表提供 Microsoft 禁用带桌面体验的 Windows Server 2016 上的系统服务的指导：

<br />

##  <a name="activex-installer-axinstsv"></a>ActiveX 安装程序 (AxInstSV)

| | |
|---|---|
|   **服务说明** |   从 Internet 提供的 ActiveX 控件安装的用户帐户控制验证并启用基于组策略设置的 ActiveX 控件安装的管理。 此服务将按需启动，并且如果禁用安装 ActiveX 控件的行为根据默认浏览器设置。    |
|   **服务名称**    |   AxInstSV    |
|   **安装**    |   仅带有桌面体验    |
|   **StartType**   |   Manual  |
|   **建议**  |   确定以禁用   |
|   **注释**    |   确定以禁用如果功能不需要 |


<br />

## <a name="alljoyn-router-service"></a>AllJoyn 路由器服务   

| | |
|---|---|
|   **服务说明** |   将 AllJoyn 消息路由的本地 AllJoyn 客户端。 如果停止此服务不具有其自己捆绑的路由器 AllJoyn 客户端将无法运行。 |
|   **服务名称**    |   AJRouter    |
|   **安装**    |   仅带有桌面体验    |
|   **StartType**   |   Manual  |
|   **建议**  | 不提供的指导       |
|   **注释**    |       |
| | |

<br />

## <a name="app-readiness"></a>应用程序准备情况

| | |
|---|---|
**服务说明** |   获取应用程序准备好使用第一次用户登录到此电脑和添加新应用时。
**服务名称**    |   AppReadiness
**安装**    |   仅带有桌面体验
**StartType**   |   Manual
**建议**  |   不要禁用
**注释**    |   
| | |

<br />

##  <a name="application-identity"></a>应用程序标识

| | |       
|---|---|   
**服务说明** |   确定并验证应用程序的标识。 禁用此服务将阻止将 AppLocker 强制实施。
**服务名称**    |   AppIDSvc
**安装**    |   始终安装
**StartType**   |   Manual
**建议**  |不提供的指导    
**注释**    |   
|||     

<br />

##  <a name="application-information"></a>应用程序信息 

| | |       
|---|---|   
|   **服务说明** |   便于交互式应用程序使用其他的管理权限的运行。  如果此服务已停止，用户将无法使用它们可能需要执行所需的用户任务的附加管理权限启动应用程序。
|   **服务名称**    |   Appinfo
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   支持 UAC 同一个桌面上提升
|||     

<br />

##  <a name="application-layer-gateway-service"></a>应用程序层网关服务       

| | |           
|---|---|           
|   **服务说明** |   第三方协议插件的 Internet 连接共享提供支持
|   **服务名称**    |   ALG
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |不提供的指导    
|   **注释**    |   
|||     

<br />

##  <a name="application-management"></a>应用程序管理      

| | |           
|---|---|       
|   **服务说明** |   处理通过组策略部署的软件的安装、 删除和枚举请求。 如果禁用该服务，则用户将无法安装、 删除或枚举通过组策略部署的软件。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   AppMgmt
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="appx-deployment-service-appxsvc"></a>AppX 部署服务 (AppXSVC)       

| | |           
|---|---|
|   **服务说明** |   部署应用商店应用程序提供基础结构支持。 按需启动此服务，如果已禁用应用商店应用程序将不会部署到系统中，并且可能无法正常工作。
|   **服务名称**    |   AppXSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="auto-time-zone-updater"></a>自动时区更新程序           

| | |           
|---|---|           
|   **服务说明** |   会自动设置系统时区。
|   **服务名称**    |   tzautoupdate
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   
|||         

<br />          

## <a name="background-intelligent-transfer-service"></a>后台智能传送服务          

| | |           
|---|---|   
|   **服务说明** |   将使用空闲的网络带宽在后台文件传输。 如果禁用该服务，则依赖于位，如 Windows 更新或 MSN 资源管理器中，任何应用程序将无法自动下载程序和其他信息。
|   **服务名称**    |   BITS
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          


## <a name="background-tasks-infrastructure-service"></a>后台任务基础结构服务      

| | |           
|---|---|   
|   **服务说明** |   Windows 基础结构服务，它控制的后台任务可以在系统上运行。
|   **服务名称**    |   BrokerInfrastructure
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="base-filtering-engine"></a>基本筛选引擎            

| | |           
|---|---|       
|   **服务说明** |   基本筛选引擎 (BFE) 是一种服务，管理防火墙和 Internet 协议安全 (IPsec) 策略和实现用户模式下筛选。 停止或禁用 BFE 服务将显著减少系统的安全性。 它还将导致不可预知的行为在 IPsec 管理和防火墙的应用程序。
|   **服务名称**    |   BFE
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="bluetooth-support-service"></a>蓝牙的支持服务            

| | |           
|---|---|   
|   **服务说明** |   蓝牙服务支持发现和关联的远程蓝牙设备。  停止或禁用此服务可能会导致已安装的 Bluetooth 设备无法正常运行，并防止发现或相关联的新设备。
|   **服务名称**    |   bthserv
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   若要禁用如果未使用的确定。 另一种禁用机制： https://technet.microsoft.com/library/dd252791.aspx
|||         

<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           

| | |           
|---|---|   
|   **服务说明** |   此用户服务用于连接的设备平台方案
|   **服务名称**    |   CDPUserSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  |   确定以禁用
|   **注释**    |   用户服务模板
|||         

<br />          


##  <a name="certificate-propagation"></a>证书传播     

| | |           
|---|---|
|   **服务说明** |   将用户证书和根证书从智能卡复制到当前用户的证书存储区中，检测到智能卡插入智能卡读卡器时，并根据需要安装智能卡即插微型驱动程序。
|   **服务名称**    |   CertPropSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="client-license-service-clipsvc"></a>客户端许可证服务 (ClipSVC)        

| | |           
|---|---|   
|   **服务说明** |   提供对 Microsoft Store 的基础结构支持。 按需启动此服务，如果购买了已禁用应用程序使用 Microsoft Store 就不能正常工作。
|   **服务名称**    |   ClipSVC
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="cng-key-isolation"></a>CNG 密钥隔离

| | |           
|---|---|   
|   **服务说明** |   CNG 密钥隔离服务托管在 LSA 进程。 该服务提供到私钥和关联的加密操作所需的通用准则的关键进程隔离。 该服务将存储并遵守一般准则要求的安全进程中使用生存期较长的密钥。
|   **服务名称**    |   KeyIso
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="com-event-system"></a>COM + 事件系统       

| | |           
|---|---|       
|   **服务说明** |   支持系统事件通知服务 (SENS)，它提供自动分发到订阅组件对象模型 (COM) 组件的事件。 如果服务已停止，SENS 将关闭，并将无法再提供登录和注销的通知。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   EventSystem
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="com-system-application"></a>COM + 系统应用程序     

| | |           
|---|---|       
|   **服务说明** |   管理配置和组件对象模型 (COM) + 基于组件的跟踪。 如果该服务已停止，大多数 COM + 的基本的组件将无法正常工作。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   COMSysApp
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="computer-browser"></a>计算机浏览器        

| | |           
|---|---|       
|   **服务说明** |   维护网络上的计算机的更新的列表，并提供此列表，以指定为浏览器的计算机。 如果此服务已停止，将不更新或维护此列表。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   浏览器
|   **安装**    |   始终安装
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   
|||         

<br />          

## <a name="connected-devices-platform-service"></a>连接的设备平台服务       

| | |           
|---|---|       
|   **服务说明** |   此服务用于连接的设备和通用玻璃方案
|   **服务名称**    |   CDPSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="connected-user-experiences-and-telemetry"></a>连接的用户体验和遥测     

| | |           
|---|---|       
|   **服务说明** |   连接的用户体验和遥测服务，支持在应用程序和已连接的用户体验的功能。 此外，该服务可管理的事件驱动的收集和传输诊断和使用情况信息 （用于改进的体验和质量的 Windows 平台） 下时启用诊断和使用情况的隐私选项设置反馈和诊断。
|   **服务名称**    |   DiagTrack
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="contact-data"></a>联系人数据        

| | |           
|---|---|       
|   **服务说明** |   索引用于快速联系人搜索联系人数据。 如果停止或禁用此服务，联系人可能缺少从搜索结果。
|   **服务名称**    |   PimIndexMaintenanceSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   用户服务模板
|||         

<br />          

## <a name="coremessaging"></a>CoreMessaging            

| | |           
|---|---|           
|   **服务说明** |   管理系统组件之间的通信。
|   **服务名称**    |   CoreMessagingRegistrar
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="credential-manager"></a>凭据管理器           

| | |           
|---|---|       
|   **服务说明** |   提供给用户、 应用程序和安全服务包的安全存储和检索的凭据。
|   **服务名称**    |   VaultSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="cryptographic-services"></a>加密服务           

| | |           
|---|---|       
|   **服务说明** |   提供了三种管理服务：目录数据库服务，它确认签名的 Windows 文件并使新程序可以安装;受保护的根服务，用于添加并从此计算机; 中删除受信任的根证书颁发机构证书并自动根证书更新服务，它从 Windows 更新中检索根证书和启用 SSL 等方案。 如果此服务已停止，这些管理服务将无法正常工作。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   CryptSvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="data-sharing-service"></a>数据共享服务         

| | |           
|---|---|       
|   **服务说明** |   提供应用程序之间协调数据。
|   **服务名称**    |   DsSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          

| | |           
|---|---|       
|   **服务说明** |   DCP （数据收集和发布） 服务支持第一方应用上传到云的数据。
|   **服务名称**    |   DcpSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="dcom-server-process-launcher"></a>DCOM 服务器进程启动器         

| | |           
|---|---|       
|   **服务说明** |   DCOMLAUNCH 服务将启动以响应对象的激活请求的 COM 和 DCOM 服务器。 如果此服务停止或禁用，使用 COM 或 DCOM 应用程序将不能正常工作。 强烈建议您有 DCOMLAUNCH 服务正在运行。
|   **服务名称**    |   DcomLaunch
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |不提供的指导    
|   **注释**    |   
|||         

<br />

##  <a name="device-association-service"></a>设备关联服务      

| | |           
|---|---|       
|   **服务说明** |   启用之间的系统和有线或无线设备配对。
|   **服务名称**    |   DeviceAssociationService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />

##  <a name="device-install-service"></a>设备安装服务

| | |
|---|---|
|   **服务说明** |   使计算机能够识别和调整硬件更改很少或没有用户输入。 停止或禁用此服务将导致系统不稳定。
|   **服务名称**    |   DeviceInstall
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导
|   **注释**    |
|||

<br />          

##  <a name="device-management-enrollment-service"></a>设备管理注册服务        

| | |           
|---|---|       
|   **服务说明** |   执行设备管理的设备注册活动
|   **服务名称**    |   DmEnrollmentSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="device-setup-manager"></a>设备安装程序管理器         

| | |           
|---|---|       
|   **服务说明** |   启用检测、 下载和安装的与设备相关的软件。 如果禁用此服务，则设备可能使用过期的软件配置，并可能无法正常工作。
|   **服务名称**    |   DsmSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="devquery-background-discovery-broker"></a>DevQuery 后台发现代理         

| | |           
|---|---|           
|   **服务说明** |   支持使用应用程序发现与背景任务的设备
|   **服务名称**    |   DevQueryBroker
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="dhcp-client"></a>DHCP 客户端          

| | |           
|---|---|       
|   **服务说明** |   注册和更新 IP 地址和此计算机的 DNS 记录。 如果此服务已停止，此计算机不会收到动态 IP 地址和 DNS 更新。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   Dhcp
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="diagnostic-policy-service"></a>诊断策略服务            

| | |           
|---|---|       
|   **服务说明** |   诊断策略服务使问题检测、 故障排除和解决 Windows 组件。  如果此服务已停止，诊断将不再起作用。
|   **服务名称**    |   DPS
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="diagnostic-service-host"></a>诊断服务主机     

| | |           
|---|---|       
|   **服务说明** |   诊断服务主机可供需要在本地服务的上下文中运行的主机诊断诊断策略服务。  如果此服务已停止，依赖于它的任何诊断将不再起作用。
|   **服务名称**    |   WdiServiceHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="diagnostic-system-host"></a>诊断系统主机           

| | |           
|---|---|       
|   **服务说明** |   诊断系统主机可供需要在本地系统上下文中运行的主机诊断诊断策略服务。  如果此服务已停止，依赖于它的任何诊断将不再起作用。
|   **服务名称**    |   WdiSystemHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="distributed-link-tracking-client"></a>分布式的链接跟踪客户端            

| | |           
|---|---|   
|   **服务说明** |   维护 NTFS 文件在计算机内或跨网络中的计算机之间的链接。
|   **服务名称**    |   TrkWks
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="distributed-transaction-coordinator"></a>分布式事务处理协调器     

| | |           
|---|---|   
|   **服务说明** |   协调跨多个资源管理器，例如数据库、 消息队列和文件系统的事务。 如果此服务已停止，这些事务将失败。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   MSDTC
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        

| | |           
|---|---|       
|   **服务说明** |   WAP 推送消息路由服务
|   **服务名称**    |   dmwappushservice
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   Intune、 MDM 和类似的管理技术，以及统一写入筛选器客户端设备上所需的服务。 不需要服务器。
|||         

<br />      

##  <a name="dns-client"></a>DNS 客户端的运行情况      

| | |           
|---|---|       
|   **服务说明** |   DNS 客户端服务(dnscache)缓存域名系统(DNS)名称并注册此计算机的计算机全名。 如果该服务已停止，将继续解析 DNS 名称。 但是，不会缓存 DNS 名称查询的结果，并且将不会注册计算机的名称。 如果该服务已禁用，将无法启动显式依赖它的服务。
|   **服务名称**    |   启动 Dnscache
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="downloaded-maps-manager"></a>下载的映射管理器     

| | |           
|---|---|   
|   **服务说明** |   已下载的地图应用程序访问权限的 Windows 服务。 此服务已启动按需由应用程序访问下载映射。 禁用此服务将阻止应用访问映射。
|   **服务名称**    |   MapsBroker
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  |   确定以禁用
|   **注释**    |   禁用依赖于该服务; 的分隔线应用确定以禁用如果不依赖于它的应用
|||         

<br />          

## <a name="embedded-mode"></a>嵌入模式            

| | |           
|---|---|       
|   **服务说明** |   嵌入模式服务，与后台应用程序相关的方案。  禁用此服务将无法从正在激活的后台应用程序。
|   **服务名称**    |   embeddedmode
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="encrypting-file-system-efs"></a>加密文件系统 (EFS)

| | |                   
|---|---|           
|   **服务说明** | 提供用于存储在 NTFS 文件系统卷上的加密的文件的核心文件加密技术。 如果已停止或禁用此服务，应用程序将无法访问加密的文件。            
|   **服务名称**  |  EFS            
|   **安装**  |  始终安装           
|   **StartType**   |  Manual           
|   **建议**  | 不提供的指导           
|   **注释**   |
|||                 

<br />  

## <a name="enterprise-app-management-service"></a>企业应用程序管理服务            

| | |           
|---|---|       
|   **服务说明** |   使企业应用程序管理。
|   **服务名称**    |   EntAppSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="extensible-authentication-protocol"></a>可扩展的身份验证协议           

| | |           
|---|---|   
|   **服务说明** |   在这种情况下为 802.1x 有线和无线网络身份验证、 VPN 和网络访问保护 (NAP)，提供了可扩展身份验证协议 (EAP) 服务。  EAP 还提供了应用程序编程接口 (Api) 使用的网络访问客户端，包括无线和 VPN 客户端身份验证过程。  如果禁用此服务时，会阻止此计算机访问需要 EAP 身份验证的网络。
|   **服务名称**    |   EapHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="function-discovery-provider-host"></a>功能发现提供程序主机服务         

| | |           
|---|---|       
|   **服务说明** |   FDPHOST 服务托管 Function Discovery (FD) 网络发现提供程序。 这些 FD 提供程序提供网络发现服务的简单服务发现协议 (SSDP) 和 Web 服务的发现 (WS-D) 协议。 使用 FD 时，停止或禁用 FDPHOST 服务将禁用这些协议的网络发现。 当此服务不可用时，网络服务使用 FD，并依赖于这些发现协议将找不到网络设备或资源。
|   **服务名称**    |   fdPHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="function-discovery-resource-publication"></a>功能发现资源发布服务      

| | |           
|---|---|       
|   **服务说明** |   发布该计算机以及附加到此计算机，因此它们可以通过网络发现的资源。  如果此服务已停止，将不再发布网络资源并将通过网络上的其他计算机不会发现它们。
|   **服务名称**    |   FDResPub
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="geolocation-service"></a>地理定位服务          

| | |           
|---|---|       
|   **服务说明** |   此服务监视系统的当前位置和管理地域隔离区 （具有关联的事件的地理位置）。  如果关闭此服务时，应用程序将无法使用或接收通知的地理位置或地域隔离区。
|   **服务名称**    |   lfsvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   禁用依赖于该服务; 的分隔线应用确定以禁用如果不依赖于它的应用
|||         

<br />          

##  <a name="group-policy-client"></a>组策略客户端     

| | |           
|---|---|       
|   **服务说明** |   该服务负责将应用配置的计算机和用户可通过组策略组件管理员设置。 如果禁用该服务，则将不会应用设置和应用程序和组件将不能通过组策略管理。 任何组件或依赖于组策略组件的应用程序无法正常运行，如果该服务被禁用。
|   **服务名称**    |   gpsvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          


## <a name="human-interface-device-service"></a>人机接口设备服务           

| | |           
|---|---|       
|   **服务说明** |   激活和维护键盘、 远程控制和其他多媒体设备上的热按钮的使用。 建议您将运行此服务。
|   **服务名称**    |   hidserv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="hv-host-service"></a>HV 主机服务     

| | |           
|---|---|   
|   **服务说明** |   提供的 HYPER-V 虚拟机监控程序来提供每个分区到主机操作系统的性能计数器的接口。
|   **服务名称**    |   HvHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   来宾 Vm 的性能增强器。 未使用的今天除显式填充 Vm，但将应用程序防护中使用
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
|   **注释**    |   请参阅 HvHost
|||         

<br />      

## <a name="hyper-v-guest-service-interface"></a>Hyper-V 来宾服务接口          

| | |           
|---|---|   
|   **服务说明** |   提供的 HYPER-V 主机与虚拟机内部运行的特定服务进行交互的接口。
|   **服务名称**    |   vmicguestinterface
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   请参阅 HvHost
|||         

<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Hyper-V 来宾关闭服务           

| | |           
|---|---|       
|   **服务说明** |   提供了一种机制来关闭物理计算机上的管理接口从此虚拟机的操作系统。
|   **服务名称**    |   vmicshutdown
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   请参阅 HvHost
|||         

<br />

## <a name="hyper-v-heartbeat-service"></a>Hyper-V 检测信号服务
| | |
|---|---|
|   **服务说明** |   通过定期报告检测信号监视此虚拟机的状态。 此服务可帮助识别正在运行的虚拟机已停止响应的。
|   **服务名称**    |   vmicheartbeat
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   请参阅 HvHost
|||

<br />          

## <a name="hyper-v-powershell-direct-service"></a>Hyper-V PowerShell Direct 服务            

| | |           
|---|---|       
|   **服务说明** |   提供了一种机制来管理使用 PowerShell，通过 VM 会话不使用虚拟网络的虚拟机。
|   **服务名称**    |   vmicvmsession
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   请参阅 HvHost
|||         

<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Hyper-V 远程桌面虚拟化服务            

| | |           
|---|---|       
|   **服务说明** |   为虚拟机和物理计算机上运行的操作系统之间的通信提供了平台。
|   **服务名称**    |   vmicrdv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   请参阅 HvHost
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
|   **注释**    |   请参阅 HvHost
|||         

<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Hyper-V 卷影复制请求程序         

| | |           
|---|---|           
|   **服务说明** |   协调使用卷影复制服务将应用程序和数据此虚拟机上从物理计算机上的操作系统所需的通信。
|   **服务名称**    |   vmicvss
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   请参阅 HvHost
|||         

<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>IKE 和 AuthIP IPsec 密钥模块          

| | |           
|---|---|       
|   **服务说明** |   IKEEXT 服务托管 Internet 密钥交换 (IKE) 和已验证 Internet 协议 (AuthIP) 密钥模块。 在 Internet 协议安全 (IPsec) 进行身份验证和密钥交换使用这些密钥的模块。 停止或禁用 IKEEXT 服务将禁用与对等计算机 IKE 和 AuthIP 密钥交换。 IPsec 通常配置为使用 IKE 或 AuthIP;因此，停止或禁用 IKEEXT 服务可能会导致在 IPsec 失败，并可能危及系统安全。 强烈建议您有 IKEEXT 服务正在运行。
|   **服务名称**    |   IKEEXT
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |    
|||         

<br />          

## <a name="interactive-services-detection"></a>交互服务检测           

| | |           
|---|---|   
|   **服务说明** |   允许用户输入的交互式服务对话框出现时创建交互式服务的路径访问用户的通知。 如果此服务已停止，通知新的交互式服务对话框将不再起作用，并且可能不会有权访问交互式服务对话。 如果禁用此服务，则通知以及对新的交互式服务对话框的访问将不再起作用。
|   **服务名称**    |   UI0Detect
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />  

## <a name="internet-connection-sharing-ics"></a>Internet 连接共享(ICS)            

| | |           
|---|---|           
|   **服务说明** |   提供网络地址转换、 寻址、 家庭或小型办公网络的名称解析和/或入侵防护服务。
|   **服务名称**    |   SharedAccess
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   所需的客户端使用 WiFi 热点，而且还在两端 Miracast 投影。 ICS 可能无法使用 GPO 设置，"禁止使用的 DNS 域网络上的 Internet 连接共享"
|||         

<br />          

## <a name="ip-helper"></a>IP 帮助程序            

| | |           
|---|---|       
|   **服务说明** |   提供使用 IPv6 转换技术 (6to4、 ISATAP、 端口代理和 Teredo) 和 IP-HTTPS 隧道连接。 如果此服务已停止，计算机将不具有这些技术提供了增强的连接性优势。
|   **服务名称**    |   iphlpsvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          


##  <a name="ipsec-policy-agent"></a>IPsec 策略代理      

| | |           
|---|---|       
|   **服务说明** |   Internet 协议安全 (IPsec) 支持网络级别的对等身份验证、 数据源身份验证、 数据完整性、 数据保密性 （加密） 和重播保护。  此服务强制实施通过 IP 安全策略管理单元或命令行工具"netsh ipsec"创建的 IPsec 策略。  如果停止此服务，你可能会遇到网络连接问题，如果您的策略要求连接使用 IPsec。  此外，此服务停止时的 Windows 防火墙远程管理不可用。
|   **服务名称**    |   PolicyAgent
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />

##  <a name="kdc-proxy-server-service-kps"></a>KDC 代理服务器服务 (KPS)      

| | |           
|---|---|       
|   **服务说明** |   KDC 代理服务器服务在服务器上运行 edge 代理 Kerberos 协议消息到企业网络上的域控制器。
|   **服务名称**    |   KPSSVC
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导    
|   **注释**    |   
|||         

<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>适用于分布式事务处理协调器的 KtmRm            

| | |           
|---|---|       
|   **服务说明** |   协调分布式事务处理协调器 (MSDTC) 和内核事务管理器 (KTM) 之间的事务。 如果不需要则建议此服务会保留已停止。 如果需要 MSDTC 和 KTM 将自动启动此服务。 如果禁用此服务，则使用内核资源管理器进行交互的任何 MSDTC 事务将失败并显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   KtmRm
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />

##  <a name="link-layer-topology-discovery-mapper"></a>链路层拓扑发现映射器        

| | |       
|---|---|       
|   **服务说明** |   创建网络映射，PC 和设备拓扑 （连接） 的信息，以及描述每个 PC 和设备的元数据组成。  如果禁用此服务，则在网络映射将不能正常工作。
|   **服务名称**    |   lltdsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   确定以禁用如果网络映射上没有依赖关系
|||         

<br />

## <a name="local-session-manager"></a>本地会话管理器                    

| | |                   
|---|---|   
|   **服务说明** |   管理本地用户会话的核心 Windows 服务。 停止或禁用此服务将导致系统不稳定。    
|   **服务名称**    |   LSM |
|   **安装**    |   始终安装    |
|   **StartType**   |   自动   |
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||                 

<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Microsoft (R) 诊断中心标准收集器         

| | |           
|---|---|           
|   **服务说明** |   管理本地用户会话的核心 Windows 服务。 停止或禁用此服务将导致系统不稳定。
|   **服务名称**    |   LSM
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />

## <a name="microsoft-account-sign-in-assistant"></a>Microsoft 帐户登录助手
| | |
|---|---|
|   **服务说明** |   使用户登录通过 Microsoft 帐户标识服务。 如果此服务已停止，用户不能登录到其 Microsoft 帐户的计算机上。
|   **服务名称**    |   wlidsvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   Microsoft 帐户是 Windows Server 上的不适用
|||

<br />          

##  <a name="microsoft-app-v-client"></a>Microsoft APP-V 客户端      

| | |           
|---|---|       
|   **服务说明** |   管理 APP-V 用户和虚拟应用程序
|   **服务名称**    |   AppVClient
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   
|||         

<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Microsoft iSCSI 发起程序服务            

| | |           
|---|---|       
|   **服务说明** |   管理 Internet SCSI (iSCSI) 会话从这台计算机传输到远程 iSCSI 目标设备。 如果此服务已停止，此计算机不能以登录或访问 iSCSI 目标。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   MSiSCSI
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   我们的诊断数据指示这客户端，以及服务器上使用。 禁用此并无好处。
|||         

<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           

| | |           
|---|---|   
|   **服务说明** |   提供用于对用户的关联的标识提供程序进行身份验证的加密密钥的进程隔离。 如果禁用此服务，则所有使用和管理这些密钥将不可用，其中包括计算机登录和单一登录对应用和网站。 此服务启动和停止自动。 建议不重新配置此服务。
|   **服务名称**    |   NgcSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   所需的 PIN/Hello 登录时，不支持在服务器
|||         

<br />          

## <a name="microsoft-passport-container"></a>Microsoft Passport 容器         

| | |           
|---|---|       
|   **服务说明** |   管理本地用户用于进行身份验证标识提供者，以及 TPM 虚拟智能卡的用户的标识键。 如果禁用此服务，本地用户标识密钥和 TPM 虚拟智能卡将无法访问。 建议不重新配置此服务。
|   **服务名称**    |   NgcCtnrSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Microsoft 软件卷影复制提供程序          

| | |           
|---|---|       
|   **服务说明** |   管理卷影复制服务所执行的基于软件的卷影副本。 如果此服务已停止，则不能管理基于软件的卷影副本。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   swprv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="microsoft-storage-spaces-smp"></a>Microsoft 存储空间 SMP         

| | |           
|---|---|       
|   **服务说明** |   Microsoft 存储空间管理提供程序的主机服务。 如果此服务已停止或禁用，不能管理存储空间。
|   **服务名称**    |   smphost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   存储管理 Api 失败，而无需此服务。 例如："Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage".
|||         

<br />          

## <a name="nettcp-port-sharing-service"></a>Net.Tcp 端口共享服务         

| | |           
|---|---|       
|   **服务说明** |   提供了功能共享 TCP 端口通过 net.tcp 协议。
|   **服务名称**    |   NetTcpPortSharing
|   **安装**    |   始终安装
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   
|||         

<br />          

## <a name="netlogon"></a>Netlogon         

| | |           
|---|---|           
|   **服务说明** |   维护此计算机与域控制器进行身份验证的用户和服务之间的安全通道。 如果此服务已停止，计算机可能不会验证用户和服务和域控制器无法注册 DNS 记录。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   Netlogon
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="network-connection-broker"></a>网络连接代理            

| | |           
|---|---|       
|   **服务说明** |   允许从 internet 接收通知的 Microsoft Store 应用的代理连接。
|   **服务名称**    |   NcbService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

##  <a name="network-connections"></a>网络连接         

| | |           
|---|---|   
|   **服务说明** |   管理网络和拨号连接的文件夹，您可以在其中查看本地网络和远程连接中的对象。
|   **服务名称**    |   Netman
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="network-connectivity-assistant"></a>网络连接助手      

| | |           
|---|---|       
|   **服务说明** |   提供了 DirectAccess 的 UI 组件的状态通知
|   **服务名称**    |   NcaSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />  

##  <a name="network-list-service"></a>网络列表服务        

| | |           
|---|---|   
|   **服务说明** |   标识的网络到的计算机已连接、 收集和存储这些网络的属性，并且这些属性更改时通知应用程序。
|   **服务名称**    |   netprofm
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="network-location-awareness"></a>网络位置感知           

| | |           
|---|---|       
|   **服务说明** |   收集和存储网络的配置信息并修改此信息时向程序发出通知。 如果此服务已停止，配置信息可能不可用。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   NlaSvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="network-setup-service"></a>网络安装程序服务       

| | |           
|---|---|       
|   **服务说明** |   网络安装程序服务管理的网络驱动程序安装并允许低级别的网络设置的配置。  如果此服务已停止，可能会取消正在进行中的任何驱动程序安装。
|   **服务名称**    |   NetSetupSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="network-store-interface-service"></a>网络存储界面服务      

| | |           
|---|---|   
|   **服务说明** |   此服务提供给用户模式下客户端网络通知 （例如添加/删除接口等）。 停止此服务将导致丢失网络连接。 如果禁用此服务，则显式依赖此服务的任何其他服务将无法启动。
|   **服务名称**    |   nsi
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="offline-files"></a>脱机文件            

| | |           
|---|---|       
|   **服务说明** |   服务执行脱机文件脱机文件缓存上的进行维护活动响应的用户登录和注销事件实现的公共 API、 内部和有趣的事件的调度感脱机文件中的活动和缓存状态的变化。
|   **服务名称**    |   CscService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   
|||         

<br />          

## <a name="optimize-drives"></a>优化驱动器          

| | |           
|---|---|   
|   **服务说明** |   可帮助更有效地运行通过优化存储驱动器上的文件的计算机。
|   **服务名称**    |   defragsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />

## <a name="performance-counter-dll-host"></a>性能计数器 DLL 主机         

| | |           
|---|---|       
|   **服务说明** |   使远程用户和 64 位进程来查询由 32 位 Dll 提供的性能计数器。 如果此服务已停止，只有本地用户和 32 位进程将可以向查询提供的 32 位 Dll 的性能计数器。
|   **服务名称**    |   PerfHost
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导    
|   **注释**    |   
|||         

<br />          

## <a name="performance-logs--alerts"></a>性能日志和警报            

| | |           
|---|---|   
|   **服务说明** |   性能日志和警报收集性能数据从本地或远程计算机根据预配置的计划参数，然后将数据写入到日志，或将触发警报。 如果此服务已停止，将不收集性能信息。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   pla
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="phone-service"></a>电话服务       

| | |           
|---|---|   
|   **服务说明** |   管理设备上的电话服务状态
|   **服务名称**    |   PhoneSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   使用的 VoIP 的新型应用程序
|||         

<br />          

##      <a name="plug-and-play"></a>Plug and Play       

| | |           
|---|---|   
|   **服务说明** |   使计算机能够识别和调整硬件更改很少或没有用户输入。 停止或禁用此服务将导致系统不稳定。
|   **服务名称**    |   PlugPlay
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="portable-device-enumerator-service"></a>便携设备枚举器服务           

| | |           
|---|---|       
|   **服务说明** |   强制实施可移动的大容量存储设备的组的策略。 使应用程序，如 Windows Media Player 和映像导入向导来传输和同步内容使用可移动的大容量存储设备。
|   **服务名称**    |   WPDBusEnum
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="power"></a>电源            

| | |           
|---|---|       
|   **服务说明** |   管理电源策略和电源策略通知传递。
|   **服务名称**    |   电源
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="print-spooler"></a>打印后台处理程序            

| | |           
|---|---|   
|   **服务说明** |   此服务后台处理打印作业，并处理与打印机之间的交互。  如果关闭此服务时，你将无法打印或查看您的打印机。
|   **服务名称**    |   后台处理程序
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   确定以禁用不打印服务器或 DC
|   **注释**    |   在域控制器上，安装 DC 角色将线程添加到负责执行打印修剪 – 从 Active Directory 中删除过时的打印队列对象的后台处理程序服务。  如果后台处理程序服务未在每个站点中至少一个 DC 上运行，AD 具有任何用于删除旧的队列不再存在。 https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         

<br />          

##  <a name="printer-extensions-and-notifications"></a>打印机扩展和通知        

| | |           
|---|---|       
|   **服务说明** |   此服务打开自定义打印机对话框，并处理来自远程打印服务器或打印机的通知。 如果关闭此服务时，你将无法看到打印机扩展或通知。
|   **服务名称**    |   PrintNotify
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用，如果不是打印服务器
|   **注释**    |   
|||         

<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>问题报告和解决方案支持控件面板     

| | |           
|---|---|   
|   **服务说明** |   此服务为查看、 发送和删除的问题报告和解决方案控制面板系统级别问题报告提供支持。
|   **服务名称**    |   wercplsupport
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="program-compatibility-assistant-service"></a>程序兼容性助手服务     

| | |           
|---|---|       
|   **服务说明** |   此服务提供支持的程序兼容性助手 (PCA)。  PCA 监视程序安装并在用户运行，并检测已知的兼容性问题。 如果此服务已停止，PCA 将不能正常工作。
|   **服务名称**    |   PcaSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

##  <a name="quality-windows-audio-video-experience"></a>高质量 Windows 音频视频体验      

| | |           
|---|---|   
|   **服务说明** |   优质 Windows 音频视频体验 (qWave) 是为音频视频 (AV) 流 IP 家庭网络上的应用程序的网络平台。 qWave 增强了 AV 流通过确保网络--服务质量 (QoS) AV 应用程序的性能和可靠性。 它提供了许可控制、 运行时间监视和实施、 应用程序反馈以及通信优先级机制。
|   **服务名称**    |   QWAVE
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   客户端的 QoS 服务
|||         

<br />          

##      <a name="radio-management-service"></a>单选管理服务        

| | |           
|---|---|   
|   **服务说明** |   单选管理和飞行模式的服务
|   **服务名称**    |   RmSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="remote-access-auto-connection-manager"></a>远程访问自动连接管理器            

| | |           
|---|---|   
|   **服务说明** |   每当程序引用的远程 DNS 或 NetBIOS 名称或地址，请创建与远程网络的连接。
|   **服务名称**    |   RasAuto
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="remote-access-connection-manager"></a>远程访问连接管理器         

| | |           
|---|---|   
|   **服务说明** |   从此计算机管理拨号和虚拟专用网络 (VPN) 连接到 Internet 或其他远程网络。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   RasMan
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="remote-desktop-configuration"></a>远程桌面配置         

| | |           
|---|---|   
|   **服务说明** |   远程桌面配置服务 (RDC) 负责所有远程桌面服务和远程桌面配置和相关的会话需要系统上下文中的维护活动。 其中包括每个会话的临时文件夹、 远程桌面主题和 RD 证书。
|   **服务名称**    |   SessionEnv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   
|||         

<br />          

## <a name="remote-desktop-services"></a>远程桌面服务          

| | |           
|---|---|   
|   **服务说明** |   允许用户以交互方式连接到远程计算机。 远程桌面和远程桌面会话主机服务器依赖于此服务。  若要阻止此计算机的远程使用，请清除系统属性控制面板项的远程选项卡上的复选框。
|   **服务名称**    |   TermService
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   
|||         

<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>远程桌面服务 UserMode 端口重定向程序        

| | |           
|---|---|   
|   **服务说明** |   允许将 RDP 连接打印机驱动器/端口重定向
|   **服务名称**    |   UmRdpService
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   在连接的服务器端上支持重定向。
|||         

<br />          

## <a name="remote-procedure-call-rpc"></a>远程过程调用 (RPC)          

| | |           
|---|---|   
|   **服务说明** |   RPCSS 服务是 COM 和 DCOM 服务器的服务控制管理器。 它适用于 COM 和 DCOM 服务器执行对象的激活请求、 对象导出程序解决方法和分布式的垃圾回收。 如果此服务停止或禁用，使用 COM 或 DCOM 应用程序将不能正常工作。 强烈建议您有 RPCSS 服务正在运行。
|   **服务名称**    |   RpcSs
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>远程过程调用 (RPC) 定位符             

| | |               
|---|---|   
|   **服务说明** |   在 Windows 2003 和 Windows 的早期版本中，远程过程调用 (RPC) 定位符服务管理 RPC 名称服务数据库。 在 Windows Vista 和更高版本的 Windows 中，此服务不提供任何功能，并且会出现应用程序兼容性。   |
|   **服务名称**    |   RpcLocator  |
|   **安装**    |   仅带有桌面体验    |
|   **StartType**   |   Manual  |
|   **建议**  | 不提供的指导   |
|   **注释**    |       |
|||             

<br />              

## <a name="remote-registry"></a>远程注册表          

| | |           
|---|---|   
|   **服务说明** |   使远程用户能够修改此计算机上的注册表设置。 如果此服务已停止，则可以仅通过此计算机上的用户修改注册表。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   RemoteRegistry
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **注释**    |   
|||         

<br />          

##  <a name="resultant-set-of-policy-provider"></a>策略提供程序的结果集            

| | |           
|---|---|       
|   **服务说明** |   提供处理请求，以模拟应用程序的目标用户或在各种情况下的计算机的组策略设置，并计算策略的结果集设置的网络服务。
|   **服务名称**    |   RSoPProv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |不提供的指导    
|   **注释**    |   
|||         

<br />          

## <a name="routing-and-remote-access"></a>路由和远程访问            

| | |           
|---|---|   
|   **服务说明** |   提供路由服务在本地区域和宽的区域中的企业网络环境。
|   **服务名称**    |   RemoteAccess
|   **安装**    |   始终安装
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   已禁用
|||         

<br />          

## <a name="rpc-endpoint-mapper"></a>RPC 终结点映射程序          

| | |           
|---|---|   
|   **服务说明** |   将 RPC 接口标识符解析为传输终结点。 如果此服务停止或禁用，使用远程过程调用 (RPC) 服务的程序将不能正常工作。
|   **服务名称**    |   RpcEptMapper
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="secondary-logon"></a>辅助登录     

| | |           
|---|---|       
|   **服务说明** |   启用启动替代凭据的过程。 如果此服务已停止，此类型的登录访问将不可用。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   seclogon
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>安全套接字隧道协议服务            

| | |               
|---|---|       
|   **服务说明** |   提供支持的安全套接字隧道协议 (SSTP) 连接到远程计算机使用 VPN。 如果禁用此服务，则用户不能使用 SSTP 来访问远程服务器。    |
|   **服务名称**    |   SstpSvc |
|   **安装**    |   始终安装    |
|   **StartType**   |   Manual  |
|   **建议**  |   不要禁用  |
|   **注释**    |   禁用分页符 RRAS   |
|||             

<br />              

## <a name="security-accounts-manager"></a>安全帐户管理器            

| | |           
|---|---|       
|   **服务说明** |   此服务的启动通知其他服务安全帐户管理器 (SAM) 已准备好接受请求。  禁用此服务将会阻止其他服务在 SAM 准备就绪后，无法通知系统中，这又可能导致无法正确启动这些服务。 不应禁用此服务。
|   **服务名称**    |   SamSs
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不要禁用
|   **注释**    |   
|||         

<br />          

## <a name="sensor-data-service"></a>传感器数据服务  

| | |           
|---|---|   
|   **服务说明** |   提供使用不同的传感器数据
|   **服务名称**    |   SensorDataService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />  

## <a name="sensor-monitoring-service"></a>传感器监视服务            

| | |           
|---|---|       
|   **服务说明** |   监视各种传感器以公开的数据和适应系统和用户状态。  如果此服务已停止或禁用，显示器亮度会不适应光照条件。 停止此服务可能会影响其他系统功能和功能。
|   **服务名称**    |   SensrSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br /><br/>
## <a name="sensor-servicebr--br------br---strongservice-descriptionstrong----a-service-for-sensors-that-manages-different-sensors39-functionality-manages-simple-device-orientation-sdo-and-history-for-sensors-loads-the-sdo-sensor-that-reports-device-orientation-changes--if-this-service-is-stopped-or-disabled-the-sdo-sensor-will-not-be-loaded-and-so-auto-rotation-will-not-occur-history-collection-from-sensors-will-also-be-stopped"></a>传感器服务<br/>| | |<br/>|---|---|<br/>|   <strong>服务说明</strong>|  用于管理不同的传感器的传感器的服务&#39;功能。 管理简单设备方向 (SDO) 和历史记录的传感器。 加载报告设备方向更改 SDO 传感器。  如果此服务停止或禁用，SDO 传感器将不会加载，因此自动轮换不会发生。 此外将停止从传感器的历史记录集合。
|   <strong>服务名称</strong>|  SensorService |  <strong>安装</strong>|  仅带有桌面体验 |  <strong>StartType</strong> |  手动 |  <strong>建议</strong>|  确定以禁用 |  <strong>注释</strong>    |<br/>|||<br/>
<br />          

## <a name="server"></a>Server           

| | |           
|---|---|   
|   **服务说明** |   支持文件、 打印和命名管道上为此计算机的网络共享。 如果此服务已停止，这些函数将不可用。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   LanmanServer
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **注释**    |   所需的远程管理、 IPC$ SMB 文件共享
|||         

<br />          

## <a name="shell-hardware-detection"></a>Shell 硬件检测             

| | |           
|---|---|       
|   **服务说明** |   提供了自动播放硬件事件的通知。
|   **服务名称**    |   ShellHWDetection
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="smart-card"></a>智能卡           

| | |           
|---|---|   
|   **服务说明** |   管理访问权限读取此计算机的智能卡。 如果此服务已停止，此计算机将无法读取智能卡。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   SCardSvr
|   **安装**    |   始终安装
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   
|||         

<br />          

## <a name="smart-card-device-enumeration-service"></a>智能卡设备枚举服务                    

| | |               
|---|---|       
|   **服务说明** |   创建的所有智能卡读卡器软件设备节点可以访问给定的会话。 如果禁用此服务，则 WinRT Api 不能枚举智能卡读卡器。   |
|   **服务名称**    |   ScDeviceEnum    |
|   **安装**    |   始终安装    |
|   **StartType**   |   Manual  |
|   **建议**  |   确定以禁用   |
|   **注释**    |   需要几乎专用于 WinRT 应用程序    |
|||             

<br />              

## <a name="smart-card-removal-policy"></a>智能卡移除策略        

| | |           
|---|---|       
|   **服务说明** |   允许系统配置为锁定在智能卡删除时的用户桌面。
|   **服务名称**    |   SCPolicySvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="snmp-trap"></a>SNMP 陷阱            

| | |           
|---|---|       
|   **服务说明** |   接收生成的本地或远程简单网络管理协议 (SNMP) 代理陷阱消息，并将消息转发到此计算机上运行的 SNMP 管理程序。 如果此服务已停止，在此计算机上基于 SNMP 的程序不会收到 SNMP 陷阱消息。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   SNMPTRAP
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="software-protection"></a>软件保护             

| | |           
|---|---|       
|   **服务说明** |   可以下载、 安装和 Windows 和 Windows 的数字许可证执行应用程序。 如果禁用该服务，则可能会在通知模式下运行的操作系统和许可的应用程序。 强烈建议您不要禁用软件保护服务。
|   **服务名称**    |   sppsvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="special-administration-console-helper"></a>特殊管理控制台帮助        

| | |           
|---|---|   
|   **服务说明** |   允许管理员远程访问命令提示符下使用紧急管理服务。
|   **服务名称**    |   sacsvr
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="spot-verifier"></a>错误校验程序            

| | |           
|---|---|   
|   **服务说明** |   验证潜在文件系统损坏。
|   **服务名称**    |   svsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="ssdp-discovery"></a>SSDP 发现           

| | |           
|---|---|   
|   **服务说明** |   发现网络的设备和服务使用 SSDP 发现协议，例如 UPnP 设备。 此外收录了 SSDP 设备和本地计算机上运行的服务。 如果此服务已停止，将不会发现基于 SSDP 的设备。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   SSDPSRV
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="state-repository-service"></a>状态存储库服务         

| | |           
|---|---|   
|   **服务说明** |   为应用程序模型提供所需的基础结构支持。
|   **服务名称**    |   StateRepository
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="still-image-acquisition-events"></a>静止图像采集事件

| | |           
|---|---|   
|   **服务说明** |   启动应用程序与仍图像采集事件相关联。
|   **服务名称**    |   WiaRpc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />  

## <a name="storage-service"></a>存储服务          

| | |           
|---|---|       
|   **服务说明** |   提供用于存储设置和外部存储扩展启用服务
|   **服务名称**    |   StorSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="storage-tiers-management"></a>存储层管理        

| | |           
|---|---|   
|   **服务说明** |   优化系统中所有分层的存储空间上的存储层中的数据的位置。
|   **服务名称**    |   TieringEngineService
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="superfetch"></a>Superfetch          

| | |           
|---|---|       
|   **服务说明** |   维护和提高一段时间内的系统性能。
|   **服务名称**    |   SysMain
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="sync-host"></a>同步主机            

| | |           
|---|---|       
|   **服务说明** |   此服务会同步邮件、 联系人、 日历和各种其他用户数据。 邮件和其他应用程序依赖于此功能将无法正常工作时不运行此服务。
|   **服务名称**    |   OneSyncSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  |   确定以禁用
|   **注释**    |   用户服务模板
|||         

<br />          

## <a name="system-event-notification-service"></a>系统事件通知服务            

| | |           
|---|---|       
|   **服务说明** |   监视系统事件，并通知这些事件的 COM + 事件系统的订阅服务器。
|   **服务名称**    |   SENS
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="system-events-broker"></a>系统事件代理             

| | |           
|---|---|       
|   **服务说明** |   协调的 WinRT 应用程序的后台工作的执行。 如果已停止或禁用此服务，可能仍不触发后台工作。
|   **服务名称**    |   SystemEventsBroker
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **注释**    |   其实真正其说明意味着仅出于 WinRT 应用程序的原因，则需要为任务计划程序、 代理基础结构服务和其他内部组件。
|||         

<br />          

## <a name="task-scheduler"></a>任务计划程序           

| | |           
|---|---|   
|   **服务说明** |   使用户能够配置和计划在此计算机上的自动化的任务。 该服务还承载多个 Windows 系统关键任务。 如果此服务停止或禁用，不会运行这些任务在其计划时间。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   计划
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="tcpip-netbios-helper"></a>TCP/IP NetBIOS 帮助程序            

| | |           
|---|---|   
|   **服务说明** |   上提供支持的 NetBIOS TCP/IP (NetBT) 服务和 NetBIOS 名称解析为客户端在网络上，使用户能够共享文件，因此打印，并且登录到网络上。 如果此服务已停止，则这些函数可能不可用。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   lmhosts
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="telephony"></a>Telephony           

| | |           
|---|---|       
|   **服务说明** |   提供的程序的控制电话服务设备的本地计算机上，并通过 LAN，同时还运行该服务的服务器上的电话服务 API (TAPI) 支持。
|   **服务名称**    |   TapiSrv
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   禁用分页符 RRAS
|||         

<br />          

## <a name="themes"></a>主题           

| | |           
|---|---|
|   **服务说明** |   提供了用户体验主题管理。
|   **服务名称**    |   主题
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **注释**    |   此服务处于禁用状态时，不能设置辅助功能主题
|||         

<br />  

## <a name="tile-data-model-server"></a>图块数据模型服务器           

| | |           
|---|---|   
|   **服务说明** |   磁贴的磁贴更新的服务器。
|   **服务名称**    |   tiledatamodelsvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **注释**    |   如果禁用此服务，启动菜单中断
|||         

<br />          

##  <a name="time-broker"></a>时间 Broker     

| | |           
|---|---|       
|   **服务说明** |   协调的 WinRT 应用程序的后台工作的执行。 如果已停止或禁用此服务，可能仍不触发后台工作。
|   **服务名称**    |   TimeBrokerSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   其实真正其说明意味着仅出于 WinRT 应用程序的原因，则需要为任务计划程序、 代理基础结构服务和其他内部组件。
|||         

<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>触摸键盘和手写面板服务         

| | |           
|---|---|   
|   **服务说明** |   可启用触摸键盘和手写面板的笔和墨迹功能
|   **服务名称**    |   TabletInputService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>更新 Windows Update 的业务流程协调程序服务           

| | |           
|---|---|       
|   **服务说明** |   管理 Windows 更新。 如果停止，你的设备不能下载并安装最新的更新。
|   **服务名称**    |   UsoSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   服务说明中 v1607; 缺少Windows 更新 （包括 WSUS） 依赖于此服务。
|||         

<br />          

## <a name="upnp-device-host"></a>UPnP 设备主机         

| | |           
|---|---|   
|   **服务说明** |   允许 UPnP 设备托管在此计算机上。 如果此服务已停止，任何托管的 UPnP 设备将停止运行，可以添加任何其他托管的设备。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   upnphost
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="user-access-logging-service"></a>用户访问日志记录服务          

| | |           
|---|---|   
|   **服务说明** |   此服务中的 IP 地址和用户名称，已安装的产品和本地服务器上的角色的形式记录唯一客户端访问请求。 此信息可以查询，通过 Powershell，管理员无需量化客户端脱机的客户端访问许可证 (CAL) 管理服务器软件的需求。 如果禁用该服务，则客户端请求将不会记录和将无法检索通过 Powershell 查询。 停止该服务不会影响查询的历史数据 （请参阅支持文档的步骤来删除历史数据）。 本地系统管理员必须咨询以确定所需的适当地授予许可; 的服务器软件的 Cal 数的自己，Windows Server 许可条款UAL 服务的使用和数据不会更改此义务。
|   **服务名称**    |   UALSVC
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="user-data-access"></a>用户数据访问        

| | |           
|---|---|   
|   **服务说明** |   提供对结构化的用户数据，包括联系信息、 日历、 邮件和其他内容的应用程序访问。 如果停止或禁用此服务，使用此数据的应用程序可能无法正常工作。
|   **服务名称**    |   UserDataSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   用户服务模板
|||         

<br />          

## <a name="user-data-storage"></a>用户数据存储            

| | |           
|---|---|       
|   **服务说明** |   处理结构化的用户数据，包括联系信息、 日历、 邮件和其他内容的存储。 如果停止或禁用此服务，使用此数据的应用程序可能无法正常工作。
|   **服务名称**    |   UnistoreSvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   用户服务模板
|||         

<br />          

## <a name="user-experience-virtualization-service"></a>用户体验虚拟化服务           

| | |           
|---|---|       
|   **服务说明** |   提供对应用程序和操作系统设置漫游支持
|   **服务名称**    |   UevAgentService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   
|||         

<br />          

##  <a name="user-manager"></a>用户管理器        

| | |           
|---|---|   
|   **服务说明** |   用户管理器提供多用户交互所需的运行时组件。  如果此服务已停止，某些应用程序可能无法正常运行。
|   **服务名称**    |   UserManager
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="user-profile-service"></a>用户配置文件服务         

| | |           
|---|---|   
|   **服务说明** |   此服务负责加载和卸载用户配置文件。 如果此服务已停止或禁用，用户将不再能够成功登录或注销、 应用程序可能会遇到问题用户的数据和组件注册以接收事件通知配置文件不会收到它们。
|   **服务名称**    |   ProfSvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="virtual-disk"></a>虚拟磁盘             

| | |           
|---|---|   
|   **服务说明** |   提供有关磁盘、 卷、 文件系统和存储阵列的管理服务。
|   **服务名称**    |   vds
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不提供的指导
|   **注释**    |   
|||         

<br />          

## <a name="volume-shadow-copy"></a>卷影复制           

| | |           
|---|---|   
|   **服务说明** |   管理并实现卷影副本用于备份和其他目的。 如果此服务已停止，卷影副本将备份不可用，并且备份可能会失败。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   VSS
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不提供的指导
|   **注释**    |   
|||         

<br />          

##  <a name="walletservice"></a>WalletService           

| | |           
|---|---|   
|   **服务说明** |   使用 wallet 的客户端的主机对象
|   **服务名称**    |   WalletService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="windows-audio"></a>Windows Audio            

| | |           
|---|---|       
|   **服务说明** |   管理基于 Windows 的程序的音频。  如果此服务已停止，音频设备和效果将不能正常工作。  如果禁用此服务，则显式依赖于它的任何服务将无法启动
|   **服务名称**    |   Audiosrv
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="windows-audio-endpoint-builder"></a>Windows 音频终结点生成器           

| | |           
|---|---|
|   **服务说明** |   管理 Windows 音频服务的音频设备。  如果此服务已停止，音频设备和效果将不能正常工作。  如果禁用此服务，则显式依赖于它的任何服务将无法启动
|   **服务名称**    |   AudioEndpointBuilder
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="windows-biometric-service"></a>Windows 生物识别服务            

| | |           
|---|---|   
|   **服务说明** |   Windows 生物识别服务使客户端应用程序能够捕获、 比较、 处理和存储生物识别数据但不直接访问任何生物识别硬件或样本。 该服务位于特权 SVCHOST 进程中。
|   **服务名称**    |   WbioSrvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="windows-camera-frame-server"></a>Windows 照相机帧服务器         

| | |           
|---|---|       
|   **服务说明** |   允许多个客户端从相机的设备访问视频帧。
|   **服务名称**    |   FrameServer
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="windows-connection-manager"></a>Windows 连接管理器           

| | |           
|---|---|   
|   **服务说明** |   使自动连接/断开决策基于 PC 当前可用的网络连接选项，使您能够管理基于组策略设置的网络连接。
|   **服务名称**    |   Wcmsvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-defender-network-inspection-service"></a>Windows Defender 网络检查服务          

| | |           
|---|---|       
|   **服务说明** |   可帮助抵御入侵企图面向网络协议中的已知和新发现漏洞
|   **服务名称**    |   WdNisSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导    
|   **注释**    |   
|||         

<br />          

## <a name="windows-defender-service"></a>Windows Defender 服务         

| | |           
|---|---|       
|   **服务说明** |   帮助保护用户免受恶意软件和其他可能不需要的软件
|   **服务名称**    |   WinDefend
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation-用户模式驱动程序框架           

| | |           
|---|---|   
|   **服务说明** |   创建和管理用户模式驱动程序进程。 无法停止此服务。
|   **服务名称**    |   wudfsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-encryption-provider-host-service"></a>Windows 加密提供程序主机服务     

| | |           
|---|---|   
|   **服务说明** |   Windows 加密提供程序主机服务中转站加密到进程需要评估和应用 EAS 策略，从第三方加密提供程序相关功能。 停止这种方式影响 EAS 合规性检查已建立的连接的邮件帐户
|   **服务名称**    |   WEPHOSTSVC
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-error-reporting-service"></a>Windows 错误报告服务          

| | |           
|---|---|       
|   **服务说明** |   允许程序停止运行或没有响应时报告的错误并允许传递现有的解决方案。 此外允许日志，以生成用于诊断和修复服务。 如果此服务已停止，错误报告可能无法正常工作并诊断服务和修复的结果可能不会显示。
|   **服务名称**    |   WerSvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   收集并发送由 MS 和第三方 Isv/Ihv 故障/挂起数据。 使用数据来诊断崩溃引入 bug，其中可能包括安全错误。 此外需要为公司错误报告
|||         

<br />          

## <a name="windows-event-collector"></a>Windows 事件收集器          

| | |           
|---|---|   
|   **服务说明** |   此服务支持 WS 管理协议的远程源从管理事件的持久订阅。 这包括 Windows Vista 事件日志、 硬件和已启用 IPMI 的事件源。 服务存储转发本地事件日志中的事件。 如果停止或禁用此服务无法创建事件订阅，并不能接受转发的事件。
|   **服务名称**    |   Wecsvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   为可管理性，诊断收集 ETW 事件 （包括安全事件）。  大量功能和第三方工具依赖于它，其中包括安全审核工具
|||         

<br />          

## <a name="windows-event-log"></a>Windows 事件日志            

| | |           
|---|---|       
|   **服务说明** |   此服务可管理事件和事件日志。 它支持日志记录事件，查询订阅事件、 存档事件日志和管理事件的元数据的事件。 XML 和纯文本格式，它可以显示事件。 停止此服务可能会危及安全性和可靠性的系统。
|   **服务名称**    |   EventLog
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-firewall"></a>Windows 防火墙         

| | |           
|---|---|   
|   **服务说明** |   Windows 防火墙可帮助保护您的计算机通过防止未经授权的用户获得对您的计算机通过 Internet 或网络访问权限。
|   **服务名称**    |   MpsSvc
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="windows-font-cache-service"></a>Windows 字体缓存服务      

| | |           
|---|---|   
|   **服务说明** |   通过缓存常用的字体数据来优化应用程序的性能。 如果未运行，应用程序将启动此服务。 可以禁用它，但这样做会降低应用程序的性能。
|   **服务名称**    |   FontCache
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-image-acquisition-wia"></a>Windows 图像采集 (WIA)          

| | |           
|---|---|   
|   **服务说明** |   提供为扫描仪和照相机图像采集
|   **服务名称**    |   stisvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

##  <a name="windows-insider-service"></a>Windows 预览体验服务     

| | |           
|---|---|   
|   **服务说明** |   wisvc
|   **服务名称**    |   wisvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   服务器不支持外部测试，因此它是服务器上一个无操作。 可以通过 GP 也禁用功能。
|||         

<br />          

##  <a name="windows-installer"></a>Windows Installer       

| | |           
|---|---|
|   **服务说明** |   添加、 修改和删除应用程序作为 Windows Installer （*.msi，*.msp） 程序包提供。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   msiserver
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-license-manager-service"></a>Windows 许可证管理器服务          

| | |           
|---|---|   
|   **服务说明** |   提供对 Microsoft Store 的基础结构支持。  此服务按需启动，并且如果禁用然后通过 Microsoft Store 获取的内容会功能失常。
|   **服务名称**    |   LicenseManager
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-management-instrumentation"></a>Windows Management Instrumentation       

| | |           
|---|---|       
|   **服务说明** |   提供通用接口和对象模型来访问有关操作系统、 设备、 应用程序和服务的管理信息。 如果此服务已停止，大多数基于 Windows 的软件将不能正常工作。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   Winmgmt
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

##  <a name="windows-mobile-hotspot-service"></a>Windows 移动热点服务          

| | |           
|---|---|       
|   **服务说明** |   提供的功能与另一台设备共享的移动电话网络数据连接。
|   **服务名称**    |   icssvc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   
|||         

<br />          

## <a name="windows-modules-installer"></a>Windows 模块安装程序        

| | |           
|---|---|   
|   **服务说明** |   启用安装、 修改和删除 Windows 更新和可选组件。 如果禁用此服务，则安装或卸载的 Windows 更新此计算机可能会失败。
|   **服务名称**    |   TrustedInstaller
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="windows-push-notifications-system-service"></a>Windows 推送通知系统服务            

| | |           
|---|---|
|   **服务说明** |   此服务在会话 0 中运行，并托管用于处理设备和 WNS 服务器之间的连接的通知平台和连接提供程序。
|   **服务名称**    |   WpnService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   自动
|   **建议**  |   确定以禁用
|   **注释**    |   所需的动态磁贴和其他功能
|||         

<br />      

## <a name="windows-push-notifications-user-service"></a>Windows 推送通知用户服务          

| | |           
|---|---|   
|   **服务说明** |   此服务承载 Windows 通知平台提供支持本地和推送通知。 受支持的通知是磁贴、 toast 和原始。
|   **服务名称**    |   WpnUserService
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   确定以禁用
|   **注释**    |   用户服务模板
|||         

<br />

## <a name="windows-remote-management-ws-management"></a>Windows 远程管理 (Ws-management)
| | |
|---|---|
|   **服务说明** |   Windows 远程管理 (WinRM) 服务为远程管理实施 WS-Management 协议。 WS-Management 是一个用于远程软件和硬件管理的标准 Web 服务协议。 WinRM 服务为 WS-Management 请求监听网络并处理它们。 WinRM 服务需要使用 winrm.cmd 命令行工具或通过组策略配置侦听器，以在网络上进行监听。 WinRM 服务提供对 WMI 数据的访问并启用事件收集。 事件收集和事件订阅要求服务正在运行。 WinRM 消息使用 HTTP 和 HTTPS 作为传输器。 WinRM 服务不依赖于 IIS 但预配置与同一台机器上的 IIS 共享一个端口。  WinRM 服务保留 /wsman URL 前缀。 为了防止与 IIS 冲突，管理员应确保 IIS 上托管的任何网站不使用 /wsman URL 前缀。
|   **服务名称**    |   WinRM
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  |   不要禁用
|   **注释**    |   所需的远程管理
|||

<br />          

##  <a name="windows-search"></a>Windows 搜索      

| | |           
|---|---|       
|   **服务说明** |   有关文件、 电子邮件和其他内容提供建立内容索引、 属性缓存和搜索结果。
|   **服务名称**    |   WSearch
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Disabled
|   **建议**  |   已禁用
|   **注释**    |   
|||         

<br />          

##  <a name="windows-time"></a>Windows 时间        

| | |           
|---|---|   
|   **服务说明** |   维护网络中的服务器和所有客户端上的日期和时间同步。 如果此服务已停止，日期和时间同步将不可用。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   W32Time
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导
|   **注释**    |   
|||         

<br />          

## <a name="windows-update"></a>Windows 更新           

| | |           
|---|---|       
|   **服务说明** |   启用检测、 下载和安装 Windows 和其他程序的更新。 如果禁用此服务，则此计算机的用户将不能使用 Windows Update 或其自动更新功能，并且程序将不能使用 Windows 更新代理 (WUA) API。
|   **服务名称**    |   wuauserv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>WinHTTP Web 代理自动发现服务         

| | |           
|---|---|   
|   **服务说明** |   WinHTTP 实现客户端 HTTP 堆栈，并使用 Win32 API 和 COM 自动化组件的开发人员提供用于发送 HTTP 请求和接收响应。 此外，WinHTTP 提供支持，用于自动发现其实现的 Web 代理自动发现 (WPAD) 协议通过代理配置。
|   **服务名称**    |   WinHttpAutoProxySvc
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  |   不要禁用
|   **注释**    |   任何使用网络堆栈，可以对此服务具有函数依赖关系。 许多组织依赖于此程序以配置其内部网络的 HTTP 代理路由。  如果没有它，在内部发起与 Internet 的 HTTP 连接将所有失败。
|||         

<br />          

## <a name="wired-autoconfig"></a>有线自动配置         

| | |           
|---|---|       
|   **服务说明** |   有线自动配置 (DOT3SVC) 服务负责执行 IEEE 802.1x 以太网接口上的身份验证。 如果你当前的有线的网络部署强制实施 802.1 X 身份验证，则应配置 DOT3SVC 服务来运行，以建立第 2 层连接性和/或提供访问的网络资源。 不强制执行 802.1x 身份验证的有线的网络不受 DOT3SVC 服务。
|   **服务名称**    |   dot3svc
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导   
|   **注释**    |   
|||         

<br />          

## <a name="wmi-performance-adapter"></a>WMI 性能适配器          

| | |           
|---|---|   
|   **服务说明** |   提供从 Windows Management Instrumentation (WMI) 提供程序到网络上的客户端性能库信息。 此服务仅在性能数据助手被激活时运行。
|   **服务名称**    |   wmiApSrv
|   **安装**    |   始终安装
|   **StartType**   |   Manual
|   **建议**  | 不提供的指导       
|   **注释**    |   
|||         

<br />          

## <a name="workstation"></a>工作站          

| | |           
|---|---|   
|   **服务说明** |   创建并维护客户端网络连接到使用 SMB 协议的远程服务器。 如果此服务已停止，这些连接将不可用。 如果禁用此服务，则显式依赖于它的任何服务将无法启动。
|   **服务名称**    |   LanmanWorkstation
|   **安装**    |   始终安装
|   **StartType**   |   自动
|   **建议**  | 不提供的指导       
|   **注释**    |   
|||         

<br />

## <a name="xbox-live-auth-manager"></a>Xbox Live 的身份验证管理器           

| | |           
|---|---|   
|   **服务说明** |   提供用于与 Xbox Live 进行交互的身份验证和授权服务。 如果此服务已停止，某些应用程序可能无法正常运行。
|   **服务名称**    |   XblAuthManager
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   应禁用
|   **注释**    |   
|||         

<br />          

## <a name="xbox-live-game-save"></a>Xbox Live 游戏保存          

| | |           
|---|---|   
|   **服务说明** |   此服务同步的 Xbox Live 中保存数据，保存已启用的游戏。  如果此服务已停止，将数据保存的游戏将不将上传到或从 Xbox Live 下载。
|   **服务名称**    |   XblGameSave
|   **安装**    |   仅带有桌面体验
|   **StartType**   |   Manual
|   **建议**  |   应禁用
|   **注释**    |   此服务同步的 Xbox Live 中保存数据，保存已启用的游戏。  如果此服务已停止，将数据保存的游戏将不将上传到或从 Xbox Live 下载。
|||         

<br /> 
<br /> 

