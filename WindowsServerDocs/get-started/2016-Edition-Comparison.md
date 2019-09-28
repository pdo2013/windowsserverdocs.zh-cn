---
title: Windows Server 2016 产品和版本
description: 介绍 Standard 和 Datacenter 版本的区别
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 01/03/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-a932-80cab33f419e
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: e3d32d596746d2ff137fe2517a6430976f9e77ce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391880"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2016"></a>Windows Server 2016 Standard 和 Datacenter 版本的比较

> 适用于：Windows Server 2016
  
## <a name="locks-and-limits"></a>锁定和限制
|锁定和限制|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|最大用户数|基于 CAL|基于 CAL|
|最大 SMB 连接数|16777216|16777216|
|最大 RRAS 连接数|无限制|无限制|
|最大 IAS 连接数|2147483647|2147483647|
|最大 RDS 连接数|65535|65535|
|最大 64 位套接字数|64|64|
|最大核心数|无限制|无限制|
|最大 RAM|24 TB|24 TB|
|可用作虚拟化来宾|是；每个许可证允许运行 2 台虚拟机以及一台 Hyper-V 主机|是；每个许可证允许运行无限台虚拟机以及一台 Hyper-V 主机|
|服务器可以加入域|是|是|
|边缘网络保护/防火墙|否|否|
|DirectAccess|是|是|
|DLNA 解码器和 Web 媒体流|是，如果安装为具有桌面体验的服务器|是，如果安装为具有桌面体验的服务器|

## <a name="server-roles"></a>服务器角色
|可用的 Windows Server 角色|角色服务|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|----------|---------------------------|  
|Active Directory 证书服务| |是|是|
|Active Directory 域服务| |是|是|
|Active Directory 联合身份验证服务| |是|是|
|AD 轻型目录服务| |是|是|
|AD Rights Management Services| |是|是|
|设备运行状况证明| |是|是|
|DHCP 服务器| |是|是|
|DNS 服务器| |是|是|
|传真服务器| |是|是|
|文件和存储服务|文件服务器|是|是|
|文件和存储服务|网络文件的 BranchCache|是|是|
|文件和存储服务|重复数据删除|是|是|
|文件和存储服务|DFS 命名空间|是|是|
|文件和存储服务|DFS 复制|是|是|
|文件和存储服务|文件服务器资源管理器|是|是|
|文件和存储服务|文件服务器 VSS 代理服务|是|是|
|文件和存储服务|iSCSI 目标服务器|是|是|
|文件和存储服务|iSCSI 目标存储提供程序|是|是|
|文件和存储服务|NFS 服务器|是|是|
|文件和存储服务|工作文件夹|是|是|
|文件和存储服务|存储服务|是|是|
|主机保护者服务| |是|是|
|Hyper-V| |是|是；包括受防护的虚拟机|
|MultiPoint 服务| |是|是|
|网络控制器| |否|是|
|网络策略和访问服务| |是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|打印和文档服务| |是|是|
|远程访问| |是|是|
|远程桌面服务| |是|是|
|批量激活服务| |是|是|
|Web 服务 (IIS)| |是|是|
|Windows 部署服务| |是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|Windows Server Essentials 体验| |是|是|
|Windows Server 更新服务| |是|是|

## <a name="features"></a>功能

|Windows Server 功能可以使用服务器管理器（或 PowerShell）安装|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5|是|是|
|.NET Framework 4.6|是|是|
|后台智能传输服务 (BITS)|是|是|
|BitLocker 驱动器加密|是|是|
|BitLocker 网络解锁|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|BranchCache|是|是|
|NFS 客户端|是|是|
|容器|是（Windows 容器无限制；Hyper-V 容器最多为 2 个）|是（所有容器类型均无限制）|
|数据中心桥接|是|是|
|直接播放|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|增强存储|是|是|
|故障转移群集|是|是|
|组策略管理|是|是|
|主机保护者 Hyper-V 支持|否|是|
|I/O 服务质量|是|是|
|IIS 可承载 Web 核心|是|是|
|Internet 打印客户端|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|IPAM 服务器|是|是|
|iSNS 服务器服务|是|是|
|LPR 端口监视器|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|管理 OData IIS 扩展|是|是|
|媒体基础|是|是|
|消息队列|是|是|
|多路径 I/O|是|是|
|多点连接器|是|是|
|网络负载平衡|是|是|
|对等名称解析协议|是|是|
|高质量 Windows 音频视频体验|是|是|
|RAS 连接管理器管理工具包|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|远程协助|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|远程差分压缩|是|是|
|RSAT|是|是|
|HTTP 代理上的 RPC|是|是|
|安装和启动事件收集|是|是|
|简单 TCP/IP 服务|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|SMB 1.0/CIFS 文件共享支持|已安装|已安装|
|SMB 带宽限制|是|是|
|SMTP 服务器|是|是|
|SNMP 服务|是|是|
|软件负载平衡器|否|是|
|存储副本|否|是|
|Telnet 客户端|是|是|
|TFTP 客户端|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|用于结构管理的 VM 防护工具|是|是|
|WebDAV 重定向程序|是|是|
|Windows Biometric Framework|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|Windows Defender 功能|已安装|已安装|
|Windows Identity Foundation 3.5|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|Windows 内部数据库|是|是|
|Windows PowerShell|已安装|已安装|
|Windows Process Activation Service|是|是|
|Windows Search 服务|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|Windows Server 备份|是|是|
|Windows Server 迁移工具|是|是|
|基于 Windows 标准的存储管理|是|是|
|Windows TIFF IFilter|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|
|WinRM IIS 扩展|是|是|
|WINS 服务器|是|是|
|无线 LAN 服务|是|是|
|WoW64 支持|已安装|已安装|
|XPS 查看器|是，在安装为具有桌面体验的服务器时|是，在安装为具有桌面体验的服务器时|

|通常可用的功能|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|最佳做法分析器|是|是|
|直接访问|是|是|
|动态内存（虚拟化）|是|是|
|热添加/替换 RAM|是|是|
|Microsoft 管理控制台|是|是|
|最精简的服务器界面|是|是|
|网络负载平衡|是|是|
|Windows PowerShell|是|是|
|服务器核心安装选项|是|是|
|Nano Server 安装选项|是|是|
|服务器管理器|是|是|
|SMB 直通和基于 RDMA 的 SMB|是|是|
|软件定义的网络|否|是|
|存储管理服务|是|是|
|存储空间|是|是|
|存储空间直通|否|是|
|批量激活服务|是|是|
|VSS（卷影复制服务）集成|是|是|
|Windows Server 更新服务|是|是|
|Windows 系统资源管理器|是|是|
|服务器许可证日志记录|是|是|
|继承激活|托管于数据中心时作为来宾|可以是主机或来宾|
|工作文件夹|是|是|

