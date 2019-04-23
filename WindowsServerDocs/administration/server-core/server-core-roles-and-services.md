---
title: 角色、 角色服务和 Windows Server 的服务器核心中包含的功能
description: Windows Server 的服务器核心安装选项中包含哪些角色和功能？
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859288"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>角色、 角色服务和 Windows Server 的服务器核心中包含的功能

> 适用于：Windows Server （半年频道） 和 Windows Server 2016

我们通常谈论[内容的*不*在 Server Core](server-core-removed-roles.md) -现在，我们将尝试不同的方法并告诉您的是的*包含*以及内容是*默认情况下安装*。 以下角色、 角色服务和功能*在*的 Windows Server 的服务器核心安装选项。 使用此信息来帮助找出如果服务器核心选项适用于你的环境。 由于这是一个大型列表，请考虑搜索特定角色或功能感兴趣-如果搜索未返回所需的它不包含在服务器核心。

例如，如果搜索"远程桌面会话主机"-你在此页上不会找到它。 这是因为 RD 会话主机未包含在 Server Core 映像。

请记住，你可以[总是看上去](server-core-removed-roles.md)什么的*不*包含。 这是只需以不同的方式来查看某些内容。

## <a name="roles-included-in-server-core"></a>服务器核心中所含的角色
服务器核心安装选项包括以下服务器角色。

| 角色                                            | 名称                           | 默认情况下安装？ |
|-------------------------------------------------|--------------------------------|-----------------------|
| Active Directory 证书服务           | AD-Certificate                 | N                     |
| Active Directory 域服务                | AD-Domain-Services             | N                     |
| Active Directory 联合身份验证服务            | ADFS-Federation                | N                     |
| Active Directory 轻型目录服务 | ADLDS                          | N                     |
| Active Directory 权限管理服务     | ADRMS                          | N                     |
| 设备运行状况证明                       | DeviceHealthAttestationService | N                     |
| DHCP 服务器                                     | DHCP                           | N                     |
| DNS 服务器                                      | DNS                            | N                     |
| 文件和存储服务                       | FileAndStorage-Services        | Y                     |
| 主机保护者服务                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| 打印和文档服务                     | Print-Services                 | N                     |
| 远程访问                                   | RemoteAccess                   | N                     |
| 远程桌面服务                         | Remote-Desktop-Services        | N                     |
| 批量激活服务                      | VolumeActivation               | N                     |
| Web 服务器 IIS                                  | Web-Server                     | N                     |
| Windows Server Essentials 体验            | ServerEssentialsRole           | N                     |
| Windows Server 更新服务                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>包括在服务器核心中的角色服务
服务器核心安装选项包括以下角色服务。

| 角色                                  | 角色服务                                                   | 名称                    | 默认情况下安装？ |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Active Directory 证书服务 | 证书颁发机构                                        | ADCS-Cert-Authority     | N                     |
|                                       | 证书注册策略 Web 服务                      | ADCS-Enroll-Web-Pol     | N                     |
|                                       | 证书注册 Web 服务                             | ADCS-Enroll-Web-Svc     | N                     |
|                                       | 证书颁发机构 Web 注册                         | ADCS-Web-Enrollment     | N                     |
|                                       | 网络设备注册服务                              | ADCS-Device-Enrollment  | N                     |
|                                       | 联机响应程序                                               | ADCS-Online-Cert        | N                     |
| Active Directory 权限管理    | Active Directory 权限管理服务器                      | ADRMS-Server            | N                     |
|                                       | 联合身份验证支持                                    | ADRMS-Identity          | N                     |
| 文件和存储服务             | 文件和 iSCSI 服务                                        | File-Services           | N                     |
|                                       | 文件服务器                                                    | FS-FileServer           | N                     |
|                                       | 网络文件的 BranchCache                                  | FS-BranchCache          | N                     |
|                                       | 重复数据删除                                             | FS-Data-Deduplication   | N                     |
|                                       | DFS 命名空间                                                 | FS-DFS-Namespace        | N                     |
|                                       | DFS 复制                                                | FS-DFS-Replication      | N                     |
|                                       | 文件服务器资源管理器                                   | FS-Resource-Manager     | N                     |
|                                       | 文件服务器 VSS 代理服务                                  | FS-VSS-Agent            | N                     |
|                                       | iSCSI 目标服务器                                            | iSCSITarget-Server      | N                     |
|                                       | iSCSI 目标存储提供程序 （VDS 和 VSS 硬件提供程序） | iSCSITarget-VSS-VDS     | N                     |
|                                       | NFS 服务器                                                 | FS-NFS-Service          | N                     |
|                                       | 工作文件夹                                                   | FS-SyncShareService     | N                     |
|                                       | 存储服务                                               | Storage-Services        | Y                     |
| 打印和文档服务           | 打印服务器                                                   | Print-Server            | N                     |
|                                       | LPD 服务                                                    | Print-LPD-Service       | N                     |
| 远程访问                         | DirectAccess 和 VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | 路由                                                        | 路由                 | N                     |
|                                       | Web 应用程序代理                                          | Web-Application-Proxy   | N                     |
| 远程桌面服务               | 远程桌面连接代理                               | RDS-Connection-Broker   | N                     |
|                                       | 远程桌面授权                                       | RDS 许可           | N                     |
|                                       | 远程桌面虚拟化主机                             | RDS-Virtualization      | N                     |
| Web 服务器 (IIS)                      | Web 服务器                                                     | Web-WebServer           | N                     |
|                                       | 常用 HTTP 功能                                           | Web-Common-Http         | N                     |
|                                       | 默认文档                                               | Web-Default-Doc         | N                     |
|                                       | 目录浏览                                             | Web 目录浏览        | N                     |
|                                       | HTTP 错误                                                    | Web-Http-Errors         | N                     |
|                                       | 静态内容                                                 | Web-Static-Content      | N                     |
|                                       | HTTP 重定向                                               | Web-Http-Redirect       | N                     |
|                                       | WebDAV 发布                                              | Web DAV 发布      | N                     |
|                                       | 运行状况和诊断                                         | Web 运行状况              | N                     |
|                                       | HTTP 日志记录                                                   | Web-Http-Logging        | N                     |
|                                       | 自定义日志记录                                                 | Web 自定义日志记录      | N                     |
|                                       | 日志记录工具                                                  | Web 日志库       | N                     |
|                                       | ODBC 日志记录                                                   | Web-ODBC-Logging        | N                     |
|                                       | 请求监视器                                              | Web-Request-Monitor     | N                     |
|                                       | 跟踪                                                        | Web-Http-Tracing        | N                     |
|                                       | 性能                                                    | Web 性能         | N                     |
|                                       | 静态内容压缩                                     | Web-Stat-Compression    | N                     |
|                                       | 动态内容压缩                                    | Web Dyn 压缩     | N                     |
|                                       | 安全性                                                       | Web-Security            | N                     |
|                                       | 请求筛选                                              | Web 筛选           | N                     |
|                                       | 基本身份验证                                           | Web-Basic-Auth          | N                     |
|                                       | 集中式的 SSL 证书支持                            | Web-CertProvider        | N                     |
|                                       | 客户端证书映射身份验证                      | Web-Client-Auth         | N                     |
|                                       | 摘要式身份验证                                          | Web-Digest-Auth         | N                     |
|                                       | IIS 客户端证书映射身份验证                  | Web-Cert-Auth           | N                     |
|                                       | IP 和域限制                                     | Web-IP-Security         | N                     |
|                                       | URL 授权                                              | Web-Url-Auth            | N                     |
|                                       | Windows 身份验证                                         | Web-Windows-Auth        | N                     |
|                                       | 应用程序开发                                        | Web-App-Dev             | N                     |
|                                       | .NET extensibility 3.5                                         | Web-Net-Ext             | N                     |
|                                       | .NET 可扩展性 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | 应用程序初始化                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | ISAPI 扩展                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI 筛选器                                                  | Web-ISAPI-Filter        | N                     |
|                                       | 服务器端包含                                           | Web 包含            | N                     |
|                                       | WebSocket 协议                                             | Web-WebSockets          | N                     |
|                                       | FTP 服务器                                                     | Web-Ftp-Server          | N                     |
|                                       | FTP 服务                                                    | Web-Ftp-Service         | N                     |
|                                       | FTP 扩展性                                              | Web-Ftp-Ext             | N                     |
|                                       | 管理工具                                               | Web-Mgmt-Tools          | N                     |
|                                       | IIS 6 管理兼容性                                 | Web-Mgmt-Compat         | N                     |
|                                       | IIS 6 元数据库兼容性                                   | Web-Metabase            | N                     |
|                                       | IIS 6 脚本工具                                          | Web-Lgcy-Scripting      | N                     |
|                                       | IIS 6 WMI 兼容性                                        | Web-WMI                 | N                     |
|                                       | IIS 管理脚本和工具                               | Web-Scripting-Tools     | N                     |
|                                       | 管理服务                                             | Web-Mgmt-Service        | N                     |
| Windows Server 更新服务        | WID 连接                                               | UpdateServices-WidDB    | N                     |
|                                       | WSUS 服务                                                  | UpdateServices-Services | N                     |
|                                       | SQL Server 连接性                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>服务器核心中包含的功能
服务器核心安装选项包括以下功能。

| 功能                                                | 名称                               | 默认情况下安装？ |
|--------------------------------------------------------|------------------------------------|-----------------------|
| .NET framework 3.5 功能                            | NET-Framework-Features             | N                     |
| .NET framework 3.5 （包括.NET 2.0 和 3.0）       | NET-Framework-Core                 | （删除）             |
| HTTP 激活                                        | NET-HTTP-Activation                | N                     |
| 非 HTTP 激活                                    | NET-Non-HTTP-Activ                 | N                     |
| .NET framework 4.6 功能                            | NET-Framework-45-Features          | Y                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4.6                                            | NET-Framework-45-ASPNET            | N                     |
| WCF 服务                                           | NET-WCF-Services45                 | Y                     |
| HTTP 激活                                        | NET-WCF-HTTP-Activation45          | N                     |
| 消息队列 (MSMQ) 激活                      | NET-WCF-MSMQ-Activation45          | N                     |
| 命名的管道激活                                  | NET-WCF-Pipe-Activation45          | N                     |
| TCP 激活                                         | NET-WCF-TCP-Activation45           | N                     |
| TCP 端口共享                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| 后台智能传输服务 (BITS)         | BITS                               | N                     |
| Compact 服务器                                         | BITS-Compact-Server                | N                     |
| BitLocker 驱动器加密                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| NFS 客户端                                         | NFS-Client                         | N                     |
| 容器                                             | 容器                         | N                     |
| 数据中心桥接                                   | 数据中心桥接               | N                     |
| 增强存储                                       | EnhancedStorage                    | N                     |
| 故障转移群集                                    | Failover-Clustering                | N                     |
| 组策略管理                                | GPMC                               | N                     |
| I/O 服务质量                                 | DiskIo-QoS                         | N                     |
| IIS 可承载 Web 核心                                  | Web-WHC                            | N                     |
| IP 地址管理 (IPAM) 服务器                    | IPAM                               | N                     |
| iSNS 服务器服务                                    | ISNS                               | N                     |
| 管理 OData IIS 扩展                         | ManagementOdata                    | N                     |
| 媒体基础                                       | Server-Media-Foundation            | N                     |
| 消息队列                                        | MSMQ                               | N                     |
| 消息队列服务                               | MSMQ-Services                      | N                     |
| 消息队列服务器                                 | MSMQ-Server                        | N                     |
| 目录服务集成                          | MSMQ-Directory                     | N                     |
| HTTP 支持                                           | MSMQ-HTTP-Support                  | N                     |
| 消息队列触发器                               | MSMQ 触发器                      | N                     |
| 路由服务                                        | MSMQ 路由                       | N                     |
| 消息队列 DCOM 代理                             | MSMQ-DCOM                          | N                     |
| 多路径 I/O                                          | 多路径 IO                       | N                     |
| 多点连接器                                   | MultiPoint-Connector               | N                     |
| 多点连接器服务                          | MultiPoint-Connector-Services      | N                     |
| MultiPoint 管理器和 MultiPoint 仪表板            | MultiPoint-Tools                   | N                     |
| 网络负载平衡                                 | NLB                                | N                     |
| 对等名称解析协议                          | PNRP                               | N                     |
| 高质量 Windows 音频视频体验                 | qWave                              | N                     |
| 远程差分压缩                        | RDC                                | N                     |
| 远程服务器管理工具                     | RSAT                               | N                     |
| 功能管理工具                           | RSAT-Feature-Tools                 | N                     |
| BitLocker 驱动器加密管理实用工具  | RSAT-Feature-Tools-BitLocker       | N                     |
| DataCenterBridging LLDP 工具                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| 故障转移群集工具                              | RSAT-Clustering                    | N                     |
| Windows PowerShell 的故障转移群集模块         | RSAT-Clustering-PowerShell         | N                     |
| 故障转移群集自动化服务器                     | RSAT-Clustering-AutomationServer   | N                     |
| 故障转移群集命令界面                     | RSAT-Clustering-CmdInterface       | N                     |
| IP 地址管理 (IPAM) 客户端                    | IPAM-Client-Feature                | N                     |
| 受防护的 VM 工具                                      | RSAT-Shielded-VM-Tools             | N                     |
| 存储副本 Windows PowerShell 模块          | RSAT-Storage-Replica               | N                     |
| 角色管理工具                              | RSAT-Role-Tools                    | N                     |
| AD DS 和 AD LDS 工具                                 | RSAT-AD-Tools                      | N                     |
| Windows PowerShell 的 Active Directory 模块         | RSAT-AD-PowerShell                 | N                     |
| AD DS 工具                                            | RSAT-ADDS                          | N                     |
| Active Directory 管理中心                 | RSAT-AD-AdminCenter                | N                     |
| AD DS 管理单元和命令行工具                  | RSAT-ADDS-Tools                    | N                     |
| AD LDS 管理单元和命令行工具                 | RSAT-ADLDS                         | N                     |
| HYPER-V 管理工具                               | RSAT-Hyper-V-Tools                 | N                     |
| Windows PowerShell 的 Hyper-V 模块                  | Hyper-V-PowerShell                 | N                     |
| Windows Server 更新服务工具                   | UpdateServices-RSAT                | N                     |
| API 和 PowerShell cmdlet                             | UpdateServices-API                 | N                     |
| DHCP 服务器工具                                      | RSAT-DHCP                          | N                     |
| DNS 服务器工具                                       | RSAT-DNS-Server                    | N                     |
| 远程访问管理工具                         | RSAT-RemoteAccess                  | N                     |
| Windows PowerShell 的远程访问模块            | RSAT-RemoteAccess-PowerShell       | N                     |
| HTTP 代理上的 RPC                                    | RPC-over-HTTP-Proxy                | N                     |
| 安装和启动事件收集                        | Setup-and-Boot-Event-Collection    | N                     |
| 简单 TCP/IP 服务                                 | Simple-TCPIP                       | N                     |
| SMB 1.0/CIFS 文件共享支持                      | FS-SMB1                            | Y                     |
| SMB 带宽限制                                    | FS-SMBBW                           | N                     |
| SNMP 服务                                           | SNMP-Service                       | N                     |
| SNMP WMI 提供程序                                      | SNMP-WMI-Provider                  | N                     |
| Telnet 客户端                                          | Telnet-Client                      | N                     |
| 用于结构管理的 VM 防护工具               | FabricShieldedTools                | N                     |
| Windows Defender 功能                              | Windows-Defender-Features          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Windows 内部数据库                              | Windows-Internal-Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Windows PowerShell 2.0 Engine                          | PowerShell-V2                      | （删除）             |
| Windows PowerShell Desired State Configuration 服务 | DSC-Service                        | N                     |
| Windows PowerShell Web 访问                          | WindowsPowerShellWebAccess         | N                     |
| Windows Process Activation Service                     | WAS                                | N                     |
| 进程模型                                          | WAS-Process-Model                  | N                     |
| .NET 3.5 环境                                   | WAS-NET-Environment                | N                     |
| 配置 API                                     | WAS-Config-APIs                    | N                     |
| Windows Server 备份                                  | Windows-Server-Backup              | N                     |
| Windows Server 迁移工具                         | 迁移                          | N                     |
| 基于 Windows 标准的存储管理             | WindowsStorageManagementService    | N                     |
| WinRM IIS 扩展                                    | WinRM-IIS-Ext                      | N                     |
| WINS 服务器                                            | WINS                               | N                     |
| WoW64 支持                                          | WoW64-Support                      | Y                     |
|                                                        |                                    |                       |
