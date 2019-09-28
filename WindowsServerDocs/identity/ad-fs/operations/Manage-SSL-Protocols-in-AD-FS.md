---
title: 管理 AD FS 的 SSL/TLS 协议和密码套件
description: AD FS 2016 的常见问题
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 44fb4c02421a431edb502daecaa38f00fb4dd2ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407536"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>管理 AD FS 的 SSL/TLS 协议和密码套件
以下文档介绍了如何禁用和启用 AD FS 使用的某些 TLS/SSL 协议和密码套件

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL、SChannel 和密码套件 AD FS

传输层安全性（TLS）和安全套接字层（SSL）是提供安全通信的协议。  Active Directory 联合身份验证服务使用这些协议进行通信。  目前有几个版本的协议。

Schannel 是实现 SSL、TLS 和 DTLS Internet 标准身份验证协议的安全支持提供程序 (SSP)。 安全支持提供程序接口 (SSPI) 是 Windows 系统用于执行安全相关功能（包括身份验证）的 API。 SSPI 用作几个安全支持提供程序 (SSP)（包括 Schannel SSP）的通用接口。

密码套件是一组加密算法。 TLS/SSL 协议的 schannel SSP 实现使用密码套件中的算法来创建密钥并对信息加密。 密码套件为以下每种任务指定一种算法：

- 密钥交换
- 批量加密
- 消息验证

AD FS 使用 Schannel 执行其安全的通信交互。  目前 AD FS 支持 Schannel 支持的所有协议和密码套件。

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>管理 TLS/SSL 协议和密码套件
> [!IMPORTANT]
> 本部分包含说明如何修改注册表的步骤。 但是，如果不正确地修改注册表，则可能会出现严重问题。 因此，请务必仔细执行这些步骤。 
> 
> 请注意，更改 SCHANNEL 的默认安全设置可能会中断或阻止特定客户端和服务器之间的通信。  如果需要安全通信，并且它们没有协议来协商与通信，则会发生这种情况。
> 
> 如果要应用这些更改，则必须将这些更改应用于场中的所有 AD FS 服务器。  应用这些更改后，需要重新启动。

在今天的时代，强化您的服务器并删除旧的或弱的密码套件会成为许多组织的主要优先级。  可使用软件套件来测试服务器，并提供有关这些协议和套件的详细信息。  为了保持符合或实现安全评级，删除或禁用较弱的协议或密码套件变为必需。  本文档的其余部分将提供有关如何启用或禁用某些协议和密码套件的指导。

以下注册表项位于同一位置：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  使用 regedit 或 PowerShell 启用或禁用这些协议和密码套件。

![注册表位置](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>启用和禁用 SSL 2。0
使用以下注册表项及其值来启用和禁用 SSL 2.0。

### <a name="enable-ssl-20"></a>启用 SSL 2。0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server]"DisabledByDefault" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 客户端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 客户端]"DisabledByDefault" = dword：00000000 

### <a name="disable-ssl-20"></a>禁用 SSL 2。0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server]"DisabledByDefault" = dword：00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 客户端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ 客户端]"DisabledByDefault" = dword：00000001

### <a name="using-powershell-to-disable-ssl-20"></a>使用 PowerShell 禁用 SSL 2。0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>启用和禁用 SSL 3。0
使用以下注册表项及其值来启用和禁用 SSL 3.0。

### <a name="enable-ssl-30"></a>启用 SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 服务器]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 服务器]"DisabledByDefault" = dword：00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 客户端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 客户端]"DisabledByDefault" = dword：00000000 

### <a name="disable-ssl-30"></a>禁用 SSL 3。0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 服务器]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 服务器]"DisabledByDefault" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 客户端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ 客户端]"DisabledByDefault" = dword：00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>使用 PowerShell 禁用 SSL 3。0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>启用和禁用 TLS 1。0
使用以下注册表项及其值来启用和禁用 TLS 1.0。

> [!IMPORTANT]
> 禁用 TLS 1.0 将中断 WAP，使其 AD FS 信任。  如果禁用 TLS 1.0，应为应用程序启用强身份验证。  请参阅[启用强身份验证](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>启用 TLS 1。0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server]"DisabledByDefault" = dword：00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 客户端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 客户端]"DisabledByDefault" = dword：00000000 

### <a name="disable-tls-10"></a>禁用 TLS 1。0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server]"DisabledByDefault" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 客户端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ 客户端]"DisabledByDefault" = dword：00000001 

### <a name="using-powershell-to-disable-tls-10"></a>使用 PowerShell 禁用 TLS 1。0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>启用和禁用 TLS 1。1
使用以下注册表项及其值来启用和禁用 TLS 1.1。

### <a name="enable-tls-11"></a>启用 TLS 1。1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server]"DisabledByDefault" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 客户端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 客户端]"DisabledByDefault" = dword：00000000

### <a name="disable-tls-11"></a>禁用 TLS 1。1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server]"DisabledByDefault" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 客户端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ 客户端]"DisabledByDefault" = dword：00000001 

### <a name="using-powershell-to-disable-tls-11"></a>使用 PowerShell 禁用 TLS 1。1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>启用和禁用 TLS 1。2

使用以下注册表项及其值来启用和禁用 TLS 1.2。

### <a name="enable-tls-12"></a>启用 TLS 1。2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server]"DisabledByDefault" = dword：00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ 客户端]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ 客户端]"DisabledByDefault" = dword：00000000

### <a name="disable-tls-12"></a>禁用 TLS 1。2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server]"DisabledByDefault" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ 客户端]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ 客户端]"DisabledByDefault" = dword：00000001

### <a name="using-powershell-to-disable-tls-12"></a>使用 PowerShell 禁用 TLS 1。2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>启用和禁用 RC4 

使用以下注册表项及其值来启用和禁用 RC4。  此密码套件的注册表项位于以下位置：

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![注册表位置](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>启用 RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128]"Enabled" = dword：00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128]"Enabled" = dword：00000001 

### <a name="disable-rc4"></a>禁用 RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128]"Enabled" = dword：00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128]"Enabled" = dword：00000000 

### <a name="using-powershell"></a>使用 PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>启用或禁用其他密码套件

可以通过从 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 中删除某些特定密码来禁用这些密码。 

![注册表位置](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

若要启用密码套件，请将其字符串值添加到函数多字符串值键。  例如，如果我们想要启用 TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521，则将其添加到字符串。

有关支持的密码套件的完整列表，请参阅[TLS/SSL （SCHANNEL SSP）中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。  本文档提供默认情况下启用的套件的表以及支持但默认情况下未启用的套件。  若要确定密码套件的优先级，请参阅[优先级 Schannel 密码套件](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)。

## <a name="enabling-strong-authentication-for-net-applications"></a>为 .NET 应用程序启用强身份验证
.NET Framework 3.5/4.0/4.5. x 应用程序可以通过启用 SchUseStrongCrypto 注册表项来将默认协议切换到 TLS 1.2。  此注册表项将强制 .NET 应用程序使用 TLS 1.2。

> [!IMPORTANT]
> 对于 Windows Server 2016 和 Windows Server 2012 R2 上的 AD FS，需要使用 .NET Framework 4.0/4.5. x 键：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft @ no__t-0. NETFramework\v4.0.30319


对于 .NET Framework 3.5，请使用以下注册表项：

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft @ no__t-0. NETFramework\v2.0.50727] "SchUseStrongCrypto" = dword：00000001

对于 .NET Framework 4.0/4.5. x，请使用以下注册表项：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft @ no__t-0. NETFramework\v4.0.30319 "SchUseStrongCrypto" = dword：00000001

![强身份验证](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>其他信息

- [TLS/SSL （Schannel SSP）中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)
- [Windows 8.1 中的 TLS 密码套件](https://msdn.microsoft.com/library/windows/desktop/mt767781.aspx)
- [排定 Schannel 密码套件的优先级](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)
- [密码和其他神秘 tongues](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
