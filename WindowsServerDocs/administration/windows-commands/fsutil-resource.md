---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: fsutil 资源
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 38b0947171b7fd8afc44a95b2244a3fd2a0c9e73
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439046"
---
# <a name="fsutil-resource"></a>fsutil 资源
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7，Windows 2008 中，Windows Vista

创建辅助事务性资源管理器，启动或停止事务性资源管理器，或显示有关事务性资源管理器的信息并修改以下行为：

-   是否默认事务性资源管理器会清除其事务性元数据在下一步装入

-   指定事务性资源管理器以首选一致性，而可用性

-   指定事务资源管理器可用性为首一致性

-   特征的正在运行事务性资源管理器

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
|         创建          |                                                                                                                                                                                                                    创建辅助事务性资源管理器。                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        指定一个事务性资源管理器的根目录的完整路径。                                                                                                                                                                                                         |
|          信息           |                                                                                                                                                                                                            显示指定事务性资源管理器的信息。                                                                                                                                                                                                            |
|      setautoreset       | 指定是否默认事务性资源管理器会清除在下一步装入的事务性元数据。<br /><br />-设置**setautoreset**参数**true**指定事务资源管理器将默认情况下清理在下一步安装的事务性元数据。<br />-设置**setautoreset**参数**false**指定事务资源管理器不会默认情况下清除下一步安装上的事务元数据。 |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       指定后接一个冒号的驱动器名称。                                                                                                                                                                                                                        |
|      setavailable       |                                                                                                                                                                                                 指定事务性资源管理器将优先于一致性首选可用性。                                                                                                                                                                                                 |
|      setconsistent      |                                                                                                                                                                                                 指定事务性资源管理器将首选的一致性，而可用性。                                                                                                                                                                                                 |
|         setlog          |                                                                                                                                                                                                  更改特征的事务性资源管理器已在运行。                                                                                                                                                                                                  |
|         增长          |                                                                                                  指定事务性资源管理器日志可以增长的量。<br /><br />可以按如下所示指定增长参数：<br /><br />-使用以下格式的容器数：*容器* **容器**<br />-   使用以下格式的百分比：*Percent* **percent**                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      指定使用由事务资源管理器的数据对象。                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                指定容器的最大数目为指定事务性资源管理器。                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                指定容器的最小数目为指定事务性资源管理器。                                                                                                                                                                                                |
|  mode {full&#124;undo}  |                                                                                                                                                                                        指定是否记录所有事务 (**完整**) 或仅回滚都记录事件 (**撤消**)。                                                                                                                                                                                         |
|         重命名          |                                                                                                                                                                                                                  更改 GUID 的事务性资源管理器。                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              指定事务性资源管理器日志可以自动减少所依据的百分比。                                                                                                                                                                                              |
|          大小           |                                                                                                                                                                                              为指定数量的指定大小的事务性资源管理器*容器*。                                                                                                                                                                                               |
|          start          |                                                                                                                                                                                                                    启动指定的事务性资源管理器。                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    停止指定的事务性资源管理器。                                                                                                                                                                                                                     |

### <a name="BKMK_examples"></a>示例
若要将日志设置为事务性资源管理器指定的 c:\test，能够自动增长的五个容器，键入：

```
fsutil resource setlog growth 5 containers c:test
```

若要将日志设置为事务性资源管理器指定 c:\test，具有 %2%自动增长的键入：

```
fsutil resource setlog growth 2 percent c:test
```

若要指定默认事务性资源管理器会清除在下一步装载驱动器 C 上的事务的元数据，请键入：

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[事务性 NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)


