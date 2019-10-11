---
title: KMS 故障排除指南
description: 提供有关 KMS 服务的信息，并推荐用于排查激活问题的工具和方法
ms.topic: troubleshooting
ms.date: 9/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: fc673d2c3e1404dbd750d4c0ef05ec6db50017aa
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963073"
---
# <a name="guidelines-for-troubleshooting-the-key-management-service-kms"></a>密钥管理服务 (KMS) 故障排除指南

在部署过程中，许多企业客户安装了密钥管理服务 (KMS)，用于在其环境中激活 Windows。 这是一种用于安装 KMS 主机的简单过程，经过此过程后，KMS 客户端可发现主机并尝试自行激活主机。 但如果该过程不起作用，会发生什么情况？ 接下来要如何操作？ 本文介绍完成问题排查所需的资源。 有关事件日志条目和 Slmgr.vbs 脚本的详细信息，请参阅 [Volume Activation Technical Reference](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502529(v=ws.11))（批量激活技术参考）。

## <a name="kms-overview"></a>KMS 概述

首先，我们来快速回顾一下 KMS 激活。 KMS 是一种客户端-服务器模型。 从概念上讲，类似于 DHCP。 KMS 会启用产品激活，而不是向客户端针对其请求发出 IP 地址。 KMS 也是一种续订模型，客户端可尝试定期重新激活。 具有两个角色：KMS 主机和 KMS 客户端。

- KMS 主机在环境中运行激活服务并启用激活。 若要配置 KMS 主机，必须从批量许可服务中心 (VLSC) 安装 KMS 密钥，然后激活服务。
- KMS 客户端是部署于环境中的 Windows 操作系统，必须激活。 KMS 客户端可运行任何使用批量激活的 Windows 版本。 KMS 客户端附带预装的密钥，称为通用批量许可密钥 (GVLK) 或 KMS 客户端安装密钥。 GVLK 的存在使系统成为了 KMS 客户端。 KMS 客户端使用 DNS SRV 记录 (_vlmcs._tcp) 来识别 KMS 主机。 然后，客户端会自动尝试发现此服务并将其用于激活客户端自身。 在 30 天开箱宽限期内，客户端尝试每两小时激活一次。 激活后，KMS 客户端尝试每七天续订一次激活。

从故障排除的角度看，可能需要查看两端（主机和客户端），确定发生的情况。

## <a name="kms-host"></a>KMS 主机

KMS 主机上需要检查两个区域。 首先，检查主机软件许可服务的状态。 其次，检查事件查看器，查看与许可或激活相关的事件。

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs 和软件许可服务

若要查看软件许可服务的详细输出，请打开提升的命令提示符窗口，并在命令提示符处输入 slmgr.vbs /dlv。 以下屏幕截图显示 Microsoft 内其中一台 KMS 主机上此命令的结果。

![KMS 主机 Slmgr 输出](./media/ee939272.kms_slmgr_output(en-us,technet.10).png)

最重要的故障排除字段如下所示。 要查找的内容可能有所不同，具体取决于要解决的问题。

- 版本信息。 slmgr.vbs /dlv 输出的顶部是软件许可服务版本。 这可能有助于确定是否已安装最新版本的服务。 例如，Windows Server 2003 上的 KMS 服务更新支持不同的 KMS 主机密钥。 此数据可用于评估版本是否为最新版本，并支持你尝试安装的 KMS 主机密钥。 有关这些更新的详细信息，请参阅[更新可用于 Windows Vista 和 Windows Server 2008，以扩展对 Windows 7 和 Windows Server 2008 R2 的 KMS 激活支持](https://support.microsoft.com/help/968912/an-update-is-available-for-windows-vista-and-for-windows-server-2008-t)。
- “名称”。 这指示 KMS 主机系统上安装的 Windows 版本。 如果在添加或更改 KMS 主机密钥时遇到问题（例如，验证该操作系统版本是否支持该密钥），此字段对于故障排除很重要。
- “说明”。 此字段中显示已安装的密钥。 使用此字段可验证用于激活服务的密钥，以及该密钥是否为已部署 KMS 客户端的正确密钥。
- 许可证状态。 此字段是 KMS 主机系统的状态。 该值应为“已获许可”。 任何其他值都表示出现问题，你可能必须重新激活主机。
- 当前计数。 显示的计数介于 0 和 50 之间。 计数在操作系统之间是累积的，指示在 30 天内尝试激活的有效系统的数目。  
  
  如果计数为 0，则服务最近已激活，或者任何有效的客户端都未连接到 KMS 主机。  
  
  无论环境中存在多少个有效系统，计数都不会增大到 50 以上。 这是因为计数设置为仅缓存 KMS 客户端返回的最大许可策略的两倍。 现在的最大策略由 Windows 客户端操作系统设置，这需要来自 KMS 主机的 25 或更大计数，才能自行激活。 因此，KMS 主机上的最大计数是 2 x 25 或 50。 请注意，在仅包含 Windows Server KMS 客户端的环境中，KMS 主机上的最大计数为 10。 这是因为 Windows Server 版本的阈值为 5（2 x 5，或 10）。  
  
  一个与计数相关的常见问题是：环境中具有已激活的 KMS 主机和足够的客户端，但计数不会增大到 1 以上。 核心问题在于，部署的客户端映像未正确配置 (sysprep /generalize)，并且系统不具有唯一的客户端计算机 ID (CMID)。 有关详细信息，请参阅 [KMS 客户端](#kms-client)和[在将基于 Windows Vista 或 Windows 7 的新客户端计算机添加到网络时，KMS 当前计数不会增加](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista)。 我们的一名支持上报工程师通过 [KMS Host Client Count not Increasing Due to Duplicate CMID’S](https://blogs.technet.microsoft.com/askcore/2009/10/16/kms-host-client-count-not-increasing-due-to-duplicate-cmids/)（由于CMID 重复导致 KMS 主机客户端计数没有增加）也发布了有关此问题的博客文章。  
  
  计数可能不会增加的另一个原因是，环境中的 KMS 主机过多，而计数在所有这些主机中分布。
- 侦听端口。 与 KMS 的通信使用匿名 RPC。 默认情况下，客户端使用 1688 TCP 端口来连接到 KMS 主机。 请确保此端口在 KMS 客户端与 KMS 主机之间打开。 可更改或配置 KMS 主机上的端口。 在通信过程中，KMS 主机会向 KMS 客户端发送端口标识。 如果更改 KMS 客户端上的端口，则在客户端与主机联系时会覆盖端口标识。

我们经常被问及 slmgr.vbs /dlv 输出的“累计请求数”部分。 通常，此数据对于故障排除没有用。 KMS 主机会持续记录每个尝试激活或重新激活的 KMS 客户端的状态。 失败的请求表示 KMS 主机不支持的 KMS 客户端。 例如，如果 Windows 7 KMS 客户端尝试针对使用 Windows Vista KMS 密钥激活的 KMS 主机进行激活，则激活会失败。 “具有许可证状态的请求”行说明过去和现在所有可能的许可证状态。 从故障排除的角度来看，仅当计数不按预期增加时，此数据才适用。 在这种情况下，应看到失败的请求数增加。 这表明应检查用于激活 KMS 主机系统的产品密钥。 另请注意，仅当重新安装 KMS 主机系统时，累积请求值才会重置。

### <a name="useful-kms-host-events"></a>有用的 KMS 主机事件

#### <a name="event-id-12290"></a>事件 ID 12290

KMS 客户端为激活而联系主机时，KMS 主机会记录事件 ID 12290。 事件 ID 12290 提供大量可用于确定与主机联系的客户端类型和故障发生原因的信息。 事件 ID 12290 项的以下段来自 KMS 主机的密钥管理服务事件日志。  

![KMS 12290 事件](./media/ee939272.kms_12290_event(en-us,technet.10).png)

事件详细信息包括下列信息：

- 激活所需的最小计数。 KMS 客户端报告 KMS 主机的计数必须为 5 才能激活。 这意味着这是 Windows Server 操作系统，但并不指示特定版本。 如果客户端未激活，请确保主机上的该计数足够大。
- 客户端计算机 ID (CMID)。 这是每个系统的唯一值。 如果此值不是唯一的，那是因为没有为分发 (sysprep /generalize) 正确准备映像。 即使环境中具有足够的客户端，此问题也会在 KMS 主机上表现为计数不会增加。 有关详细信息，请参阅[在将基于 Windows Vista 或 Windows 7 的新客户端计算机添加到网络时，KMS 当前计数不会增加](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista)。
- 许可证状态和状态到期时间。 这是客户端的当前许可证状态。 它有助于区分尝试首次激活的客户端与尝试重新激活的客户端。 时间项会说明如果没有任何更改，客户端将保留该状态多长时间。

如果要对客户端进行故障排除，并且在 KMS 主机上找不到相应的事件 ID 12290，则客户端未连接到 KMS 主机。 事件 ID 12290 项可能不存在的原因如下：

- 发生了网络中断。
- 主机未解析或未在 DNS 中注册。
- 防火墙阻止 TCP 1688。
   该端口可能在环境中的多个位置被阻止，包括 KMS 主机系统本身。 KMS 主机默认具有针对 KMS 的防火墙例外，但不会自动启用。 必须打开该例外。
- 事件日志已满。

KMS 客户端记录两个对应事件，即事件 ID 12288 和事件 ID 12289。 有关这些事件的信息，请参阅 [KMS 客户端](#kms-client)部分。

#### <a name="event-id-12293"></a>事件 ID 12293

在 KMS 主机上查找的另一个相关事件是事件 ID 12293。 此事件表示主机未在 DNS 中发布所需的记录。 已知这种情况会导致故障，因此应该在安装主机之后并在部署客户端之前进行验证。 有关 DNS 问题的详细信息，请参阅 [KMS 和 DNS 问题的常见排查过程](common-troubleshooting-procedures-kms-dns.md)。

## <a name="kms-client"></a>KMS 客户端

在客户端上使用相同的工具（Slmgr 和事件查看器）来排查激活问题。

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs 和软件许可服务

若要查看软件许可服务的详细输出，请打开提升的命令提示符窗口，并在命令提示符处输入 slmgr.vbs /dlv。 以下屏幕截图显示 Microsoft 内其中一台 KMS 主机上此命令的结果。

![KMS 客户端 Slmgr 输出](./media/ee939272.kms_client_slmgr_output(en-us,technet.10).png)

以下列表包括最重要的故障排除字段。 要查找的内容可能有所不同，具体取决于要解决的问题。

- “名称”。 此值是 KMS 客户端系统上安装的 Windows 版本。 使用它可验证尝试激活的 Windows 版本是否可使用 KMS。 例如，我们的技术支持遇到了以下情况：客户尝试在不使用批量激活的 Windows 版本（例如 Windows Vista Ultimate）上安装 KMS 客户端安装密钥。
- “说明”。 此值显示已安装的密钥。 VOLUME_KMSCLIENT 指示已安装 KMS 客户端安装密钥（或 GVLK）（批量许可媒体的默认配置），并且此系统自动尝试使用 KMS 主机进行激活。 如果此处显示其他值（如 MAK），则必须重新安装 GVLK，才能将此系统配置为 KMS 客户端。 可以使用 slmgr.vbs /ipk &lt;GVLK&gt;（如 [KMS 客户端安装密钥](kmsclientkeys.md)中所述）或使用批量激活管理工具 (VAMT) 来手动安装密钥。 有关如何获取和使用 VAMT 的信息，请参阅[批量激活管理工具 (VAMT) 技术参考](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-management-tool)。
- 部分产品密钥。 作为“名称”字段，你可以使用此信息来确定此计算机上是否已安装正确的 KMS 客户端安装密钥（换言之，密钥与 KMS 客户端上安装的操作系统是否匹配）。 默认情况下，正确的密钥存在于使用批量许可服务中心 (VLSC) 门户中的媒体生成的系统上。 在某些情况下，客户可以使用多次激活密钥 (MAK) 激活，直到环境中具有足够多的系统来支持 KMS 激活为止。 必须在这些系统上安装 KMS 客户端安装密钥，才能将系统从 MAK 转换为 KMS。 使用 VAMT 安装此密钥，并确保应用正确的密钥。
- 许可证状态。 此值显示 KMS 客户端系统的状态。 对于使用 KMS 激活的系统，此值应为“已获许可”。 任何其他值都可能表示存在问题。 例如，如果 KMS 主机运行正常且 KMS 客户端未激活（例如，保持“宽限期”状态），则表明某些原因可能阻止了客户端到达主机系统，例如防火墙问题、网络中断或类似的原因。
- 客户端计算机 ID (CMID)。 每个 KMS 客户端应具有唯一的 CMID。 如 [KMS 主机](#kms-host)部分所述，一个与计数相关的常见问题是：环境中具有已激活的 KMS 主机和足够的客户端，但计数不会增加到 1 以上。 有关详细信息，请参阅[在将基于 Windows Vista 或 Windows 7 的新客户端计算机添加到网络时，KMS 当前计数不会增加](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista)。
- DNS 中的 KMS 计算机名称。 此值显示客户端成功用于激活的 KMS 主机 FQDN，以及用于通信的 TCP 端口。
- KMS 主机缓存。 最终值显示是否启用缓存。 默认启用缓存。 这意味着，KMS 客户端缓存用于激活的 KMS 主机名称，并在重新激活时直接与此主机通信，而不需要查询 DNS。 如果客户端无法与缓存的 KMS 主机联系，则会查询 DNS 来发现新的 KMS 主机。

### <a name="useful-kms-client-events"></a>有用的 KMS 客户端事件

#### <a name="event-id-12288-and-event-id-12289"></a>事件 ID 12288 和事件 ID 12289

KMS 客户端成功激活或重新激活时，客户端会记录两个事件：事件 ID 12288 和事件 ID 12289。 事件 ID 12288 项的以下段来自 KMS 客户端的密钥管理服务事件日志。

![KMS 客户端事件 ID 12288](./media/ee939272.client_12288(en-us,technet.10).png)

如果只看到事件 ID 12288（而没有对应的事件 ID 12289），这意味着 KMS 客户端无法联系 KMS 主机，KMS 主机无响应，或者客户端未收到响应。 在这种情况下，请验证 KMS 主机是否可发现，以及 KMS 客户端是否可以联系该主机。  

事件 ID 12288 中最相关的信息是“信息”部分中的数据。 例如，此部分显示客户端的当前状态，以及客户端在尝试激活时使用的 FQDN 和 TCP 端口。 可使用 FQDN 来解决 KMS 主机上的计数不会增加的问题。 例如，如果客户端可用的 KMS 主机过多（合法或未授权系统），则该计数可能在所有客户端中分布。

激活失败并不总是意味着客户端有 12288，而没有 12289。 激活或重新激活失败可能也有这两个事件。 在这种情况下，必须检查第二个事件来验证失败的原因。

![KMS 客户端事件 ID 12289](./media/ee939272.client_12289(en-us,technet.10).png)

事件 ID 12289 的信息部分提供以下信息：

- 激活标志。 此值指示激活是成功 (1) 还是失败 (0)。
- KMS 主机上的当前计数。 客户端尝试激活时，此值反映 KMS 主机上的计数值。 如果激活失败，则可能是因为此客户端操作系统的计数不足，或者环境中没有足够的系统来生成计数。

## <a name="what-does-support-ask-for"></a>支持人员需要你提供什么信息？

如果必须致电支持人员才能解决激活问题，支持工程师通常会要求提供以下信息：

- 来自 KMS 主机和 KMS 客户端系统的 Slmgr.vbs /dlv 输出。 无论是使用 wscript，还是使用 cscript 运行命令，都可以使用 Ctrl+C 来复制输出，然后将其粘贴到记事本中，以便将其发送给支持人员。
- 来自 KMS 主机的事件日志（密钥管理服务日志）和来自 KMS 客户端系统的事件日志（应用程序日志）

## <a name="see-also"></a>另请参阅
- [询问核心团队：#激活](https://blogs.technet.microsoft.com/askcore/tag/Activation/)


