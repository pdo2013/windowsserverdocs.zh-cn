---
title: 远程桌面客户端-支持的配置
description: 了解您可以使用远程桌面客户端访问的 Pc
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
ms.sourcegitcommit: 96e968bbe8dc50ebb1535ae1c8ce92fa73c83171
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "1978044"
---
# <a name="remote-desktop-client---supported-configuration"></a>远程桌面客户端-支持的配置

## <a name="supported-pcs"></a>受支持的电脑
您可以连接到 Pc，运行以下 Windows 操作系统：
- Windows 10 专业版
- Windows 10 企业版
- Windows 8 企业版
- Windows 8 专业版
- Windows7 专业版
- Windows7 企业版
- Windows7 旗舰版
- Windows7 旗舰版
- Windows Server 2008
- WindowsServer 2008 R2
- WindowsServer 2012
- Windows Server 2012 R2
- WindowsServer 2016
- Windows 多点 Server 2011
- Windows 多点 Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

以下计算机可以运行远程桌面网关：

- Windows Server 2008
- WindowsServer 2008 R2
- WindowsServer 2012
- Windows Server 2012 R2
- WindowsServer 2016
- Windows Small Business Server 2011

下列操作系统可以充当 RD Web Access 或 RemoteApp 服务器：
- WindowsServer 2008 R2
- WindowsServer 2012
- Windows Server 2012 R2
- WindowsServer 2016

## <a name="unsupported-windows-versions-and-editions"></a>不受支持的 Windows 版本

远程桌面客户端无法连接到这些 Windows 版本和版本：

- Windows 7 简易版
- Windows 7 主页
- Windows 8 主页
- Windows 8.1 主页
- Windows 10 家庭版

如果您想要访问已安装了这些 Windows 版本之一的计算机，我们建议您升级到支持 RDP 的 Windows 版本。

## <a name="rd-gateway-messaging-is-not-supported"></a>不支持 RD 网关消息
远程桌面客户端不支持 RD 网关消息。 确认远程桌面资源访问策略 (RD RAP) RD 网关服务器不指定**只允许对 RD 网关消息支持的计算机**，或者您将无法连接。