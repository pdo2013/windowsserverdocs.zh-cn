---
title: 不在 Windows Server 中的角色、角色服务和功能-Server Core
description: 了解 Windows Server 服务器核心安装选项中未包含的角色和功能。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 882410792c7b8df8a8275c357d64fc17c9f3479e
ms.sourcegitcommit: feec5cbe983c8c5800ccd4fc214914084fcceaba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975264"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>不在 Windows Server 中的角色、角色服务和功能-Server Core

> 适用于：Windows Server 2019、Windows Server 2016 和 Windows Server （半年频道）

以下角色、角色服务和功能已从 Windows Server 的服务器核心安装选项中删除。 使用此信息来帮助确定服务器核心选项是否适用于您的环境。

> [!NOTE]
> 你还可以查看[包含在服务器核心中](server-core-roles-and-services.md)的角色、角色服务和功能的列表。 这是一个非常大的列表，因此为了获得最佳结果，请在此列表中搜索你感兴趣的特定角色或功能。

## <a name="roles-not-in-server-core"></a>不在服务器核心中的角色

- 传真服务器（**传真**）
- MultiPoint Services （**MultiPointServerRole**）
- 网络策略和访问服务（**NPAS**）
- Windows 部署服务（**WDS**） *（Windows Server 版本1803之前）*

## <a name="role-services-not-in-server-core"></a>不在服务器核心中的角色服务
请注意，某些远程桌面角色服务包含在服务器核心（连接代理、授权、虚拟化主机）中，但其他的不是（网关、RD 会话主机、Web 访问）。

- 打印和文件服务 \ 分布式扫描服务器（**打印扫描-服务器**）
- 打印和文件服务 \ Internet 打印（**打印-internet**）
- 远程桌面服务 \ 远程桌面网关（**RDS-网关**）
- 远程桌面服务 \ 远程桌面会话主机（**RDS-Server**）
- 远程桌面服务 \ 远程桌面 Web 访问（**RDS-Web 访问**）
- 角色服务 Web 服务器（IIS） \ 管理工具 \ IIS 管理控制台（**Web 管理控制台**）
- 角色服务 Web 服务器（IIS） \ 管理工具 \ IIS 6 管理兼容性 \ IIS 6 管理控制台（**Lgcy**）
- Windows 部署服务 \ 部署服务器（**WDS-部署**）
- Windows 部署服务传输服务器（**WDS**传输） *（Windows Server 版本1803之前）*

## <a name="features-not-in-server-core"></a>不在服务器核心中的功能
- 后台智能传输服务（BITS） \ IIS 服务器扩展（**bits-IIS-Ext**）
- BitLocker 网络解锁（**bitlocker-NetworkUnlock**）
- 直接播放（**直接播放**）
- Internet 打印客户端（**internet-打印-客户端**）
- LPR 端口监视器（**lpr-端口**监视器）
- 消息队列 \ 消息队列服务 \ 多播支持（**MSMQ-多播**）
- RAS 连接管理器管理工具包（**CMAK**）
- 远程协助（**远程协助**）
- 远程服务器管理工具 \ 功能管理工具 \ SMTP 服务器工具（**RSAT-SMTP**）
- 远程服务器管理工具 \ 功能管理工具 \ BitLocker 驱动器加密管理实用工具 \ BitLocker 驱动器加密工具（**RSAT-功能-bitlocker-remoteadmintool**）
- 远程服务器管理工具 \ 功能管理工具 \ BITS 服务器扩展工具（**RSAT-服务器**）
- 远程服务器管理工具 \ 功能管理工具 \ 网络负载平衡工具（**RSAT-NLB**）
- 远程服务器管理工具 \ 功能管理工具 \ SNMP 工具（**RSAT-snmp**）
- 远程服务器管理工具 \ 功能管理工具 \ WINS 服务器工具（**RSAT-WINS**）
- 远程服务器管理工具 \ 角色管理工具 \ hyper-v 管理工具 \ hyper-v GUI 管理工具（**Hyper-v 工具**）
- 远程服务器管理工具 \ 角色管理工具 \ 远程桌面服务工具 \ 远程桌面网关工具（**RSAT-RDS-Tools**）
- 远程服务器管理工具 \ 角色管理工具 \ 远程桌面服务工具 \ 远程桌面网关工具（**RSAT-RDS-网关**）
- 远程服务器管理工具 \ 角色管理工具 \ 远程桌面服务工具 \ 远程桌面许可诊断程序工具（**RSAT**）
- 远程服务器管理工具 \ 角色管理工具 \ 远程桌面服务工具 \ 远程桌面授权工具（**RDS-许可-UI**）
- 远程服务器管理工具 \ 角色管理工具 \ Windows Server Update Services 工具 \ 用户界面管理控制台（**updateservices-api-UI**）
- 远程服务器管理工具 \ 角色管理工具 \ Active Directory 证书服务工具（**RSAT-ADCS**）
- 远程服务器管理工具 \ 角色管理工具 \ Active Directory 证书服务工具 \ 证书颁发机构管理工具（**RSAT-ADCS**）
- 远程服务器管理工具 \ 角色管理工具 \ Active Directory 证书服务工具 \ 联机响应程序工具（**RSAT-联机**响应）
- 远程服务器管理工具 \ 角色管理工具 \ Active Directory Rights Management Services 工具（**RSAT-ADRMS**）
- 远程服务器管理工具 \ 角色管理工具 \ 传真服务器工具（**RSAT-传真**）
- 远程服务器管理工具 \ 角色管理工具 \ 文件服务工具（**RSAT-file-Services**）
- 远程服务器管理工具 \ 角色管理工具 \ 文件服务工具 \ DFS 管理工具（**RSAT**）
- 远程服务器管理工具 \ 角色管理工具 \ 文件服务工具 \ 文件服务器资源管理器工具（**RSAT-FSRM-管理**）
- 远程服务器管理工具 \ 角色管理工具 \ 文件服务工具 \ 用于网络文件系统管理工具的服务（**RSAT-NFS-管理员**）
- 远程服务器管理工具 \ 角色管理工具 \ 网络策略和访问服务工具（**RSAT-NPAS**）
- 远程服务器管理工具 \ 角色管理工具 \ 打印和文件服务工具（**RSAT-打印服务**）
- 远程服务器管理工具 \ 角色管理工具 \ 批量激活工具（**RSAT-VA-tools**）
- 远程服务器管理工具 \ 角色管理工具 \ Windows 部署服务工具（**AdminPack**）
- 简单 TCP/IP 服务（**简单-TCPIP**）
- SMTP 服务器（**smtp-服务器**）
- TFTP 客户端（**tftp-客户端**）
- WebDAV 重定向**程序（webdav-重定向程序**）
- Windows Biometric Framework （**生物识别框架**）
- Windows defender 功能 \ 适用于 Windows Defender 的 GUI （**windows-Defender-gui**）
- Windows Identity Foundation 3.5 （**Windows 身份-基础**）
- Windows PowerShell \ Windows PowerShell ISE （**PowerShell**）
- Windows 搜索服务（**搜索服务**）
- Windows TIFF IFilter （**windows-tiff-IFilter**）
- 无线 LAN 服务（**无线网络**）
- XPS 查看器（**xps-查看器**）
