---
title: 更改驱动器号
description: 如何更改或将 Windows 中的驱动器号分配使用磁盘管理。
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5f25d49ac399633c048b0c8581551d862145ca76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812158"
---
# <a name="change-a-drive-letter"></a>更改驱动器号

> **适用于：** Windows 10、 Windows 8.1、 Windows 7、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

如果您不喜欢分配到的驱动器的驱动器号，或如果您有尚没有驱动器号的驱动器，可以使用磁盘管理以对其进行更改。

> [!IMPORTANT]
> 如果您更改安装 Windows 或应用程序的驱动器的驱动器号，应用可能会遇到问题，运行或查找该驱动器。 出于此原因，建议你不要更改 Windows 或应用程序安装在其的驱动器的驱动器号。

下面介绍了如何更改驱动器号 (而是装载到驱动器的空文件夹，以便它显示为另一个文件夹，请参阅[向驱动器分配装入点文件夹路径](assign-a-mount-point-folder-path-to-a-drive.md))。

1. 使用管理员权限打开磁盘管理。 <br>为此，请在任务栏上的搜索框中，键入**磁盘管理**、 选择和保存 （或右键单击）**磁盘管理**，然后选择**以管理员身份运行** > **是**。 如果您不能以管理员身份打开它，请键入**计算机管理**相反，然后转到**存储** > **磁盘管理**。
1. 在磁盘管理，右键单击你想要更改或添加驱动器号，并选择的驱动器**更改驱动器号和路径**。<br>
![显示驱动器的磁盘管理](media/change-drive-letter.png)
    > [!TIP]
    > 如果没有看到**更改驱动器号和路径**选项也将灰显，可能该卷未准备好接收驱动器号，如果驱动器未分配，并且必须是可以出现这种情况[初始化](initialize-new-disks.md). 或者，也许它并不是说进行访问，这是 EFI 系统分区和恢复分区的这种情况。 如果确认后，具有可以访问驱动器盘符的格式化的卷，并且你仍不能更改它，遗憾的是本主题可能不能帮助你，因此我们建议[与 Microsoft 联系](https://support.microsoft.com/contactus/)的制造商或您有关更多帮助的 PC。

1. 若要更改驱动器号，请选择**更改**。 若要添加的驱动器号，如果驱动器已有帐户，不选择**添加**。<br>![更改驱动器号和路径对话框](media/change-drive-letter2.png)
3. 选择新的驱动器号，请选择**确定**，然后选择**是**提示关于如何依赖于驱动器号的程序可能无法正常运行。<br>![更改驱动器号或路径对话框中显示更改的驱动器号](media/change-drive-letter3.png)