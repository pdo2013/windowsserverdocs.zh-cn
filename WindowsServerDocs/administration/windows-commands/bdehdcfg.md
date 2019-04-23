---
title: bdehdcfg
description: Windows 命令主题**bdehdcfg** -准备硬盘驱动器的 BitLocker 驱动器加密所需的分区。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a4fda562f4aa6b8caa885fcd70155b7150c91d12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869698"
---
# <a name="bdehdcfg"></a>bdehdcfg



准备硬盘驱动器的 BitLocker 驱动器加密所需的分区。 大多数安装的 Windows 7 将不需要使用此工具，因为 BitLocker 安装程序包含准备和重新分区所需的驱动器的能力。

> [!WARNING]
> 与已知冲突**拒绝对不受 BitLocker 保护的固定驱动器的写访问**组策略设置位于**计算机配置 \ 管理模板 \windows 组件 \bitlocker驱动器加密数据驱动器**。</br>> 如果启用此策略设置时，Bdehdcfg 的计算机上运行，可能会遇到以下问题：</br>>-如果你尝试压缩驱动器并创建系统驱动器中，将成功减少驱动器大小，将创建原始分区。 但是，原始分区将不会格式化。 显示以下错误消息："新的活动驱动器无法格式化。 您可能需要手动为 BitLocker 准备驱动器。"</br>>-如果你尝试使用未分配的空间创建系统驱动器，将创建原始分区。 但是，原始分区将不会格式化。 显示以下错误消息："新的活动驱动器无法格式化。 您可能需要手动为 BitLocker 准备驱动器。"</br>>-如果你尝试将现有的驱动器合并到系统驱动器，该工具将无法将复制到目标驱动器，若要创建系统驱动器上的所需的启动文件。 显示以下错误消息："BitLocker 安装程序无法启动文件复制。 您可能需要手动为 BitLocker 准备驱动器。"</br>> 如果实施了此策略设置，因为受保护的驱动器，硬盘空间不能进行重新分区。 如果你所在组织的以前版本的 Windows 升级的计算机，而这些计算机配置具有单个分区，则应将策略设置应用到计算机之前创建所需的 BitLocker 系统分区。

有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[bdehdcfg: driveinfo](bdehdcfg-driveinfo.md)|显示指定的驱动器上的驱动器号、 总大小、 最大可用空间和分区的分区特征。 列出了有效的分区。 如果已存在四个主要或扩展分区，未列出的未分配的空间。|
|[bdehdcfg： 目标](bdehdcfg-target.md)|定义要用作系统驱动器的驱动器的哪个部分，并使部分处于活动状态。|
|[bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md)|将新的驱动器号分配给用作系统驱动器的驱动器的部分。|
|[bdehdcfg： 大小](bdehdcfg-size.md)|在创建新的系统驱动器时，请确定系统分区的大小。|
|[bdehdcfg： 静音](bdehdcfg-quiet.md)|禁止显示所有操作和命令行界面中的错误，并将定向 Bdehdcfg 若要使用的"是"任何是答案/无提示在后续过程中可能发生的驱动器准备。|
|[bdehdcfg： 重新启动](bdehdcfg-restart.md)|指示计算机的驱动器准备已完成后重新启动。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_Examples"></a>示例

以下示例展示了 Bdehdcfg 与默认驱动器用于创建为 500 MB 系统分区。 指定无驱动器号，因为新的系统分区没有驱动器号。
```
bdehdcfg -target default -size 500
```
以下示例展示了 Bdehdcfg 与默认驱动器用于创建系统分区 （p:）默认大小留出 300 mb 的驱动器上的未分配空间不足。 该工具不会提示用户进行任何进一步的输入，也不会显示任何错误。 创建系统驱动器后，计算机将自动重新启动。
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)