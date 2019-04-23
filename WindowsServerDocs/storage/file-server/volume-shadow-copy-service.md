---
Title: 卷影复制服务
ms.date: 01/30/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7b61a0494b8a63168b40bfaed42dedf0fff40c35
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887258"
---
# <a name="volume-shadow-copy-service"></a>卷影复制服务

适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2、 Windows Server 2008、 Windows 10、 Windows 8.1，Windows 8、 Windows 7

备份和还原关键业务数据可能非常复杂，由于以下问题：

  - 数据通常需要备份生成数据的应用程序仍在运行时。 这意味着某些数据文件可能处于打开状态，或可能处于不一致的状态。  
      
  - 如果数据集较大，它可能很难一次备份所有它。  
      

正确执行备份和还原操作需要备份的应用程序、 进行了备份，业务线应用程序和存储管理硬件和软件之间的紧密协调。 卷影复制服务 (VSS)，Windows Server® 2003年中引入，便于使用户可更好地结合这些组件之间的会话。 所有组件都支持 VSS，可以使用它们来备份无需使应用程序脱机的应用程序数据。

VSS 协调创建的是要备份的数据的一致的卷影副本 （也称为快照或时间点副本） 所需的操作。 卷影副本可用作-，或在如下所示的方案中使用它：

  - 你想要备份应用程序数据和系统状态信息，包括归档数据到另一个磁盘驱动器、 磁带或其他可移动介质。  
      
  - 是数据挖掘。  
      
  - 正在执行磁盘到磁盘的备份。  
      
  - 通过将数据恢复到原始 LUN 或替换失败的原始 LUN 全新 LUN 需要从数据丢失的快速恢复。  
      

Windows 功能和使用 VSS 的应用程序包括：

  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [共享文件夹的卷影副本](http://go.microsoft.com/fwlink/?linkid=142874)(http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892) (http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [系统还原](http://go.microsoft.com/fwlink/?linkid=180893)(http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>卷影复制服务的工作原理

完整的 VSS 解决方案需要的所有以下基本部分：

**VSS 服务**   属于 Windows 操作系统，以确保其他组件可以正确地相互通信并协同工作。

**VSS 请求者**   请求的实际创建的卷影副本 （或其他高级操作，例如导入或删除它们） 的软件。 通常情况下，这是备份应用程序。 Windows Server Backup 实用工具和 System Center Data Protection Manager 应用程序 VSS 请求者。 非 Microsoft® VSS 请求者包括几乎所有在 Windows 运行的备份软件。

**VSS 编写器**   保证我们具有一致的数据集，若要备份的组件。 这通常是作为业务线应用程序，例如 SQL Server® 或 Exchange Server 的一部分提供。 与 Windows 操作系统中包含各种 Windows 组件，如注册表中，VSS 编写器。 许多应用程序需要在备份的数据一致性保证注册的 Windows 中包含非 Microsoft VSS 编写器。

**VSS 提供程序**   的组件的创建和维护卷影副本。 这可能在该软件或硬件中。 Windows 操作系统包括使用写入时复制的 VSS 提供程序。 如果使用存储区域网络 (SAN)，则务必安装 VSS 硬件提供程序为 SAN，如果提供。 硬件提供程序可创建和维护卷影副本从主机操作系统的任务。

下图说明了如何 VSS 服务与请求者、 作者和提供程序创建卷影副本卷的协调。

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**图 1**   卷影复制服务的体系结构图示

### <a name="how-a-shadow-copy-is-created"></a>如何创建卷影副本

本部分将的请求者、 编写器和提供程序的各种角色放入上下文，通过列出需要采取以创建卷影副本的步骤。 下图显示了卷影复制服务如何控制总体协调的请求者、 编写器和提供程序。

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**图 2**卷影副本创建过程

若要创建的卷影副本，请求者、 编写器和提供程序，请执行以下操作：

1.  请求者会要求卷影复制服务来枚举编写器、 收集编写器元数据，并为卷影副本创建准备。  
      
2.  每个编写器创建需要备份的组件和数据存储的 XML 说明，并将其提供给卷影复制服务。 编写器还定义用于对所有组件的还原方法。 卷影复制服务提供给请求者，选择将备份的组件编写器的说明。  
      
3.  卷影复制服务通知其使数据准备好进行卷影副本的所有编写器。  
      
4.  每个编写器准备作为相应的数据，如完成所有打开的事务，回滚事务日志，并刷新缓存。 准备好进行卷影复制的数据时，编写器会通知卷影复制服务。  
      
5.  卷影复制服务告知来暂时冻结应用程序写入 I/O 请求 （I/O 请求仍有可能读取） 的编写器用于创建卷或卷的卷影副本所需的几秒。 不允许应用程序冻结花费的时间超过 60 秒。 卷影复制服务刷新文件系统缓冲区，然后会冻结文件系统中，这将确保正确地记录文件系统元数据和要卷影复制的数据写入顺序一致。  
      
6.  卷影复制服务告知提供程序创建卷影副本。 卷影副本创建段持续时间不超过 10 秒，在此期间所有写入到文件系统 I/O 请求仍保持冻结状态。  
      
7.  卷影复制服务释放文件系统写入 I/O 请求。  
      
8.  VSS 通知编写器来解冻应用程序写入 I/O 请求。 此时，应用程序可随意继续向正在卷影复制的磁盘写入数据。  
      

> [!NOTE]
> 如果处于冻结状态的时间超过 60 秒或如果提供程序花费的时间超过 10 秒提交的卷影复制进行编写器，可以中止卷影副本创建。 
<br>

9.  请求者可以重试过程 （请回到步骤 1），或者通知管理员稍后重试。  
      
10. 如果已成功创建卷影副本，卷影复制服务将返回给请求者的卷影副本的位置信息。 在某些情况下，卷影副本可以暂时可为读写卷以便该 VSS 和一个或多个应用程序的卷影复制完成之前，可以更改此卷影副本的内容。 VSS 和应用程序进行更改后，卷影副本由只读的。 此阶段称为自动恢复，它用于撤消文件系统或应用程序的任何事务在卷影副本卷上创建卷影副本之前未完成。  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>提供程序如何创建卷影副本

硬件或软件卷影复制提供程序使用以下方法之一来创建卷影副本：

**完成复制**   此方法将原始卷的完整副本 （称为"完整复制"或"克隆"） 在给定时间的时间。 此副本是只读的。

**写入时复制**   此方法不会复制的原始卷。 相反，它通过复制所有 （已完成的写入 I/O 请求） 所做的更改到卷给定的时间点后的时间来将不同的副本。

**写入时重定向**   此方法不会复制的原始卷，并且它不会进行任何更改的原始卷到给定的时间点后的时间。 相反，它通过将所有更改重都定向到不同的卷将不同的副本。

## <a name="complete-copy"></a>完整副本

完整副本通常来创建"拆分镜像"，如下所示：

1.  原始卷和卷影副本卷是镜像的卷组。  
      
2.  卷影副本卷被分开的原始卷。 这将中断镜像连接。  
      

镜像连接已断开后，原始卷和卷影副本卷是独立的。 原始卷将继续接受所有更改 （写入 I/O 请求） 时的卷影副本卷保持在中断时的原始数据的确切只读副本。

### <a name="copy-on-write-method"></a>写入时复制方法

在写入时复制方法中，对原始卷的更改发生时 （但在写入之前完成的 I/O 请求是），要修改每个块是读取，然后写入到卷的卷影副本存储区域 （也称为其"diff area"）。 卷影副本存储区域可以位于同一个卷或其他卷上。 这样可保留一份原始卷上的数据块之前更改将覆盖它。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>时间</th>
<th>源数据 （状态和数据）</th>
<th>卷影副本 （状态和数据）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>原始数据：1 2 3 4 5</p></td>
<td><p>任何副本:-</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>数据在缓存中已更改：3 到 3"</p></td>
<td><p>卷影副本创建 （仅的差异）：3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>覆盖原始数据：1 2 3’ 4 5</p></td>
<td><p>差异和存储在卷影副本上的索引：3</p></td>
</tr>
</tbody>
</table>

**表 1**   创建卷影副本的写入时复制方法

写入时复制方法是用于创建卷影复制的快速方法，因为它会复制更改的数据。 可以将卷还原到其状态进行任何更改之前对原始卷上已更改的数据与组合中差异区域的复制基块。 如果有很多更改，写入时复制方法可以变得成本高昂。

### <a name="redirect-on-write-method"></a>写入时重定向方法

在写入时重定向方法中，每当原始卷收到更改 （写入 I/O 请求） 时，此更改不适用于的原始卷。 相反，将更改写入到另一个卷的卷影副本存储区域。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>时间</th>
<th>源数据 （状态和数据）</th>
<th>卷影副本 （状态和数据）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>原始数据：1 2 3 4 5</p></td>
<td><p>任何副本:-</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>数据在缓存中已更改：3 到 3"</p></td>
<td><p>卷影副本创建 （仅的差异）：3’</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>原始数据保持不变：1 2 3 4 5</p></td>
<td><p>差异和存储在卷影副本上的索引：3’</p></td>
</tr>
</tbody>
</table>

**表 2**   创建卷影副本的写入重定向方法

写入时复制方法，如写入时重定向方法是用于创建卷影复制的快速方法，因为它将仅将更改复制到数据。 可以与要创建完整的最新数据的副本的原始卷上未更改的数据组合差异区域中的复制基块。 如果有多个读取的 I/O 请求，写入时重定向方法可以变得成本高昂。

## <a name="shadow-copy-providers"></a>卷影复制提供程序

有两种类型的卷影复制提供程序： 基于硬件的提供程序和基于软件的提供程序。 此外，还有系统提供程序，它是内置于 Windows 操作系统中的软件提供程序。

### <a name="hardware-based-providers"></a>基于硬件的提供程序

基于硬件的卷影复制提供程序作为卷影复制服务和通过使用硬件存储适配器或控制器结合使用的硬件级别之间的接口。 创建和维护卷影副本的工作由存储阵列执行。

硬件提供程序始终采用整个 LUN 的卷影副本，但卷影复制服务仅公开或多个请求的卷的卷影副本。

基于硬件的卷影复制提供程序利用了卷影复制服务定义中的时间点的功能允许数据同步、 管理卷影副本和备份应用程序提供一个通用接口。 但是，卷影复制服务未指定基础机制所依据的基于硬件的提供程序生成和维护卷影副本。

### <a name="software-based-providers"></a>基于软件的提供程序

通常，基于软件的卷影复制提供程序截距和进程读取和写入文件系统和卷管理器软件之间的软件层中的 I/O 请求。

这些提供程序将作为用户模式 DLL 组件和至少一个内核模式设备驱动程序，通常是存储筛选器驱动程序。 与不同的是基于硬件的提供程序，基于软件的提供程序软件级别而不是硬件级别在创建卷影副本。

基于软件的卷影复制提供程序必须通过有权访问可用于重新创建卷影副本创建时间之前的卷的状态数据集维护卷的"时间点"视图。 例如，系统提供程序的写入时复制技术。 但是，卷影复制服务会基于软件的提供程序使用来创建和维护卷影副本哪一种方法没有限制。

软件提供商提供适用于比基于硬件的提供程序，更广泛的存储平台，它应与基本磁盘或逻辑卷同样能正常工作。 （逻辑卷是通过组合两个或多个磁盘上的可用空间创建的卷。）与硬件卷影副本不同软件提供商使用操作系统资源来维护卷影副本。

有关基本磁盘的详细信息，请参阅[基本磁盘和卷是什么？](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) TechNet 上。

### <a name="system-provider"></a>系统提供程序

Windows 操作系统中提供的一个卷影复制提供程序，系统提供程序。 尽管在 Windows 中提供的默认提供程序，则其他供应商是免费提供针对其存储硬件和软件应用程序进行了优化的实现的。

若要维护的卷，卷影副本中包含的"时间点"视图，系统提供程序，请使用写入时复制技术。 副本卷上的卷影副本创建以来已修改的块存储在卷影副本存储区域。

系统提供程序可以公开生产卷，可以写入到和从正常读取。 当需要在卷影副本时，逻辑上生产卷，以便将完整的卷影副本上适用于数据的差异。

对于系统提供程序，卷影副本存储区域必须是 NTFS 卷上。 要进行卷影复制的卷不需要是 NTFS 卷，但至少一个系统上装载的卷必须是 NTFS 卷。

构成了系统提供程序的组件文件是 swprv.dll 和 volsnap.sys。

### <a name="in-box-vss-writers"></a>框 VSS 编写器

Windows 操作系统包括一套 VSS 编写器，负责枚举所需的各种 Windows 功能的数据。

有关这些编写器的详细信息，请参阅以下 Microsoft 网站：

  - [框 VSS 编写器](http://go.microsoft.com/fwlink/?linkid=180895)(http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [适用于 Windows Server 2008 和 Windows Vista SP1 的新内置 VSS 编写器](http://go.microsoft.com/fwlink/?linkid=180896)(http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [适用于 Windows Server 2008 R2 和 Windows 7 的新内置 VSS 编写器](http://go.microsoft.com/fwlink/?linkid=180897)(http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>如何使用卷影副本

除了备份应用程序数据和系统状态信息，卷影副本可以用于各种目的，其中包括：

  - 还原 Lun （LUN 重新同步和 LUN 交换）  
      
  - 还原单个文件 （用于共享文件夹的卷影副本）  
      
  - 通过使用可传送卷影副本的数据挖掘  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>还原 Lun （LUN 重新同步和 LUN 交换）

在 Windows Server 2008 R2 和 Windows 7 中，VSS 请求者可以使用名为 LUN 重新同步 （或"LUN 重新同步"） 的硬件卷影复制提供程序功能。 这是使应用程序管理员可以从卷影副本还原数据，到原始 LUN 或新 LUN 的快速恢复方案。

卷影副本可以是完整的克隆或不同的卷影副本。 在任一情况下，末尾的重新同步操作，目标 LUN 将具有相同的内容作为 LUN 的卷影副本。 在重新同步操作时，该数组执行块级别复制从卷影复制到目标 LUN。


> [!NOTE]
> 卷影副本必须是可传送的硬件卷影副本。 
<br>


大多数数组允许继续重新同步操作开始后不久生产 I/O 操作。 正在重新同步操作时，请求将重定向到 LUN 的卷影副本中读取和写入请求发送到目标 LUN。 这允许恢复极大型数据集并在几秒钟之后恢复正常操作的数组。

LUN 重新同步是不同的 LUN 交换。 LUN 交换是自 Windows Server 2003 SP1 以来已支持的 VSS 快速恢复方案。 在 LUN 交换，卷影副本是导入并随后将转换为读写卷。 转换是不可逆操作，并且卷和基础 LUN 都不能在此之后中控制使用 VSS Api。 以下列表描述了 LUN 的重新同步使用 LUN 交换的进行比较：

  - 在 LUN 重新同步，卷影副本不更改，因此它可以使用几次。 在 LUN 交换，卷影副本可用于一次恢复。 对于最注重安全的管理员来说，这很重要。 使用 LUN 重新同步时，请求者可以重试整个还原操作，如果出现问题第一次。  
      
  - 在 LUN 交换结束时，LUN 的卷影副本用于生产 I/O 请求。 出于此原因，卷影副本 LUN 必须使用相同质量的存储作为原始生产 LUN 来确保性能不会影响恢复操作完成后。 如果改为使用 LUN 重新同步，则硬件提供程序可以维护比生产品质的存储开销较低的存储上的卷影副本。  
      
  - 如果目标不可用，并且需要重新创建的 LUN，LUN 交换可能更经济因为它不需要的目标 LUN。  
      


> [!WARNING]
> 所有列出的操作都是 LUN 级的操作。 如果你尝试使用 LUN 重新同步恢复特定的卷，您明智地将还原正在共享 LUN 的所有其他卷。 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>还原单个文件 （用于共享文件夹的卷影副本）

共享文件夹的卷影副本使用卷影复制服务来共享的网络资源，如文件服务器上提供的位于文件的时间点副本。 使用共享文件夹的卷影副本，用户可以快速恢复已删除或更改存储在网络的文件。 因为用户可以这样做不需要管理员协助，共享文件夹的卷影副本可以提高工作效率并降低管理成本。

共享文件夹卷影副本的详细信息，请参阅[共享文件夹的卷影副本](http://go.microsoft.com/fwlink/?linkid=180898)(http://go.microsoft.com/fwlink/?LinkId=180898) TechNet 上。

### <a name="data-mining-by-using-transportable-shadow-copies"></a>通过使用可传送卷影副本的数据挖掘

使用专为与卷影复制服务一起使用的硬件提供程序，可以创建可导入到相同的子系统 (例如，SAN) 中的服务器上的可传送卷影副本。 可以使用这些卷影副本设定种子生产或测试与数据挖掘的只读数据的安装。

使用卷影复制服务和设计用于卷影复制服务的硬件提供程序使用的存储阵列，就可以在一台服务器上创建源数据卷的卷影副本，然后导入到另一台服务器上的卷影副本 （或回复到同一台服务器）。 在几分钟，而不考虑数据的大小中完成此过程。 通过一系列使用支持可传送卷影副本的卷影复制请求者 （存储管理应用程序） 的步骤完成传输过程。

## <a name="to-transport-a-shadow-copy"></a>若要传输的卷影副本

1.  在服务器上创建源数据的可传送卷影副本。

2.  将卷影副本导入到连接到 SAN 的服务器 （你可以导入到另一台服务器或在同一台服务器）。

3.  数据现可供使用。

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**图 3**   创建卷影副本和两个服务器之间的传输


> [!NOTE]
> 可传送卷影副本在 Windows Server 2003 上创建无法导入到运行 Windows Server 2008 的服务器或 Windows Server 2008 R2 上。 可传送卷影副本创建在 Windows Server 2008 或 Windows Server 2008 R2 上不能导入到运行 Windows Server 2003 的服务器。 但是，可以运行 Windows Server 2008 R2，反过来也一样的服务器上导入 Windows Server 2008 创建的卷影副本。 
<br>


卷影副本是只读的。 如果你想要将卷影副本转换为一个读/写 LUN，可以使用的基于虚拟磁盘服务的存储管理应用程序 （包括一些请求者） 除了卷影复制服务。 使用此应用程序，可以从卷影复制服务管理中删除卷影副本，并将其转换为一个读/写 LUN。

卷影复制服务传输是运行 Windows Server 2003 Enterprise Edition、 Windows Server 2003 Datacenter Edition、 Windows Server 2008 或 Windows Server 2008 R2 的计算机上的高级的解决方案。 它仅适用于存储阵列上没有硬件提供程序。 卷影复制传输可以用于各种目的，包括磁带备份，数据挖掘，和测试。

## <a name="frequently-asked-questions"></a>常见问题

此 FAQ 解答问题有关卷影复制服务 (VSS) 的系统管理员。 有关 VSS 的应用程序编程接口的信息，请参阅[卷影复制服务](http://go.microsoft.com/fwlink/?linkid=180899)(http://go.microsoft.com/fwlink/?LinkId=180899) Windows 开发人员中心库中。

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>何时引入了卷影复制服务？ 哪些 Windows 操作系统版本上是否可用？

Windows XP 中引入了 VSS。 它是可在 Windows XP、 Windows Server 2003、 Windows Vista®、 Windows Server 2008、 Windows 7 和 Windows Server 2008 R2 上。

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>卷影副本和备份之间的区别是什么？

在硬盘驱动器上备份的情况下创建的卷影副本也是备份。 可以将数据复制还原的卷影副本关闭或卷影副本可用于快速恢复方案，例如，LUN 重新同步或 LUN 交换。

是将数据从卷影副本复制到磁带或其他可移动介质，存储在媒体的内容构成了备份。 从其复制数据后，可以删除卷影副本本身。

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>什么是卷影复制服务支持的最大大小卷？

卷影复制服务支持最多 64 TB 的卷的大小。

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>我在 Windows Server 2008 上进行备份。 可以在 Windows Server 2008 R2 上还原它？

这取决于您使用的备份软件。 大多数备份程序支持此方案，为数据而不是系统状态备份。

可以在另使用这些版本的 Windows 中的任何一个创建的卷影副本。

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>我在 Windows Server 2003 上进行备份。 可以在 Windows Server 2008 上还原它？

这取决于您使用的备份软件。 如果在 Windows Server 2003 上创建卷影副本，则无法在 Windows Server 2008 上使用它。 此外，如果在 Windows Server 2008 上创建卷影副本，则无法还原它在 Windows Server 2003 上。

### <a name="how-can-i-disable-vss"></a>如何禁用 VSS？

就可以使用 Microsoft 管理控制台禁用卷影复制服务。 但是，您不应这样做。 产生负面禁用 VSS 会影响任何依赖于它，如系统还原和 Windows Server Backup 的软件使用。

有关详细信息，请参阅以下 Microsoft TechNet 网站：

  - [系统还原](http://go.microsoft.com/fwlink/?linkid=157113)(http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>可以从卷影副本以节省空间排除文件？

VSS 旨在创建整个卷的卷影副本。 临时文件，如分页文件，将自动省略从卷影副本，以节省空间。

若要从卷影副本中排除特定文件，请使用以下注册表项：**FilesNotToSnapshot**。


> [!NOTE]
> <STRONG>FilesNotToSnapshot</STRONG>旨在只能由应用程序使用的注册表项。 尝试使用它的用户将会遇到限制如下所示：
<br>
<UL>
<LI>从 Windows Server 使用以前的版本功能创建的卷影副本，而不能删除文件。<BR><BR>
<LI>从共享文件夹的卷影副本，而不能删除文件。<BR><BR>
<LI>它可以从已通过使用卷影副本中删除文件[Diskshadow](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/diskshadow)实用工具，但它不能从已通过使用卷影副本中删除文件[Vssadmin](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/vssadmin)实用程序。<BR><BR>
<LI>从最大程度上的卷影副本会删除文件。 这意味着，它们不保证被删除。<BR><BR></LI></UL>


有关详细信息，请参阅[不包括文件从卷影副本](http://go.microsoft.com/fwlink/?linkid=180904)(http://go.microsoft.com/fwlink/?LinkId=180904) MSDN 上。

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>我的非 Microsoft 备份程序失败，出现 VSS 错误。 我可以做什么？

检查创建的备份程序的公司的网站上的产品支持部分。 可能存在的产品更新，可以下载和安装来解决该问题。 如果没有，请与公司的产品支持部门联系。

系统管理员可以使用在以下 Microsoft TechNet 库网站上的 VSS 故障排除信息，以收集有关 VSS 与相关的问题的诊断信息。

有关详细信息，请参阅[卷影复制服务](http://go.microsoft.com/fwlink/?linkid=180905)(http://go.microsoft.com/fwlink/?LinkId=180905) TechNet 上。

### <a name="what-is-the-diff-area"></a>什么是"diff area"？

卷影副本存储区域 （或"diff area"） 是存储系统软件提供商创建的卷影复制的数据的位置。

### <a name="where-is-the-diff-area-located"></a>差异区域位于何处？

差异区域可以位于本地的任何卷。 但是，它必须位于 NTFS 卷具有足够空间来存储它。

### <a name="how-is-the-diff-area-location-determined"></a>差异区域位置确定的方式

按此顺序，以确定差异区域位置计算以下条件：

  - 如果卷上已有现有的卷影副本，则使用该位置。  
      
  - 如果不存在的原始卷和卷影副本卷位置之间的预配置手动关联，则使用该位置。  
      
  - 如果以前的两个条件未提供的位置，卷影复制服务将选择基于可用空间的位置。 如果多个卷是卷影复制，卷影复制服务创建基于可用空间，按降序大小可能快照位置的列表。 提供的位置数等于进行卷影复制的卷数。  
      
  - 如果其中一个可能的位置进行卷影复制的卷，然后创建本地关联。 否则将创建与具有最多可用空间的卷的关联。  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>VSS 可以创建非 NTFS 卷的卷影副本？

是。 但是，可以仅为 NTFS 卷进行持久卷影副本。 此外，至少一个系统上装载的卷必须是 NTFS 卷。

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>我可以一次创建卷影副本的最大号码是什么？

在单个卷影副本集中的卷影复制卷的最大数目为 64。 请注意，这并不相同的卷影副本数。

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>最大数软件卷影副本的由系统提供程序，我可以维护卷创建什么？

最大数量是软件卷影副本的每个卷为 512。 但是，默认情况下您可以仅维护 64 使用的共享文件夹的卷影副本功能的卷影副本。 若要更改共享文件夹的卷影副本功能的限制，请使用以下注册表项：**MaxShadowCopies**。

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>如何才能控制为卷影副本存储空间使用的空间？

类型**vssadmin 重设大小 shadowstorage**命令。

有关详细信息，请参阅[Vssadmin 重设大小 shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) TechNet 上。

### <a name="what-happens-when-i-run-out-of-space"></a>当我运行空间不足时，会发生什么情况？

将删除该卷的卷影副本，并从开始最旧的卷影副本。

## <a name="volume-shadow-copy-service-tools"></a>卷影复制服务工具

Windows 操作系统提供用于处理 VSS 具有以下工具：

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow 是可用于管理硬件和软件的所有快照，你可以在系统上的 VSS 请求者。 DiskShadow 包括如下所示的命令：

  - **列表**:列出了 VSS 编写器、 VSS 提供程序和卷影副本  
      
  - **创建**:创建新的卷影副本  
      
  - **导入**:导入可传送卷影副本  
      
  - **公开**:公开的永久性卷影副本 （为驱动器号，例如）  
      
  - **还原**:将恢复到指定的卷影副本卷恢复  
      

此工具旨在用于 IT 专业人员，但开发人员可能还会发现它很有用测试 VSS 编写器或 VSS 提供程序时。

DiskShadow 是仅在 Windows Server 操作系统上可用。 不是 Windows 客户端操作系统上可用。

### <a name="vssadmin"></a>VssAdmin

VssAdmin 用于创建、 删除和列出有关卷影副本的信息。 此外可以用于重设大小的卷影副本存储区域 ("diff area")。

VssAdmin 包括如下所示的命令：

  - **创建卷影**:创建新的卷影副本  
      
  - **删除阴影**:删除卷影副本  
      
  - **列出提供程序**:列出所有已注册的 VSS 提供程序  
      
  - **list**:列出所有订阅的 VSS 写入程序  
      
  - **重设大小 shadowstorage**:更改卷影副本存储区域的最大大小  
      

VssAdmin 仅用于管理系统软件提供商创建的卷影副本。

VssAdmin 是 Windows 客户端和 Windows Server 操作系统版本上可用。

## <a name="volume-shadow-copy-service-registry-keys"></a>卷影复制服务注册表项

以下注册表项是可与 VSS 一起使用：

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

此密钥用于指定哪些用户有权访问卷影副本。

有关详细信息，在 MSDN 网站上看到以下条目：

  - [编写器的安全注意事项](http://go.microsoft.com/fwlink/?linkid=157739)(http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [请求者的安全注意事项](http://go.microsoft.com/fwlink/?linkid=180908)(http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

此键指定的最大可以存储在计算机的每个卷的客户端访问的卷影副本数。 共享文件夹的卷影副本使用客户端访问的卷影副本。

有关详细信息，在 MSDN 网站上看到以下条目：

**MaxShadowCopies**下[注册表项的备份和还原](http://go.microsoft.com/fwlink/?linkid=180909)(http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

此键指定的最小的初始大小，以 mb 为单位的卷影副本存储区域。

有关详细信息，在 MSDN 网站上看到以下条目：

**MinDiffAreaFileSize**下[注册表项的备份和还原](http://go.microsoft.com/fwlink/?linkid=180910)(http://go.microsoft.com/fwlink/?LinkId=180910)

`##`# 支持的操作系统版本

下表列出了 VSS 功能的最小支持的操作系统版本。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>VSS 功能</th>
<th>最低受支持的客户端</th>
<th>最低受支持的服务器</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>LUN 重新同步</p></td>
<td><p>无受支持的版本</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p><strong>FilesNotToSnapshot</strong>注册表项</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>可传送卷影副本</p></td>
<td><p>无受支持的版本</p></td>
<td><p>Windows Server 2003 SP1</p></td>
</tr>
<tr class="even">
<td><p>硬件卷影副本</p></td>
<td><p>无受支持的版本</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>以前版本的 Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>使用 LUN 交换的快速恢复</p></td>
<td><p>无受支持的版本</p></td>
<td><p>Windows Server 2003 SP1</p></td>
</tr>
<tr class="odd">
<td><p>多个导入的硬件卷影副本</p>
<div class="alert">
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><img src="media/volume-shadow-copy-service/Dd560667.note(WS.10).gif" />注意</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>这是不止一次导入的卷影副本的能力。 只有一个导入操作可以执行一次。
<p></p></td>
</tr>
</tbody>
</table>
<p></p>
</div></td>
<td><p>无受支持的版本</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>共享文件夹的卷影副本</p></td>
<td><p>无受支持的版本</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>自动恢复可传送卷影副本</p></td>
<td><p>无受支持的版本</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>并发备份会话 （最多 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>并发备份的单一还原会话</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 SP2</p></td>
</tr>
<tr class="even">
<td><p>最多 8 个并发备份的还原会话</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>请参阅

[在 Windows 开发人员中心中的卷影复制服务](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)