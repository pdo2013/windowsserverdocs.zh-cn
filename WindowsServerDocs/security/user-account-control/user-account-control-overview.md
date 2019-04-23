---
title: 用户帐户控制概述
description: Windows Server 安全
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887668"
---
# <a name="user-account-control-overview"></a>用户帐户控制概述
用户帐户控制\(UAC\)是 Microsoft 的整体安全构想一个基本组件。  UAC 帮助减轻恶意程序所带来的影响。

## <a name="BKMK_OVER"></a>功能说明
UAC 允许所有用户使用标准用户帐户登录到他们的计算机。 使用标准用户令牌启动的进程可能会使用授予标准用户的访问权限执行任务。 例如，Windows 资源管理器会自动继承标准用户级别权限。 此外，任何使用 Windows 资源管理器执行的程序\(例如，通过双\-单击应用程序快捷方式\)也使用的标准用户权限集运行。 许多应用程序，其中包括操作系统本身，包括用于在这种方式正常工作。

其他应用程序，尤其是那些不专门设计使用安全设置的一点是，通常需要其他权限才能成功运行。 这些类型的程序称为旧应用程序。 此外，操作，例如安装新软件并对程序，如 Windows 防火墙进行配置更改需要更多的权限，而不是可用于标准用户帐户。

当使用多个标准用户权限来运行应用程序需求时，UAC 可以还原到令牌的其他用户组。 这使用户具有显式控制要对其计算机或设备进行系统级更改的计划。

## <a name="BKMK_APP"></a>实际应用程序
在 UAC 的管理员批准模式可帮助防止恶意程序的管理员不知情的情况下以无提示方式安装。 它还帮助防止意外的系统\-宽的更改。 最后，它可以用于强制执行更高级别的合规性，其中管理员必须主动同意或为每个管理进程提供凭据。



