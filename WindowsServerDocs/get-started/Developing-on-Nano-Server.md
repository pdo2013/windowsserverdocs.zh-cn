---
title: 针对 Nano Server 进行开发
description: PowerShell 远程处理和 CIM 会话
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 57079470-a1c1-4fdc-af15-1950d3381860
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 8d793dde9c41bc99b55eeb0da3a5ee4b025f08d6
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443638"
---
# <a name="developing-for-nano-server"></a>针对 Nano Server 进行开发

>适用于：Windows Server 2016

> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本操作系统映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解这意味着什么。 

这些主题说明了 Nano Server 上 PowerShell 的重要差异，还提供了开发自己的 PowerShell cmdlet 以用于 Nano Server 的相关指导。

- [Nano Server 上的 PowerShell](PowerShell-on-Nano-Server.md)
- [针对 Nano Server 开发 PowerShell Cmdlet](Developing-PowerShell-Cmdlets-for-Nano-Server.md)

## <a name="using-windows-powershell-remoting"></a>使用 Windows PowerShell 远程控制  
若要使用 Windows PowerShell 远程控制管理 Nano Server，则需要将 Nano Server 的 IP 地址添加到受信任主机的管理计算机列表，将所使用的帐户添加到 Nano Server 的管理员，并启用 CredSSP（如果计划使用该功能）。  

> [!NOTE]
> 如果目标 Nano Server 和管理计算机处于相同的 AD DS 林中（或处于具有信任关系的林中），则不应将 Nano Server 添加到受信任的主机列表中 - 可以通过使用其完全限定的域名（例如：PS C:\>Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)）连接到 Nano Server
  
  
若要将 Nano Server 添加到受信任的主机列表，请在提升的 Windows PowerShell 提示符下运行此命令：  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
若要启动远程 Windows PowerShell 会话，请启动提升的本地 Windows PowerShell 会话，然后运行以下命令：  
  
  
```  
$ip = "\<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
现在可以在 Nano Server 上正常运行 Windows PowerShell 命令。  
  
> [!NOTE]  
> 并非所有的 Windows PowerShell 命令都在此版本的 Nano Server 中可用。 若要查看可用的命令，请运行 `Get-Command -CommandType Cmdlet`  
  
使用命令 `Exit-PSSession` 停止远程会话  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>通过 WinRM 使用 Windows PowerShell CIM 会话  
可以在 Windows PowerShell 中使用 CIM 会话和实例通过 Windows 远程管理 (WinRM) 来运行 WMI 命令。  
  
通过在 Windows PowerShell 提示符中运行以下命令启动 CIM 会话：  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
建立会话后，你可以运行各种 WMI 命令，例如：  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  