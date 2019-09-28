---
title: Windows Server-Server Core 中包含的角色、角色服务和功能
description: Windows Server 的服务器核心安装选项中包括哪些角色和功能？
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 7b5d5d5ad38b1b03e409c26485860f43799f1322
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383328"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Windows Server-Server Core 中包含的角色、角色服务和功能

> 适用于：Windows Server 2019、Windows Server 2016 和 Windows Server （半年频道）

我们通常会探讨[*不*在服务器核心中的内容](server-core-removed-roles.md)，现在我们将尝试一种不同的方法，并告诉您*包含*哪些内容以及*默认情况下是否安装*了某些内容。 以下角色、角色服务和功能在 Windows Server 的服务器核心安装选项*中*。 使用此信息来帮助确定服务器核心选项是否适用于您的环境。 由于这是一个大型列表，因此请考虑搜索你感兴趣的特定角色或功能-如果该搜索不返回你要查找的内容，则它不会包含在服务器核心中。

例如，如果搜索 "远程桌面会话主机"，则在此页上找不到它。 这是因为服务器核心映像中不包含 RD 会话主机。

请记住，您[始终可以查看](server-core-removed-roles.md)*不*包含的内容。 这只是一种不同的查看方式。

## <a name="roles-included-in-server-core"></a>服务器核心中包含的角色
服务器核心安装选项包括以下服务器角色。

| Role                                            | 名称                           | 是否默认安装？ |
|-------------------------------------------------|--------------------------------|-----------------------|
| Active Directory 证书服务           | AD-证书                 | N                     |
| Active Directory 域服务                | AD-域服务             | N                     |
| Active Directory 联合身份验证服务            | ADFS-联合身份验证                | N                     |
| Active Directory 轻型目录服务 | ADLDS                          | N                     |
| Active Directory 权限管理服务     | ADRMS                          | N                     |
| 设备运行状况证明                       | DeviceHealthAttestationService | N                     |
| DHCP 服务器                                     | DHCP                           | N                     |
| DNS 服务器                                      | DNS                            | N                     |
| 文件和存储服务                       | FileAndStorage-服务        | Y                     |
| 主机保护者服务                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| 打印和文档服务                     | 打印-服务                 | N                     |
| 远程访问                                   | RemoteAccess                   | N                     |
| 远程桌面服务                         | 远程桌面服务        | N                     |
| 批量激活服务                      | VolumeActivation               | N                     |
| Web 服务器 IIS                                  | Web 服务器                     | N                     |
| Windows Server Essentials 体验            | ServerEssentialsRole           | N                     |
| Windows Server 更新服务                  | Updateservices-api                 | N                     |

## <a name="role-services-included-in-server-core"></a>服务器核心中包含的角色服务
服务器核心安装选项包括以下角色服务。

| Role                                  | 角色服务                                                   | 名称                    | 是否默认安装？ |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Active Directory 证书服务 | 证书颁发机构                                        | ADCS 证书颁发机构     | N                     |
|                                       | 证书注册策略 Web 服务                      | ADCS-Registry.pol     | N                     |
|                                       | 证书注册 Web 服务                             | ADCS-     | N                     |
|                                       | 证书颁发机构 Web 注册                         | ADCS-Web 注册     | N                     |
|                                       | 网络设备注册服务                              | ADCS-设备注册  | N                     |
|                                       | 联机响应程序                                               | ADCS-Cert        | N                     |
| Active Directory Rights Management    | Active Directory 权限管理服务器                      | ADRMS-服务器            | N                     |
|                                       | 联合身份验证支持                                    | ADRMS-标识          | N                     |
| 文件和存储服务             | 文件和 iSCSI 服务                                        | 文件-服务           | N                     |
|                                       | 文件服务器                                                    | FS-程序           | N                     |
|                                       | 网络文件的 BranchCache                                  | FS-BranchCache          | N                     |
|                                       | 重复数据删除                                             | FS-数据删除   | N                     |
|                                       | DFS 命名空间                                                 | FS-DFS 命名空间        | N                     |
|                                       | DFS 复制                                                | FS-DFS 复制      | N                     |
|                                       | 文件服务器资源管理器                                   | FS-资源管理器     | N                     |
|                                       | 文件服务器 VSS 代理服务                                  | FS-VSS-代理            | N                     |
|                                       | iSCSI 目标服务器                                            | iSCSITarget-服务器      | N                     |
|                                       | iSCSI 目标存储提供程序（VDS 和 VSS 硬件提供程序） | iSCSITarget-VSS-VDS     | N                     |
|                                       | NFS 服务器                                                 | FS-NFS-服务          | N                     |
|                                       | 工作文件夹                                                   | FS-SyncShareService     | N                     |
|                                       | 存储服务                                               | 存储服务        | Y                     |
| 打印和文档服务           | 打印服务器                                                   | 打印-服务器            | N                     |
|                                       | LPD 服务                                                    | Print-LPD-服务       | N                     |
| 远程访问                         | DirectAccess 和 VPN （RAS）                                     | DirectAccess-VPN        | N                     |
|                                       | 路由                                                        | 路由                 | N                     |
|                                       | Web 应用程序代理                                          | Web 应用程序-代理   | N                     |
| 远程桌面服务               | 远程桌面连接代理                               | RDS-连接-Broker   | N                     |
|                                       | 远程桌面授权                                       | RDS-许可           | N                     |
|                                       | 远程桌面虚拟化主机                             | RDS-虚拟化      | N                     |
| Web 服务器 (IIS)                      | Web 服务器                                                     | Web-web 服务器           | N                     |
|                                       | 常用 HTTP 功能                                           | Web 公共-Http         | N                     |
|                                       | 默认文档                                               | Web-默认-文档         | N                     |
|                                       | 目录浏览                                             | Web 目录浏览        | N                     |
|                                       | HTTP 错误                                                    | Web Http-错误         | N                     |
|                                       | 静态内容                                                 | Web 静态内容      | N                     |
|                                       | HTTP 重定向                                               | Web Http-重定向       | N                     |
|                                       | WebDAV 发布                                              | Web-DAV-发布      | N                     |
|                                       | 运行状况和诊断                                         | Web 运行状况              | N                     |
|                                       | HTTP 日志记录                                                   | Web Http-日志记录        | N                     |
|                                       | 自定义日志记录                                                 | Web-自定义-日志记录      | N                     |
|                                       | 日志记录工具                                                  | Web 日志库       | N                     |
|                                       | ODBC 日志记录                                                   | Web ODBC 日志记录        | N                     |
|                                       | 请求监视器                                              | Web 请求-监视器     | N                     |
|                                       | 跟踪                                                        | Web Http 跟踪        | N                     |
|                                       | 性能                                                    | Web 性能         | N                     |
|                                       | 静态内容压缩                                     | Web Stat-压缩    | N                     |
|                                       | 动态内容压缩                                    | Web-Dyn-压缩     | N                     |
|                                       | 安全性                                                       | Web 安全            | N                     |
|                                       | 请求筛选                                              | Web 筛选           | N                     |
|                                       | 基本身份验证                                           | Web-基本-身份验证          | N                     |
|                                       | 集中式 SSL 证书支持                            | Web-CertProvider        | N                     |
|                                       | 客户端证书映射身份验证                      | Web 客户端-身份验证         | N                     |
|                                       | 摘要式身份验证                                          | Web 摘要式身份验证         | N                     |
|                                       | IIS 客户端证书映射身份验证                  | Web 证书-身份验证           | N                     |
|                                       | IP 和域限制                                     | Web IP-安全性         | N                     |
|                                       | URL 授权                                              | Web Url-身份验证            | N                     |
|                                       | Windows 身份验证                                         | Web-Windows 身份验证        | N                     |
|                                       | 应用程序开发                                        | Web 应用-开发             | N                     |
|                                       | .NET 扩展性3。5                                         | Web-Net-Ext             | N                     |
|                                       | .NET 扩展性4。6                                         | 网络-Ext45           | N                     |
|                                       | 应用程序初始化                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web ASP                 | N                     |
|                                       | ASP.NET 3。5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web CGI                 | N                     |
|                                       | ISAPI 扩展                                               | Web ISAPI-Ext           | N                     |
|                                       | ISAPI 筛选器                                                  | Web ISAPI-筛选器        | N                     |
|                                       | 服务器端包含                                           | Web-包括            | N                     |
|                                       | WebSocket 协议                                             | Web-Websocket          | N                     |
|                                       | FTP 服务器                                                     | Web Ftp 服务器          | N                     |
|                                       | FTP 服务                                                    | Web Ftp-服务         | N                     |
|                                       | FTP 扩展性                                              | Web Ftp-Ext             | N                     |
|                                       | 管理工具                                               | Web 管理工具          | N                     |
|                                       | IIS 6 管理兼容性                                 | Web 管理-兼容         | N                     |
|                                       | IIS 6 元数据库兼容性                                   | Web-元数据库            | N                     |
|                                       | IIS 6 脚本工具                                          | Web-Lgcy-脚本      | N                     |
|                                       | IIS 6 WMI 兼容性                                        | Web WMI                 | N                     |
|                                       | IIS 管理脚本和工具                               | Web 脚本-工具     | N                     |
|                                       | 管理服务                                             | Web 管理-服务        | N                     |
| Windows Server 更新服务        | WID 连接                                               | Updateservices-api-WidDB    | N                     |
|                                       | WSUS 服务                                                  | Updateservices-api-服务 | N                     |
|                                       | SQL Server 连接                                        | Updateservices-api-DB       | N                     |

## <a name="features-included-in-server-core"></a>服务器核心中包含的功能
服务器核心安装选项包括以下功能。

| 功能                                                | 名称                               | 是否默认安装？ |
|--------------------------------------------------------|------------------------------------|-----------------------|
| .NET Framework 3.5 功能                            | 网络框架-功能             | N                     |
| .NET Framework 3.5 （包括 .NET 2.0 和3.0）       | NET-核心                 | 删除             |
| HTTP 激活                                        | NET-HTTP-激活                | N                     |
| 非 HTTP 激活                                    | NET-非 HTTP Activ                 | N                     |
| .NET Framework 4.6 功能                            | NET-45-功能          | Y                     |
| .NET Framework 4.6                                     | NET-45-核心              | Y                     |
| ASP.NET 4.6                                            | NET Framework-45-ASPNET            | N                     |
| WCF 服务                                           | NET-WCF-Services45                 | Y                     |
| HTTP 激活                                        | NET-WCF-HTTP-Activation45          | N                     |
| 消息队列（MSMQ）激活                      | NET-WCF-Activation45          | N                     |
| 命名管道激活                                  | NET-Activation45          | N                     |
| TCP 激活                                         | NET-Activation45           | N                     |
| TCP 端口共享                                       | NET-PortSharing45          | Y                     |
| 后台智能传输服务 (BITS)         | BITS                               | N                     |
| Compact 服务器                                         | BITS-压缩服务器                | N                     |
| BitLocker 驱动器加密                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| NFS 客户端                                         | NFS-客户端                         | N                     |
| 容器                                             | 容器                         | N                     |
| 数据中心桥接                                   | 数据中心-桥接               | N                     |
| 增强存储                                       | EnhancedStorage                    | N                     |
| 故障转移群集                                    | 故障转移-群集                | N                     |
| 组策略管理                                | GPMC                               | N                     |
| I/O 服务质量                                 | DiskIo-QoS                         | N                     |
| IIS 可承载 Web 核心                                  | Web-WHC                            | N                     |
| IP 地址管理 (IPAM) 服务器                    | IPAM                               | N                     |
| iSNS 服务器服务                                    | ISNS                               | N                     |
| 管理 OData IIS 扩展                         | ManagementOdata                    | N                     |
| 媒体基础                                       | 服务器-介质-基础            | N                     |
| 消息队列                                        | MSMQ                               | N                     |
| 消息队列服务                               | MSMQ 服务                      | N                     |
| 消息队列服务器                                 | MSMQ 服务器                        | N                     |
| 目录服务集成                          | MSMQ-目录                     | N                     |
| HTTP 支持                                           | MSMQ-HTTP-支持                  | N                     |
| 消息队列触发器                               | MSMQ 触发器                      | N                     |
| 路由服务                                        | MSMQ 路由                       | N                     |
| 消息队列 DCOM 代理                             | MSMQ-DCOM                          | N                     |
| 多路径 I/O                                          | 多路径-IO                       | N                     |
| 多点连接器                                   | MultiPoint-连接器               | N                     |
| MultiPoint 连接器服务                          | MultiPoint-连接器-服务      | N                     |
| MultiPoint 管理器和 MultiPoint 仪表板            | MultiPoint-工具                   | N                     |
| 网络负载平衡                                 | NLB                                | N                     |
| 对等名称解析协议                          | PNRP                               | N                     |
| 高质量 Windows 音频视频体验                 | qWave                              | N                     |
| 远程差分压缩                        | RECEIVED                                | N                     |
| 远程服务器管理工具                     | RSAT                               | N                     |
| 功能管理工具                           | RSAT-功能-工具                 | N                     |
| BitLocker 驱动器加密管理实用工具  | RSAT-功能-工具-BitLocker       | N                     |
| DataCenterBridging LLDP 工具                          | RSAT-DataCenterBridging-工具 | N                     |
| 故障转移群集工具                              | RSAT-群集                    | N                     |
| Windows PowerShell 的故障转移群集模块         | RSAT-Clustering-PowerShell         | N                     |
| 故障转移群集自动化服务器                     | RSAT-群集-AutomationServer   | N                     |
| 故障转移群集命令界面                     | RSAT-群集-Failovercluster-cmdinterface       | N                     |
| IP 地址管理（IPAM）客户端                    | IPAM-客户端-功能                | N                     |
| 受防护的 VM 工具                                      | RSAT-受防护-VM-工具             | N                     |
| 适用于 Windows PowerShell 的存储副本模块          | RSAT-存储-副本               | N                     |
| 角色管理工具                              | RSAT 角色-工具                    | N                     |
| AD DS 和 AD LDS 工具                                 | RSAT-AD-工具                      | N                     |
| Windows PowerShell 的 Active Directory 模块         | RSAT-AD-PowerShell                 | N                     |
| AD DS 工具                                            | RSAT-添加                          | N                     |
| Active Directory 管理中心                 | RSAT-AD-AdminCenter                | N                     |
| AD DS 管理单元和命令行工具                  | RSAT-添加-工具                    | N                     |
| AD LDS 管理单元和命令行工具                 | RSAT-ADLDS                         | N                     |
| HYPER-V 管理工具                               | RSAT-Hyper-v-工具                 | N                     |
| Windows PowerShell 的 Hyper-V 模块                  | Hyper-V-PowerShell                 | N                     |
| Windows Server Update Services 工具                   | Updateservices-api-RSAT                | N                     |
| API 和 PowerShell cmdlet                             | Updateservices-api-API                 | N                     |
| DHCP 服务器工具                                      | RSAT-DHCP                          | N                     |
| DNS 服务器工具                                       | RSAT-DNS 服务器                    | N                     |
| 远程访问管理工具                         | RSAT-RemoteAccess                  | N                     |
| Windows PowerShell 的远程访问模块            | RSAT-RemoteAccess-PowerShell       | N                     |
| HTTP 代理上的 RPC                                    | RPC over HTTP 代理                | N                     |
| 安装和启动事件收集                        | 设置并引导-事件收集    | N                     |
| 简单 TCP/IP 服务                                 | 简单-TCPIP                       | N                     |
| SMB 1.0/CIFS 文件共享支持                      | FS-SMB1                            | Y                     |
| SMB 带宽限制                                    | FS-SMBBW                           | N                     |
| SNMP 服务                                           | SNMP 服务                       | N                     |
| SNMP WMI 提供程序                                      | SNMP-WMI 提供程序                  | N                     |
| Telnet 客户端                                          | Telnet 客户端                      | N                     |
| 用于结构管理的 VM 防护工具               | FabricShieldedTools                | N                     |
| Windows Defender 功能                              | Windows Defender-功能          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Windows 内部数据库                              | Windows-内部-数据库          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5。1                                 | PowerShell                         | Y                     |
| Windows PowerShell 2.0 引擎                          | PowerShell-V2                      | 删除             |
| Windows PowerShell Desired State Configuration 服务 | DSC-服务                        | N                     |
| Windows PowerShell Web 访问                          | WindowsPowerShellWebAccess         | N                     |
| Windows Process Activation Service                     | 尝试                                | N                     |
| 进程模型                                          | WAS 进程模型                  | N                     |
| .NET 环境3。5                                   | WAS-网络环境                | N                     |
| 配置 API                                     | WAS-Config-Api                    | N                     |
| Windows Server 备份                                  | Windows-服务器-备份              | N                     |
| Windows Server 迁移工具                         | 迁移                          | N                     |
| 基于 Windows 标准的存储管理             | WindowsStorageManagementService    | N                     |
| WinRM IIS 扩展                                    | WinRM-IIS-Ext                      | N                     |
| WINS 服务器                                            | WINS                               | N                     |
| WoW64 支持                                          | WoW64-支持                      | Y                     |
|                                                        |                                    |                       |
