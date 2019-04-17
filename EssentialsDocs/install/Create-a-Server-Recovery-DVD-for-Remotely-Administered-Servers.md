---
title: "远程管理服务器创建服务器恢复 DVD"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>远程管理服务器创建服务器恢复 DVD

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_HeadlessRecovery"></a>创建远程管理服务器的服务器恢复 DVD  
 有两种型号的出厂重置 server 恢复和异客户收到的硬件上。  
  
 **远程管理的服务器**  
  
 此服务器恢复选项要求，该过程从客户端计算机上运行同一个网络。 由于出厂重置需要特定硬件图像会附带服务器，合作伙伴必须创作服务器恢复 DVD。  
  
 **本地管理服务器**  
  
 这是经典模型服务器管理 server 控制台上的位置。 服务器的安装媒体用于运行一次恢复。 此操作需要服务器随的功能，以查看包括 DVD 阅读器除了视频输出。 客户该服务器安装媒体，从启动，然后选择适当恢复方法。 不需要创建服务器恢复 DVD 本地管理的服务器。  
  
> [!NOTE]
>  对于本地管理的服务器模型，客户可能会完成工厂重置通过执行全新安装。 但是，他们将无法合作伙伴自定义设置。  
  
 有两种类型的服务器恢复。  
  
 **出厂重置**  
  
 此恢复为原始状态时服务器出厂存在返回服务器。 并下出厂重置，系统会要求你执行服务器的初始配置，就像你在首次打开它，所有设置和自定义项打包都都将丢失。 这也称为天 0。？ 由于出厂重置需要特定硬件图像会附带服务器，合作伙伴必须创作服务器恢复 DVD。  
  
 **裸露 metal 还原**  
  
 此恢复假设你配置服务器备份并服务器备份已成功完成服务器故障之前至少一次。 裸露 metal 还原 (BMR) 支持仅从以前服务器备份系统和启动驱动器恢复。  
  
 按照 BMR，服务器退回用于还原的备份次存在的状态。 这通常是最新备份，但在某些情况下，它可能之前的备份。 系统还原通过还原的文件和文件夹向导后完成数据恢复。 BMR 是首选的服务器恢复方法因为返回所有设置和配置，而出厂重置返回服务器天 0 状态。  
  
### <a name="remotely-administered-server-recovery"></a>远程管理服务器恢复  
 本部分介绍合作伙伴必须执行需要自定义和必须附带每个服务器最终媒体。 观察，然后再深入详细信息，请告知我们的客户体验。  
  
 在此情况下，客户"美分的服务器不再工作。 这可能致病毒、 硬盘故障或某些其他原因。 客户将服务器恢复 DVD 插入到位于服务器的相同网络的客户端计算机。 服务器恢复应用将它们演示来恢复其服务器的三个步骤：  
  
1.  创建可启动 U 盘来重启服务器中恢复模式。 U 盘必须为 8 GB 或更大。  
  
2.  检测到现在恢复模式的服务器。  
  
3.  使客户可以选择出厂重置或裸露 metal 还原，然后将其服务器返回到正常工作状态。  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>创建多个语言的服务器恢复 DVD 步骤支持  
 有六个主要步骤创建服务器恢复 DVD  
  
1.  [（可选）更新 WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [收集出厂重置图像和 XML 文件](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [创建服务器恢复 DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [自定义的向导](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [创建的 ISO 文件](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [测试恢复 DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a>第 1 步: （可选） 的更新 WinPE  
 ADK 包含一份自定义的 Windows PE。 启动该图像时，它将自动启动的客户端恢复应用用于连接到恢复模式中安装了服务器的信号。  
  
 需要进一步可通过添加任何特定于硬件的驱动程序、 网络等或磁盘控制器驱动程序来自定义 Windows PE。 后启动 WinPE 从，系统上的硬盘需要可识别，必须使用该网络。  
  
####  <a name="BKMK_Collecting"></a>第 2 步： 收集恢复出厂设置的图像和 XML 文件  
 重置为出厂默认设置的服务器，需要捕获以下两个映像：  
  
-   驱动器的系统映像  
  
-   系统保留分区  
  
 捕获这些图像，请提供 GenDiskXML.exe 工具。 GenDiskXML.exe 还会收集一个名为 disk.xml 恢复过程用于重新创建磁盘配置文件。  
  
1.  Sysprep 之后, 重新系统启动使用任何 64 位版本的 Windows PE。  
  
2.  若要输出.xml 和.wim 文件复制到外部源，运行`GenDiskXML /outputdir:<path>`输出到任何外部源.xml 和.wim 的文件。 这些文件将被添加到下一步中 DVD。  
  
    > [!NOTE]
    >  将拆分系统.wim 文件以满足 FAT32 要求的任何大于 4 GB 的文件。 在此过程，用于捕获.wim 文件目标的所需的容量需要大于满足拆分过程 8 GB。  
  
####  <a name="BKMK_Creating"></a>第 3 步： 创建服务器恢复 DVD  
 每个服务器出厂必须伴随服务器恢复 DVD。 ADK 工具 DVD 包括创建 DVD 所需的文件。  
  
###### <a name="to-create-the-server-recovery-dvd"></a>若要创建服务器恢复 DVD  
  
1.  创建的工作文件夹存储最终 ISO 使用作为保存位置。  
  
2.  从合作伙伴 CD 复制的内容**服务器恢复**步骤 1 中创建的工作文件夹的文件夹。  
  
3.  将运行 GenDiskXML.exe 到时创建了该 disk.xml 文件复制**重置**文件夹。  
  
4.  复制映像文件运行时创建的**GenDiskXML.exe**到**重置**文件夹。 这些文件是.wim 和.swm 的文件，并文件的数目可能会有所不同。  
  
5.  删除 GenDiskXML.exe 该文件夹中。 它仅用于出厂目的，它不应包含一起交付给客户的 dvd。  
  
####  <a name="BKMK_Customizing"></a>第 4 步： 自定义的向导  
 使用此设备并介绍了如何启动到恢复模式的特定设备的文本的映像，必须自定义服务器恢复应用程序。 由于还原的文件和文件夹向导中的此页是特定的硬件，启动到恢复模式的服务器的步骤会有所不同。  
  
> [!NOTE]
>  必须完全匹配列出的文件名。  
  
1.  向导页面提供有关其他帮助了链接。 如果存在此.chm 文件，它将重写 web 帮助的 FWLink。 帮助文件位于：  
  
     < DVD Root\ > < 文化 name\ > \\$OEM$\Customization\\ \RestartHelp.chm  
  
2.  此文件包含客户看到向导中的页面的文本。 文本应该说明如何进行启动到恢复模式的服务器。 控制是可滚动，这地方实际限制，可以添加了文本量。  
  
     使用替换示例图片在向导中，以下文件并且主要有关品牌。 必须.png 文件。 文件大小必须 256 像素 x 256 像素，或时它会显示在该向导将剪裁它。  
  
     < DVD Root\ > < 文化 name\ > \\$OEM$\Customization\\ \RestartInstructions.rtf  
  
3.  < DVD Root\ > < 文化 name\ > \\$OEM$\Customization\\ \ServerImage.png  
  
 要转换的服务器恢复 DVD 支持多种语言，请确保你以下：  
  
1.  你始终必须短-我们文件夹。 如果服务器恢复应用没有找到相匹配客户端计算机运行的文化特定文件，则回退到短-我们。  
  
2.  在你创建每个文化文件夹中，添加 （.png，.chm，.rtf） 的三个自定义文件。  
  
3.  将语言 Packs\\ < CultureName\ > \Server 恢复从这两个文化文件夹复制到服务器恢复 DVD 根中。 例如： ES 和 ES ES 文件夹将复制到 DVD 后，以支持西班牙语根。  
  
4.  确定 ISO 文件。  
  
 受支持的文化名称包括：  

|-|-|  
| 的客户服务 CZ<br /><br /> -de DE<br /><br /> -EN-US<br /><br /> -es ES<br /><br /> -法国法国<br /><br /> -hu HU<br /><br /> -it IT<br /><br /> -JA-JP<br /><br /> -KO-KR<br /><br /> -nl NL |-pl PL<br /><br /> -磅 BR<br /><br /> -磅磅<br /><br /> -RU-RU<br /><br /> -sv SE<br /><br /> -异<br /><br /> -ZH-CN<br /><br /> -中文香港特别行政区<br /><br /> -ZH-TW
  
####  <a name="BKMK_CreatingISO"></a>步骤 5： 创建的 ISO 文件  
 创建文件夹并所有内容都可以刻录到 DVD。 这是将其新服务器提供给客户的 DVD。  
  
####  <a name="BKMK_Testing"></a>第 6 步： 测试恢复 DVD  
 在完成后服务器安装，配置服务器的备份，运行服务器备份，然后进行测试恢复 DVD。  
  
###### <a name="to-configure-and-run-a-server-backup"></a>若要配置和运行服务器备份  
  
1.  打开仪表板上，单击**设备**选项卡，然后单击**设置的备份，服务器**中**任务**窗格。  
  
2.  按照该向导中的说明配置备份服务器备份。 你需要为备份的外部硬盘。  
  
3.  单击**开始服务器备份**中**任务**窗格中，然后按照向导中的说明进行操作。  
  
4.  备份完毕后，确认其已成功。  
  
###### <a name="to-restore-a-server"></a>若要还原服务器  
  
1.  恢复你创建的 DVD 插入连接到服务器集线器或切换到相同网络的客户端计算机。  
  
2.  双击**setup.exe**。 服务器恢复向导将提示您通过客户将遵循相同的过程。  
  
3.  单击**从备份中还原该服务器**，然后按照向导中的说明进行操作。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)