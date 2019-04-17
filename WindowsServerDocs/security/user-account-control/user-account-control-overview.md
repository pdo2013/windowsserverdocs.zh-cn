---
title: "用户帐户控制概述"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7a39cd-fc10-4408-befd-4b2c45806732
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 90ce72cb3d1850563d16a12d09a6872d107c0690
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="user-account-control-overview"></a>用户帐户控制概述
用户帐户控制 \(UAC\) 是 Microsoft 的整体安全愿景基本组件。  UAC 可帮助减少的恶意程序影响。

## <a name="BKMK_OVER"></a>功能描述
UAC 允许所有用户登录到他们使用标准用户帐户的计算机上。 使用标准用户令牌启动的进程可能会使用执行任务标准的用户授予访问权限。 例如，Windows 资源管理器将自动继承标准用户级别权限。 此外，使用 Windows 资源管理器中执行的所有程序 \ (例如，方法是依次单击 double\ 应用程序 shortcut\) 也运行标准组的用户的权限。 许多应用程序，包括操作系统本身，包含用于以这种方式正常工作。

其他应用程序，尤其不专设计安全设置，记住，通常需要额外的权限成功地运行。 这些类型的程序称为旧版应用程序。 此外的操作，如安装新的软件和更改 Windows 防火墙，例如程序配置需要多可用的标准用户帐户的权限。

当应用需要使用多个标准用户权限运行时，UAC 可以还原令牌到的其他用户组。 这使用户可以控制显式进行系统级别更改为其计算机或设备的程序。

## <a name="BKMK_APP"></a>实际应用
在 UAC 管理员审核模式有助于防止恶意程序管理员不知情的情况下自动安装。 它还有助于防止意外 system\ 范围内的更改。 最后，它可以用于强制合规性： 管理员必须积极同意或提供的每个管理进程凭据程度更高版本。



