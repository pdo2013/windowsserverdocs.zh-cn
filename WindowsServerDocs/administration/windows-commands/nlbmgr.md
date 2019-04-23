---
title: nlbmgr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ed11e702aeae66458f888e454c1bc1d1bc22630
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887078"
---
# <a name="nlbmgr"></a>nlbmgr

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用网络负载平衡管理器，可以配置和管理从一台计算机的网络负载平衡群集和所有群集主机，也可以将群集配置复制到其他主机。 您可以从命令行使用此命令启动网络负载平衡管理器**nlbmgr.exe**，在安装的哪些**systemroot\System32**文件夹。
## <a name="syntax"></a>语法
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/help|在命令提示符下显示帮助。|
|/noping|可防止网络负载平衡管理器执行 ping 操作之前尝试联系它们通过 Windows Management Instrumentation (WMI) 的主机。 如果在所有可用的网络适配器上禁用了 Internet 控制消息协议 (ICMP)，请使用此选项。 如果网络负载平衡管理器尝试联系主机不可用，您将使用此选项时遇到延迟。|
|/hostlist <filename>|加载到网络负载平衡管理器在 filename 中指定的主机。|
|/autorefresh <interval>|网络负载平衡管理器以刷新其主机和群集的信息将导致每个<interval>秒。 如果指定无间隔，则刷新的信息是每隔 60 秒。|
|/?|在命令提示符下显示帮助。|
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)

