---
title: bdehdcfg
description: 适用于**bdehdcfg**的 Windows 命令主题-使用 BitLocker 驱动器加密所需的分区准备硬盘驱动器。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acfe2582905402f7be149148520784be6ad47453
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382190"
---
# <a name="bdehdcfg"></a>bdehdcfg



使用 BitLocker 驱动器加密所需的分区准备硬盘驱动器。 大多数 Windows 7 安装都不需要使用此工具，因为 BitLocker 安装程序包括根据需要准备和重新分区驱动器。

> [!WARNING]
> 与位于计算机配置 \ 管理模板 \Windows 组件 \Bitlocker 驱动器加密 \ 数据驱动器中的 "**拒绝对不受组策略 BitLocker 保护的固定驱动器的写访问权限**" 设置有关的已知冲突.</br>> 如果启用此策略设置，则在计算机上运行 Bdehdcfg 时，可能会遇到以下问题：</br>>-如果你试图收缩驱动器并创建系统驱动器，则驱动器大小将会成功减少，并将创建一个原始分区。 但是，将不会格式化原始分区。 将显示以下错误消息："无法对新的活动驱动器进行格式化。 你可能需要手动准备你的 BitLocker 驱动器。</br>>-如果你尝试使用未分配空间创建系统驱动器，则将创建一个原始分区。 但是，将不会格式化原始分区。 将显示以下错误消息："无法对新的活动驱动器进行格式化。 你可能需要手动准备你的 BitLocker 驱动器。</br>>-如果你尝试将现有驱动器合并到系统驱动器中，则该工具将无法将所需的启动文件复制到目标驱动器上以创建系统驱动器。 将显示以下错误消息："BitLocker 安装程序未能复制启动文件。 你可能需要手动准备你的 BitLocker 驱动器。</br>> 如果要强制实施此策略设置，则无法重新分区硬盘驱动器，因为驱动器受到保护。 如果要从以前版本的 Windows 升级组织中的计算机，并且这些计算机配置了单个分区，则应在将策略设置应用到计算机之前创建所需的 BitLocker 系统分区。

有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[Bdehdcfg： system.io.driveinfo](bdehdcfg-driveinfo.md)|显示指定驱动器上的驱动器号、总大小、最大可用空间和分区特征。 仅列出了有效的分区。 如果已存在四个主分区或扩展分区，则不会列出未分配的空间。|
|[Bdehdcfg：目标](bdehdcfg-target.md)|定义要用作系统驱动器的驱动器部分，并使该部分处于活动状态。|
|[Bdehdcfg： newdriveletter](bdehdcfg-newdriveletter.md)|将新的驱动器号分配给用作系统驱动器的驱动器部分。|
|[Bdehdcfg： size](bdehdcfg-size.md)|确定在创建新的系统驱动器时系统分区的大小。|
|[Bdehdcfg： quiet](bdehdcfg-quiet.md)|禁止显示命令行界面中的所有操作和错误，并指示 Bdehdcfg 使用 "是" 响应在后续驱动器准备过程中可能出现的任何 "是/否" 提示。|
|[Bdehdcfg：重新启动](bdehdcfg-restart.md)|指示在驱动器准备完成后重新启动计算机。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例描述了 Bdehdcfg 与默认驱动器一起使用，以创建 500 MB 的系统分区。 由于未指定驱动器号，新的系统分区将没有驱动器号。
```
bdehdcfg -target default -size 500
```
下面的示例描述了 Bdehdcfg 与默认驱动器一起使用以创建系统分区（P：）在驱动器上的未分配空间中，默认大小为 300 MB。 该工具不会提示用户输入任何其他输入，也不会显示任何错误。 创建系统驱动器后，计算机将自动重新启动。
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)