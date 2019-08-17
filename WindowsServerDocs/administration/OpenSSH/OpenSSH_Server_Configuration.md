---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, 安装, 安装程序
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: 适用于 Windows 的 OpenSSH 服务器配置
ms.openlocfilehash: ed424c33c4cd2c19a9b5e985ab6083bcbcb9fbdc
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546263"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>适用于 Windows 10 1809 和服务器2019的 OpenSSH 服务器配置

本主题介绍适用于 OpenSSH 服务器的特定于 Windows 的配置 (sshd)。 

OpenSSH 维护[OpenSSH.com](https://www.openssh.com/manual.html)上联机配置选项的详细文档, 这在此文档集中不重复。 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>在 Windows 中为 OpenSSH 配置默认 shell

默认命令 shell 提供用户使用 SSH 连接到服务器时看到的体验。 初始默认窗口是 Windows 命令行界面 (cmd.exe)。 Windows 还包括 PowerShell 和 Bash, 第三方命令行界面也可用于 Windows, 并可配置为服务器的默认外壳。

若要设置默认命令行界面, 请首先确认 OpenSSH 安装文件夹位于系统路径上。 对于 Windows, 默认安装文件夹为 SystemDrive: WindowsDirectory\System32\openssh。 以下命令显示了当前路径设置, 并向其添加了默认的 OpenSSH 安装文件夹。 

命令行界面 | 要使用的命令
------------- | -------------- 
Command | path
PowerShell | $env:p a

在 Windows 注册表中配置默认的 ssh shell 是在 Windows 注册表中完成的, 方法是将 shell 可执行文件的完整路径添加到字符串值 DefaultShell 中的 Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH。 

例如, 以下 Powershell 命令将默认 shell 设置为 ngen.exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Sshd_config 中的 Windows 配置 

在 Windows 中, 默认情况下, sshd 从%programdata%\ssh\sshd_config 读取配置数据, 或者可以通过使用-f 参数启动 sshd 来指定不同的配置文件。
如果该文件不存在, 则在启动该服务时, sshd 将生成一个具有默认配置的。

下面列出的元素通过 sshd_config 中的条目提供可能的 Windows 特定配置。 此处未列出的其他配置设置可能不会在本文中列出, 因为联机[Win32 OpenSSH 文档](https://github.com/powershell/win32-openssh/wiki)中详细介绍了这些设置。 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

使用 AllowGroups、AllowUsers、DenyGroups 和 DenyUsers 指令控制可以连接到服务器的用户和组。 按以下顺序处理 allow/deny 指令:DenyUsers、AllowUsers、DenyGroups 和 finally AllowGroups。 必须以小写形式指定所有帐户名称。 有关通配符模式的详细信息, 请参阅 ssh_config 中的模式。

当使用域用户或组配置基于用户/组的规则时, 请使用以下格式``` user?domain* ```:。
Windows 允许多种格式来指定域主体, 但许多与标准 Linux 模式发生冲突。 为此, 请将 * 添加到涵盖 Fqdn。 此外, 此方法使用 "？" (而不是 @) 来避免与username@host格式冲突。 

工作组用户/组和连接到 internet 的帐户始终解析为其本地帐户名称 (不包括域部分, 类似于标准 Unix 名称)。 域用户和组严格地解析为[NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format)格式-domain_short_name\user_name。 所有基于用户/组的配置规则都需要遵循此格式。

域用户和组的示例 

```
DenyUsers contoso\admin@192.168.2.23 : blocks contoso\admin from 192.168.2.23
DenyUsers contoso\* : blocks all users from contoso domain
AllowGroups contoso\sshusers : only allow users from contoso\sshusers group
```

本地用户和组的示例 

```
AllowUsers localuser@192.168.2.23
AllowGroups sshusers
```

### <a name="authenticationmethods"></a>AuthenticationMethods 

对于 Windows OpenSSH, 唯一可用的身份验证方法是 "密码" 和 "publickey"。

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

默认值为 "ssh/authorized_keys/authorized_keys2"。 如果路径不是绝对路径, 则将其与用户的主目录 (或配置文件图像路径) 相对应。 例如. c:\users\ 用户名.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (在 v 7.7.0.0 中添加了支持)

仅在 sftp 会话中支持此指令。 到 cmd.exe 的远程会话并不服从这一点。 若要设置仅限 sftp 的 chroot 服务器, 请将 ForceCommand 设置为内部 sftp。 还可以通过实现仅允许使用 scp 和 sftp 的自定义 shell, 来设置 chroot。

### <a name="hostkey"></a>HostKey

默认值为% programdata%/ssh/ssh_host_ecdsa_key% programdata%/ssh/ssh_host_ed25519_key 和% programdata%/ssh/ssh_host_rsa_key。 如果默认值不存在, 则 sshd 会在服务启动时自动生成这些默认值。

### <a name="match"></a>匹配

请注意, 本部分中的模式规则。 用户和组的名称应为小写。

### <a name="permitrootlogin"></a>PermitRootLogin

在 Windows 中不适用。 若要防止管理员登录, 请使用具有 DenyGroups 指令的管理员。

### <a name="syslogfacility"></a>SyslogFacility

如果需要基于文件的日志记录, 请使用 LOCAL0。 日志是在%programdata%\ssh\logs. 下生成的。
任何其他值 (包括默认值 "身份验证") 会将日志记录定向到 ETW。 有关详细信息, 请参阅 Windows 中的日志记录功能。

### <a name="not-supported"></a>不支持 

Windows Server 2019 和 Windows 10 1809 随附的 OpenSSH 版本中不提供以下配置选项:

* AcceptEnv
* AllowStreamLocalForwarding
* AuthorizedKeysCommand
* AuthorizedKeysCommandUser
* AuthorizedPrincipalsCommand
* AuthorizedPrincipalsCommandUser
* 压缩
* ExposeAuthInfo
* GSSAPIAuthentication
* GSSAPICleanupCredentials
* GSSAPIStrictAcceptorCheck
* HostbasedAcceptedKeyTypes
* HostbasedAuthentication
* HostbasedUsesNameFromPacketOnly
* IgnoreRhosts
* IgnoreUserKnownHosts
* KbdInteractiveAuthentication
* KerberosAuthentication
* KerberosGetAFSToken
* KerberosOrLocalPasswd
* KerberosTicketCleanup
* PermitTunnel
* PermitUserEnvironment
* PermitUserRC
* PidFile
* PrintLastLog
* RDomain
* StreamLocalBindMask
* StreamLocalBindUnlink
* StrictModes
* X11DisplayOffset
* X11Forwarding
* X11UseLocalhost
* XAuthLocation

