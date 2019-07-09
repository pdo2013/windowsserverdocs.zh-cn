---
title: 远程桌面 - 允许访问你的电脑
description: 了解用于远程访问你的电脑的选项
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d03dcd307696aea55ab6a1569ab907635994772a
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66804989"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>远程桌面 - 允许访问你的电脑

>适用于：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

可以使用远程桌面通过 [Microsoft 远程桌面客户端](remote-desktop-clients.md)（适用于 Windows、iOS、macOS 和 Android）从远程设备连接并控制你的电脑。 当你允许远程连接到你的电脑时，可以使用其他设备连接到你的电脑并访问所有应用、文件和网络资源，就像坐在办公桌前一样。  

> [!NOTE]
> 可以使用远程桌面连接到 Windows 10 专业版和企业版、Windows 8.1 和 8 企业版和专业版、Windows 7 专业版、企业版和旗舰版以及比 Windows Server 2008 更高的 Windows Server 版本。 不能连接到运行家庭版的计算机（如 Windows 10 家庭版）。 

若要连接到远程电脑，必须打开该电脑，它必须具有网络连接，必须启用远程桌面，你必须具有对远程电脑的网络访问权限（可以通过 Internet 访问），并且必须具有连接权限。 若要获得连接权限，你必须在用户列表中。 开始连接之前，最好先找到要连接的计算机的名称，并确保允许远程桌面连接通过其防火墙。

## <a name="how-to-enable-remote-desktop"></a>如何启用远程桌面

若要允许从远程设备访问你的电脑，最简单的方法是使用“设置”下的远程桌面选项。 由于此功能是在 Windows 10 Fall Creators update (1709) 中加入的，因此还提供了一个单独的可下载应用，可为早期 Windows 版本提供类似的功能。 你也可以使用旧方法来启用远程桌面，但此方法提供的功能和验证较少。

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creator Update (1709) 或更高版本

只需几个简单的步骤便可为电脑配置远程访问。
1. 在要连接的设备上，选择“开始”，然后单击左侧的“设置”图标   。
2. 选择后跟[“远程桌面”](ms-settings:remotedesktop)项的“系统”组   。
3. 使用滑块启用远程桌面。
4. 另外，建议让电脑保持唤醒和可检测到的状态，以便于连接。 单击“显示设置”即可启用  。
5. 通过单击“选择可以远程访问此电脑的用户”，根据需要添加可以进行远程连接的用户  。
   1. Administrators 组的成员自动拥有访问权限。
6. 记下“如何连接到此电脑”下此电脑的名称  。 需要用它来配置客户端。

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 和早期版本的 Windows 10

若要为电脑配置远程访问，请下载并运行 [Microsoft 远程桌面助手](https://www.microsoft.com/download/details.aspx?id=50042)。 此助手将更新系统设置以启用远程访问，确保计算机处于可连接的唤醒状态，并检查防火墙是否允许远程桌面连接。 

### <a name="all-versions-of-windows-legacy-method"></a>所有 Windows 版本（旧方法）

若要使用旧系统属性启用远程桌面，请按照[使用“远程桌面连接”连接到另一台计算机](https://windows.microsoft.com/windows/remote-desktop-connection-faq)中的说明进行操作。

## <a name="should-i-enable-remote-desktop"></a>我应该启用远程桌面吗？

如果你只想真正坐在电脑前访问电脑，则无需启用远程桌面。 启用远程桌面会在你的电脑上打开本地网络可见的端口。 应仅在受信任的网络中启用远程桌面，比如家里。 你也不会想在任何严格控制访问权限的电脑上启用远程桌面。

请注意，允许访问远程桌面，即表示授予 Administrators 组中的任何人以及所选的任何其他用户在计算机上远程访问其帐户的能力。

应确保可以访问你的电脑的每个帐户都配置了强密码。

## <a name="why-allow-connections-only-with-network-level-authentication"></a>为什么仅允许使用网络级别身份验证进行连接？ 

如果要限制谁可以访问你的电脑，请选择仅允许使用网络级别身份验证 (NLA) 进行访问。 启用此选项后，用户必须先向网络验证身份，然后才能连接到你的电脑。 仅允许使用 NLA 从运行远程桌面的计算机进行连接是一种更安全的身份验证方法，可以帮助保护计算机免受恶意用户和软件的侵害。 若要了解有关 NLA 和远程桌面的详细信息，请查看[为 RDS 连接配置 NLA](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx)。

如果从家庭网络外部远程连接到该网络上的电脑，请不要选择此选项。
