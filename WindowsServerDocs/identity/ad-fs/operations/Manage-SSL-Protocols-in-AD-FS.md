---
title: "广告 fs 管理 SSL 月 TLS 协议和密码套件"
description: "对于广告 FS 2016：常见问题"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 23055a1b727e4f1a6b1ccafdea9a410c6021c432
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2018
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>广告 fs 管理 SSL 月 TLS 协议和密码套件
以下文档如何禁用启用某些 TLS 月 SSL 协议和密码套件所使用的广告 FS 上提供信息

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS 月 SSL、 SChannel 和密码套件广告 FS 中

传输层安全性 (TLS) 和安全套接字层 (SSL) 都能提供的安全通信的协议。  Active Directory 联合身份验证服务会使用这些协议的通信。  今天这些协议的几个版本存在。

Schannel 是实现 SSL、 TLS 和 DTLS Internet 标准身份验证协议安全支持提供商 (SSP)。 安全支持提供程序接口 (SSPI) 是 Windows 系统用于执行安全相关的功能，包括身份验证的 API。 SSPI 充当几个安全支持提供商 (Ssp)，包括 Schannel SSP 常见接口

密码套件是一套加密算法。 TLS 月 SSL 协议 schannel SSP 实现使用密码套件从算法创建键和加密信息。 密码套件指定为每台以下任务一个算法：

- 关键 exchange
- 批量加密
- 消息身份验证

广告 FS 使用 Schannel.dll 执行其安全通信交互。  当前广告 FS 支持的所有协议和密码套件 Schannel.dll 支持。

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>管理 TLS 月 SSL 协议和密码套件
> [!IMPORTANT]
> 此部分中包含的说明如何修改注册表的步骤。 然而，如果你错误地修改注册表，则可能发生严重的问题。 因此，请确保你认真遵循这些步骤。 
> 
> 请注意，更改默认 SCHANNEL 的安全设置可能 break 或阻止特定的客户端和服务器之间进行通信。  如果需要安全通信，并且没有协商与通信的协议将导致该问题。
> 
> 如果你要将这些更改的应用，他们必须广告 FS 服务器你电场的日落中的所有应用。  在这些更改的应用后，才能重新启动。

在今天天和年龄、 强化服务器和删除较旧或弱密码套件变得越来越许多组织的主要优先级。  软件套件有，将你服务器的测试和提供有关这些协议和套件详细的信息。  为了保持兼容或获得安全等级，删除或禁用较弱的协议或密码套件已成为必须。  本文的其余内容将如何启用或禁用的某些协议和密码套件提供指南。

以下注册表项都位于同一个位置： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols。  使用 regedit 或 PowerShell 启用或禁用这些协议和密码套件。

![注册表位置](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>启用和禁用 SSL 2.0
使用以下注册表项，其值启用和禁用 SSL 2.0。

### <a name="enable-ssl-20"></a>启用 SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server]"DisabledByDefault"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client]"DisabledByDefault"= dword:00000000 

### <a name="disable-ssl-20"></a>禁用 SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server]"DisabledByDefault"= dword:00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client]"DisabledByDefault"= dword:00000001

### <a name="using-powershell-to-disable-ssl-20"></a>使用 PowerShell 禁用 SSL 2.0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>启用和禁用 SSL 3.0
使用以下注册表项，其值启用和禁用 SSL 3.0。

### <a name="enable-ssl-30"></a>启用 SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]"DisabledByDefault"= dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client]"DisabledByDefault"= dword:00000000 

### <a name="disable-ssl-30"></a>禁用 SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]"DisabledByDefault"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client]"DisabledByDefault"= dword:00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>使用 PowerShell 禁用 SSL 3.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>启用和禁用 TLS 1.0
使用以下注册表项，其值启用和禁用 TLS 1.0。

> [!IMPORTANT]
> 禁用 TLS 1.0 将中断 WAP 广告 FS 信任。  如果你禁用 TLS 1.0 应强 auth 启用你的应用程序。  请参阅[启用强的身份验证](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>启用 TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]"DisabledByDefault"= dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]"DisabledByDefault"= dword:00000000 

### <a name="disable-tls-10"></a>禁用 TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]"DisabledByDefault"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]"DisabledByDefault"= dword:00000001 

### <a name="using-powershell-to-disable-tls-10"></a>使用 PowerShell 禁用 TLS 1.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>启用和禁用 TLS 1.1
使用以下注册表项，其值启用和禁用 TLS 1.1。

### <a name="enable-tls-11"></a>启用 TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]"DisabledByDefault"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]"DisabledByDefault"= dword:00000000

### <a name="disable-tls-11"></a>禁用 TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]"DisabledByDefault"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]"DisabledByDefault"= dword:00000001 

### <a name="using-powershell-to-disable-tls-11"></a>使用 PowerShell 禁用 TLS 1.1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>启用和禁用 TLS 1.2

使用以下注册表项，其值启用和禁用 TLS 1.2。

### <a name="enable-tls-12"></a>启用 TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]"DisabledByDefault"= dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]"DisabledByDefault"= dword:00000000

### <a name="disable-tls-12"></a>禁用 TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]"DisabledByDefault"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]"DisabledByDefault"= dword:00000001

### <a name="using-powershell-to-disable-tls-12"></a>使用 PowerShell 禁用 TLS 1.2

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

使用以下注册表项，其值启用和禁用 RC4。  此密码套件注册表项位于以下位置：

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![注册表位置](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>启用 RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128]"启用"= dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128]"启用"= dword:00000001 

### <a name="disable-rc4"></a>禁用 RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128]"启用"= dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128]"启用"= dword:00000000 

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

您可以通过删除 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\0010002 禁用某些特定密码 

![注册表位置](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

若要启用编码器套件，它是对功能多字符串值密钥字符串值。  例如，如果我们想要启用 TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521 然后我们会将其添加到字符串。

有关支持的密码的完整列表套件看到[TLS 月 SSL (Schannel SSP) 中的密码套件](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)。  本文提供了套件启用的默认值，也会受支持，但未启用默认情况下表。  来优先安排要密码套件看到[排列优先顺序 Schannel 密码套件](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)。

## <a name="enabling-strong-authentication-for-net-applications"></a>有关.NET 应用启用强身份验证
.NET Framework 3.5/4.0/4.5.x 应用程序可通过启用 SchUseStrongCrypto 注册表项可以切换到 TLS 1.2 默认协议。  此注册表项将强制.NET 应用程序使用 TLS 1.2。

> [!IMPORTANT]
> 你需要在 Windows Server 2016 和 Windows Server 2012 R2 上广告 fs 使用.NET Framework 4.5.x 4.0/键： HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\。NETFramework\v4.0.30319


对于.NET Framework 3.5 使用以下注册表项：

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\。NETFramework\v2.0.50727]"SchUseStrongCrypto"= dword:00000001

.NET Framework 4.5.x 4.0/使用以下注册表项： HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\。NETFramework\v4.0.30319"SchUseStrongCrypto"= dword:00000001

![强大的身份验证](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>其他信息

- [密码套件中 TLS 月 SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)
- [在 Windows 8.1 中 TLS 密码套件](https://msdn.microsoft.com/en-us/library/windows/desktop/mt767781.aspx)
- [设置优先级 Schannel 密码套件](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)
- [密码和其他 Enigmatic 语言所编写说话](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
