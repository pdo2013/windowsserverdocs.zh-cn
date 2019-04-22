---
title: msdt
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 781e83615d44a4adb20fd85fe5ce6ebc7c2d4966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818828"
---
# <a name="msdt"></a>msdt



调用故障排除的包，在命令行或作为一部分的自动执行的脚本，并提供了无需用户输入的附加选项。

## <a name="syntax"></a>语法

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parameters

下表包含的参数和 msdt.exe 支持选项。

|参数|描述|
|---------|-----------|
|/id\<包名称 >|指定要运行的诊断程序包。 有关可用包的列表，请参阅"可用故障排除程序包中疑难解答包 ID？ 本主题后面的部分。|
|/path \<directory | .diagpkg 文件 | .diagcfg 文件 >|指定诊断程序包的完整路径。 如果指定目录，该目录必须包含诊断程序包。 不能结合使用 /path 参数 */i d*， */dci*，或 */cab*参数。|
|/dci \<passkey>|预填充 msdt 中的密钥字段。 支持提供商已提供密钥时，才使用此参数。|
|/dt \<directory>|显示故障排除历史记录中指定的目录。 诊断结果存储在用户的 **%LOCALAPPDATA%\Diagnostics**或 **%LOCALAPPDATA%\ElevatedDiagnostics**目录。|
|/af\<答案文件 >|指定包含对一个或多个诊断交互的响应的 XML 格式的应答文件。|
