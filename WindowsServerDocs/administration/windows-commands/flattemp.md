---
title: flattemp
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fc14a6fe1a355f7c20c130fba3fb1f17e49b6f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872928"
---
# <a name="flattemp"></a>flattemp

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

启用或禁用单层临时文件夹。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
flattemp {/query | /enable | /disable}
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/query|查询的当前设置。|
|/enable|启用单层临时文件夹。 用户将共享的临时文件夹，除非临时文件夹驻留在用户主文件夹中。|
|/disable|禁用单层临时文件夹。 每个用户临时文件夹将驻留在单独的文件夹 （由用户的会话 ID）。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **Flattemp**已在运行 Windows Server 2008 R2 的计算机上运行 Windows Server 2008 或 rd 会话主机角色服务的计算机上安装终端服务器角色服务时，命令才可用。
-   必须具有管理凭据才能运行**flattemp**。
-   每个用户都有唯一的临时文件夹后，使用**flattemp /enable**启用单层临时文件夹。
-   创建临时文件夹 （通常由指向 TEMP 和 TMP 环境变量） 的多个用户的默认方法是创建子文件夹中的**\Temp**文件夹中的，通过使用 logonID 作为子文件夹名称。 例如，如果 TEMP 环境变量指向 C:\Temp，分配给用户 logonID 4 的临时文件夹是 C:\Temp\4。 使用**flattemp**，可以直接指向 \Temp 文件夹并防止形成子文件夹。 当你想要包含在主文件夹中，无论它们是在 rd 会话主机服务器的本地驱动器或共享的网络驱动器上的用户临时文件夹时，这很有用。 应使用**flattemp /enable**命令仅当每个用户具有单独的临时文件夹时。
-   如果用户的临时文件夹位于网络驱动器上，可能会遇到应用程序错误。 暂时无法访问网络上的共享的网络驱动器后，将发生这种情况。 应用程序的临时文件是不可访问或不同步，因为其响应就好像该磁盘已停止。 不建议将临时文件夹移到网络驱动器。 默认设置是保留在本地硬盘上的临时文件夹。 如果您遇到意外的行为或与特定应用程序的磁盘损坏错误，稳定网络，或将临时文件夹移回本地硬盘。
-   如果使用单独的临时文件夹的每个会话，禁用**flattemp**设置将被忽略。 远程桌面服务配置工具中设置此选项。

## <a name="BKMK_examples"></a>示例
-   若要显示单层临时文件夹的当前设置，请键入：
    ```
    flattemp /query
    ```
-   若要启用平面临时文件夹，请键入：
    ```
    flattemp /enable
    ```
-   若要禁用单层临时文件夹，请键入：
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)

[远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
