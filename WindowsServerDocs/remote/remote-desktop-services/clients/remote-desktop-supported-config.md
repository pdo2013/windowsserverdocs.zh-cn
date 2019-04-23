---
title: 远程桌面客户端的受支持的配置
description: 了解使用远程桌面客户端可以访问哪些 Pc
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d38008b6387385917ad21ce7e169b8ff3f4d18ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884688"
---
# <a name="remote-desktop-client---supported-configuration"></a>远程桌面客户端的受支持的配置

## <a name="supported-pcs"></a>受支持的电脑
你可以连接到运行以下 Windows 操作系统的电脑：
- Windows 10 专业版
- Windows 10 企业版
- Windows 8 企业版
- Windows 8 专业版
- Windows 7 专业版
- Windows 7 企业版
- Windows 7 旗舰版
- Windows 7 旗舰版
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

以下计算机可以运行远程桌面网关：

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

在以下操作系统可用作 RD Web 访问或远程应用程序服务器：
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>不受支持的 Windows 版本和版本

远程桌面客户端不会连接到这些 Windows 版本和版本：

- Windows 7 简易版
- Windows 7 主页
- Windows 8 主页
- Windows 8.1 主页
- Windows 10 家庭版

如果你想要访问已安装这些 Windows 版本之一的计算机，我们建议升级到支持 RDP 的 Windows 版本。

## <a name="rd-gateway-messaging-is-not-supported"></a>不支持 RD 网关消息传送
远程桌面客户端不支持 RD 网关消息传送。 验证远程桌面资源访问策略 (RD RAP) 为 RD 网关服务器不指定**仅允许支持 RD 网关消息传送的计算机**或你将不能连接。