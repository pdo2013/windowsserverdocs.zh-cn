---
title: 为远程管理的服务器创建服务器恢复 DVD
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7bfe1686ac84962cdb4ab1cde8d6ca5226cb9d44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844348"
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>为远程管理的服务器创建服务器恢复 DVD

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_HeadlessRecovery"></a> 创建用于远程管理的服务器的服务器恢复 DVD  
 恢复出厂设置和服务器恢复有两种模式，这两种模式因客户收到的硬件而有所差异。  
  
 **远程管理的服务器**  
  
 此服务器恢复选项要求从同一网络上的客户端计算机运行该过程。 由于恢复出厂设置需要服务器随附的特定于硬件的映像，合作伙伴必须授权服务器恢复 DVD。  
  
 **本地管理的服务器**  
  
 这是一种经典模式，通过服务器控制台管理服务器。 服务器安装介质用于运行恢复。 这要求服务器除了包含 DVD 读取器外还需具备查看视频输出的功能。 客户从该服务器安装介质启动计算机，然后选择相应的恢复方法。 你无需为本地管理的服务器创建服务器恢复 DVD。  
  
> [!NOTE]
>  在本地管理的服务器模式下，客户可通过执行全新安装恢复出厂设置。 但他们将丢失合作伙伴自定义。  
  
 有两种类型的服务器恢复。  
  
 **恢复出厂设置**  
  
 此恢复会将服务器恢复到工厂发货时的原始状态。 恢复出厂设置后，系统要求你执行服务器的初始配置，就像第一次打开时一样，所有设置和自定义都将丢失。 也称为第 0 天。？ 由于恢复出厂设置需要服务器随附的特定于硬件的映像，合作伙伴必须授权服务器恢复 DVD。  
  
 **裸机还原**  
  
 此恢复假定你配置了服务器备份，且在服务器出现故障之前至少成功完成了一次服务器备份。 裸机还原 (BMR) 仅支持从先前的服务器备份来还原系统和启动驱动器。  
  
 进行 BMR 后，服务器会恢复到创建用于还原的备份时的状态。 这通常会是最近的备份，但在某些情况下会是更早的备份。 使用还原文档和文件夹向导即可在系统还原后恢复数据。 BMR 是首选的服务器恢复方法，因为此方法可以恢复所有设置和配置，而出厂重置会将服务器恢复到“Day 0”状态。  
  
### <a name="remotely-administered-server-recovery"></a>远程管理的服务器恢复  
 本部分描述了合作伙伴必须执行的所需自定义和每台服务器必须随附的最终介质。 在深入了解详细信息前，让我们先来了解一下客户体验。  
  
 在此方案中，客户"，的服务器不再工作。 这可能是由病毒、硬盘故障或其他原因造成的。 客户将服务器恢复 DVD 插入到与服务器位于同一网络的客户端计算机中， 服务器恢复应用程序会通过以下三步完成服务器恢复：  
  
1.  创建可启动 U 盘，用于在恢复模式下重新启动服务器。 U 盘必须大于等于 8 GB。  
  
2.  检测处于恢复模式的服务器。  
  
3.  允许客户在恢复出厂设置与裸机还原间选择，然后将服务器返回工作模式。  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>为多语言支持创建服务器恢复 DVD 的步骤  
 创建服务器恢复 DVD 包含六个主要步骤  
  
1.  [（可选）更新 WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [收集出厂重置映像和 XML 文件](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [创建服务器恢复 DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [自定义向导](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [创建 ISO 文件](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [测试恢复 DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a> 步骤 1:（可选）更新 WinPE  
 ADK 包括了一份自定义的 Windows PE 副本。 此映像被启动后会自动启动客户端恢复应用程序用于连接到处于恢复模式的服务器的信号。  
  
 Windows PE 需要通过添加特定于硬件的驱动程序（例如网络或磁盘控制器驱动程序）进行进一步自定义。 从 WinPE 启动后，系统上的硬盘应是可以识别的，并且网络必须处于工作状态。  
  
####  <a name="BKMK_Collecting"></a> 步骤 2:收集出厂重置映像和 XML 文件  
 要为服务器重设出厂默认值，需要截取以下两个映像：  
  
-   系统驱动器映像  
  
-   系统保留分区  
  
 GenDiskXML.exe 工具可用于截取这些映像。 GenDiskXML.exe 还会收集在恢复过程中用于重新创建磁盘配置的文件，名为 disk.xml。  
  
1.  在 Sysprep 之后，使用任意 64 位版 Windows PE 重新启动系统。  
  
2.  运行 `GenDiskXML /outputdir:<path>` 可将 .xml 和 .wim 文件输出到任意外部源。 在下一步骤中，会将文件添加到 DVD。  
  
    > [!NOTE]
    >  为了满足文件不得大于 4 GB 的 FAT32 要求，系统 .wim 文件会被分割。 在此过程中，用于截取 .wim 文件的目标的容量要大于 8 GB 才能满足分割程序的要求。  
  
####  <a name="BKMK_Creating"></a> 步骤 3:创建服务器恢复 DVD  
 每台服务器出厂时都必须随附一份服务器恢复 DVD。 你的 ADK 工具 DVD 中包含创建 DVD 必需的文件。  
  
###### <a name="to-create-the-server-recovery-dvd"></a>创建服务器恢复 DVD  
  
1.  创建一个存储最终 ISO 的工作文件夹。  
  
2.  将合作伙伴 CD 中 **Server Recovery** 文件夹的内容复制到你在步骤  1 中所创建的工作文件夹中。  
  
3.  将运行 GenDiskXML.exe 时创建的 disk.xml 文件复制到 **“重置”** 文件夹中。  
  
4.  将运行 **GenDiskXML.exe** 时创建的映像文件复制到 **“重置”** 文件夹。 这些文件即 .wim 和 .swm 文件，文件的数量会因情况而异。  
  
5.  将 GenDiskXML.exe 从文件夹中删除。 此工具仅供工厂处理之用，不应包含在发运给客户的 DVD 中。  
  
####  <a name="BKMK_Customizing"></a> 步骤 4:自定义向导  
 服务器恢复应用程序必须使用设备图像和说明如何以恢复模式启动特定设备的文本来进行自定义。 因为还原文件和文件夹向导页面是特定于硬件的，因此以恢复模式启动服务器的步骤会因情况而异。  
  
> [!NOTE]
>  所列文件名要完全匹配。  
  
1.  向导页面提供了获取更多帮助的链接。 如果此 .chm 文件存在，则会覆盖 Web 帮助的 FWLink。 帮助文件位于：  
  
     < DVD 根目录\>\\$OEM$ \Customization\\< 区域性名称\>\RestartHelp.chm  
  
2.  该文件包含客户在向导页面看到的文本。 文本应该说明如何以恢复模式启动服务器。 控件是可滚动的，这就为可添加的文本量设置了实际的限制。  
  
     如下描述的文件用于替换向导中的示例图片，此文件主要与商标有关。 此文件必须为 .png 文件。 文件大小必须为 256 x 256 像素，否则在向导中显示时会被剪切。  
  
     < DVD 根目录\>\\$OEM$ \Customization\\< 区域性名称\>\RestartInstructions.rtf  
  
3.  < DVD 根目录\>\\$OEM$ \Customization\\< 区域性名称\>\ServerImage.png  
  
 将服务器恢复 DVD 转变为支持多个语言的时候，请确保完成以下操作：  
  
1.  必须始终保留 en-us 文件夹。 如果服务器恢复应用程序找不到与运行应用程序的客户端计算机匹配的区域特定的文件，则会返回到 en-us。  
  
2.  在创建的每个语言文件夹中，添加三个自定义文件（.png、.chm 和 .rtf）。  
  
3.  从语言包复制这两个区域性文件夹\\< CultureName\>\Server 恢复到服务器恢复 DVD 的根目录。 例如：ES 和 ES-ES 文件夹都要复制到 DVD 的根目录以支持西班牙语。  
  
4.  完成 ISO 文件。  
  
 支持的语言名称包括：  

|-|-|  
|- cs-CZ<br /><br /> -DE-DE<br /><br /> -EN-US<br /><br /> - es-ES<br /><br /> - fr-FR<br /><br /> -hu HU<br /><br /> -it IT<br /><br /> -JA-JP<br /><br /> - ko-KR<br /><br /> - nl-NL|- pl-PL<br /><br /> - pt-BR<br /><br /> - pt-PT<br /><br /> -ru RU<br /><br /> - sv-SE<br /><br /> - tr-TR<br /><br /> - zh-CN<br /><br /> - zh-HK<br /><br /> - zh-TW
  
####  <a name="BKMK_CreatingISO"></a> 步骤 5:创建 ISO 文件  
 创建的文件夹以及所有的内容都可以刻入 DVD。 此 DVD 将随新服务器一起提供给客户。  
  
####  <a name="BKMK_Testing"></a> 步骤 6:测试恢复 DVD  
 完成服务器安装后，配置服务器备份，运行服务器备份，然后测试恢复 DVD。  
  
###### <a name="to-configure-and-run-a-server-backup"></a>配置和运行服务器备份  
  
1.  打开仪表板，单击 **“设备”** 选项卡，然后单击 **“任务”** 窗格中的 **“为服务器设置备份”** 。  
  
2.  根据向导中的说明配置备份服务器备份。 你需要一个移动硬盘用于备份。  
  
3.  单击 **“任务”** 窗格中的 **“为此服务器启动备份”** ，然后按照向导中的说明执行操作。  
  
4.  完成备份后，验证备份是否成功。  
  
###### <a name="to-restore-a-server"></a>还原服务器  
  
1.  将创建的恢复 DVD 插入通过集线器或交换机连接到与服务器同一网络上的客户端计算机。  
  
2.  双击 **setup.exe**。 服务器恢复向导会提示你完成客户将会遵循的相同流程。  
  
3.  单击 **“从备份还原服务器”**，然后按照向导中的说明执行操作。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)