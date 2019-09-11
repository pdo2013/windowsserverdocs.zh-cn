---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: Fsutil 资源
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ea97f7f71d1b484c7ac63c7c429f291fba607bba
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867067"
---
# <a name="fsutil-resource"></a>Fsutil 资源
>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10，Windows Server 2012 R2，Windows 8.1，Windows Server 2012，Windows 8，Windows Server 2008 R2，Windows 7，Windows 2008，Windows Vista

创建辅助事务资源管理器，启动或停止事务资源管理器，或显示有关事务性资源管理器的信息并修改以下行为：

-   默认的事务资源管理器是否会在下一次装载时清除其事务元数据

-   指定的事务资源管理器，以优先于可用性

-   指定的事务资源管理器更喜欢一致性一致性

-   正在运行的事务性资源管理器的特征

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil resource [create] <RmRootPathname>
fsutil resource [info] <RmRootPathname>
fsutil resource [setautoreset] {true|false} <DefaultRmRootPathname>
fsutil resource [setavailable] <RmRootPathname>
fsutil resource [setconsistent] <RmRootPathname>
fsutil resource [setlog] [growth {<Containers> containers|<Percent> percent} <RmRootPathname>] [maxextents <Containers> <RmRootPathname>] [minextents <Containers> <RmRootPathname>] [mode {full|undo} <RmRootPathname>] [rename <RmRootPathname>] [shrink <percent> <RmRootPathname>] [size <Containers> <RmRootPathname>]
fsutil resource [start] <RmRootPathname> [<RmLogPathname> <TmLogPathname>
fsutil resource [stop] <RmRootPathname>
```

### <a name="parameters"></a>Parameters

|        参数        |                                                                                                                                                                                                                                        描述                                                                                                                                                                                                                                         |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         创建          |                                                                                                                                                                                                                    创建辅助事务资源管理器。                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        指定事务资源管理器根目录的完整路径。                                                                                                                                                                                                         |
|          信息           |                                                                                                                                                                                                            显示指定的事务性资源管理器的信息。                                                                                                                                                                                                            |
|      setautoreset       | 指定默认的事务资源管理器是否将在下一次装入时清除事务元数据。<br /><br />-将**setautoreset**参数设置为**true** ，以指定事务资源管理器在默认情况下将在下一次装入时清除事务元数据。<br />-将**setautoreset**参数设置为**false** ，以指定事务资源管理器默认情况下不会清除下一次装入时的事务元数据。 |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       指定驱动器名称后跟一个冒号。                                                                                                                                                                                                                        |
|      setavailable       |                                                                                                                                                                                                 指定事务资源管理器优先于可用性。                                                                                                                                                                                                 |
|      setconsistent      |                                                                                                                                                                                                 指定事务资源管理器将优先于可用性。                                                                                                                                                                                                 |
|         setlog          |                                                                                                                                                                                                  更改已在运行的事务性资源管理器的特性。                                                                                                                                                                                                  |
|         年          |                                                                                                  指定事务资源管理器日志可增长的量。<br /><br />可按如下所示指定增长参数：<br /><br />-使用以下格式的容器数：_容器_**容器**<br />-使用以下格式的百分比：_百分比_**百分比**                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      指定事务性资源管理器使用的数据对象。                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                指定指定的事务资源管理器的最大容器数。                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                指定指定的事务资源管理器的最小容器数。                                                                                                                                                                                                |
|  模式 {完全&#124;撤消}  |                                                                                                                                                                                        指定是记录所有事务（ **full**）还是只记录回滚事件（**undo**）。                                                                                                                                                                                         |
|         重命名          |                                                                                                                                                                                                                  更改事务资源管理器的 GUID。                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              指定事务资源管理器日志可自动减少的百分比。                                                                                                                                                                                              |
|          大小           |                                                                                                                                                                                              指定事务资源管理器的大小，以指定数目的*容器*。                                                                                                                                                                                               |
|          start          |                                                                                                                                                                                                                    启动指定的事务资源管理器。                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    停止指定的事务资源管理器。                                                                                                                                                                                                                     |

### <a name="BKMK_examples"></a>示例
若要设置 c:\test 指定的事务资源管理器的日志，以自动增长5个容器，请键入：

```
fsutil resource setlog growth 5 containers c:test
```

若要设置 c:\test 指定的事务资源管理器的日志，以自动增长两个百分比，请键入：

```
fsutil resource setlog growth 2 percent c:test
```

若要指定默认的事务资源管理器将在驱动器 C 上的下一个装载中清除事务元数据，请键入：

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[事务性 NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)


