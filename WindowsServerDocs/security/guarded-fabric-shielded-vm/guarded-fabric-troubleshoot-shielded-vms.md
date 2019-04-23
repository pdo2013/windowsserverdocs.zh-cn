---
title: 对受防护的 Vm 进行故障排除
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: 13ff0dad1519d394ce74a91efbfcc9e2f237e4a5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850028"
---
# <a name="troubleshoot-shielded-vms"></a>对受防护的 Vm 进行故障排除

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

从 Windows Server 1803 版开始，虚拟机连接 (VMConnect) 增强的会话模式和 PS 直通会重新启用完全受防护的 vm。 虚拟化管理员仍需要 VM 来宾凭据才能访问 VM，但使其更易于宿主时其网络配置已遭到破坏，受防护的 VM 进行故障排除。

若要启用受防护的 Vm VMConnect 和 PS 直接，只需将它们移到运行 Windows Server 版本 1803 版或更高版本的 HYPER-V 主机上。 允许对这些功能的虚拟设备将自动重新启用。 如果受防护的 VM 移到运行的主机和早期版本的 Windows Server，VMConnect 和 PS 直接将再次禁用。

对于安全敏感客户担心如果主机具有到 VM 的任何访问权限，并且想要返回到原始行为，应在来宾 OS 中禁用以下功能：

- 禁用虚拟机中的 PowerShell Direct 服务：

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- VMConnect 增强会话模式仅在至少将来宾 OS 是否禁用 Windows Server 2019 或 Windows 10，版本 1809年。 若要禁用 VMConnect 增强会话控制台连接在 VM 中添加以下注册表项：

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
