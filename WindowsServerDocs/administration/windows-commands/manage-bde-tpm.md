---
title: 管理 bde tpm
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f2b8390d49257c3c6acd9ed4fd45773e5a65aac4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840578"
---
# <a name="manage-bde-tpm"></a>管理 bde: tpm

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

> [!IMPORTANT]
> 此命令不支持在运行 Windows 8 和 Windows Server 2012 或更高版本操作系统的计算机上使用。 对于这些计算机，你可以使用[TPM 管理适用于 Windows PowerShell cmdlet](https://technet.microsoft.com/library/jj603116.aspx)。
如果在运行 Windows 7 或 Windows Server 2008 的计算机上使用此命令，仍可以配置计算机的受信任的平台模块 (TPM) 使用此命令。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。
## <a name="syntax"></a>语法
```
manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|-打开|启用和激活 TPM，TPM 所有者密码设置。 此外可以使用 **-t**作为此命令的简化版本。|
|-takeownership|通过设置所有者密码，采用 TPM 的所有权。 此外可以使用 **-o**作为此命令的简化版本。|
|<OwnerPassword>|表示指定的 TPM 所有者密码。|
|-computername|指定将使用该管理 bde.exe 修改另一台计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。|
|<Name>|表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|显示简短的命令提示符处的帮助。|
|-help 或-h|显示完整的命令提示符处的帮助。|
## <a name="BKMK_Examples"></a>示例
下面的示例演示如何使用**tpm**命令来打开 TPM。
```
manage-bde  tpm -turnon
```
下面的示例演示如何使用 * * tpm * * 命令取得 TPM 所有权并将所有者密码设置为0wnerP@ss。
```
manage-bde  tpm  takeownership 0wnerP@ss
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
