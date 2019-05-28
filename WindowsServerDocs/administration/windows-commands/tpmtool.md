---
title: tpmtool
description: Windows 命令 tpmtool-主题获取有关受信任的平台模块的信息。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: d2939b693c3f9bd8d0d8c8e1fefdd5d8f892e4c9
ms.sourcegitcommit: 3582c38d87f23cc54467d63bf00c29ef07cdb7c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517190"
---
>[!IMPORTANT]
>有些信息与预发布产品相关，这些产品在商业发行之前可能发生重大更改。 Microsoft 不对此处提供的信息作任何明示或默示的担保。

# <a name="tpmtool"></a>tpmtool

此实用程序可用于获取其相关信息[受信任的平台模块 (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview)。

有关如何使用此命令的示例，请参阅[示例](#tpmtool_examples)。

## <a name="syntax"></a>语法

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|getdeviceinformation|显示 TPM 的基本信息。|
|gatherlogs [输出目录路径]|收集的 TPM 日志并将其置于指定目录中。 默认情况下，将它们放在当前目录中。|
|/?|在命令提示符下显示帮助。|

## <a name="tpmtool_examples"></a>示例

若要显示的 TPM 的基本信息，请键入：
```
tpmtool getdeviceinformation
```
为收集的 TPM 日志并将其放在当前目录中，类型：
```
tpmtool gatherlogs
```
为收集 TPM 日志，并将其放置在`C:\Users\Public`，类型：
```
tpmtool gatherlogs C:\Users\Public
```
