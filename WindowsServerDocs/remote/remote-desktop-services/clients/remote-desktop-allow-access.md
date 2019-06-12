---
title: 远程桌面-允许访问您的电脑
description: 了解用于远程访问您的 PC 的选项
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
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804989"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>远程桌面-允许访问您的电脑

>适用于：Windows 10 中，Windows 8.1、 Windows Server 2019、 Windows Server 2016 中，Windows Server 2012 R2

可以使用远程桌面连接到并通过使用来控制您的 PC 的远程设备[Microsoft 远程桌面客户端](remote-desktop-clients.md)（适用于 Windows、 iOS、 macOS 和 Android）。 如果您允许远程连接到您的 PC，可以使用另一台设备连接到您的 PC 并对所有的应用程序、 文件和网络资源具有访问权限，就像您正坐在办公桌旁。  

> [!NOTE]
> 可以使用远程桌面连接到 Windows 10 专业版和企业、 Windows 8.1 和 8 的企业和 Pro、 Windows 7 专业版、 企业版和旗舰版和 Windows Server 版本比 Windows Server 2008 更新的文件。 无法连接到运行家庭版 （如 Windows 10 家庭版） 的计算机。 

若要连接到远程电脑，必须开启计算机，它必须具有网络连接，必须启用远程桌面，必须具有网络访问远程计算机 （这可能是通过 Internet），并且必须具有连接权限。 若要连接的权限，您必须是在用户列表。 启动连接之前，它是计算机的一个好主意，查找要连接到的名称，请确保远程桌面连接允许通过其防火墙。

## <a name="how-to-enable-remote-desktop"></a>如何启用远程桌面

若要允许从远程设备访问您的 PC 的最简单方法使用设置下的远程桌面选项。 由于此功能已添加在 Windows 10 Fall Creators update (1709) 单独的可下载应用程序也是可为早期版本的 Windows 提供类似的功能。 但是，此方法可提供较少的功能和验证，还可以使用以启用远程桌面的传统的方式。

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creator Update (1709) 或更高版本

可以通过几个简单步骤来配置您的 PC 以远程访问。
1. 在你想要连接到设备上，选择**启动**，然后单击**设置**在左侧的图标。
2. 选择**系统**组后跟[**远程桌面**](ms-settings:remotedesktop)项。
3. 使用滑块可启用远程桌面。
4. 建议还使 PC 唤醒状态和可发现性，便于建立连接。 单击**显示其设置**启用。
5. 根据需要添加可以通过单击远程连接的用户**选择用户可以远程访问这台电脑**。
   1. 管理员组的成员自动具有访问权限。
6. 请记下此计算机上的名称**如何连接到这台电脑**。 你将需要它来配置客户端。

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 和早期版本的 Windows 10

若要配置用于远程访问您的 PC，请下载并运行[Microsoft 远程桌面助手](https://www.microsoft.com/download/details.aspx?id=50042)。 此助手更新您的系统设置以启用远程访问，可确保您的计算机处于唤醒状态的连接，并检查你的防火墙允许的远程桌面连接。 

### <a name="all-versions-of-windows-legacy-method"></a>所有版本的 Windows （旧方法）

若要启用远程桌面使用旧有系统属性，按照说明[连接到另一台计算机使用远程桌面连接](https://windows.microsoft.com/windows/remote-desktop-connection-faq)。

## <a name="should-i-enable-remote-desktop"></a>应启用远程桌面？

如果只想要访问您的 PC，您实际坐在它前面时，您不必启用远程桌面。 启用远程桌面打开您的 PC 所见到你的本地网络上的端口。 如您家，应仅在受信任网络中，启用远程桌面。 也不想启用远程桌面的访问受到严格控制任何 PC 上。

请注意，启用远程桌面访问将向管理员组中的任何人授予时以及任何其他用户选择，远程访问其帐户的计算机上的能力。

您应确保使用强密码配置了每个帐户都有权访问您的 PC。

## <a name="why-allow-connections-only-with-network-level-authentication"></a>为什么允许仅带网络级身份验证的连接？ 

如果你想要限制谁可以访问您的电脑，选择允许访问带网络级身份验证 (NLA) 中的仅。 如果启用此选项，用户必须向网络验证自身，才能连接到您的 PC。 允许仅从运行带 NLA 的远程桌面的计算机的连接是一种更安全的身份验证方法，可帮助保护计算机免受恶意用户和软件。 若要了解有关 NLA 和远程桌面的详细信息，请查看[RDS 连接配置 NLA](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx)。

如果要从该网络外的家庭网络上远程连接到 PC，不选择此选项。
