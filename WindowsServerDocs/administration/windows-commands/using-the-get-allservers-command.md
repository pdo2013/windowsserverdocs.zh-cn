---
title: 使用 get AllServers 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 080836e406b329cf8c15f95ef6afc99973bb3e4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853088"
---
# <a name="using-the-get-allservers-command"></a>使用 get AllServers 命令



检索有关所有 Windows 部署服务服务器的信息。

> [!NOTE]
> 此命令可能需要一段较的长的时间才能完成，如果您的环境中有许多 Windows 部署服务服务器或链接服务器的网络连接速度较慢。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ 显示: {配置 | 映像 | 所有}|指定要返回的信息的类型。</br>-   **配置**返回服务器配置信息。</br>-   **映像**返回服务器上的映像组、 启动映像和安装映像信息。</br>-   **所有**返回服务器配置和映像信息。|
|[/ 详细]|当结合使用时 **/Show:Images**或 **/Show:All**，返回所有图像中的每个图像元数据。 如果 **/详细**不指定选项，默认行为是返回的映像名称、 说明和文件名称。|
|[/ 林: {是 | No}]|指定是否要返回其整个林或本地的域的信息。 如果未指定此选项的值，默认行为是返回服务器中本地域。|

## <a name="BKMK_examples"></a>示例

若要查看所有服务器的信息，请键入：
```
WDSUTIL /Get-AllServers /Show:Config
```
若要查看所有服务器的详细的信息，请键入：
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)