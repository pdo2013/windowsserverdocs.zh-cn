---
title: 远程桌面的允许访问你的电脑
description: 了解用于远程访问你的电脑选项
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
ms.openlocfilehash: d3c85c1c765583eb26cef60ecd245708e0e02772
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297216"
---
# 远程桌面的允许访问你的电脑

>适用于： Windows 10、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

你可以使用远程桌面连接到并通过使用[Microsoft 远程桌面客户端](remote-desktop-clients.md)（适用于 Windows、 iOS、 macOS 和 Android） 来控制你的电脑从远程设备。 当你的电脑允许远程连接时，可以使用另一台设备来连接到电脑，并且你的应用、 文件和网络资源的所有具有访问权限，就像坐在办公桌前面一样。  

> [!NOTE]
> 你可以使用远程桌面连接到 Windows 10 专业版和企业、 Windows 8.1 和 8 的企业版和专业版、 Windows 7 专业版、 企业版和旗舰版和 Windows Server 版本 Windows Server 2008 更高。 你无法连接到计算机 （如 Windows 10 家庭版） 运行家庭版。 

若要连接到远程电脑，计算机必须处于打开状态，它必须有网络连接，必须启用远程桌面，你必须有网络访问远程计算机 （这可能是通过 Internet），并且必须具有连接的权限。 若要连接的权限，你必须是用户的列表上。 启动连接之前，它是一个好主意查找要连接到计算机的名称，请确保通过其防火墙允许远程桌面连接。

## 如何启用远程桌面

允许从远程设备访问你的电脑的最简单方法使用设置下的远程桌面选项。 由于此功能已添加在 Windows 10 Fall Creators update (1709)、 可下载单独的应用，还提供用于较早版本的 Windows 提供类似的功能。 但是，此方法提供的功能和验证，你还可以使用启用远程桌面的传统方式。

### Windows 10 Fall 创建者 Update (1709) 或更高版本

可以使用几个简单步骤来配置远程访问电脑。
1. 在设备上你想要连接、 选择**开始菜单**，然后单击左侧的**设置**图标。
2. 选择跟[**远程桌面**](ms-settings:remotedesktop)项目的**系统**组。
3. 使用滑块来启用远程桌面。
4. 建议还使电脑唤醒状态且易于发现便于建立连接。 单击**显示设置**以启用。
5. 根据需要添加用户可以通过单击**选择的可以远程访问这台电脑的用户**远程连接。
   1. 管理员组的成员自动具有访问权限。
6. 请记下**如何连接到此电脑**这台电脑的名称。 你将需要此功能以配置客户端。

### Windows 7 和早期版本的 Windows 10

若要配置远程访问你的电脑，下载并运行[Microsoft 远程桌面助手](https://www.microsoft.com/download/details.aspx?id=50042)。 此助手更新你的系统设置以启用远程访问，可确保计算机处于唤醒状态的连接，并检查防火墙允许远程桌面连接。 

### 所有版本的 Windows （传统方法）

若要启用远程桌面使用传统的系统属性，按照[连接到另一台计算机使用远程桌面连接](https://windows.microsoft.com/windows/remote-desktop-connection-faq)到的说明。

## 应启用远程桌面？

如果你仅想要访问你的电脑，当你以物理方式坐在它时，你无需启用远程桌面。 启用远程桌面打开你对本地网络可见的电脑上的端口。 如家里，仅应在受信任的网络中启用远程桌面。 你还不想要启用受到严格控制访问任何电脑上的远程桌面。

请注意，当启用访问远程桌面，你将授予管理员组中的任何人都以及任何其他用户选择，远程访问他们的计算机上的帐户的能力。

你应确保有权访问你的电脑的每个帐户配置使用强密码。

## 为什么允许仅可使用网络级别身份验证的连接？ 
 
如果你想要限制哪些用户可以访问你的电脑，选择允许使用网络级别身份验证 (NLA) 中的仅的访问权限。 启用此选项时，用户必须在可以连接到你的电脑之前验证自身到网络。 允许仅的计算机使用 NLA 运行远程桌面连接是一种更安全的身份验证方法，可帮助保护计算机免受恶意用户和软件。 若要了解有关 NLA 和远程桌面的详细信息，请查看[配置 NLA RDS 连接](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx)。 

如果你远程连接到电脑上家庭网络中的网络之外，不要选择此选项。
