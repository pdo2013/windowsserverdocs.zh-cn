---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH、 SSH、 SSHD，安装，安装程序
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: 用于 Windows OpenSSH 服务器配置
ms.openlocfilehash: 7eff3d3e1af67c9daf7a68c67c3609c0ee89fc93
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280038"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>用于 Windows 10 1809年和 Server 2019 # OpenSSH 服务器配置

本主题介绍特定于 Windows 的服务器的配置的 OpenSSH (sshd)。 

OpenSSH 维护详细的文档中的配置选项联机处[OpenSSH.com](https://www.openssh.com/manual.html)，这不会复制本文档集中。 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>在 Windows 中的 OpenSSH 为配置的默认 shell

默认命令行界面提供连接到使用 SSH 服务器时，会看到用户体验。 初始默认 Windows 是 Windows 命令 shell (cmd.exe)。 Windows 还包括 PowerShell 和 Bash，和第三方命令外壳，还提供了 Windows 和可能配置为在服务器的默认外壳。

若要设置默认命令行界面，请首先确认 OpenSSH 安装文件夹位于系统路径。 对于 Windows，默认安装文件夹是 SystemDrive:WindowsDirectory\System32\openssh。 以下命令显示当前路径设置，并向其添加默认 OpenSSH 安装文件夹。 

命令行界面 | 若要使用的命令
------------- | -------------- 
Command | path
PowerShell | $env： 路径

配置默认的 ssh 命令行程序是通过 Windows 注册表中添加到 shell 中的字符串值 DefaultShell Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH 到可执行的完整路径。 

例如，以下 Powershell 命令设置为 PowerShell.exe 的默认 shell:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshdconfig"></a>在 sshd_config Windows 配置 

在 Windows，sshd 默认情况下，从 %programdata%\ssh\sshd_config 读取配置数据或可通过启动 sshd.exe 使用-f 参数指定不同的配置文件。
如果该文件不存在，sshd 启动该服务时生成一个默认配置。

下面列出的元素提供特定于 Windows 的配置可能的通过 sshd_config 中的条目。 有的其他配置设置可以在此处未列出如联机中详细介绍了他们[Win32 OpenSSH 文档](https://github.com/powershell/win32-openssh/wiki)。 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups，AllowUsers，DenyGroups DenyUsers 

控制哪些用户和组可以连接到服务器完成使用 AllowGroups、 AllowUsers、 DenyGroups 和 DenyUsers 指令。 允许/拒绝指令按以下顺序进行处理：DenyUsers、 AllowUsers、 DenyGroups，和最后 AllowGroups。 所有帐户名称必须以小写形式都指定。 请参阅中的通配符模式的详细信息的 ssh_config 的模式。

当配置用户/组基于与域用户或组的规则时，请使用以下格式： ``` user?domain* ```。
Windows 允许的格式的多个用于指定域主体，但许多与标准 Linux 模式发生冲突。 为此，* 添加以涵盖 Fqdn。 此外，此方法使用"？"，而不是 @，以避免与冲突username@host格式。 

工作组用户/组和连接到 internet 的客户始终解析为其本地帐户名称 （没有域部分，类似于标准 Unix 名称） 中。 域用户和组将严格解析为[NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format)格式-domain_short_name\user_name。 所有用户/组都基于规则需要符合此格式的配置。

为域用户和组的示例 

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

对于 Windows OpenSSH 的唯一可用的身份验证方法是"密码"和"公钥"。

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

默认值为".ssh/authorized_keys.ssh/authorized_keys2"。 如果该路径不是绝对的它会相对于用户的主目录 （或配置文件图像路径）。 例如， c:\users\user。

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory （v7.7.0.0 中添加的支持）

与 sftp 会话仅支持此指令。 在 cmd.exe 的远程会话不会遵循此。 若要设置的仅限 sftp chroot 服务器，设置到内部 sftp ForceCommand。 您也可以设置 scp chroot，与通过实现的自定义外壳程序只允许 scp 和 sftp。

### <a name="hostkey"></a>HostKey

默认值为 %programdata%/ssh/ssh_host_ecdsa_key、 %programdata%/ssh/ssh_host_ed25519_key 和 %programdata%/ssh/ssh_host_rsa_key。 如果默认值不存在，请将 sshd 自动生成这些在服务启动。

### <a name="match"></a>匹配

请注意，模式在本部分中的规则。 用户和组名称应以小写形式。

### <a name="permitrootlogin"></a>PermitRootLogin

在 Windows 中不适用。 若要防止管理员登录名，使用 DenyGroups 指令中的管理员。

### <a name="syslogfacility"></a>SyslogFacility

如果需要基于文件的日志记录，使用 LOCAL0。 在 %programdata%\ssh\logs 下生成的日志。
任何其他值，包括身份验证的默认值将定向到 ETW 日志记录。 有关详细信息，请参阅 Windows 中的日志记录功能。

### <a name="not-supported"></a>不支持 

以下配置选项不提供在 Windows Server 2019 和 Windows 10 1809年的 OpenSSH 版本中可用：

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

