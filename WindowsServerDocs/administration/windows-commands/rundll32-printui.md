---
title: rundll32 printui.dll,PrintUIEntry
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12fb48b6-5dd8-4cc0-8808-e6a681aceb84 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/25/2018
ms.openlocfilehash: c90641820bfa01c19ae7bf587c5467d3f9c5a01c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836838"
---
# <a name="rundll32-printuidllprintuientry"></a>rundll32 printui.dll,PrintUIEntry

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可以自动执行许多打印机配置任务。 printui.dll 是包含由打印机配置对话框的函数的可执行文件。 这些函数还从脚本或命令行批处理文件中中, 调用，或可以从命令提示符下以交互方式运行它们。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。  
## <a name="syntax"></a>语法  
```  
rundll32 printui.dll PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
尽管本主题中的示例使用前面的语法中，还可以使用以下的备用语法：  
```  
rundll32 printui.dll,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
## <a name="parameters"></a>Parameters  
有两种类型的参数： 基本参数和修改参数。 基参数指定的命令是要执行的函数。 只有一个这些参数可以出现在给定的命令行。 然后，可以通过使用一个或多个修改参数，如果它们是适用于 （不是所有修改参数都支持的所有基参数） 的基本参数修改基本参数。  
|基参数|描述|  
|----------|--------|  
|/dl|删除本地打印机。|  
|/dn|删除网络打印机连接。|  
|/dd|删除打印机驱动程序。|  
|/e|显示给定打印机的打印首选项。|  
|/ga|添加每个计算机打印机连接 （登录时，连接将是可供该计算机上的任何用户）。|  
|/ge|显示每个计算机的计算机上的打印机连接。|  
|/gd|删除每个计算机打印机连接 （连接已删除用户下次登录时）。|  
|/ia|使用.inf 文件来安装打印机驱动程序。|  
|/id|使用添加打印机驱动程序向导来安装打印机驱动程序。|  
|/if|使用.inf 文件来安装打印机。|  
|/ii|使用.inf 文件中使用添加打印机向导安装打印机。|  
|/il|使用添加打印机向导安装打印机。|  
|/in|连接到远程网络打印机。|  
|/ip|使用网络打印机安装向导 （可从打印管理中的用户界面） 安装打印机。|  
|/k|在打印机上打印测试页。|  
|/o|显示队列的打印机。|  
|/p|显示打印机的属性。 如果使用此参数，还必须指定修改参数的值 **/n [名称]**。|  
|/s|显示打印服务器的属性。 如果你想要查看本地打印服务器，您不需要使用修改参数。 但是，如果你想要查看远程打印服务器，则必须指定 **/c [名称]** 修改参数。|  
|/Ss|指定将存储什么类型的打印机的信息。 如果没有值，则 **/Ss**指定，默认行为是因为如果所有这些指定。 使用以下值放在命令行的末尾使用此基本参数：<br /><br />-   **2**:用于存储打印机 s printER_INFO_2 结构中包含的信息。 此结构包含有关如其名称、 服务器名称、 端口名称和共享名打印机的基本信息。<br />-   **7**:使用存储 printER_INFO_7 结构中包含的目录服务信息。<br />-   **c**:用于存储打印机的颜色配置文件信息。<br />-   **d**:用于存储打印机特定的数据，例如打印机的硬件 id。<br />-   **s**:用于存储打印机的安全描述符。<br />-   **g**:使用打印机 s 全局 DEVmode 结构中存储的信息。<br />-   **m**:使用存储的打印机的最小设置。 这相当于同时指定**2** **d**，并**g**。<br />-   **u**:用于将信息存储在每个用户 DEVmode 结构的打印机 s。|  
|/Sr|指定还原有关打印机的哪些信息以及如何处理中设置的冲突。 使用具有以下值放在命令行的末尾：<br /><br />-   **2**:使用还原打印机 s printER_INFO_2 结构中包含的信息。 此结构包含有关如其名称、 服务器名称、 端口名称和共享名打印机的基本信息。<br />-   **7**:使用还原 printER_INFO_7 结构中包含的目录服务信息。<br />-   **c**:使用还原打印机的颜色配置文件信息。<br />-   **d**:使用还原打印机特定的数据，例如打印机的硬件 id。<br />-   **s**:使用还原打印机的安全描述符。<br />-   **g**:使用还原打印机 s 全局 DEVmode 结构中的信息。<br />-   **m**:使用还原打印机的最低设置。 这相当于同时指定**2**， **d**，并**g**。<br />-   **u**用于还原每个用户 DEVmode 结构的 printe s 中的信息。<br />-   **r**： 如果存储在文件中的打印机名称不同于要还原到打印机的名称，然后使用当前的打印机名称。 这不能与指定**f**。 如果既没有**r**也不**f**指定的名称不匹配，设置还原会失败。<br />-   **f**： 如果存储在文件中的打印机名称不同于要还原到打印机的名称，然后在文件中使用的打印机名称。 这不能与指定**r**。 如果既没有**f**也不**r**指定的名称不匹配，设置还原会失败。<br />-   **p**： 如果要从还原的文件中的端口名称不匹配正在还原到的打印机的当前端口名称，则使用打印机 s 当前端口名称。<br />-   **h**： 如果无法使用已保存的设置文件中的资源共享名称共享还原到的打印机，然后尝试使用当前的共享名称或新生成的共享名称共享该打印机，如果既没有**H**也不**h**指定并还原到的打印机不能共享已保存的共享名称，然后还原会失败。<br />-   **h**： 如果不能与已保存的共享名称共享还原到的打印机，然后不共享打印机。 如果既没有**H**也不**h**指定并还原到的打印机不能共享已保存的共享名称，然后还原会失败。<br />-   **我**： 如果保存的设置文件中的驱动程序不匹配，正在还原打印机的驱动程序，则还原会失败。|  
|/Xg|检索打印机的设置。|  
|/Xs|设置打印机的设置。|  
|/y|设置为默认打印机正在安装的打印机。|  
|/?|显示在产品帮助的命令和其关联的参数。|  
|@[file]|指定命令行参数文件，并直接将该文件中的文本插入到命令行。|  
|修改参数|描述|  
|--------------|--------|  
|/a[file]|指定二进制文件名称。|  
|/b[name]|指定的基的打印机名称。|  
|/c[name]|如果要执行的操作是在远程计算机上，指定的计算机名称。|  
|/f[file]|指定通用命名约定 (UNC) 路径和名称的.inf 文件名或输出文件的名称，具体取决于正在执行的任务。 使用 **/F [文件]** 指定依赖.inf 文件。|  
|/F[file]|指定的 UNC 路径和指定.inf 文件的.inf 文件的名称 **/f [文件]** 取决于。|  
|/h[architecture]|指定驱动程序体系结构。 使用以下值之一： **x86**， **x64**，或**Itanium**。|  
|/j[provider]|指定打印提供程序名称。|  
|/l[path]|指定正在使用的打印机驱动程序文件的位置的 UNC 路径。|  
|/m[model]|指定驱动程序模型名称。 （此值可以指定在.inf 文件中。）|  
|/n[name]|指定的打印机名称。|  
|/q|运行命令时没有通知给用户。|  
|/r[port]|指定的端口名称。|  
|/u|指定要使用现有的打印机驱动程序，如果已安装。|  
|/t[#]|指定的从零开始的索引页上启动。|  
|/v[version]|指定驱动程序版本。 如果未指定的值 **/K**，你必须指定以下值之一：**类型 2 的内核模式**或**键入 3-用户模式下**。|  
|/w|如果在指定的.inf 文件中找不到驱动程序将提示用户输入一个驱动程序 **/f**。|  
|/Y|指定打印机名称不应会自动生成。|  
|/z|指定不会自动共享正在安装的打印机。|  
|/K|更改参数的含义 **/h [体系结构]** 以接受**2**来代替**x86**， **3**来代替**x64**，或**4**来代替**Itanium**。 它也会更改参数的值 **/v [version]** 以接受**2**来代替**类型 2 的内核模式**并**3**代替**键入 3 的用户模式下**。|  
|/Z|共享正在安装的打印机。 只能使用与 **/if**参数。|  
|/Mw[message]|在命令行中指定将更改提交之前，将向用户显示一条警告消息。|  
|/Mq[message]|在命令行中指定将更改提交之前，将向用户显示一条确认消息。|  
|/W[flags]|指定任何参数或选项的添加打印机向导、 添加打印机驱动程序向导和网络打印机安装向导。<br /><br />**R**:可以从最后一页，必须重新启动的向导。|  
|/G[flags]|指定全局参数和你想要使用的选项。<br /><br />**w**:禁止显示给用户的安装程序驱动程序警告。|  
## <a name="remarks"></a>备注  
-   **PrintUIEntry**关键字是区分大小写，并且您必须为此命令输入语法，与本主题中的示例所示的确切大小写。  
-   请参阅[示例](#BKMK_Examples)的语法为某些常见任务，本文档中。 有关更多示例，在命令提示符下键入： **rundll32 printui.dll,PrintUIEntry /？**  
## <a name="BKMK_Examples"></a>示例  
若要添加新的远程打印机，printer1，对于计算机，客户端 1，这是可见的用户帐户运行此命令时，键入：  
```  
rundll32 printui.dll PrintUIEntry /in /n\\client1\printer1  
```  
添加打印机使用添加打印机向导和使用.inf 文件，InfFile.inf，位于驱动器 c： 在 Infpath，类型：  
```  
rundll32 printui.dll PrintUIEntry /ii /f c:\Infpath\InfFile.inf  
```  
若要删除现有的打印机，printer1 的计算机上，客户端 1，请键入：  
```  
rundll32 printui.dll PrintUIEntry /dn /n\\client1\printer1  
```  
若要添加的每个计算机打印机连接，了 printer2，为所有用户 （用户登录时，将应用连接） 的计算机，客户端 2，类型：  
```  
rundll32 printui.dll PrintUIEntry /ga /n\\client2\printer2  
```  
若要删除的每个计算机打印机连接，了 printer2，为所有用户 （用户登录时，将删除连接） 的计算机，客户端 2，类型：  
```  
rundll32 printui.dll PrintUIEntry /gd /n\\client2\printer2  
```  
若要查看的打印服务器、 printServer1、 类型的属性：  
```  
rundll32 printui.dll PrintUIEntry /s /t1 /c\\printserver1  
```  
若要查看打印机、 printer3、 类型的属性：  
```  
rundll32 printui.dll PrintUIEntry /p /n\\printer3  
```  
## <a name="additional-references"></a>其他参考  
  
-   [rundll32](rundll32.md)  
-   [打印命令参考](print-command-reference.md)  
