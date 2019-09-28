---
title: pnpunattend
description: 了解如何在计算机上审核设备驱动程序，以及如何执行无提示的驱动程序安装。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 77a6ab1ea45322e3c53e8b095c412cf8838be60d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372268"
---
# <a name="pnpunattend"></a>pnpunattend

审核计算机上的设备驱动程序，执行无人参与的驱动程序安装，或者在不安装的情况下搜索驱动程序，还可以在命令行中报告结果。 使用此命令为特定硬件设备指定特定驱动程序的安装。 请参阅备注。

## <a name="syntax"></a>语法

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|auditSystem|指定联机驱动程序安装。</br>必需，但在运行**pnpunattend**时，无论是 **/help**还是 **/？** parameters.|
|/s|可选。 指定在不安装的情况下搜索驱动程序。|
|/L|可选。 指定在命令提示符中显示此命令的日志信息。|
|/?|可选。 在命令提示符下显示此命令的帮助。|

## <a name="remarks"></a>备注

需要初步准备。 在使用此命令之前，必须完成以下任务：

1. 为要安装的驱动程序创建一个目录。 例如，在**C:\Drivers\Video**中为视频适配器驱动程序创建一个文件夹。
2. 下载并提取设备的驱动程序包。 将包含操作系统版本 INF 文件的子文件夹的内容复制到所创建的视频文件夹中的所有子文件夹。 例如，将视频驱动程序文件复制到 C:\Drivers\Video。
3. 将系统环境路径变量添加到步骤1中创建的文件夹。例如， **C:\Drivers\Video**。
4. 创建以下注册表项，然后为创建的**DriverPaths**键将**值数据**设置为**1**。
5. 对于 Windows®7导航注册表路径：**HKEY_LOCAL_Machine\Software\Microsoft\Windows NT\CurrentVersion @ no__t**，然后创建密钥：**UnattendSettings\PnPUnattend\DriverPaths @ no__t-1**
6. 对于 Windows Vista，导航到注册表路径：**HK_LM\Software\Microsoft\Windows NT\CurrentVersion @ no__t**，然后创建密钥 = **\UnattendSettings\PnPUnattend\DriverPaths**。

## <a name="examples"></a>示例

下面的示例命令演示了如何使用**PNPUnattend**来审核计算机的可能的驱动程序更新，然后将发现报告给命令提示符。

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)