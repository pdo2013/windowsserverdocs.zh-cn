---
title: 远程桌面客户端 - 支持的配置
description: 了解可以使用远程桌面客户端访问哪些电脑
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2dc8eb68aebc904640aa4adc3e75cdeda34e97b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387652"
---
# <a name="remote-desktop-client---supported-configuration"></a>远程桌面客户端 - 支持的配置

## <a name="supported-pcs"></a>受支持的电脑
可以连接到运行以下 Windows 操作系统的电脑：
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

以下操作系统可用作 RD Web 访问或 RemoteApp 服务器：
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>不受支持的 Windows 版本

远程桌面客户端不会连接到以下 Windows 版本：

- Windows 7 简易版
- Windows 7 家庭版
- Windows 8 家庭版
- Windows 8.1 家庭版
- Windows 10 家庭版

如果要访问安装了上述其中一个 Windows 版本的计算机，建议升级到支持 RDP 的 Windows 版本。

## <a name="rd-gateway-messaging-is-not-supported"></a>不支持 RD 网关消息传送
远程桌面客户端不支持 RD 网关消息传送。 验证 RD 网关服务器的远程桌面资源访问策略 (RD RAP) 没有指定“仅允许支持 RD 网关消息传送的计算机”  ，否则将无法进行连接。