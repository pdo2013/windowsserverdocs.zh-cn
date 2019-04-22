---
title: 连接到远程计算机
description: 本文介绍如何从文件服务器资源管理器连接到远程计算机以管理存储资源
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 93d2be926437b65ed8eb84a828ea0d7da6a51086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818858"
---
# <a name="connect-to-a-remote-computer"></a>连接到远程计算机 

> 适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

若要管理远程计算机上的存储资源，你可以从文件服务器资源管理器连接到该计算机。 在连接状态下，你可以通过文件服务器资源管理器，借助这些远程资源管理配额、屏蔽文件、管理分类、安排文件管理任务及生成报告。

> [!Note]
> 文件服务器资源管理器可以管理本地计算机或远程计算机上的资源，但不能同时进行管理。

## <a name="to-connect-to-a-remote-computer-from-file-server-resource-manager"></a>若要从文件服务器资源管理器连接到远程计算机，请执行以下操作：

1.  在**管理工具**中，单击**文件服务器资源管理器**。

2.  在控制台树中，右键单击**文件服务器资源管理器**，然后单击**连接到另一台计算机**。

3.  单击**连接到另一台计算机**对话框中的**另一台计算机**。 然后键入要连接到的服务器的名称（或者单击**浏览**搜索远程计算机）。

4.  单击 **“确定”**。

> [!Important]
> 只有在从**管理工具**打开文件服务器资源管理器后，**连接到另一台计算机**的命令才可用。 当从服务器管理器中访问文件服务器资源管理器时，命令不可用。

## <a name="additional-considerations"></a>其他注意事项

若要使用文件服务器资源管理器管理远程资源：

-   必须使用属于远程计算机上的**管理员**组成员的域帐户登录本地计算机。
-   远程计算机必须正在运行 Windows Server，并且必须安装文件服务器资源管理器。
-   远程计算机上的**远程文件服务器资源管理器管理**例外必须处于启用状态。 通过使用“控制面板”中的 Windows 防火墙启用此例外。

## <a name="see-also"></a>请参阅

-   [管理远程存储资源](managing-remote-storage-resources.md)