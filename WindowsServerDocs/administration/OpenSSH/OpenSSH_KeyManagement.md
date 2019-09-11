---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH，SSH，SSHD，安装，安装程序
contributor: maertendMSFT
author: maertendMSFT
title: 适用于 Windows 的 OpenSSH 服务器配置
ms.product: w10
ms.openlocfilehash: ed9f3653c79f1329b1334f52fe14c1184bc99539
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866868"
---
# <a name="openssh-key-management"></a>OpenSSH 密钥管理

Windows 环境中的大多数身份验证均使用用户名-密码对完成。
这适用于共享公共域的系统。 当跨域（例如在本地和云托管的系统之间）工作时，这会变得更加困难。

相比之下，Linux 环境通常使用公钥/私钥对来驱动身份验证。
OpenSSH 包含用于帮助支持此操作的工具，具体如下：

* 用于生成安全密钥的__ssh-keygen__
* __ssh-代理__和__ssh-添加__安全存储私钥
* 在初始使用服务器的过程中， __scp__和__sftp__可安全复制公钥文件

本文档概述了如何在 Windows 上使用这些工具开始使用 SSH 进行密钥身份验证。 如果你不熟悉 SSH 密钥管理，我们强烈建议你查看[NIST 文档 IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) ，其标题为 "使用安全外壳（SSH）进行交互式和自动访问管理的安全性"。

## <a name="about-key-pairs"></a>关于密钥对

密钥对指的是特定身份验证协议使用的公钥和私钥文件。 

SSH 公钥身份验证使用非对称加密算法来生成两个密钥文件–一个 "专用"，另一个 "公用"。 私钥文件等效于密码，并应在所有情况下受到保护。 如果有人获取了私钥，他们可以登录到您有权访问的任何 SSH 服务器。 公钥是放置在 SSH 服务器上的，并且可以在不影响私钥的情况下共享。

将密钥身份验证与 SSH 服务器一起使用时，SSH 服务器和客户端会比较为私钥提供的用户名的公钥。 如果无法根据客户端私钥验证公钥，则身份验证将失败。 

多重身份验证可通过密钥对实现，方法是要求在生成密钥对时提供密码（请参阅下面的密钥生成）。 在身份验证期间，系统会提示用户输入通行短语，该密码与 SSH 客户端上的私钥一起使用以对用户进行身份验证。 

## <a name="host-key-generation"></a>主机密钥生成

公钥具有特定的 ACL 要求，在 Windows 上，它等同于只允许访问管理员和系统。 若要简化此过程， 

* 已创建 OpenSSHUtils PowerShell 模块，以便正确设置密钥 Acl，并且应在服务器上安装
* 首次使用 sshd 时，将自动生成主机的密钥对。 如果 ssh 代理正在运行，则密钥将自动添加到本地存储中。 

若要使用 SSH 服务器轻松进行密钥身份验证，请在权限提升的 PowerShell 提示符下运行以下命令：

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

由于没有与 sshd 服务关联的用户，因此主机密钥存储在 \ProgramData\ssh. 下。


## <a name="user-key-generation"></a>用户密钥生成

若要使用基于密钥的身份验证，首先需要为客户端生成一些公钥/私钥对。 从 PowerShell 或 cmd，使用 ssh-keygen 生成一些密钥文件。

```powershell
cd ~\.ssh\
ssh-keygen
```

这应显示类似于下面的内容（其中 "username" 替换为你的用户名）

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

可以按 Enter 以接受默认值，或指定要在其中生成密钥的路径。 此时，系统会提示你使用密码来加密你的私钥文件。
密码使用密钥文件来提供双因素身份验证。 在此示例中，我们将密码留空。 

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in C:\Users\username\.ssh\id_ed25519.
Your public key has been saved in C:\Users\username\.ssh\id_ed25519.pub.
The key fingerprint is: 
SHA256:OIzc1yE7joL2Bzy8!gS0j8eGK7bYaH1FmF3sDuMeSj8 username@server@LOCAL-HOSTNAME

The key's randomart image is:
+--[ED25519 256]--+
|        .        |
|         o       |
|    . + + .      |
|   o B * = .     |
|   o= B S .      |
|   .=B O o       |
|  + =+% o        |
| *oo.O.E         |
|+.o+=o. .        |
+----[SHA256]-----+
```

现在，你有一个公共/私有 ED25519.PUB 密钥对（pub 文件是公钥，而其余的是私钥）：

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

请记住，私钥文件等效于密码应采用的相同方式来保护密码。
若要帮助解决此情况，请使用 ssh 代理将私钥安全地存储在与 Windows 登录名关联的 Windows 安全上下文中。 为此，请以管理员身份启动 ssh 代理服务，并使用 ssh 添加来存储私钥。 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

完成这些步骤后，每次从此客户端进行身份验证时，ssh 代理会自动检索本地私钥，并将其传递到 SSH 客户端。

> [!NOTE]
> 强烈建议你将私钥备份到安全位置，然后在将其添加到 ssh 代理*后*将其从本地系统中删除。
> 无法从代理中检索私钥。
> 如果你失去了对私钥的访问权限，则必须创建新的密钥对并在你与之交互的所有系统上更新公钥。

## <a name="deploying-the-public-key"></a>部署公钥

若要使用上面创建的用户密钥，需要将公钥放置在服务器上，将其放在 users\username\ssh. 下名为*authorized_keys*的文本文件中。 OpenSSH 工具包括 scp，它是一种安全的文件传输实用程序，可帮助解决此情况。

若要将公钥内容（~\.ssh\id_ed25519.pub）移到服务器/主机上名为 authorized_keys\.的文本文件中。

此示例使用前面说明中已安装在主机上的 OpenSSHUtils 模块中的 AuthorizedKeyPermissions 函数。

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

这些步骤完成了在 Windows 上通过 SSH 使用基于密钥的身份验证所需的配置。
完成此项后，用户可以从具有私钥的任何客户端连接到 sshd 主机。

