---
title: 角色、 角色服务和不在 Windows Server 的服务器核心中的功能
description: 了解有关角色和功能不包括在 Windows Server 的服务器核心安装选项。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825528"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>角色、 角色服务和不在 Windows Server 的服务器核心中的功能

> 适用于：Windows Server （半年频道） 和 Windows Server 2016

已从 Windows Server 的服务器核心安装选项下的角色、 角色服务和功能。 使用此信息来帮助找出如果服务器核心选项适用于你的环境。

> [!NOTE]
> 此外可以查看一组角色、 角色服务和功能[包含在服务器核心](server-core-roles-and-services.md)。 它是一个非常大的列表，因此为了获得最佳结果，列表中搜索该特定角色或你感兴趣的功能。

## <a name="roles-not-in-server-core"></a>不在服务器核心角色

- 传真
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>不在服务器核心角色服务
请注意，某些远程桌面角色服务包含在服务器核心 （连接代理、 许可、 虚拟化主机），但有些则不是 （网关，RD 会话主机 Web 访问）。

- 打印扫描服务器
- Internet 打印
- RDS-Gateway
- RDS-RD-Server
- RDS-Web-Access
- Web-Mgmt-Console
- Web-Lgcy-Mgmt-Console
- WDS-Deployment
- WDS 传输 *（之前 Windows Server 版本 1803年)*

## <a name="features-not-in-server-core"></a>不在服务器核心功能

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- 直接播放
- Internet-Print-Client
- LPR-Port-Monitor
- MSMQ-Multicasting
- CMAK
- 远程协助
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- RSAT-Bits-Server
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Hyper-V-Tools
- RSAT-RDS-Tools
- RSAT-RDS-Gateway
- RSAT-RDS-Licensing-Diagnosis-UI
- RDS-Licensing-UI
- UpdateServices-UI
- RSAT-ADCS
- RSAT-ADCS-Mgmt
- RSAT-Online-Responder
- RSAT-ADRMS
- RSAT-Fax
- RSAT-File-Services
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- RSAT-NFS-Admin
- RSAT-NPAS
- RSAT-Print-Services
- RSAT-VA-Tools
- WDS-AdminPack
- SMTP-Server
- TFTP-Client
- WebDAV-Redirector
- Biometric-Framework
- Windows-Defender-Gui
- Windows-Identity-Foundation
- PowerShell-ISE
- 搜索服务
- Windows-TIFF-IFilter
- 无线网络
- XPS-Viewer

