---
ms.date: 01/07/2019
contributor: damaerteMSFT
author: maertendMSFT
keywords: OpenSSH, SSH, Win32-OpenSSH
title: 在 Windows 中的 OpenSSH
ms.product: w10
ms.openlocfilehash: 68ced1ff133495d7658e486e7e72321e18857b21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843358"
---
# <a name="openssh-in-windows"></a>在 Windows 中的 OpenSSH

OpenSSH 是用于 Linux 的管理员和其他非 Windows 的远程系统的跨平台管理的安全外壳 (SSH) 工具的开放源代码版本。 OpenSSH 已添加到 Windows 截至 2018 年秋季，且包含在 Windows 10 和 Windows Server 2019。 

SSH 基于其中用户正在处理的系统是客户端，正在管理的远程系统是在服务器的客户端-服务器体系结构。 OpenSSH 包括一系列组件和工具，旨在提供远程系统管理、 安全且简单的方法包括：

* sshd.exe，这是必须在远程管理的系统运行的 SSH 服务器组件 
* ssh.exe，这是用户的本地系统运行的 SSH 客户端组件
* ssh keygen.exe 生成、 管理和转换适用于 SSH 的身份验证密钥 
* ssh agent.exe 将用于公钥身份验证的专用密钥存储
* ssh add.exe 将私钥添加到允许服务器使用的列表
* 中从多个主机收集的 SSH 主机公钥的 ssh keyscan.exe 辅助工具
* sftp.exe 是提供安全文件传输协议，并通过 SSH 运行的服务
* scp.exe 是 SSH 运行一个文件复制实用程序

在本部分文档重点介绍上 Windows，包括安装和特定于 Windows 的配置和使用情况下使用 OpenSSH 的方式。 以下是的主题：
* 安装和卸载 Windows Server 2019 和 Windows 10 的 OpenSSH

常见 OpenSSH 功能的更多详细的文档提供了在线[OpenSSH.com](https://www.openssh.com/manual.html)。 

主[OpenSSH 开源项目](https://www.openssh.com)由开发者在最初是 OpenBSD 项目管理。 此项目的 Microsoft 分叉处于[Github](https://github.com/PowerShell/openssh-portable)。
在 Windows OpenSSH 的反馈欢迎，可以通过创建 Github 问题中的提供我们[OpenSSH Github 存储库](https://github.com/PowerShell/openssh-portable)。 
