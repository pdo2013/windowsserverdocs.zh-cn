---
title: 子命令开始-命名空间
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55fe4a6136fe4f8e886dc62fff746a1e5ff1898f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370747"
---
# <a name="subcommand-start-namespace"></a>子命令：起始-命名空间

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012 启动计划强制转换的命名空间。
> ## <a name="syntax"></a>语法
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>Parameters
> 
> |          参数          |                                                                                                                                                                                             描述                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | /Namespace： <Namespace name> | 指定命名空间的名称。 请注意，这不是友好名称，并且必须是唯一的。<br /><br />-   **部署服务器**：命名空间名称的语法为/Namspace： WDS： <Image group> @ no__t @ no__t-2 @ no__t-3 @ no__t-4。 例如：**WDS： ImageGroup1/install/1**<br />-   **传输服务器**：此名称必须与在服务器上创建命名空间时为命名空间指定的名称相匹配。 |
> |   [/Server： @no__t]   |                                                                                                           指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>示例
> 若要启动命名空间，请键入下列内容之一：
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>其他参考
> [命令行语法键](command-line-syntax-key.md)
> [使用 AllNamespaces 命令](using-the-get-allnamespaces-command.md)
>  使用[新的命名空间](using-the-new-namespace-command.md)命令 
> [使用移除命名空间](using-the-remove-namespace-command.md)命令
