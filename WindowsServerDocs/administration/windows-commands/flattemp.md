---
title: flattemp
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b1a458e8742ca354eeca821e93590386bca56dff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377126"
---
# <a name="flattemp"></a>flattemp

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

启用或禁用平面临时文件夹。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>语法
```
flattemp {/query | /enable | /disable}
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/query|查询当前设置。|
|/enable|启用单层临时文件夹。 用户将共享临时文件夹，除非临时文件夹位于用户的主文件夹中。|
|/disable|禁用平面临时文件夹。 每个用户的临时文件夹将驻留在单独的文件夹中（由用户的会话 ID 决定）。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   仅当你在运行 windows server 2008 的计算机上安装了终端服务器角色服务或运行 Windows Server 2008 R2 的计算机上的 rd 会话主机角色服务时， **flattemp**命令才可用。
-   您必须具有管理凭据才能运行**flattemp**。
-   每个用户都有一个唯一的临时文件夹后，使用**flattemp/enable**启用单层临时文件夹。
-   用于创建多个用户（通常由 TEMP 和 TMP 环境变量指向）的临时文件夹的默认方法是在 **\temp**文件夹中创建子文件夹，方法是使用 logonID 作为子文件夹名称。 例如，如果 TEMP 环境变量指向 C：\Temp，则分配给 user logonID 4 的临时文件夹为 C:\Temp\4。 使用**flattemp**，可以直接指向 \temp 文件夹，防止子文件夹形成。 如果希望将用户临时文件夹包含在主文件夹中（无论是在 rd 会话主机服务器本地驱动器上，还是位于共享的网络驱动器上），这非常有用。 仅当每个用户都有单独的临时文件夹时，才应使用**flattemp/enable**命令。
-   如果用户的临时文件夹位于网络驱动器上，则可能会遇到应用程序错误。 如果网络上的共享网络驱动器暂时无法访问，则会发生这种情况。 由于应用程序的临时文件无法访问或不同步，因此它将作为磁盘停止的响应。 建议不要将临时文件夹移动到网络驱动器。 默认情况下，将临时文件夹保留在本地硬盘上。 如果遇到与某些应用程序有关的意外行为或磁盘损坏错误，请使网络稳定，或将临时文件夹移回本地硬盘。
-   如果禁用每个会话使用单独的临时文件夹，则将忽略**flattemp**设置。 此选项在远程桌面服务配置工具中设置。

## <a name="BKMK_examples"></a>示例
-   若要显示单层临时文件夹的当前设置，请键入：
    ```
    flattemp /query
    ```
-   若要启用平面临时文件夹，请键入：
    ```
    flattemp /enable
    ```
-   若要禁用平面临时文件夹，请键入：
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

[远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
