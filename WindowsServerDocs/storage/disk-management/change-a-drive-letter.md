---
title: 更改驱动器号
description: 如何使用磁盘管理在 Windows 中更改或分配驱动器号。
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b972ab05c192dca9a9a0a2bda4f083d2906acadb
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812480"
---
# <a name="change-a-drive-letter"></a>更改驱动器号

> **适用于：** Windows 10、Windows 8.1、Windows 7、Windows Server（半年频道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果你不喜欢某个驱动器的驱动器号，或者某个驱动器尚未分配驱动器号，可以使用磁盘管理来更改或分配驱动器号。

> [!IMPORTANT]
> 如果更改驱动器号的驱动器中安装了 Windows 或应用，在运行或查找该驱动器时，应用可能会遇到问题。 出于此原因，我们建议不要更改安装了 Windows 或应用的驱动器的驱动器号。

下面介绍如何更改驱动器号（若要在空文件夹中装载驱动器，使其看上去像是另一个文件夹，请参阅[分配驱动器的装入点文件夹路径](assign-a-mount-point-folder-path-to-a-drive.md)）。

1. 使用管理员权限打开磁盘管理。 
    为此，请在任务栏上的搜索框中键入“磁盘管理”，选择并按住（或右键单击）“磁盘管理”，然后选择“以管理员身份运行” > “是”。     如果无法以管理员身份打开它，请键入“计算机管理”，然后转到“存储” > “磁盘管理”。   
1. 在磁盘管理，右键单击要更改或添加驱动器号的驱动器，然后选择“更改驱动器号和路径”。 

    ![显示了驱动器的磁盘管理](media/change-drive-letter.png)
    > [!TIP]
    > 如果“更改驱动器号和路径”选项未显示或灰显，则可能表示该卷尚未准备好设置驱动器号。如果驱动器尚未分配并需要[初始化](initialize-new-disks.md)，则可能会出现这种情况。  或者，可能表示该卷不可访问。EFI 系统分区和恢复分区存在这种情况。 如果确认已使用可访问的驱动器号格式化了该卷，但依旧无法更改它，则很遗憾，本主题可能帮不到你，我们建议[联系 Microsoft](https://support.microsoft.com/contactus/) 或电脑制造商来获得更多帮助。

1. 若要更改驱动器号，请选择“更改”。  若要添加驱动器号（如果驱动器没有驱动器号），请选择“添加”。 

    ![“更改驱动器号和路径”对话框](media/change-drive-letter2.png)
1. 选择新的驱动器号，选择“确定”，然后在出现有关依赖于该驱动器号的程序可能无法正常运行的提示时选择“是”。  

    ![显示更改驱动器号的“更改驱动器号和路径”对话框](media/change-drive-letter3.png)