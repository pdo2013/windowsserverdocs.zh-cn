---
title: 不在 Windows Server 中的角色、角色服务和功能-Server Core
description: 了解 Windows Server 服务器核心安装选项中未包含的角色和功能。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce5107e8e0ab573df7588428db65c8b223cf1f13
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476516"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>不在 Windows Server 中的角色、角色服务和功能-Server Core

> 适用于：Windows Server 2019、Windows Server 2016 和 Windows Server (半年频道)

以下角色、角色服务和功能已从 Windows Server 的服务器核心安装选项中删除。 使用此信息来帮助确定服务器核心选项是否适用于您的环境。

> [!NOTE]
> 你还可以查看[包含在服务器核心中](server-core-roles-and-services.md)的角色、角色服务和功能的列表。 这是一个非常大的列表, 因此为了获得最佳结果, 请在此列表中搜索你感兴趣的特定角色或功能。

## <a name="roles-not-in-server-core"></a>不在服务器核心中的角色

- 传真
- MultiPointServerRole
- NPAS
- 映像

## <a name="role-services-not-in-server-core"></a>不在服务器核心中的角色服务
请注意, 某些远程桌面角色服务包含在服务器核心 (连接代理、授权、虚拟化主机) 中, 但其他的不是 (网关、RD 会话主机、Web 访问)。

- 打印扫描-服务器
- 打印-Internet
- RDS-网关
- RDS-RD-服务器
- RDS-Web 访问
- Web 管理-控制台
- Web-Lgcy
- WDS-部署
- WDS 传输 *(Windows Server 版本1803之前)*

## <a name="features-not-in-server-core"></a>不在服务器核心中的功能

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- 直接播放
- Internet-打印-客户端
- LPR-端口监视器
- MSMQ-多播
- CMAK
- 远程协助
- RSAT-SMTP
- RSAT-功能-工具-BitLocker-Bitlocker-remoteadmintool
- RSAT-服务器
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Hyper-v-工具
- RSAT-RDS-工具
- RSAT-RDS-网关
- RSAT-RDS-授权-诊断-UI
- RDS-授权-UI
- Updateservices-api-UI
- RSAT-ADCS
- RSAT-ADCS
- RSAT-联机-响应方
- RSAT-ADRMS
- RSAT-传真
- RSAT-文件-服务
- RSAT-DFS-管理
- RSAT-FSRM-管理
- RSAT-NFS-管理
- RSAT-NPAS
- RSAT-打印-服务
- RSAT-VA-工具
- WDS-AdminPack
- SMTP-服务器
- TFTP-客户端
- WebDAV-重定向程序
- 生物识别框架
- Windows-Defender-Gui
- Windows-标识-基础
- PowerShell-ISE
- 搜索-服务
- Windows-TIFF-IFilter
- 无线网络
- XPS-查看器

