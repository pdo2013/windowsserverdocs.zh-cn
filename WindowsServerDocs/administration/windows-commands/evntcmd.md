---
title: Evntcmd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a149e78170f2849512dcfc0a0a82f9eed979abe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885978"
---
# <a name="evntcmd"></a>Evntcmd

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

配置事件与陷阱、 陷阱目标或两者都基于配置文件中信息的转换。   
## <a name="syntax"></a>语法  
```  
evntcmd [/s <computerName>] [/v <verbosityLevel>] [/n] <FileName>  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/s <computerName>|按名称，指定你想要配置事件到陷阱、 陷阱目标或两者的转换的计算机。 如果不指定计算机，本地计算机上进行配置。|  
|/v <verbosityLevel>|指定配置的状态消息显示为陷阱和陷阱目标的类型。 此参数必须是介于 0 和 10 之间的整数。 如果指定 10，显示的所有类型的消息，包括跟踪消息和警告有关陷阱配置是否成功。 如果指定 0，不显示任何消息。|  
|/n|指定是否此计算机收到陷阱配置更改不重新启动 SNMP 服务。|  
|<FileName>|指定的名称，包含有关事件到陷阱和陷阱目标的转换信息你想要配置的配置文件。|  
|/?|在命令提示符下显示帮助。|  
## <a name="remarks"></a>备注  
-   如果你想要配置陷阱，但不是陷阱目标，可以通过使用事件陷阱转换器，它一种图形工具来创建有效的配置文件。 如果您有安装了 SNMP 服务，可以通过键入开始事件陷阱转换器**evntwin**在命令提示符。 定义所需的陷阱后，单击导出以创建文件适用于**evntcmd**。 陷阱转换器事件可用于轻松地创建配置文件，然后使用与配置文件**evntcmd**在命令提示符下，若要快速配置多台计算机上的陷阱。  
-   配置一个陷阱的语法如下所示：  
    **#pragma add***<EventLogFile> <EventSource> <EventID> [<Count> [<Period>]]*  
    -   文本 **#pragma**必须出现在文件中的每个条目的开头。  
    -   将参数**添加**指定你想要添加一个事件，它捕获配置。  
    -   参数*EventLogFile*， *EventSource*，并*EventID*所需。 将参数*EventLogFile*指定在其中记录事件的文件。 将参数*EventSource*指定生成事件的应用程序。 *EventID*参数指定唯一标识每个事件的数目。 若要查找与特定事件相对应的值，通过键入启动事件陷阱转换器**evntwin**在命令提示符。 单击**自定义**，然后单击**编辑**。 下**事件源**，浏览的文件夹，直到找到想要配置，请单击它，然后单击的事件**添加**。 有关事件源、 事件日志文件，以及事件 ID 信息显示在**源，这样日志**，并**捕获特定 ID**分别。  
    -   *计数*参数是可选的并指定发送陷阱消息之前，事件必须发生的次数。 如果不使用*计数*参数，则发送陷阱消息在事件发生一次后。  
    -   *期*参数是可选的但它要求您使用*计数*参数。 *期*参数指定的事件必须在此期间发生的陷阱消息发送之前，使用计数参数指定的次数 （以秒为单位） 的时间长度。 如果不使用*期*参数，则发送陷阱消息在事件发生的与指定的次数后*计数*参数，无论之间出现的时间间隔。  
-   删除一个陷阱的语法如下所示：  
    **#pragma delete***<EventLogFile> <EventSource> <EventID>*  
    -   文本 **#pragma**必须出现在文件中的每个条目的开头。  
    -   将参数*删除*指定你想要删除的事件来捕获配置。  
    -   参数*EventLogFile*， *EventSource*，并*EventID*所需。 将参数*EventLogFile*指定日志中记录事件。 将参数*EventSource*指定生成事件的应用程序。 *EventID*参数指定唯一标识每个事件的数目。  
-   配置陷阱目标的语法如下所示：  
    **#pragma add_TRAP_DEST***<CommunityName> <HostID>*  
    -   文本 **#pragma**必须出现在文件中的每个条目的开头。  
    -   将参数**add_TRAP_DEST**指定您希望陷阱消息发送到社区中指定的主机。  
    -   将参数*CommunityName*按名称，指定在哪些陷阱消息都会发送的社区。  
    -   将参数*HostID*按名称或 IP 地址，指定你想陷阱消息发送到的主机。  
-   删除陷阱目标的语法如下所示：  
    **#pragma delete_TRAP_DEST***<CommunityName> <HostID>*  
    -   文本 **#pragma**必须出现在文件中的每个条目的开头。  
    -   将参数*delete_TRAP_DEST*指定不希望陷阱消息发送到社区中指定的主机。  
    -   将参数*CommunityName*按名称，指定在哪些陷阱消息都会发送的社区。  
    -   将参数*HostID*按名称或 IP 地址，指定你不想陷阱消息发送的主机。  
## <a name="BKMK_Examples"></a>示例  
以下示例说明了的配置文件中的条目**evntcmd**命令。 它们不用于在命令提示符下键入。  
发送陷阱消息如果事件日志服务重新启动，请键入：  
```  
#pragma add System "Eventlog" 2147489653  
```  
发送陷阱消息，如果事件日志服务重新启动两次在三分钟内，键入：  
```  
#pragma add System "Eventlog" 2147489653 2 180  
```  
若要停止时重新启动该事件日志服务发送陷阱消息，请键入：  
```  
#pragma delete System "Eventlog" 2147489653  
```  
若要发送到的 IP 地址 192.168.100.100 主机名为 Public 社区内的陷阱消息，请键入：  
```  
#pragma add_TRAP_DEST public 192.168.100.100  
```  
若要发送到名为 Host1 的主机名为 Private 社区内的陷阱消息，请键入：  
```  
#pragma add_TRAP_DEST private Host1  
```  
若要停止发送内配置陷阱目标在同一台计算机到名为 Private 社区的陷阱消息，请键入：  
```  
#pragma delete_TRAP_DEST private localhost  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
