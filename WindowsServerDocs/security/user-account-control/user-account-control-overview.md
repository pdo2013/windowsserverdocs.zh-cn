---
title: 用户帐户控制概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bdc9f4dc4b8e19d62288f12a4f2b4e8c86b93b68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403317"
---
# <a name="user-account-control-overview"></a>用户帐户控制概述
用户帐户控制 \(UAC @ no__t 是 Microsoft 总体安全愿景的基本组件。  UAC 帮助减轻恶意程序所带来的影响。

## <a name="BKMK_OVER"></a>功能说明
UAC 允许所有用户使用标准用户帐户登录到他们的计算机。 使用标准用户令牌启动的进程可能会使用授予标准用户的访问权限执行任务。 例如，Windows 资源管理器会自动继承标准用户级别权限。 此外，使用 Windows 资源管理器执行的任何程序 @no__t 0for 示例中，通过 double @ no__t-1clicking 应用程序快捷方式 @ no__t 也是使用标准的用户权限集运行的。 许多应用程序（包括操作系统本身包含的应用程序）均设计为以这种方式正常运行。

其他应用程序（尤其是那些未特意设计安全设置的应用程序）通常需要额外的权限才能成功运行。 这些类型的程序称为旧版应用程序。 此外，在 Windows 防火墙等程序中安装新软件和对程序进行配置更改等操作所需的权限比标准用户帐户可用的权限更多。

当应用程序需要使用超过标准用户权限运行时，UAC 可以将其他用户组还原到该令牌。 这样，用户就可以对对计算机或设备进行系统级更改的程序进行显式控制。

## <a name="BKMK_APP"></a>实用应用程序
UAC 中的管理员批准模式有助于防止恶意程序无提示安装，而无需管理员的知识。 它还有助于防止意外的 system @ no__t-0wide 更改。 最后，它可以用于强制执行更高级别的合规性，其中管理员必须主动同意或为每个管理进程提供凭据。



