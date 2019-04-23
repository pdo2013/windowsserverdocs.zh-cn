---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH、 SSH、 SSHD，安装，安装程序
contributor: maertendMSFT
author: maertendMSFT
title: 用于 Windows OpenSSH 服务器配置
ms.product: w10
ms.openlocfilehash: 3e2981cbbdfe34bf28a77d5121bff0b663e0d3bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837148"
---
# <a name="openssh-key-management"></a>OpenSSH 密钥管理

在 Windows 环境中的大多数身份验证是通过用户名和密码对。
这适用于共享一个公共域的系统。 在处理跨域时，例如在本地和云托管的系统之间变得更加困难。

相比之下，Linux 环境通常使用公共密钥/专用密钥对到驱动器身份验证。
OpenSSH 包括工具来帮助支持此操作，具体而言：

* __ssh keygen__生成安全的密钥
* __ssh 代理__并__ssh 配合使用-添加__用于安全地存储私钥
* __scp__并__sftp__来安全地在一台服务器的初始使用过程中复制公共密钥文件

本文档概述了如何在 Windows 上使用这些工具以开始使用 SSH 密钥身份验证。 如果您不熟悉 SSH 密钥管理，我们强烈建议您查看[NIST 文档 IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf)标题为"交互和自动访问管理使用安全外壳 (SSH) 的安全"。

## <a name="about-key-pairs"></a>有关密钥对

密钥对，请参阅特定身份验证协议使用的公钥和私钥文件。 

SSH 公钥身份验证使用非对称加密算法来生成两个密钥文件： 一个"专用"和其他"public"。 私钥文件相当于密码，并应受保护的所有情况下。 如果有人获取了私钥，他们可以登录为你有权访问的任何 SSH 服务器。 公钥是什么放在 SSH 服务器，并且可能不会危及私有密钥共享。

使用密钥验证时使用 SSH 服务器，SSH 服务器和客户端进行比较的公钥对的私钥提供用户名。 如果公共密钥不能验证客户端的私钥，身份验证失败。 

通过要求提供通行短语生成密钥对时，可能会使用密钥对实现多重身份验证 （请参阅下面的密钥生成）。 在身份验证时提示用户提供通行短语，期间用于 SSH 客户端上的私钥存在以及对用户进行身份验证。 

## <a name="host-key-generation"></a>主机密钥生成

公共密钥有特定 ACL 要求，在 Windows 中，等同与仅允许对管理员和系统的访问。 若要简化此过程， 

* OpenSSHUtils PowerShell 模块已创建，以将项的 Acl 设置正确，并且应在服务器上安装
* 上的 sshd 初次使用时，将自动生成主机的密钥对。 如果 ssh 代理正在运行，密钥将自动添加到本地存储。 

为了方便密钥身份验证与 SSH 服务器，请从提升的 PowerShell 提示符运行以下命令：

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

没有与 sshd 服务相关联的用户，因为主机密钥存储在 \ProgramData\ssh 下。


## <a name="user-key-generation"></a>用户密钥生成

若要使用基于密钥的身份验证，首先需要为你的客户端生成一些公钥/私钥对。 从 PowerShell 或 cmd，使用 ssh keygen 生成某些关键文件。

```powershell
cd ~\.ssh\
ssh-keygen
```

这应会显示类似于下面的 （其中"username"将替换为您的用户名称）

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

您可以按 Enter 以接受默认值，或指定你想要生成你的密钥的路径。 此时，系统会提示使用通行短语来加密你的私钥文件。
通行短语可以处理要提供双因素身份验证的密钥文件。 对于此示例中，我们都保留为空密码。 

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

现在具有的公钥/私钥 ED25519 密钥对 （.pub 文件是公钥以及其他私钥）：

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

请记住，私钥文件密码的等效项应受到保护，相同的方式保护你的密码。
若要帮助解决此问题，请使用 ssh 代理来安全地存储在 Windows 安全上下文中，与 Windows 登录名关联的私钥。 为此，请以管理员身份启动 ssh 代理服务并使用 ssh 配合使用-添加用于存储私钥。 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

完成这些步骤后，每当此客户端身份验证需要私钥 ssh 代理将自动检索本地私有密钥并将其传递到 SSH 客户端。

> [!NOTE]
> 强烈建议您备份你的私匙到安全位置，然后将其删除从本地系统*后*将其添加到 ssh 代理。
> 无法从代理检索私钥。
> 如果您无法访问的私钥，你将必须创建新的密钥对，并更新与交互的所有系统上的公共密钥。

## <a name="deploying-the-public-key"></a>部署的公钥

若要使用上面创建的用户密钥，公共密钥需要在服务器上放置到名为文本文件*authorized_keys* users\username\ssh 下。 OpenSSH 工具包括 scp，它是一个安全文件传输实用工具，以帮助实现此目的。

若要将您的公钥的内容移 (~\.ssh\id_ed25519.pub) 成文本文件调用 authorized_keys ~\.ssh\ 服务器主机上的。

此示例使用在上面的说明中的主机已安装的 OpenSSHUtils 模块中修复 AuthorizedKeyPermissions 函数。

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

这些步骤完成后需使用基于密钥的身份验证和 Windows 上的 SSH 配置。
在此之后，用户可以连接到 sshd 主机从任何客户端具有私钥。

