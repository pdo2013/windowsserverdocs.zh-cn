---
title: manage-bde tpm
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 577f5f2ecb85ac8c0c28fef2ca343635796454d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373826"
---
# <a name="manage-bde-tpm"></a>manage-bde： tpm

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012
> 
> [!IMPORTANT]
> 此命令不支持在运行 Windows 8、Windows Server 2012 或更高版本操作系统的计算机上使用。 对于这些计算机，你可以使用[适用于 Windows PowerShell 的 TPM 管理 cmdlet](https://docs.microsoft.com/powershell/module/trustedplatformmodule/)。
> 如果在运行 Windows 7 或 Windows Server 2008 的计算机上使用此命令，则仍可使用此命令配置计算机的受信任的平台模块（TPM）。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。
> ## <a name="syntax"></a>语法
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> ### <a name="parameters"></a>Parameters
> 
> |    参数    |                                                                              描述                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -turnon     |              启用和激活 TPM，允许设置 TPM 所有者密码。 你还可以使用 **-t**作为此命令的缩写形式。              |
> | -takeownership  |                      通过设置所有者密码来取得 TPM 的所有权。 你还可以使用 **-o**作为此命令的缩写形式。                       |
> | <OwnerPassword> |                                                      表示你为 TPM 指定的所有者密码。                                                       |
> |  -computername  | 指定 manage-bde.exe 将用于修改另一台计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。 |
> |     <Name>      |    表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。     |
> |    -? 或 /?     |                                                               在命令提示符下显示 brief help。                                                               |
> |   -help 或-h   |                                                             在命令提示符下显示完整的帮助。                                                              |
> 
> ## <a name="BKMK_Examples"></a>示例
> 下面的示例演示如何使用 **-tpm**命令打开 tpm。
> ```
> manage-bde  tpm -turnon
> ```
> 下面的示例演示如何使用**tpm**命令获取 tpm 的所有权，并将所有者密码设置为 0wnerP@ss。
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>其他参考
> -   [命令行语法项](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
