---
title: diskraid
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a565a1d5fa1bc3ff57d1578fb54cfa4553e3bb26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818868"
---
# <a name="diskraid"></a>diskraid



DiskRAID 是一个命令行工具，可用于配置和管理冗余阵列 (RAID) 的独立 （或成本较低） 磁盘存储子系统。

RAID 是一种方法用于进行标准化和分类的容错磁盘系统。 RAID 级别提供性能、 可靠性和成本的各种的组合。 服务器上通常使用 RAID。 某些服务器提供三个 RAID 级别：级别 0 （条带化）、 等级 1 （镜像） 和 5 （条带化的奇偶校验）。

硬件 RAID 子系统使用逻辑单元号 (LUN) 从另一个区分以物理方式可寻址存储单元。 LUN 对象必须具有至少一个复杂，并且可以具有任意数量的其他丛。 每个复杂包含 LUN 对象上的数据的副本。 可以添加到和从 LUN 对象中删除丛。

大多数 DiskRAID 命令对特定的主机总线适配器 (HBA) 端口、 发起程序适配器、 发起方门户、 提供程序、 子系统、 控制器、 端口、 驱动器、 LUN、 目标门户、 目标或目标门户组执行操作。 使用 SELECT 命令来选择某一对象。 所选的对象称为具有焦点。 焦点简化了常见配置任务，如创建相同的子系统中的多个 Lun。

> [!NOTE]
> DiskRAID 命令行工具仅适用于支持虚拟磁盘服务 (VDS) 的存储子系统。

## <a name="diskraid-commands"></a>DiskRAID 命令

若要查看命令语法，请单击命令:
-   [add](#BKMK_1)
-   [associate](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [chap](#BKMK_5)
-   [create](#BKMK_6)
-   [delete](#BKMK_7)
-   [detail](#BKMK_8)
-   [dissociate](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [help](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [initiator](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [list](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [maintenance](#BKMK_22)
-   [名称](#BKMK_23)
-   [offline](#BKMK_24)
-   [online](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerate](#BKMK_27)
-   [refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [删除](#BKMK_30)
-   [replace](#BKMK_31)
-   [reset](#BKMK_32)
-   [select](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [standby](#BKMK_35)
-   [unmask](#BKMK_36)

### <a name="BKMK_1"></a>add

将现有 LUN 添加到当前所选的 LUN，或将 iSCSI 目标门户添加到当前所选的 iSCSI 目标门户组。

#### <a name="syntax"></a>语法

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>Parameters

**复杂 lun**=*n*

指定要将每个丛作为添加到当前所选的 LUN 的 LUN 号。

> [!CAUTION]
> 将删除添加为复杂的 LUN 上的所有数据。

**tpgroup tportal = * * * n*

指定的 iSCSI 目标门户编号，以将添加到当前所选的 iSCSI 目标门户组。

**noerr**

指定执行此操作时出现任何故障将被忽略。 这可在脚本模式下。

### <a name="BKMK_2"></a>associate

设置指定的控制器列表的端口为活动状态为当前所选的 LUN （其他控制器端口都处于非活动状态），或将指定的控制器端口添加到现有的主动控制器端口的列表中，为当前所选的 LUN，或将相关联指定的 iSCSI 目标的当前所选的 LUN。

#### <a name="syntax"></a>语法

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>Parameters

**controllers**

与 VDS 1.0 提供程序一起使用。 将添加到或替换与当前所选的 LUN 关联的控制器的列表。

**ports**

与 VDS 1.1 提供程序一起使用。 将添加到或替换与当前所选的 LUN 关联的控制器端口的列表。

**targets**

与 VDS 1.1 提供程序一起使用。 将添加到或替换与当前所选的 LUN 关联的 iSCSI 目标的列表。

**add**

VDS 1.0 提供程序将添加到与该 LUN 关联的控制器的现有列表的指定的控制器。 如果未指定此参数，控制器的列表将替换现有控制器与该 LUN 关联的列表。

对于 VDS 1.1 提供程序，将指定的控制器端口添加到与该 LUN 关联的控制器端口的现有列表。 如果未指定此参数，控制器端口的列表将替换现有的控制器端口与该 LUN 关联的列表。
```
<n>[,<n> [, ...]]
```
以用于**控制器**或**目标**参数。 指定控制器或 iSCSI 目标设置为活动或相关联的数字。
```
<n-m>[,<n-m>[,…]]
```
以用于**端口**参数。 指定要设置活动使用控制器编号的控制器端口 (*n*) 和端口号 (*m*) 对。

#### <a name="example"></a>示例

下面的示例演示如何将相关联并将端口添加到使用 VDS 1.1 提供程序的 LUN:
```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)
```

### <a name="BKMK_3"></a>automagic

设置或清除标志，如何配置 LUN 上的提供程序的提示。 使用不带任何参数， **automagic**操作显示的标志列表。

#### <a name="syntax"></a>语法

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>Parameters

**set**

将指定的标志设置为指定的值。

**clear**

清除指定的标志。 **所有**关键字清除所有 automagic 标志。

**apply**

当前的标志应用于所选的 LUN。

\<flag>

由三个字母缩写词标识标志。

|Flag|描述|
|----|-----------|
|FCR|所需的快速故障恢复|
|FTL|具备容错能力|
|MSR|主要是读取|
|MXD|最大的驱动器|
|MXS|预期的最大大小|
|ORA|最佳读取对齐方式|
|OR|最佳读取大小|
|OSR|针对顺序读取进行优化|
|OSW|对于顺序写入优化|
|OWA|最佳写入对齐方式|
|OWS|最佳写入大小|
|RBP|重新生成优先级|
|RBV|阅读过往验证是否已启用|
|RMP|启用重新映射|
|STS|条带大小|
|WTC|写通式缓存已启用|
|YNK|可移动|

### <a name="BKMK_4"></a>中断

从当前所选的 LUN 删除丛。 复杂和它所包含的数据不会保留，并且可以回收的驱动器扩展盘区。

#### <a name="syntax"></a>语法

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>Parameters

**plex**

指定复杂删除数。 复杂和它所包含的数据不会保留，并使用此复杂的资源将被回收。 不保证 LUN 上包含的数据保持一致。 如果你想要保留此复杂，使用卷影复制服务 (VSS)。

**noerr**

指定执行此操作时出现任何故障将被忽略。 这可在脚本模式下。

#### <a name="remarks"></a>备注

> [!NOTE]
> 必须先在使用之前选择镜像的 LUN**中断**命令。

> [!CAUTION]
> 将删除丛上的所有数据。

> [!CAUTION]
> 包含原始 LUN 上的所有数据并非一定要一致。

### <a name="BKMK_5"></a>chap

设置质询握手身份验证协议 (CHAP) 共享机密，以便 iSCSI 发起程序和 iSCSI 目标可以相互通信。

#### <a name="syntax"></a>语法

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>Parameters

**发起程序集**

设置时发起方进行身份验证目标使用相互 CHAP 身份验证的本地 iSCSI 发起程序服务中的共享的机密。

**请记住发起程序**

通信本地 iSCSI 发起程序服务到 iSCSI 目标 CHAP 的机密，以便发起方服务可以使用密钥以在 CHAP 身份验证过程验证自身身份的目标。

**target set**

当前所选的 iSCSI 目标目标对发起方进行身份验证时用于 CHAP 身份验证中设置的共享的机密。

**请记住目标**

通信到当前的焦点在 iSCSI 目标 iSCSI 发起程序 CHAP 机密，以便目标可以使用以在相互 CHAP 身份验证过程验证自身身份向发起方的机密。

**secret**

指定要使用的机密。 如果为空将清除机密。

**target**

指定要将与密钥相关联的当前所选子系统中的目标。 在发起程序上设置机密时，这是可选的而剩余的指示机密将用于所有还没有关联的机密的目标。

**initiatorname**

指定发起方 iSCSI 名称要与密钥关联。 在目标中设置机密时，这是可选，而剩余的指示机密将用于所有发起程序都没有关联的机密。

### <a name="BKMK_6"></a>创建

在当前所选子系统上创建新的 LUN 或 iSCSI 目标或当前所选的目标上创建目标门户组。 您可以查看实际绑定使用**DiskRAID 列表**命令。

#### <a name="syntax"></a>语法

```
create lun simple [size=<n>] [drives=<n>] [noerr]
create lun stripe [size=<n>] [drives=<n, n> [,...]]  [stripesize=<n>] [noerr]
create lun raid [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun mirror [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun automagic size=<n> [noerr]
create target name=<name> [iscsiname=<iscsiname>] [noerr]
create tpgroup [noerr]
```

#### <a name="parameter"></a>参数

**simple**

创建简单 LUN。

**stripe**

创建带区的 LUN。

**RAID**

创建具有奇偶校验的带区的 LUN。

**mirror**

创建镜像的 LUN。

**automagic**

创建一个 LUN 使用*automagic*当前中效果的提示。 请参阅**automagic**子命令的详细信息。

**size**=

指定的总的 LUN 大小以兆字节为单位。 如果**大小 =** 未指定参数，创建 LUN 将允许所有指定的驱动器的最大可能大小。

提供程序通常创建 LUN 至少与请求的大小一样大，但该提供程序可能需要向上舍入到在某些情况下的下一步最大大小。 例如，如果如.99 GB 和提供程序可以仅 GB 磁盘将区分配指定大小，则生成的 LUN 为 1 GB。

若要指定使用其他单位的大小，大小之后立即使用一个已识别的以下后缀：
-   **B**个字节。
-   **KB**为千字节。
-   **MB**为兆字节。
-   **GB**为千兆字节。
-   **TB** terabyte 的。
-   **PB**的千万亿字节。

**驱动器**=

指定*drive_number*驱动器用于创建 LUN。 如果**大小 =** 参数未指定，则创建的 LUN 是所有指定的驱动器允许的最大可能大小。 如果**大小 =** 指定参数、 提供程序将从指定的驱动器列表创建 LUN 中选择驱动器。 提供程序将尝试使用驱动器时可能指定的顺序。

**stripesize**=

指定的大小以兆字节为单位*stripe*或*RAID* LUN。 创建 LUN 后，不能更改 stripesize。

若要指定使用其他单位的大小，大小之后立即使用一个已识别的以下后缀：
-   **B**个字节。
-   **KB**为千字节。
-   **MB**为兆字节。
-   **GB**为千兆字节。
-   **TB** terabyte 的。
-   **PB**的千万亿字节。

**target**

当前所选子系统上创建一个新的 iSCSI 目标。

**名称**

提供目标的友好名称。

**iscsiname**

提供的 iSCSI 目标的名称，并可以省略以让提供程序生成的名称。

**tpgroup**

在当前所选的目标系统上创建新的 iSCSI 目标门户组。

**noerr**

指定执行此操作时出现任何故障将被忽略。 这可在脚本模式下。

#### <a name="remarks"></a>备注

-   任一**大小**= 或**驱动器**= 必须指定参数。 它们还可在一起。
-   创建后不能更改 LUN 的条带大小。

### <a name="BKMK_7"></a>delete

删除当前所选的 LUN，iSCSI 目标 （只要有没有任何关联与 iSCSI 目标 Lun） 或 iSCSI 目标门户组。

#### <a name="syntax"></a>语法

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>Parameters

**lun**

删除当前所选的 LUN 上的所有数据。

**uninstall**

指定将清除与该 LUN 关联的本地系统上的磁盘，删除 LUN 之前。

**target**

如果没有 Lun 是与目标关联，请删除当前所选的 iSCSI 目标。

**tpgroup**

删除当前所选的 iSCSI 目标门户组。

**noerr**

指定执行此操作时出现任何故障将被忽略。 这可在脚本模式下。

### <a name="BKMK_8"></a>详细信息

显示有关当前所选对象的指定类型的详细的信息。

#### <a name="syntax"></a>语法

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>Parameters

**hbaport**

列出有关当前所选的主机总线适配器 (HBA) 端口的详细的信息。

**iadapter**

列出有关当前所选的 iSCSI 发起程序适配器的详细的信息。

**iportal**

列出有关当前所选的 iSCSI 发起程序门户的详细的信息。

**provider**

列出有关当前所选提供程序的详细的信息。

**subsystem**

列出有关当前所选子系统的详细的信息。

**controller**

列出有关当前所选的控制器的详细的信息。

**port**

列出有关当前所选的控制器端口的详细的信息。

**drive**

列出有关当前所选的驱动器，包括占据 Lun 的详细的信息。

**lun**

列出有关当前所选的 LUN，包括发布的详细的信息驱动。 根据该 LUN 是光纤通道或 iSCSI 子系统的一部分，输出会稍有不同。 如果未屏蔽的主机列表包含仅一个星号，这意味着 LUN 取消屏蔽的所有主机。

**tportal**

列出有关当前所选的 iSCSI 目标门户的详细的信息。

**target**

列出有关当前所选的 iSCSI 目标的详细的信息。

**tpgroup**

列出有关当前所选的 iSCSI 目标门户组的详细的信息。

**verbose**

仅与该 LUN 参数一起使用。 列出的其他信息，包括其丛。

### <a name="BKMK_9"></a>dissociate

设置为当前所选的 LUN （其他控制器端口不会受到影响），为非活动指定的控制器端口的列表或当前所选的 lun dissociates 指定的 iSCSI 目标列表。

#### <a name="syntax"></a>语法

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>参数

**controllers**

与 VDS 1.0 提供程序一起使用。 从与当前所选的 LUN 关联的控制器的列表中移除控制器。

**ports**

与 VDS 1.1 提供程序一起使用。 从与当前所选的 LUN 关联的控制器端口的列表中移除控制器端口。

**targets**

与 VDS 1.1 提供程序一起使用。 从与当前所选的 LUN 关联的 iSCSI 目标的列表中删除目标。
```
<n> [,<n> [,…]]
```
以用于**控制器**或**目标**参数。 指定控制器或 iSCSI 目标设置为非活动状态或取消关联的数字。
```
<n-m>[,<n-m>[,…]]
```
以用于**端口**参数。 指定要使用控制器编号设置为非活动状态的控制器端口 (*n*) 和端口号 (*m*) 对。

#### <a name="example"></a>示例

```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)

DISKRAID> DISSOCIATE PORTS 0-0,1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 1)
```

### <a name="BKMK_10"></a>exit

退出 DiskRAID。

#### <a name="syntax"></a>语法

```
exit
```

### <a name="BKMK_11"></a>extend

通过了 LUN 的末尾添加扇区来扩展当前所选的 LUN。 并非所有提供程序支持扩展 Lun。 不会扩展任何卷或 LUN 上包含的文件系统。 扩展 LUN 后，应扩展使用的关联的磁盘上结构**DiskPart 扩展**命令。

#### <a name="syntax"></a>语法

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>Parameters

**size=**

指定的大小以兆字节为单位来扩展该 LUN。 如果**大小 =** 未指定参数、 LUN 可通过允许指定的所有驱动器的最大可能大小进行扩展。 如果**大小 =** 指定参数，提供程序从指定的列表中选择驱动器**驱动器 =** 参数来创建 LUN。

若要指定使用其他单位的大小，大小之后立即使用一个已识别的以下后缀：
-   **B**个字节。
-   **KB**为千字节。
-   **MB**为兆字节。
-   **GB**为千兆字节。
-   **TB**为 terabyte
-   **PB**的千万亿字节

**drives=**

指定\<drive_number > 创建 LUN 时要使用的驱动器。 如果**大小 =** 参数未指定，则创建的 LUN 是所有指定的驱动器允许的最大可能大小。 提供程序使用的驱动器时可能指定的顺序。

**noerr**

指定应忽略执行此操作时出现任何故障。 这可在脚本模式下。

#### <a name="remarks"></a>备注

任一*大小*或\<驱动器 > 必须指定参数。 它们还可在一起。

### <a name="BKMK_12"></a>flushcache

清除当前所选的控制器上缓存。

#### <a name="syntax"></a>语法

```
flushcache controller
```

### <a name="BKMK_13"></a>帮助

显示所有 DiskRAID 命令的列表。

#### <a name="syntax"></a>语法

```
help
```

### <a name="BKMK_14"></a>importtarget

获取或设置当前设置为当前所选子系统的卷影复制服务 (VSS) 导入目标。

#### <a name="syntax"></a>语法

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>参数

**设置目标**

如果指定，则设置为当前所选子系统的 VSS 导入目标当前所选的目标。 如果未指定，该命令将检索当前设置为当前所选子系统的 VSS 导入目标。

### <a name="BKMK_15"></a>发起方

检索有关本地 iSCSI 发起程序的信息。

#### <a name="syntax"></a>语法

```
initiator
```

### <a name="BKMK_16"></a>invalidatecache

使当前所选的控制器上缓存无效。

#### <a name="syntax"></a>语法

```
invalidatecache controller
```

### <a name="BKMK_18"></a>lbpolicy

在当前所选的 LUN 上设置负载平衡策略。

#### <a name="syntax"></a>语法

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>Parameters

**type**

指定负载平衡策略。 如果未指定类型，则**路径**必须指定参数。 类型可以是以下值之一：

**故障转移**:与其他正在备份路径的路径将使用一个主路径。

**ROUNDROBIN**:使用轮循机制方式，按顺序尝试每个路径中的所有路径。

**SUBSETROUNDROBIN**:使用轮循机制方式; 中的所有主路径仅当所有主路径发生故障时，才使用备份路径。

**DYNLQD**:具有最少使用的路径的活动请求数。

**加权**:使用最小权重 （每个路径必须分配有权重） 使用的路径。

**LEASTBLOCKS**:用最少的块中使用的路径。

**VENDORSPECIFIC**:使用特定于供应商的策略。

**paths**

指定路径是否**主**或具有特定\<权重 >。 未指定任何路径隐式设置为备份。 列出的任何路径必须是当前所选的 LUN 的路径之一。

### <a name="BKMK_19"></a>list

显示指定类型的对象的列表。

#### <a name="syntax"></a>语法

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>Parameters

**hbaports**

列出所有已知的 VDS 的 HBA 端口有关的摘要信息。 当前所选的 HBA 端口被标为星号 （*）。

**iadapters**

列出所有已知的 VDS 的 iSCSI 发起程序适配器的摘要信息。 一个星号 （*） 标记当前所选的发起程序适配器。

**iportals**

列出有关当前所选的发起程序适配器中的所有 iSCSI 发起程序门户的摘要信息。 当前所选的发起方门户被标记为星号 （*）。

**providers**

列出了已知 VDS 的每个提供程序的摘要信息。 一个星号 （*） 标记当前所选提供程序。

**subsystems**

列出有关在系统中每个子系统的摘要信息。 一个星号 （*） 标记当前所选的子系统。

**controllers**

列出有关当前所选子系统中的每个控制器的摘要信息。 一个星号 （*） 标记当前所选的控制器。

**ports**

列出有关当前所选的控制器中的每个控制器端口的摘要信息。 当前所选的端口被标为星号 （*）。

**drives**

列出有关当前所选子系统中的每个驱动器的摘要信息。 一个星号 （*） 标记当前所选的驱动器。

**luns**

列出有关当前所选子系统中的每个 LUN 的摘要信息。 一个星号 （*） 标记当前所选的 LUN。

**tportals**

列出有关当前所选子系统中的所有 iSCSI 目标门户的摘要信息。 一个星号 （*） 标记当前所选的目标门户。

**targets**

列出有关当前所选子系统中的所有 iSCSI 目标的摘要信息。 一个星号 （*） 标记当前所选的目标。

**tpgroups**

列出有关中的所有 iSCSI 目标门户组当前所选目标的摘要信息。 当前所选的门户组标记为星号 （*）。

### <a name="BKMK_20"></a>登录名

登录到当前所选的 iSCSI 目标指定的 iSCSI 发起程序适配器。

#### <a name="syntax"></a>语法

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>Parameters

**type**

指定的登录名执行类型：**手动**，**持久**，或**启动**。 如果未指定，将执行一个手动的登录名。

**手动**-登录名手动。

**永久性**-自动重新启动计算机时使用相同的登录名。

**启动**-(此选项是将来的开发和当前未使用 *。*)

**chap**

指定要使用 CHAP 身份验证的类型：**无**， **oneway** CHAP，或**相互**CHAP; 如果未指定，将使用无身份验证。

**tportal**

指定要使用的登录的当前所选子系统中的可选目标门户。

**iportal**

指定可选的发起程序门户中指定的发起程序适配器要使用的登录。

\<flag>

由三个字母缩写词：

**IP**:需要 IPsec

**EMP**:启用多路径

**EHD**:启用标题摘要

**EDD**:启用数据摘要

### <a name="BKMK_21"></a>注销

记录指定的 iSCSI 发起程序适配器从当前所选的 iSCSI 目标。

#### <a name="syntax"></a>语法

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>Parameters

**iadapter**

指定发起程序适配器与登录会话中注销。

### <a name="BKMK_22"></a>维护

执行对指定类型的当前所选对象的维护操作。

#### <a name="syntax"></a>语法

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>Parameters

\<object>

指定要对其执行该操作的对象的类型。 *对象*类型可以是**子系统**，**控制器**，**端口驱动器**或**LUN**。

\<operation>

指定要执行的维护操作。 *操作*类型可以是**旋转**，**驱动器降速功能**，**闪烁**，**提示音**或**ping**. *操作*必须指定。

**count=**

指定的次数重复*操作*。 这通常与一起使用**闪烁**，**提示音**，或**ping**。

### <a name="BKMK_23"></a>name

将当前所选子系统、 LUN 或 iSCSI 目标的友好名称设置为指定的名称。

#### <a name="syntax"></a>语法

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>参数

\<name>

指定的子系统、 LUN 或目标的名称。 名称必须少于 64 个字符的长度。 如果未提供的名称，现有的名称，如果存在，被删除。

### <a name="BKMK_24"></a>offline

设置在指定的类型的当前所选对象的状态**脱机**。

#### <a name="syntax"></a>语法

```
offline <object>
```

#### <a name="parameter"></a>参数

\<object>

指定要对其执行此操作的对象的类型。 \<对象 >

类型可以是**子系统**，**控制器**，**驱动器**， **LUN**，或**tportal**。

### <a name="BKMK_25"></a>online

设置在指定的类型所选对象的状态**online**。 如果对象是**hbaport**，路径的状态更改为当前所选 HBA 端口到**联机**。

#### <a name="syntax"></a>语法

```
online <object> 
```

#### <a name="parameter"></a>参数

\<object>

指定要对其执行此操作的对象的类型。 \<对象 >

类型可以是**hbaport**，**子系统**，**控制器**，**驱动器**， **LUN**，或**tportal**。

### <a name="BKMK_26"></a>recover

执行操作所需，如重新同步或热备份修复当前所选的容错的 LUN。 例如，恢复可能会导致热备用要绑定到具有故障的磁盘或范围重新分配其他磁盘的 RAID 集。

#### <a name="syntax"></a>语法

```
recover <lun>
```

### <a name="BKMK_27"></a>reenumerate

重新枚举指定类型的对象。 如果使用的扩展 LUN 命令，必须使用刷新命令来使用 reenumerate 命令之前更新磁盘大小。

#### <a name="syntax"></a>语法

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>Parameters

**subsystems**

查询提供程序来发现已在当前所选提供程序中添加任何新的子系统。

**drives**

查询内部的 I/O 总线，以发现已在当前所选子系统中添加任何新驱动器。

### <a name="BKMK_28"></a>refresh

刷新当前所选提供程序的内部数据。

#### <a name="syntax"></a>语法

```
refresh provider
```

### <a name="BKMK_29"></a>rem

使用注释的脚本。

#### <a name="syntax"></a>语法

```
Rem <comment>
```

### <a name="BKMK_30"></a>remove

从当前所选的目标门户组中删除指定的 iSCSI 目标门户。

#### <a name="syntax"></a>语法

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>参数

**tpgroup tportal=** \<tportal>

指定要删除的 iSCSI 目标门户。

**noerr**

指定应忽略执行此操作时出现任何故障。 这可在脚本模式下。

### <a name="BKMK_31"></a>replace

替换当前所选驱动器指定的驱动器。

#### <a name="syntax"></a>语法

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>参数

**drive=**

指定\<drive_number > 要替换的驱动器。

#### <a name="remarks"></a>备注

-   指定的驱动器可能不是当前所选的驱动器。

### <a name="BKMK_32"></a>reset

重置当前所选的控制器或端口。

#### <a name="syntax"></a>语法

```
Reset {controller | port}
```

#### <a name="parameters"></a>Parameters

**controller**

重置控制器。

**port**

重置该端口。

### <a name="BKMK_33"></a>select

显示或更改当前选定的对象。

#### <a name="syntax"></a>语法

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>Parameters

**object**

指定要选择对象的类型。 \<对象 > 类型可以是**提供程序**，**子系统**，**控制器**，**驱动器**，或**LUN**.

**hbaport** [\<n>]

将焦点设置到指定的本地 HBA 端口。 如果未不指定任何 HBA 端口，则该命令显示当前所选的 HBA 端口 （如果有）。 指定无效的 HBA 端口索引结果中没有的焦点 HBA 端口。 选择 HBA 端口取消选择任何所选的发起程序适配器和发起方门户。

**iadapter** [\<n>]

将焦点设置到指定的本地 iSCSI 发起程序适配器。 如果不指定了任何发起程序适配器，该命令显示当前所选的发起程序适配器 （如果有）。 指定无效的发起程序适配器索引导致未处于焦点发起程序适配器。 选择的发起程序适配器取消选择任何所选的 HBA 端口和发起方门户。

**iportal** [\<n>]

将焦点设置到指定的本地 iSCSI 发起程序门户中所选的 iSCSI 发起程序适配器。 如果指定无发起程序门户，则该命令显示当前所选的发起方门户 （如果有）。 指定无效的发起方门户索引会不导致任何所选的发起方门户。

**provider** [\<n>]

将焦点设置到指定的提供程序。 如果不指定任何提供程序，该命令显示当前所选提供程序 （如果有）。 指定的提供程序无效索引导致未处于焦点提供程序。

**subsystem** [\<n>]

将焦点设置到指定的子系统。 如果不指定任何子系统，则该命令将显示具有焦点的子系统 （如果有）。 指定无效的子系统索引会导致没有焦点子系统。 隐式选择子系统选择其关联的提供程序。

**controller** [\<n>]

将焦点设置到当前所选子系统中指定的控制器。 如果指定没有控制器，则该命令显示当前所选的控制器 （如果有）。 指定无效控制器索引会导致无焦点控制器。 选择一个控制器取消选择任何所选的控制器端口、 驱动器、 Lun、 目标端口、 目标和目标门户组。

**port** [\<n>]

将焦点设置到当前所选的控制器中指定的控制器端口。 如果未不指定任何端口，则该命令显示当前所选的端口 （如果有）。 指定的端口无效索引导致未选择的端口。

**drive** [\<n>]

将焦点设置到指定的驱动器或物理心轴，当前所选子系统中。 如果未指定驱动器，该命令显示当前所选的驱动器 （如果有）。 指定驱动器无效索引导致无焦点的驱动器。 选择一个驱动器，取消选择任何所选的控制器、 控制器端口、 Lun、 目标端口、 目标和目标门户组。

**lun** [\<n>]

将焦点设置到当前所选子系统中指定的 LUN。 如果不指定任何 LUN，则该命令显示当前所选的 LUN （如果有）。 指定无效的 LUN 索引会不导致任何所选的 LUN。 选择一个 LUN 取消选择任何所选的控制器、 控制器端口、 驱动器、 目标端口、 目标和目标门户组。

**tportal** [\<n>]

将焦点设置到指定的 iSCSI 目标门户中当前所选的子系统。 如果不指定任何目标门户，则该命令显示当前所选的目标门户 （如果有）。 指定无效的目标门户索引会不导致任何所选的目标门户。 选择目标门户取消选择任何控制器、 控制器端口、 驱动器、 Lun、 目标和目标门户组。

**target** [\<n>]

将焦点设置到当前所选子系统中指定的 iSCSI 目标。 如果未指定目标，则该命令显示当前所选的目标 （如果有）。 指定无效的目标索引会不导致任何所选目标。 选择目标取消选择任何控制器、 控制器端口、 驱动器、 Lun、 目标端口和目标门户组。

**tpgroup** [\<n>]

将焦点设置到指定的 iSCSI 目标门户组中当前所选的 iSCSI 目标。 如果不指定任何目标门户组，则该命令显示当前所选的目标门户组 （如果有）。 指定无效的目标门户组索引会导致无焦点目标门户组。

[\<n>]

指定\<对象数 > 若要选择。 如果<object number>指定不是有效，指定类型的对象的任何现有所选内容将被清除。 如果没有<object number>指定，则会显示当前的对象。

### <a name="BKMK_34"></a>setflag

将当前所选的驱动器设置为热备用。

#### <a name="syntax"></a>语法

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>Parameters

**true**

选择当前所选的驱动器作为热备用。

**false**

取消当前所选的驱动器选择为热备用。

#### <a name="remarks"></a>备注

不能为普通 LUN 绑定操作使用热备用。 它们是保留供故障仅处理。 驱动器必须是当前未绑定到任何现有 LUN。

### <a name="BKMK_shrink"></a>收缩

减少了所选的 LUN 的大小。

#### <a name="syntax"></a>语法

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>Parameters

**size=**

通过指定所需的兆字节 (MB) 以降低 LUN 的大小的空间量。 若要指定使用其他单位的大小，大小之后立即使用一个已识别的后缀 （B、 KB、 MB、 GB、 TB 和 PB）。

**noerr**

指定执行此操作时出现任何故障将被忽略。 这可在脚本模式下。

### <a name="BKMK_35"></a>备用服务器

更改为当前所选的主机总线适配器 (HBA) 端口到备用路径的状态。

#### <a name="syntax"></a>语法

```
standby hbaport
```

#### <a name="parameters"></a>Parameters

**hbaport**

更改为当前所选的主机总线适配器 (HBA) 端口到备用路径的状态。

### <a name="BKMK_36"></a>取消屏蔽

从指定的主机设置可访问当前所选的 Lun。

#### <a name="syntax"></a>语法

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>Parameters

**all**

指定 LUN 应进行可从所有主机访问。 但是，不能取消屏蔽到 iSCSI 子系统中的所有目标 LUN。

> [!IMPORTANT]
> 你必须注销然后再运行取消屏蔽所有命令目标。

**none**

指定 LUN 不应访问的任何主机。

> [!IMPORTANT]
> 你必须注销然后再运行取消屏蔽 LUN 无命令目标。

**add**

指定指定的主机，必须添加到现有的此 LUN 是从可访问的主机列表。 如果未指定此参数，提供的主机的列表将替换现有的此 LUN 是从可访问的主机列表。

**WWN=**

指定十六进制数字表示从该 LUN 或主机应进行访问的全球范围内名称的列表。 若要掩码/取消屏蔽到一组特定的主机光纤通道子系统中，可以在感兴趣的主机计算机上键入 WWN 的端口的以分号分隔的列表。

**initiator=**

指定对其当前所选的 LUN 应进行可访问的 iSCSI 发起程序的列表。 若要掩码/取消屏蔽到一组特定的 iSCSI 子系统中的主机，可以感兴趣的主机计算机上键入发起程序的 iSCSI 发起程序名称之间用分号分隔的列表。

**uninstall**

如果指定，则卸载之前 LUN 屏蔽与本地系统上的 LUN 关联的磁盘。

## <a name="scripting-diskraid"></a>脚本 DiskRAID

可以使用关联的 VDS 硬件提供程序运行 Windows Server 2008 或 Windows Server 2003 的任何计算机上编写 DiskRAID 的脚本。 若要调用的 DiskRAID 脚本，在命令提示符下键入：
```
diskraid /s <script.txt>
```
默认情况下，DiskRAID 停止处理命令，并返回错误代码，如果在脚本中的问题。 若要继续运行脚本并忽略错误，包括命令的 NOERR 参数。 这将允许使用单个脚本删除而不考虑总的 Lun 数目的子系统中的所有 Lun 作为此类有用实践。 并非所有的命令支持 NOERR 参数。 有关命令语法错误，而不考虑是否包含 NOERR 参数，始终返回错误

### <a name="diskraid-error-codes"></a>DiskRAID 错误代码

|错误代码|错误描述|
|----------|-----------------|
|0|未发生错误。 整个脚本运行而不会失败。|
|1|出现致命异常。|
|2|DiskRAID 命令行上指定的参数不正确。|
|3|DiskRAID 无法打开指定的脚本或输出文件。|
|4|返回故障 DiskRAID 使用的服务之一。|
|5|命令语法出错。 该脚本失败，因为对象不正确地选择，或与此命令一起使用时无效。|

## <a name="example-interactively-view-status-of-subsystem"></a>例如：以交互方式查看子系统的状态

如果你想要查看计算机上的子系统 0 的状态，请在命令行键入以下：
```
diskraid
```
按 Enter。 显示以下消息：
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
若要选择子系统 0，在 DiskRAID 提示符下键入以下命令：
```
select subsystem 0
```
按 Enter。 将显示类似于以下输出：
```
Subsystem 0 is now the selected subsystem.

DISKRAID> list drives

  Drive ###  Status      Health          Size      Free    Bus  Slot  Flags
  ---------  ----------  ------------  --------  --------  ---  ----  -----
  Drive 0    Online      Healthy         107 GB    107 GB    0     1
  Drive 1    Offline     Healthy          29 GB     29 GB    1     0
  Drive 2    Online      Healthy         107 GB    107 GB    0     2
  Drive 3    Not Ready   Healthy          19 GB     19 GB    1     1
```
若要退出 DiskRAID，DiskRAID 提示符处键入以下命令：
```
exit
```