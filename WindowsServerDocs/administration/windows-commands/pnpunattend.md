---
title: pnpunattend
description: 了解如何审核的计算机上的设备驱动程序，以及执行无提示的驱动程序安装。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 53b72459d497ac5d079336c2a00ba65634b2e3a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436332"
---
# <a name="pnpunattend"></a>pnpunattend

审核计算机，以使设备驱动程序，执行无人参与的驱动程序安装或搜索驱动程序而无需安装并，（可选） 向命令行中报告结果。 使用此命令指定的特定硬件设备的特定驱动程序安装。 请参阅备注。

## <a name="syntax"></a>语法

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|auditSystem|指定联机驱动程序安装。</br>必需的例外**pnpunattend**与运行 **/help**或 **/？** 参数。|
|/s|可选。 指定要搜索的驱动程序，而无需安装。|
|/L|可选。 指定要在命令提示符中显示此命令的日志信息。|
|/?|可选。 显示在命令提示符下此命令的帮助。|

## <a name="remarks"></a>备注

初步准备是必需的。 在使用此命令之前, 必须完成以下任务：

1. 创建你想要安装的驱动程序的目录。 例如，创建在文件夹**C:\Drivers\Video**视频适配器的驱动程序。
2. 下载并提取你的设备的驱动程序包。 将你的操作系统版本的 INF 文件所在的子文件夹及其所有子文件夹的内容复制到你创建的视频文件夹。 例如，将视频驱动程序文件复制到 C:\Drivers\Video。
3. 将系统环境路径变量添加到的文件夹，例如，创建在步骤 1. **C:\Drivers\Video**。
4. 创建以下注册表项，然后**DriverPaths**密钥创建，请设置**值数据**到**1**。
5. 有关 Windows® 7 导航注册表路径：**HKEY_LOCAL_Machine\Software\Microsoft\Windows NT\CurrentVersion\\** ，然后创建这些项：**UnattendSettings\PnPUnattend\DriverPaths\\**
6. 对于 Windows Vista 中，导航到注册表路径：**HK_LM\Software\Microsoft\Windows NT\CurrentVersion\\** ，然后创建密钥 = **\UnattendSettings\PnPUnattend\DriverPaths**。

## <a name="examples"></a>示例

以下示例命令演示如何使用**PNPUnattend.exe**要审核的可能的驱动程序更新的计算机，然后报告结果记录到命令提示符。

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)