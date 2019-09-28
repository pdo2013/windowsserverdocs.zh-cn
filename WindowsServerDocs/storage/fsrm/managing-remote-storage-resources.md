---
title: 管理远程存储资源
description: 本文介绍了如何管理远程计算机上的存储资源
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5c6dc9c931e130e36e01655de05fbd209f50f3dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394083"
---
# <a name="managing-remote-storage-resources"></a>管理远程存储资源

> 适用于：Windows Server 2019，Windows Server 2016，Windows Server （半年频道），Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

以下两个选项可用于管理远程计算机上的存储资源：

-   可从文件服务器资源管理器 Microsoft<sup>®</sup> 管理控制台 (MMC) 管理单元（随后可用于管理远程资源）连接到远程计算机。
-   使用通过文件服务器资源管理器安装的命令行工具。

无论选择哪个选项，均可借助这些远程资源处理配额、屏蔽文件、管理分类、安排文件管理任务及生成报告。

> [!Note]
> 文件服务器资源管理器可以管理本地计算机或远程计算机上的资源，但不能同时进行管理。

例如，你可以：

-   使用文件服务器资源管理器 MMC 管理单元连接到该域中的另一台计算机，并检查远程计算机上的卷或文件夹的存储使用率。
-   在本地服务器上创建配额和文件屏蔽模板，然后使用命令行工具将这些模板导入位于分支机构的文件服务器中。

本部分包括以下主题：

-   [连接到远程计算机](connect-to-remote-computer.md)
-   [命令行工具](command-line-tools.md)
