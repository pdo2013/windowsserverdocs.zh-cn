---
ms.assetid: acc0803b-fa05-4fc3-b94d-2916abf4fdbd
title: "了解重复数据删除"
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: cb17329fb0556a25bc49c2fdb6b16f878aa34194
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="understanding-data-deduplication"></a>了解重复数据删除

> 适用于：Windows Server（半年频道）、Windows Server 2016

本文档介绍[重复数据删除](overview.md)的工作原理。

## <a name="how-does-dedup-work"></a>重复数据删除是如何工作的？

WindowsServer 中的重复数据删除使用以下两个原则创建：

1. **优化不应妨碍到磁盘的写入**  
    重复数据删除通过使用后处理模型来优化数据。 所有数据都在未优化的情况下写入磁盘，然后再通过重复数据删除进行优化。

2. **优化不应更改访问语义**  
    访问优化卷上的数据的用户和应用程序完全不知道他们所访问的文件已删除了重复。

为卷启用重复数据删除后，会在后台运行重复数据删除，完成以下操作：

- 确定该卷上各文件间的重复模式。
- 使用指向该区块唯一备份的特殊指针（称为[重新分析点](#dedup-term-reparse-point)），无缝移动这些分区或区块。

此操作将执行以下四步：

1. 扫描文件系统中的文件是否符合优化策略。  
![扫描文件系统](media/understanding-dedup-how-dedup-works-1.gif)  
2. 将文件分为可变大小的区块。  
![将文件分为区块](media/understanding-dedup-how-dedup-works-2.gif)
3. 标识唯一区块。  
![标识唯一区块](media/understanding-dedup-how-dedup-works-3.gif)
4. 将区块置于区块存储中并进行压缩（可选）。  
![移动到区块存储](media/understanding-dedup-how-dedup-works-4.gif)
5. 将当前优化文件的原始文件流替换为区块存储的重新分析点。  
![将文件流替换为重新分析点](media/understanding-dedup-how-dedup-works-5.gif)

读取优化文件时，文件系统会将具有重新分析点的文件发送到重复数据删除文件系统筛选器 (Dedup.sys)。 筛选器会将读取操作重定向到相应的区块，这些区块在区块存储中构成该文件的流。 对已删除重复的文件范围进行的修改在未优化的情况下写入磁盘，并在下次运行[优化作业](understand.md#job-info)时优化。

## <a id="usage-type"></a>使用类型
以下使用类型为常见工作负荷提供合理的重复数据删除配置：  

| 使用类型 | 理想的工作负荷 | 区别 |
|------------|-----------------|------------------|
| <a id="usage-type-default"></a>默认值 | 常规用途文件服务器：<ul><li>团队共享</li><li>工作文件夹</li><li>文件夹重定向</li><li>软件开发共享</li></ul> | <ul><li>后台优化</li><li>默认优化策略：<ul><li>最短文件保留时间 = 3 天</li><li>优化使用中的文件 = 否</li><li>优化部分文件 = 否</li></ul></li></ul> |
| <a id="usage-type-hyperv"></a>Hyper-V | 虚拟桌面基础结构 (VDI) 服务器 | <ul><li>后台优化</li><li>默认优化策略：<ul><li>最短文件保留时间 = 3 天</li><li>优化使用中的文件 = 是</li><li>优化部分文件 = 是</li></ul></li><li>Hyper-V 互操作的“后台”调整</li></ul> |
| <a id="usage-type-backup"></a>备份 | 虚拟化备份应用程序，如 [Microsoft Data Protection Manager (DPM)](https://technet.microsoft.com/library/hh758173.aspx) | <ul><li>优先级优化</li><li>默认优化策略：<ul><li>最短文件保留时间 = 0 天</li><li>优化使用中的文件 = 是</li><li>优化部分文件 = 否</li></ul></li><li>使用 DPM/DPM 式解决方案的互操作的“后台”调整</li></ul> |

## <a id="job-info"></a>Jobs
重复数据删除使用后处理策略优化和维护卷的空间效率。

| 作业名称 | 作业描述 | 默认计划 |
|----------|------------------|------------------|
| <a id="job-info-optimization"></a>优化 | **优化**作业通过根据卷策略设置对卷上的数据进行分块、（可选）压缩这些区块以及在区块存储中唯一地存储区块来进行重复数据删除。 [重复数据删除是如何工作的？](understand.md#how-does-dedup-work)中详细介绍了重复数据删除使用的优化过程。 | 每小时一次 |
| <a id="job-info-gc"></a>垃圾回收 | **垃圾回收**作业通过删除不必要的区块（最近修改或删除的文件不再引用这些区块）来回收磁盘空间。 | 每周六凌晨 2:35 |
| <a id="job-info-scrubbing"></a>完整性清理 | **完整性清理**作业确定因磁盘故障或扇区损坏造成的区块存储损坏。 如果可能，重复数据删除可自动使用卷功能（如存储空间卷上的镜像或奇偶校验）来重新构造已损坏的数据。 此外，当区块在一个区域（称为热点）的引用次数超过 100 次时，则表示是常用区块，重复数据删除会保留它们的副本。 | 每周六凌晨 3:35 |
| <a id="job-info-unoptimization"></a>取消优化 | **取消优化**作业是一项特殊作业，它只能手动运行，会撤消重复数据删除完成的优化并为该卷禁用重复数据删除。 | [仅按需](run.md#disabling-dedup) |

## <a id="dedup-term"></a>重复数据删除术语
| 术语 | 定义 |
|------|------------|
| <a id="dedup-term-chunk"></a>区块 | 区块是重复数据删除区块算法（可能在其他类似文件中出现）所选择的文件的一部分。 |
| <a id="dedup-term-chunk-store"></a>区块存储 | 区块存储是重复数据删除用于以唯一方式存储区块的系统卷信息文件夹中所组织的一系列容器文件。 |
| <a id="dedup-term-dedup"></a>Dedup | 重复数据删除的缩写，经常在 PowerShell、WindowsServer API 和组件以及 WindowsServer 社区中使用。 |
| <a id="dedup-term-file-metadata"></a>文件元数据 | 每个文件均包含元数据，元数据描述与文件的主要内容不相关的有趣属性。 例如，创建日期、最后阅读日期、作者等。 |
| <a id="dedup-term-file-stream"></a>文件流 | 文件流是文件的主要内容。 这是重复数据删除优化的文件的一部分。 |
| <a id="dedup-term-file-system"></a>文件系统 | 文件系统是软件和磁盘上的数据结构，它允许操作系统在存储媒体上存储文件。 在 NTFS 格式卷上支持重复数据删除。 |
| <a id="dedup-term-file-system-filter"></a>文件系统筛选器 | 文件系统筛选器是修改文件系统默认行为的插件。 为了保留访问语义，重复数据删除使用文件系统筛选器 (Dedup.sys) 将对优化内容的读取完全透明地重定向到发出读取请求的用户/应用程序。 |
| <a id="dedup-term-optimization"></a>优化 | 如果某个文件已经区块化，且其唯一区块已存储在区块存储中，则该文件被视为已通过重复数据删除优化（或已删除了重复）。 |
| <a id="dedup-term-in-policy"></a>优化策略 | 优化策略指定哪些文件应考虑进行重复数据删除。 例如，如果文件是全新的、处于打开状态、处于卷上的某个路径或属于某种文件类型，则可能被视为不符合策略。 |
| <a id="dedup-term-reparse-point"></a>重新分析点 | [重新分析点](https://msdn.microsoft.com/library/windows/desktop/aa365503.aspx)是一种特殊标记，它通知文件系统将 IO 传递给指定的文件系统筛选器。 当某个文件的文件流被优化后，重复数据删除会将此文件流替换为重新分析点，使重复数据删除保留该文件的访问语义。 |
| <a id="dedup-term-volume"></a>卷 | 卷是逻辑存储驱动器的 Windows 构造，它可以在一个或多个服务器上跨多个物理存储设备。 基于卷在卷上启用删除重复。 |
| <a id="dedup-term-workload"></a>工作负荷 | 工作负荷是在 WindowsServer 上运行的应用程序。 示例工作负荷包括常规用途文件服务器、Hyper-V 和 SQL Server。 |

> [!Warning]  
> 不要试图手动修改区块存储，除非由授权的 Microsoft 技术支持人员指示。 执行此操作可能导致数据损坏或丢失。

## <a name="frequently-asked-questions"></a>常见问题
**重复数据删除与其他优化产品有何区别？**  
重复数据删除与其他常见的存储优化产品之间有几个重要区别：

* *重复数据删除与单实例存储有何区别？*  
    单实例存储 (SIS) 是重复数据删除的前身，在 Windows Storage Server 2008 R2 中首次引入。 单实例存储通过标识完全相同的文件，并将其替换为指向存储在 SIS 公用存储中的单个文件副本的逻辑链接，对卷进行优化。 与单实例存储的区别在于，重复数据删除可从并不相同但共享很多常见模式的文件，以及从文件本身包含很多重复模式的文件节省空间。 单实例存储已在 WindowsServer 2012 R2 中被弃用并在支持重复数据删除的 WindowsServer 2016 中被删除。

* *重复数据删除与 NTFS 压缩有何区别？*  
    NTFS 压缩是可在卷级别启用（可选）的 NTFS 功能。 使用 NTFS 压缩，每个文件都将在写入时通过压缩单独优化。 与 NTFS 压缩的区别在于，重复数据删除可为卷上的所有文件节省空间。 这与 NTFS 压缩相比更具优势，因为文件可能<u>同时</u>具有内部复制（由 NTFS 压缩解决）且与卷上的其他文件具有相似之处（不由 NTFS 压缩解决）。 此外，重复数据删除采用后处理模型，这意味着新文件或修改的文件将在未优化的情况下写入磁盘，并将在稍后由重复数据删除进行优化。

* *重复数据删除与存档文件格式（如 zip、rar、7z、cab 等）有何区别？*  
    存档文件格式（如 zip、rar、7z、cab 等）对一组指定的文件执行压缩。 与重复数据删除相同，会优化文件内的重复模式或文件间的重复模式。 但是，必须选择要包含在存档中的文件。 访问语义也将有所不同。 若要访问存档内的特定文件，必须打开存档、选择特定的文件，并解压缩该文件以供使用。 重复数据删除对用户和管理员是透明运行的，且不需要手动启动。 此外，重复数据删除保留了访问语义，文件在优化后看起来没有任何更改。

**我是否可以为我选择的使用类型更改重复数据删除设置？**  
是的。 尽管重复数据删除为**建议的工作负荷**提供合理的默认值，用户仍可能希望对重复数据删除设置进行调整，充分利用存储。 此外，其他工作负荷将[需要进行一些调整，以确保重复数据删除不会干扰工作负荷](install-enable.md#enable-dedup-sometimes-considerations)。

**我是否可以手动运行重复数据删除作业？**  
可以，[所有重复数据删除作业均可手动运行](run.md#running-dedup-jobs-manually)。 当计划作业由于没有足够的系统资源或由于错误而不能运行时，此操作是可取的。 此外，取消优化作业只能手动运行。

**我是否可以监视重复数据删除作业的历史记录结果？**  
可以，[所有重复数据删除作业都会在 Windows 事件日志中生成条目](run.md#monitoring-dedup)。

**我是否可以在我的系统上更改重复数据删除作业的默认计划？**  
可以，[所有计划都是可配置的](advanced-settings.md#modifying-job-schedules)。 修改默认重复数据删除计划尤为可取，以确保重复数据删除计划作业有时间完成且不会与工作负荷争用资源。
