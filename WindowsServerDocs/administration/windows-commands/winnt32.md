---
title: winnt32
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a0a6fb3-ba4e-4ace-8984-7f6d3875560e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28675ac6d5a1f1a7f56a9b72ef11a3e99e4ed130
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851258"
---
# <a name="winnt32"></a>winnt32

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Windows Server 2003 中执行的安装或升级到产品。 你可以运行**winnt32**在命令提示符下运行 Windows Server 2003 中的 Windows 95、 Windows 98、 Windows Millennium edition、 Windows NT、 Windows 2000，Windows XP 或产品的计算机上。 如果在运行**winnt32**上运行 Windows NT 4.0 的计算机，你必须首先应用 Service Pack 5 或更高版本。
## <a name="syntax"></a>语法
```
winnt32 [/checkupgradeonly] [/cmd: <CommandLine>] [/cmdcons] [/copydir:{i386|ia64}\<FolderName>] [/copysource: <FolderName>] [/debug[<Level>]:[ <FileName>]] [/dudisable] [/duprepare: <pathName>] [/dushare: <pathName>] [/emsport:{com1|com2|usebiossettings|off}] [/emsbaudrate: <BaudRate>] [/m: <FolderName>]  [/makelocalsource] [/noreboot] [/s: <Sourcepath>] [/syspart: <DriveLetter>] [/tempdrive: <DriveLetter>] [/udf: <ID>[,<UDB_File>]] [/unattend[<Num>]:[ <AnswerFile>]]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/checkupgradeonly|检查计算机上的 Windows Server 2003 中的产品的升级兼容性。<br /><br />如果使用此选项与 **/ 无人参与**，不需要用户输入是必需的。  否则为结果将显示在屏幕上，并可以将它们保存在指定的文件名称。 默认文件名称是**upgrade.txt** systemroot 文件夹中。|
|/cmd|指示安装程序安装程序的最后阶段之前执行特定命令。 您的计算机已重启并安装程序已收集必要的配置信息之后,、 但在安装程序完成后，将发生这种情况。|
|\<CommandLine>|指定命令行安装程序的最后阶段之前执行。|
|/cmdcons|在基于 x86 的计算机上，为启动选项中安装恢复控制台。  恢复控制台是一个命令行界面，从中你可以执行诸如启动和停止服务以及访问本地驱动器 （包括使用 NTFS 格式化的驱动器） 的任务。 您只能使用 **/cmdcons**选项在安装完成之后。|
|/copydir|创建在其中安装操作系统文件的文件夹中的其他文件夹。  例如，对于 x86 和基于 x64 的计算机，可以创建一个名为文件夹*Private_drivers*用于安装和位置驱动程序文件的文件夹中的 i386 source 文件夹中。 类型**/copydir:i386\\* * * Private_drivers*让安装程序将该文件夹复制到新安装计算机，使新的文件夹位置**systemroot** \\*Private_drivers *。<br /><br />-   **i386**指定 i386<br />-   **ia64**指定 ia64<br /><br />可以使用 **/copydir**创建任意多个所需的其他文件夹。|
|\<FolderName>|指定您创建的用于保存修改为你网站的文件夹。|
|/copysource|创建在其中安装操作系统文件的文件夹中的临时其他文件夹。 可以使用 **/copysource**创建任意多个所需的其他文件夹。<br /><br />与不同的文件夹 **/copydir**创建， **/copysource**安装程序完成后，会删除文件夹。|
|/debug|例如，指定，级别创建调试日志 **/debug4:Debug.log**。  默认日志文件是**C:\systemroot\winnt32.log**，和|
|\<level>|级别值和说明<br /><br />-   0:严重错误<br />-   1:错误<br />-   2:默认级别。 警告<br />-   3:信息<br />-4： 详细的调试信息<br /><br />每个级别包括其下的层。|
|/dudisable|会阻止运行动态更新。 没有动态更新，安装程序仅运行原始的安装程序文件。 此选项将禁用动态更新，即使使用答案文件并在该文件中指定动态更新选项。|
|/duprepare|执行安装共享上的准备工作，以便它可以用于从 Windows Update Web 站点下载的动态更新文件。 然后可以为多个客户端安装 Windows XP 使用此共享。|
|\<pathName>|指定完整路径名称。|
|/dushare|指定在其你以前下载的动态更新文件 （用于安装更新的文件） 的共享从 Windows 更新 Web 站点和其在先前运行 **/duprepare: * * * < 路径 >*。 当客户端上运行，指定客户端安装将使用更新后的文件中指定的共享上的<pathName>。|
|/emsport|启用或禁用紧急管理服务，在安装过程中和安装服务器操作系统之后。 使用紧急管理服务，你可以远程管理在紧急情况下通常要求本地键盘、 鼠标和显示器，例如网络时不可用的服务器或服务器不正常。 紧急管理服务都有特定的硬件要求，并仅适用于 Windows Server 2003 中的产品。<br /><br />-   **com1**仅适用于基于 x86 的计算机 （不体系结构基于 Itanium 的计算机）。<br />-   **com2**仅适用于基于 x86 的计算机 （不体系结构基于 Itanium 的计算机）。<br />-默认值。 使用 BIOS 串行端口控制台重定向 (SPCR) 表中，或在 Itanium 体系结构基于系统中，通过 EFI 控制台设备路径指定的设置。 如果指定**usebiossettings**并且没有任何 SPCR 表或 EFI 控制台设备的相应路径，将不会启用紧急管理 Serices。<br />-   **关闭**禁用紧急管理服务。 更高版本可以通过修改启动设置来启用它。|
|/emsbaudrate|对于基于 x86 的计算机，紧急管理服务中指定的波特率。 （该选项不适用的体系结构基于 Itanium 的计算机。）必须将用于 **/emsport:com1**或 **/emsport:com2** (否则为 **/emsbaudrate**将被忽略)。|
|\<BaudRate>|指定 9600、 baudrate 19200、 57600 或 115200。 默认值为 9600。|
|/m|指定安装程序将替换文件复制从备用位置。  指示安装程序在第一次，查看在备用位置，如果文件存在，则应使用它们而不是默认位置中的文件。|
|/makelocalsource|指示安装程序将安装源的所有文件都复制到本地硬盘。  使用 **/makelocalsource**从 cd 时该 cd 有不在安装更高版本中提供安装文件安装时。|
|/noreboot|指示安装程序安装程序文件复制阶段，以便可以运行另一个命令完成后不重新启动计算机。|
|/s|指定为您的安装文件的源位置。 若要同时从多个服务器中复制文件，请键入 **/s:**\<Sourcepath > 多个时间 （最多八个最多） 的选项。 如果多次输入选项，指定的第一个服务器必须可用，或者安装程序将失败。|
|\<Sourcepath>|指定完整源路径名称。|
|/syspart|在基于 x86 的计算机上，指定可以将安装程序启动文件复制到硬盘，将标记为活动状态，磁盘，然后将磁盘安装到另一台计算机。 当您启动该计算机时，它会自动启动安装程序的下一个阶段。<br /><br />必须始终使用 **/tempdrive**参数与 **/syspart**参数。<br /><br />可以启动**winnt32**与 **/syspart**在 Windows Server 2003 中运行 Windows NT 4.0、 Windows 2000，Windows XP 或产品的基于 x86 的计算机上的选项。 如果计算机正在运行 Windows NT 4.0，则需要 Service Pack 5 或更高版本。 计算机不能运行 Windows 95、 Windows 98 或 Windows Millennium edition。|
|\<DriveLetter>|指定的驱动器号。|
|/tempdrive|指导安装程序将指定的分区上的临时文件。<br /><br />此外将用于新安装上指定的分区, 上安装服务器操作系统。<br /><br />为升级 **/tempdrive**选项会影响仅临时文件的位置; 将在你从中运行的分区中升级操作系统**winnt32**。|
|/udf|指示一个标识符 (\<ID >) 安装程序用来指定唯一数据库 (UDB) 文件如何修改答案文件 (请参阅 **/ 无人参与**选项)。  UDB 替代答案文件中的值和标识符会决定使用 UDB 文件中的哪些值。 例如， **/udf:RAS_user,Our_company.udb**重写 RAS_user 标识符 Our_company.udb 文件中指定的设置。 如果没有\<UDB_file > 指定，则安装程序将提示用户插入包含一张磁盘 **$Unique$.udb**文件。|
|\<ID>|指示用于指定唯一数据库 (UDB) 文件如何修改答案文件的标识符。|
|\<UDB_file>|指定的唯一数据库 (UDB) 文件。|
|/unattend|在基于 x86 的计算机上，将升级以前版本的 Windows NT 4.0 Server （带有 Service Pack 5 或更高版本） 或 Windows 2000 中无人参与的安装模式。 所有用户设置都会使用从以前的安装，因此在安装期间不不需要任何用户干预。|
|\<num>|指定设置的时间间隔的秒数完成复制文件和重新启动您的计算机。 可以使用\<Num > 在 Windows Server 2003 中运行 Windows 98、 Windows Millennium edition、 Windows NT、 Windows 2000，Windows XP 或产品的任何计算机上。 如果计算机正在运行 Windows NT 4.0，则需要 Service Pack 5 或更高版本。|
|\<AnswerFile>|为安装程序提供自定义规范|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
如果客户端计算机上部署 Windows XP，您可以使用附带了 Windows XP 的 winnt32.exe 的版本。 若要部署 Windows XP 的另一种方法是使用的 winnt32.msi，这非常适合通过 Windows 安装程序，智能镜像的一部分设置的技术。 有关客户端部署的详细信息，请参阅 Windows Server 2003 部署工具包中, 所述[使用 Windows 部署和资源工具包](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx)。

在基于 Itanium 的计算机上， **winnt32**从可扩展固件接口 (EFI) 或从 Windows Server 2003 Enterprise、 Windows Server 2003 R2 Enterprise、 Windows Server 2003 R2 Datacenter 或 Windows Server 2003 可以运行数据中心。 此外，在 Itanium 体系结构基于计算机上， **/cmdcons**并 **/syspart**不可用和与升级有关的选项将不可用。
有关硬件兼容性的详细信息，请参阅[硬件兼容性](https://technet.microsoft.com/library/cc757927(v=ws.10).aspx)。
详细了解如何使用动态更新和安装多个客户端的详细信息，请参阅 Windows Server 2003 部署工具包中, 所述[使用 Windows 部署和资源工具包](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx)。
有关修改启动设置的信息，请参阅 Windows 部署和资源工具包适用于 Windows Server 2003。 有关详细信息，请参阅[使用 Windows 部署和资源工具包](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx)。
使用 **/ 无人参与**命令行选项来自动执行安装程序确认你已阅读并接受 Microsoft 许可协议的 Windows Server 2003。 使用此命令行选项来代表你自己的以外的组织中安装 Windows Server 2003 之前, 必须确认的最终用户 （无论是个人还是单个实体） 已收到、 读取，并接受 Microsoft 许可条款该产品的协议。  Oem 销售给最终用户的计算机上不能指定此密钥。

## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)

