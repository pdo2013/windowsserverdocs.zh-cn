---
title: 设置入口点域控制器疑难解答
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b12dd0e8-1d80-4d4b-bb45-586f19d17ef0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 10a0f7952fc27d0185d4383da21f0614885ddac3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367047"
---
# <a name="troubleshooting-setting-the-entry-point-domain-controller"></a>设置入口点域控制器疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍与 `Set-DAEntryPointDC` 命令相关的问题疑难解答信息。 要确认你收到的错误是否与设置入口点域控制器有关，请检查 Windows 事件日志的事件 ID 10065。  
  
## <a name="SaveGPOSettings"></a>正在保存服务器 GPO 设置  
**接收到错误**。 将远程访问设置保存到 GPO < GPO_name > 时出错。  
  
若要解决此错误，请参阅保存服务器 GPO 设置。  
  
## <a name="remote-access-is-not-configured"></a>未配置远程访问  
**接收到错误**。 < Server_name > 上未配置远程访问。 请指定属于多站点部署的服务器的名称。  
  
或  
  
服务器 < server_name > 上未配置远程访问。 请指定启用了 DirectAccess 的计算机。  
  
**原因**  
  
参数 *ComputerName* 指定的计算机未配置远程访问。  
  
只有在属于配置的多站点部署的服务器上才能使用 `Set-DaEntryPointDC` cmdlet。  
  
**解决方案**  
  
运行该命令并确保为 *ComputerName* 参数指定已经配置为属于多站点部署的一部分的服务器的名称。  
  
## <a name="multisite-is-not-enabled"></a>未启用多站点  
**接收到错误**。 必须先启用多站点部署，然后才能执行此操作。 请使用 `Enable-DAMultiSite` cmdlet 完成此操作。  
  
**原因**  
  
参数 *ComputerName* 指定的服务器未启用多站点。  
  
只有在属于配置的多站点部署的服务器上才能使用 `Set-DaEntryPointDC` cmdlet。  
  
**解决方案**  
  
运行该命令并确保为 *ComputerName* 参数指定已经配置为属于多站点部署的一部分的服务器的名称。  
  
## <a name="entry-point-and-domain-controller-not-provided-in-cmdlet"></a>cmdlet 中未提供入口点和域控制器  
`Set-DaEntryPointDC` cmdlet 允许你更改与不同入口点相关的域控制器，例如，在特定域控制器不再可用的情况下更改。 你可以更新某个特定入口点，使其使用不同域控制器，也可以更新使用特定域控制器的所有入口点，让它们使用新的域控制器。 在第一种情况下，你应使用 *EntryPointName* 参数来指定要更新的入口点。 在第二种情况下，你应使用 *ExistingDC* 参数来指定要替换的域控制器。 你只能指定其中一个参数。  
  
**接收到错误**。 未指定所需的参数。 请提供入口点或现有域控制器的名称。  
  
或  
  
Cmdlet `Set-DaEntryPointDC` 缺少所需的全部参数。  
  
**原因**  
  
没有为 @no__t cmdlet 指定*EntryPointName*或*ExistingDC*参数，或同时指定了这两个参数。  
  
**解决方案**  
  
运行该命令并确保指定 *EntryPointName* 参数或 *ExistingDC* 参数。  
  
## <a name="could-not-locate-domain-controller"></a>找不到域控制器  
**接收到错误**。 无法自动定位新的域控制器。 请稍候重试或验证域控制器设置。  
  
**原因**  
  
无法通过 RPC 访问 *ComputerName* 参数指定的计算机或域不包含任何可用的可写域控制器。  
  
**解决方案**  
  
确保远程计算机可以通过 RPC 访问且域内有可写域控制器。 如果域内有可写域控制器，你还可以使用 *NewDC* 参数指定它的显示名称。  
  
## <a name="could-not-connect-to-domain-controller"></a>无法连接域控制器  
  
-   **问题1**  
  
    **接收到错误**。 无法访问域控制器 < domain_controller >。 请检查网络连接和服务器的可用性。  
  
    **原因**  
  
    无法访问域控制器。 只有当管理员在 *NewDC* 或 *ExistingDC* 参数中指定域控制器时才会出现此问题。  
  
    **解决方案**  
  
    确保域控制器的名称拼写正确。 如果你采用短名称指定了名称，请使用 FQDN 并重试一次。  
  
-   **问题2**  
  
    **接收到错误**。 无法联系域控制器 < domain_controller >。  
  
    **原因**  
  
    可能是 *NewDC* 参数中指定的域控制器出现网络问题，也可能是配置中的任何其他现有域控制器无法访问。  
  
    **解决方案**  
  
    确保域控制器的名称拼写正确，确保其存在、是可写的并且正在运行，确保域控制器和域之间是信任关系。  
  
-   **问题3**  
  
    **接收到错误**。 对于% 2！ s！，无法访问域控制器 < domain_controller >。  
  
    **原因**  
  
    要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 当管理入口点服务器 GPO 的域控制器不可用时，无法读取或修改远程访问配置设置。  
  
    **解决方案**  
  
    按照 [2.4 中所述的过程 "更改管理服务器 Gpo 的域控制器"。配置 Gpo @ no__t。  
  
-   **问题4**  
  
    **接收到错误**。 无法访问域 < domain_name > 中的主域控制器。  
  
    **原因**  
  
    要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 客户端 GPO 由主域控制器进行管理。 如果主域控制器不可用，则无法读取或修改远程访问配置设置。  
  
    **解决方案**  
  
    按照 [2.4 中所述的 "传输 PDC 仿真器角色" 过程进行操作。配置 Gpo @ no__t。  
  
## <a name="read-only-domain-controller"></a>只读域控制器  
**接收到错误**。 < Domain_controller > 的域控制器是只读的。 请指定一个非只读域控制器。  
  
**原因**  
  
参数 *NewDC* 指定的域控制器是只读的。  
  
**解决方案**  
  
使用 `Set-DAEntryPointDC` 时，*NewDC* 参数用于更新与特定入口点相关的域控制器，或更新与域控制器相关的所有入口点。 因此，新的域控制器必须是可写的。 在 *NewDC* 参数中指定可写域控制器，然后重试一次。  
  
## <a name="cannot-retrieve-gpo"></a>无法检索 GPO  
  
-   **问题1**  
  
    **接收到错误**。 无法从域控制器 < replacement_domain_controller > 检索域 > < 控制器上的 GPO < GPO_name >，因为它们不在同一域中。  
  
    **原因**  
  
    远程访问服务器和域控制器不在同一个域中；因此无法检索到 GPO。  
  
    **解决方案**  
  
    如果你尝试更新了指定的入口点，应确保新的域控制器与入口点服务器位于同一个域中。 如果你尝试更新了指定的域控制器，应确保新的域控制器与要替换的域控制器位于同一个域中。  
  
-   **问题2**  
  
    **接收到错误**。 无法从域控制器 < replacement_domain_controller > 检索域控制器 < previous_domain_controller > 上的 GPO < GPO_name >。 请在域复制完成后再重试一次。  
  
    **原因**  
  
    在尝试更新入口点域控制器时，cmdlet 试图从新的域控制器读取服务器 GPO；但由于尚未完成复制，因此无法在新的域控制器上找到 GPO。  
  
    **解决方案**  
  
    新的域控制器上不存在服务器 GPO。 确保 GPO 已成功复制到新的域控制器，然后重试一次。  
  
-   **问题3**  
  
    **接收到错误**。 你没有访问 GPO < GPO_name > 的权限。  
  
    **原因**  
  
    在尝试更新入口点域控制器时，cmdlet 试图从新的域控制器读取服务器 GPO；但由于你没有正确权限，因此无法在新的域控制器上读取 GPO。  
  
    **解决方案**  
  
    GPO 存在于域控制器上，但无法读取。 确保你拥有所需权限，然后重试一次。  
  
## <a name="entry-point-not-part-of-multisite-deployment"></a>入口点不是多站点部署的一部分  
**接收到错误**。 Entry_point_name > < 入口点不是多站点部署的一部分。 请指定其他值。  
  
**原因**  
  
未找到你指定的入口点名称。  
  
**解决方案**  
  
确保入口点名称拼写正确且 GPO 已被复制到所需域控制器中，然后重试一次。 要查看各个入口点分配的域控制器，请使用 `Get-DAEntryPointDC`。  
  
## <a name="remote-access-server-settings"></a>远程访问服务器设置  
  
-   **问题1**  
  
    **接收到错误**。 服务器 < server_name > 入口点 < entry_point_name > 无法访问。  
  
    **原因**  
  
    在尝试更新入口点域控制器时，cmdlet 试图从所有相关远程访问服务器读写入口点域控制器。 cmdlet 无法从一个或多个远程访问服务器读取数据。  
  
    **解决方案**  
  
    确保所有相关远程访问服务器正在运行并且你拥有对所有这些服务器的本地管理员权限，然后重试一次。  
  
-   **问题2**  
  
    **接收到错误**。 在入口点 < entry_point_name > 中，无法将设置保存到服务器 < server_name > 上的注册表中。  
  
    **原因**  
  
    在尝试更新入口点域控制器时，cmdlet 试图从所有相关远程访问服务器读写入口点域控制器。 cmdlet 无法将数据写入一个或多个远程访问服务器。  
  
    **解决方案**  
  
    确保所有相关远程访问服务器正在运行并且你拥有对所有这些服务器的本地管理员权限，然后重试一次。  
  
-   **问题3**  
  
    **接收到错误**。 无法在 < server_name > 上应用 GPO 更新。 进行下次策略刷新后改动才能生效。  
  
    **原因**  
  
    使用 cmdlet `Set-DAEntryPointDC` 时，指定的 *ComputerName* 参数是某个入口点中的远程访问服务器，但该入口点不是最后添加到多站点部署的入口点。  
  
    **解决方案**  
  
    使用远程访问管理控制台的 **“仪表板”** 中的 **“配置状态”** ，可以查看所更新的任何服务器。 这不会引发任何功能性问题；然而，你可以在任何未更新的服务器上运行 `gpupdate /force`，以立即更新配置状态。  
  
## <a name="problem-resolving-fqdn"></a>解析 FQDN 时出现问题  
**接收到错误**。 服务器 < server_name > 入口点 < entry_point_name > 无法访问。  
  
**原因**  
  
当获取 DirectAccess 服务器列表以进行修改时，cmdlet 无法从其计算机 SID 解析某台服务器的完全限定的域名 (FQDN)。  
  
**解决方案**  
  
错误消息中指定的入口点与域控制器相关联。 确保该入口点的域控制器可用。 如果指定 SID 所属的计算机已从域中移除，请忽略此消息，然后从多站点部署中移除该服务器。  
  
## <a name="no-entry-points-to-update"></a>没有要更新的入口点  
**收到警告**。 未修改域控制器设置。 如果你认为需要修改，请确保 cmdlet 参数配置正确，且 GPO 已被复制到所需的域控制器中。  
  
**原因**  
  
当调用 `Set-DaEntryPointDC` cmdlet 的 *ExistingDC* 参数时，DirectAccess 会检查所有入口点并更新与特定域控制器相关的入口点。 然而，没有入口点使用该指定的 *ExistingDC*。  
  
**解决方案**  
  
要查看入口点及其关联域控制器的列表，请使用 `Get-DAEntryPointDC` cmdlet。 如果应当作出修改，请确保 cmdlet 参数拼写正确且 GPO 已被复制到所需的域控制器中，然后重试一次。  
  


