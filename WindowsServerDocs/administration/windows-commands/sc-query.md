---
title: Sc 查询
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60b6e945c4b2944f97d40cbc27694acc2915c615
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441620"
---
# <a name="sc-query"></a>Sc 查询



获取并显示有关指定的服务、 驱动程序、 服务类型或类型的驱动程序的信息。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
sc [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

## <a name="parameters"></a>Parameters

|       参数        |                                                                                                                          描述                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<ServerName>      |                       指定的服务所在的远程服务器的名称。 该名称必须使用通用命名约定 (UNC) 格式 (例如， \\ \\myserver)。 若要本地运行 SC.exe，忽略此参数。                        |
|     \<ServiceName>     |                                      指定返回的服务名称**getkeyname**操作。 这**查询**结合使用与其他未使用参数**查询**参数 (而不*ServerName*)。                                      |
|     type= {driver      |                                                                                                                            服务                                                                                                                            |
|       type= {own       |                                                                                                                             共享                                                                                                                             |
|     state= {active     |                                                                                                                           非活动状态                                                                                                                            |
| bufsize= \<BufferSize> |                     指定枚举缓冲区的大小 （以字节为单位）。 默认缓冲区大小为 1,024 字节。 显示从查询结果超过 1,024 字节时，应增加枚举缓冲区的大小。                      |
|   ri= \<ResumeIndex>   | 指定枚举的时间来开始或恢复的索引号。 默认值是**0** （零）。 将此参数与结合**bufsize =** 参数不是默认缓冲区可以显示由查询返回的详细信息时。 |
|  group= \<GroupName>   |                                                                             指定要枚举的服务组。 默认情况下，枚举所有组 (**组 =""** )。                                                                              |
|           /?           |                                                                                                             在命令提示符下显示帮助。                                                                                                              |

## <a name="remarks"></a>备注

- 而无需参数和它的值之间有空格 (即**类型 = 自己**，而非**类型 = 自己**)，则操作将失败。
- **查询**操作将显示有关服务的以下信息：SERVICE_NAME （服务的注册表子项名称），键入，状态 （以及状态不可用的），WIN32_EXIT_B、 SERVICE_EXIT_B、 检查点和 WAIT_HINT。
- **类型 =** 两次在某些情况下，可以使用参数。 出现的第一个**类型 =** 参数指定是否要查询服务、 驱动程序，或两者 (**所有**)。 第二个的外观**类型 =** 参数指定从类型**创建**操作以进一步缩小查询的作用域。
- 时的显示结果**查询**命令超过枚举缓冲区的大小，显示一条类似于以下消息：  
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```  
  若要显示剩余**查询**信息，请重新运行**查询**，并设置**bufsize =** 为字节数和设置数**ri =** 到指定的索引。 例如，剩余的输出会显示通过在命令提示符下键入以下内容：  
  ```
  sc query bufsize= 1822 ri= 79
  ```

## <a name="BKMK_examples"></a>示例

若要显示活动的服务的信息，请键入以下命令之一：
```
sc query
sc query type= service
```
若要显示活动服务的信息并指定为 2,000 字节的缓冲区大小，请键入：
```
sc query type= all bufsize= 2000
```
若要显示 WUAUSERV 服务的信息，请键入：
```
sc query wuauserv
```
若要显示的所有服务 （活动和非活动） 的信息，请键入：
```
sc query state= all
```
若要显示的所有服务 （活动和非活动），开始行 56 的信息，请键入：
```
sc query state= all ri= 56
```
若要显示交互式服务的信息，请键入：
```
sc query type= service type= interact
```
若要显示的仅限驱动程序的信息，请键入：
```
sc query type= driver
```
若要在网络驱动程序接口规范 (NDIS) 组中显示的驱动程序的信息，请键入：
```
sc query type= driver group= ndis
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)