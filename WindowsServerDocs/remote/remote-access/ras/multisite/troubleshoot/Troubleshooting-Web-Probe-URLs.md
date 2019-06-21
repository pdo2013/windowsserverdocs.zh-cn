---
title: Web 探测 URL 疑难解答
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6dfffd1e-f4f4-43b6-9e3c-49015ce34338
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44bc409af33ce217e21c5d6e252ac4804718d7a8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282466"
---
# <a name="troubleshooting-web-probe-urls"></a>Web 探测 URL 疑难解答

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍与 `Set-DAEntryPointDC` 命令相关的问题疑难解答信息。 要确认你收到的错误是否与设置入口点域控制器有关，请检查 Windows 事件日志的事件 ID 10065。  
  
## <a name="SaveGPOSettings"></a>正在保存服务器 GPO 设置  
**收到错误**。 将远程访问设置保存到 GPO < gpo_ 名称 > 时出错。  
  
若要解决此错误，请参阅正在保存服务器 GPO 设置。  
  
## <a name="remote-access-is-not-configured"></a>未配置远程访问  
**收到错误**。 < 服务器名称 > 上未配置远程访问。 请指定属于多站点部署的服务器的名称。  
  
或  
  
服务器 < 服务器名称 > 上未配置远程访问。 请指定启用了 DirectAccess 的计算机。  
  
**原因**  
  
参数 *ComputerName* 指定的计算机未配置远程访问。  
  
只有在属于配置的多站点部署的服务器上才能使用 `Set-DaEntryPointDC` cmdlet。  
  
**解决方案**  
  
运行该命令并确保为 *ComputerName* 参数指定已经配置为属于多站点部署的一部分的服务器的名称。  
  
## <a name="multisite-is-not-enabled"></a>未启用多站点  
**收到错误**。 必须启用多站点部署之前执行此操作。 请使用 `Enable-DAMultiSite` cmdlet 完成此操作。  
  
**原因**  
  
参数 *ComputerName* 指定的服务器未启用多站点。  
  
只有在属于配置的多站点部署的服务器上才能使用 `Set-DaEntryPointDC` cmdlet。  
  
**解决方案**  
  
运行该命令并确保为 *ComputerName* 参数指定已经配置为属于多站点部署的一部分的服务器的名称。  
  
## <a name="entry-point-and-domain-controller-not-provided-in-cmdlet"></a>cmdlet 中未提供入口点和域控制器  
`Set-DaEntryPointDC` cmdlet 允许你更改与不同入口点相关的域控制器，例如，在特定域控制器不再可用的情况下更改。 你可以更新某个特定入口点，使其使用不同域控制器，也可以更新使用特定域控制器的所有入口点，让它们使用新的域控制器。 在第一种情况下，你应使用 *EntryPointName* 参数来指定要更新的入口点。 在第二种情况下，你应使用 *ExistingDC* 参数来指定要替换的域控制器。 你只能指定其中一个参数。  
  
**收到错误**。 不指定任何所需的参数。 请提供入口点或现有域控制器的名称。  
  
或  
  
Cmdlet `Set-DaEntryPointDC` 缺少所需的全部参数。  
  
**原因**  
  
*EntryPointName*或*ExistingDC*未指定参数，或指定了这两个参数，为`Set-DaEntryPointDC`cmdlet。  
  
**解决方案**  
  
运行该命令并确保指定 *EntryPointName* 参数或 *ExistingDC* 参数。  
  
## <a name="could-not-locate-domain-controller"></a>找不到域控制器  
**收到错误**。 无法自动找到新的域控制器。 请稍候重试或验证域控制器设置。  
  
**原因**  
  
无法通过 RPC 访问 *ComputerName* 参数指定的计算机或域不包含任何可用的可写域控制器。  
  
**解决方案**  
  
确保远程计算机可以通过 RPC 访问且域内有可写域控制器。 如果域内有可写域控制器，你还可以使用 *NewDC* 参数指定它的显示名称。  
  
## <a name="could-not-connect-to-domain-controller"></a>无法连接域控制器  
  
-   **问题 1**  
  
    **收到错误**。 无法访问域控制器 < 域 _ 控制器 >。 请检查网络连接和服务器的可用性。  
  
    **原因**  
  
    无法访问域控制器。 只有当管理员在 *NewDC* 或 *ExistingDC* 参数中指定域控制器时才会出现此问题。  
  
    **解决方案**  
  
    确保域控制器的名称拼写正确。 如果你采用短名称指定了名称，请使用 FQDN 并重试一次。  
  
-   **问题 2**  
  
    **收到错误**。 无法联系域控制器 < 域 _ 控制器 >。  
  
    **原因**  
  
    可能是 *NewDC* 参数中指定的域控制器出现网络问题，也可能是配置中的任何其他现有域控制器无法访问。  
  
    **解决方案**  
  
    确保域控制器的名称拼写正确，确保其存在、是可写的并且正在运行，确保域控制器和域之间是信任关系。  
  
-   **问题 3**  
  
    **收到错误**。 有关 %2 2!s ！ 不能访问域控制器 < 域 _ 控制器 >。  
  
    **原因**  
  
    要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 时管理的入口点的服务器 GPO 的域控制器不可用，无法读取或修改远程访问配置设置。  
  
    **解决方案**  
  
    请按照"若要更改管理服务器 Gpo 的域控制器"中所述的过程[2.4。配置 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。  
  
-   **问题 4**  
  
    **收到错误**。 无法访问在 < 域名 > 中的主域控制器。  
  
    **原因**  
  
    要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 客户端 GPO 由主域控制器进行管理。 如果主域控制器不可用，则无法读取或修改远程访问配置设置。  
  
    **解决方案**  
  
    请按照"若要将 PDC 仿真器角色传输"中所述的过程[2.4。配置 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。  
  
## <a name="read-only-domain-controller"></a>只读域控制器  
**收到错误**。 域控制器 < 域 _ 控制器 > 是只读的。 请指定一个非只读域控制器。  
  
**原因**  
  
参数 *NewDC* 指定的域控制器是只读的。  
  
**解决方案**  
  
使用 `Set-DAEntryPointDC` 时，*NewDC* 参数用于更新与特定入口点相关的域控制器，或更新与域控制器相关的所有入口点。 因此，新的域控制器必须是可写的。 在 *NewDC* 参数中指定可写域控制器，然后重试一次。  
  
## <a name="cannot-retrieve-gpo"></a>无法检索 GPO  
  
-   **问题 1**  
  
    **收到错误**。 无法从域控制器 < 替代 _ 域 _ > 检索域控制器 < previous_domain_controller > 上的 GPO < gpo_ 名称 >，因为它们不是同一个域中。  
  
    **原因**  
  
    远程访问服务器和域控制器不在同一个域中；因此无法检索到 GPO。  
  
    **解决方案**  
  
    如果你尝试更新了指定的入口点，应确保新的域控制器与入口点服务器位于同一个域中。 如果你尝试更新了指定的域控制器，应确保新的域控制器与要替换的域控制器位于同一个域中。  
  
-   **问题 2**  
  
    **收到错误**。 无法从域控制器 < 替代 _ 域 _ > 检索域控制器 < previous_domain_controller > 上的 GPO < gpo_ 名称 >。 请在域复制完成后再重试一次。  
  
    **原因**  
  
    在尝试更新入口点域控制器时，cmdlet 试图从新的域控制器读取服务器 GPO；但由于尚未完成复制，因此无法在新的域控制器上找到 GPO。  
  
    **解决方案**  
  
    新的域控制器上不存在服务器 GPO。 确保 GPO 已成功复制到新的域控制器，然后重试一次。  
  
-   **问题 3**  
  
    **收到错误**。 你没有访问 GPO < gpo_ 名称 > 的权限。  
  
    **原因**  
  
    在尝试更新入口点域控制器时，cmdlet 试图从新的域控制器读取服务器 GPO；但由于你没有正确权限，因此无法在新的域控制器上读取 GPO。  
  
    **解决方案**  
  
    GPO 存在于域控制器上，但无法读取。 确保你拥有所需权限，然后重试一次。  
  
## <a name="entry-point-not-part-of-multisite-deployment"></a>入口点不是多站点部署的一部分  
**收到错误**。 入口点 < entry_point_name > 不是多站点部署的一部分。 请指定其他值。  
  
**原因**  
  
未找到你指定的入口点名称。  
  
**解决方案**  
  
确保入口点名称拼写正确且 GPO 已被复制到所需域控制器中，然后重试一次。 要查看各个入口点分配的域控制器，请使用 `Get-DAEntryPointDC`。  
  
## <a name="remote-access-server-settings"></a>远程访问服务器设置  
  
-   **问题 1**  
  
    **收到错误**。 无法访问入口点 < entry_point_name > 服务器 < 服务器名称 >。  
  
    **原因**  
  
    在尝试更新入口点域控制器时，cmdlet 试图从所有相关远程访问服务器读写入口点域控制器。 cmdlet 无法从一个或多个远程访问服务器读取数据。  
  
    **解决方案**  
  
    确保所有相关远程访问服务器正在运行并且你拥有对所有这些服务器的本地管理员权限，然后重试一次。  
  
-   **问题 2**  
  
    **收到错误**。 设置不能保存到注册表上服务器 < 服务器名称 > 中入口点 < entry_point_name >。  
  
    **原因**  
  
    在尝试更新入口点域控制器时，cmdlet 试图从所有相关远程访问服务器读写入口点域控制器。 cmdlet 无法将数据写入一个或多个远程访问服务器。  
  
    **解决方案**  
  
    确保所有相关远程访问服务器正在运行并且你拥有对所有这些服务器的本地管理员权限，然后重试一次。  
  
-   **问题 3**  
  
    **收到错误**。 不能在 < 服务器名称 > 上应用 GPO 更新。 进行下次策略刷新后改动才能生效。  
  
    **原因**  
  
    使用 cmdlet `Set-DAEntryPointDC` 时，指定的 *ComputerName* 参数是某个入口点中的远程访问服务器，但该入口点不是最后添加到多站点部署的入口点。  
  
    **解决方案**  
  
    使用远程访问管理控制台的 **“仪表板”** 中的 **“配置状态”** ，可以查看所更新的任何服务器。 这不会引发任何功能性问题；然而，你可以在任何未更新的服务器上运行 `gpupdate /force`，以立即更新配置状态。  
  
## <a name="problem-resolving-fqdn"></a>解析 FQDN 时出现问题  
**收到错误**。 无法访问入口点 < entry_point_name > 服务器 < 服务器名称 >。  
  
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
  


