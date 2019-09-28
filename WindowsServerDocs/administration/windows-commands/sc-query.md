---
title: Sc 查询
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4d2f3f603ad173b5ab90bc56a9a4e589c0fe9d8a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384342"
---
# <a name="sc-query"></a>Sc 查询



获取并显示有关指定服务、驱动程序、服务类型或驱动程序类型的信息。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
sc [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

## <a name="parameters"></a>Parameters

|       参数        |                                                                                                                          描述                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<ServerName >      |                       指定服务所在的远程服务器的名称。 名称必须使用通用命名约定（UNC）格式（例如 \\ @ no__t-1myserver）。 若要在本地运行 SC.EXE，请省略此参数。                        |
|     \<ServiceName >     |                                      指定**getkeyname**操作返回的服务名称。 此**查询**参数不与其他**查询**参数一起使用（ *ServerName*除外）。                                      |
|     type = {driver      |                                                                                                                            服务                                                                                                                            |
|       type = {自有       |                                                                                                                             共享                                                                                                                             |
|     state = {active     |                                                                                                                           不用                                                                                                                            |
| bufsize = \<BufferSize > |                     指定枚举缓冲区的大小（以字节为单位）。 默认缓冲区大小为1024个字节。 当查询生成的显示超过1024个字节时，应增加枚举缓冲区的大小。                      |
|   ri = \<ResumeIndex >   | 指定枚举开始或恢复的索引号。 默认值为**0** （零）。 当查询返回的详细信息超过默认缓冲区可显示的信息时，请将此参数与**bufsize =** 参数一起使用。 |
|  group = \<GroupName >   |                                                                             指定要枚举的服务组。 默认情况下，将枚举所有组（**group = ""** ）。                                                                              |
|           /?           |                                                                                                             在命令提示符下显示帮助。                                                                                                              |

## <a name="remarks"></a>备注

- 如果参数与其值（即， **type = 自有**，not **type = 自有**）之间没有空格，则操作将失败。
- **查询**操作显示有关服务的下列信息：SERVICE_NAME （服务的注册表子项名称）、类型、状态（以及不可用的状态）、WIN32_EXIT_B、SERVICE_EXIT_B、检查点和 WAIT_HINT。
- 在某些情况下， **type =** 参数可以使用两次。 **Type =** 参数的第一种外观指定是否查询服务、驱动程序或两者（**全部**）。 **Type =** 参数的第二个外观从**create**操作指定一个类型，以进一步缩小查询范围。
- 当**查询**命令所产生的显示超出枚举缓冲区的大小时，将显示一条类似于以下内容的消息：  
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```  
  若要显示剩余的**查询**信息，请重新运行**查询**，将**bufsize =** 设置为字节数，并将**ri =** 设置为指定索引。 例如，在命令提示符下键入以下内容，将显示剩余的输出：  
  ```
  sc query bufsize= 1822 ri= 79
  ```

## <a name="BKMK_examples"></a>示例

若要仅显示活动服务的信息，请键入以下命令之一：
```
sc query
sc query type= service
```
若要显示活动服务的信息，并指定缓冲区大小（2000字节），请键入：
```
sc query type= all bufsize= 2000
```
若要显示 WUAUSERV 服务的信息，请键入：
```
sc query wuauserv
```
若要显示所有服务的信息（活动和非活动），请键入：
```
sc query state= all
```
若要显示所有服务的信息（活动和非活动），请从第56行开始，键入：
```
sc query state= all ri= 56
```
若要显示交互式服务的信息，请键入：
```
sc query type= service type= interact
```
若要仅显示驱动程序的信息，请键入：
```
sc query type= driver
```
若要显示网络驱动程序接口规范（NDIS）组中的驱动程序的信息，请键入：
```
sc query type= driver group= ndis
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)