---
title: 移动到 Windows Server 2016 的建议
description: 移动到 Windows Server 2016 的建议。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 10/18/2016
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 379808e861f087bdda800ae6877c73c02d242a7b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391632"
---
# <a name="recommendations-for-moving-to-windows-server-2016"></a>移动到 Windows Server 2016 的建议

>适用于：Windows Server 2016


|如果运行的是：|Windows Server 2012 R2 或 Windows Server 2012|Windows Server 2008 R2 或 Windows Server 2008|  
|-------------------|----------|--------------|--------------|---------------------------------------|  
|**Windows Server 角色基础结构**|根据[特定角色指南](https://technet.microsoft.com/windowsserver/jj554790)选择升级或迁移。|- 若要充分利用 Windows Server 2016 中的新功能，可部署新硬件，或在现有主机上的虚拟机中安装 Windows Server 2016。 某些新功能在运行 Hyper-V 的 Windows Server 2016 物理主机上工作性能最佳。 <br>- 按照[特定角色指南](https://technet.microsoft.com/windowsserver/jj554790)操作。|
|**Microsoft 服务器管理和应用程序工作负载**|- 应用程序升级应包括到 Windows Server 2016 的*迁移*。 请参阅[兼容性列表](Server-Application-Compatibility.md)。 <br>- 仅升级到 Windows Server 2016（即无需升级应用程序）应使用特定于应用程序的指南。|- 若要充分利用 Windows Server 2016 中的新功能，可部署新硬件，或在现有主机上的虚拟机中安装 Windows Server 2016。 某些新功能在运行 Hyper-V 的 Windows Server 2016 物理主机上工作性能最佳。 按照适用的迁移指南操作。 <br>- 或者，仍保留在当前的操作系统上，在 Windows Server 2016 主机上或 Microsoft Azure 上的虚拟机中运行。 通过[软件保障](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx)与 EA 分销商、TAM 或 Microsoft 联系，了解扩展支持选项的信息。|
|**ISV 应用程序工作负载**|- 升级到 Windows Server 2016 应使用特定于应用程序的指南。 <br>- 有关 Windows Server 与非 Microsoft 应用程序兼容性的详细信息，请访问 [Windows Server 徽标认证门户](https://msdn.microsoft.com/enterprisecloudcertified)。|- 若要充分利用 Windows Server 2016 中的新功能，可部署新硬件，或在现有主机上的虚拟机中安装 Windows Server 2016。 某些新功能在运行 Hyper-V 的 Windows Server 2016 物理主机上工作性能最佳。 按照适用的迁移指南操作。 <br>- 或者，仍保留在当前的操作系统上，在 Windows Server 2016 主机上或 Microsoft Azure 上的虚拟机中运行。 通过[软件保障](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx)与 EA 分销商、TAM 或 Microsoft 联系，了解扩展支持选项的信息。|
|**自定义应用程序工作负载**|- 向应用程序开发人员咨询 Windows Server 2016 的兼容性和升级指南。 <br>- 先利用 Microsoft Azure 在 Windows Server 2016 上测试应用程序，再进行切换。 <br>- 在下一节中参阅完整选项。|- 向应用程序开发人员咨询 Windows Server 2016 的兼容性和升级指南。 <br>- 先利用 Microsoft Azure 在 Windows Server 2016 上测试应用程序，再进行切换。 <br>- 若要充分利用 Windows Server 2016 中的新功能，可部署新硬件，或在现有主机上的虚拟机中安装 Windows Server 2016。 某些新功能在运行 Hyper-V 的 Windows Server 2016 物理主机上工作性能最佳。 <br>- 在下一节中参阅完整选项。|

## <a name="complete-options-for-moving-servers-running-custom-or-in-house-applications-on-older-versions-of-windows-server-to-windows-server-2016"></a>将在旧版本 Windows Server 上运行自定义或“内部”应用程序的服务器移动到 Windows Server 2016 的完备选项

与之前相比，有更多选项可以帮助你和客户利用 Windows Server 2016 中的功能，且对当前服务和工作负载影响极小。

- 通过下载评估版的 [Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) 进行本地测试，使用应用程序试用最新的操作系统。 测试完毕且确认质量后，可以使用零售许可证密钥执行简单的许可证转换（需要重启）。

- [Microsoft Azure](https://azure.microsoft.com) 还可在试用的基础上进行测试，确保自定义应用程序可在最新的服务器操作系统上工作。 测试完毕且确认质量后，本地[迁移到最新的 Windows Server 版本](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade#upgrade)。 

- 或者，测试完毕且确认质量后，可选择将 [Microsoft Azure](https://azure.microsoft.com) 用作自定义应用程序或服务的永久存储位置。 这样，在准备好切换到 Azure 中的新服务器前，旧服务器保持可用。

    - 如果已有 Windows Server 软件保障，可使用 [Azure 混合使用权益](https://azure.microsoft.com/pricing/hybrid-use-benefit/)进行部署来节省资金。 

- 在大多数情况下，[Microsoft Azure](https://azure.microsoft.com) 可用于托管现在正运行的较旧版本 Windows Server 上的相同应用程序。 使用 [Azure 市场](https://azure.microsoft.com/marketplace/)映像，将应用程序和工作负荷迁移到含所选操作系统的虚拟机。

    - 如果已有 Windows Server 软件保障，可使用 [Azure 混合使用权益](https://azure.microsoft.com/pricing/hybrid-use-benefit/)进行部署来节省资金。 

- Windows Server [软件保障](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx)计划提供了新版本权限权益。 除一系列其他权益外，具有软件保障的服务器还可在适当时候升级到 Window Server 的最新版本，无需购买新的许可证。 

## <a name="additional-resources"></a>其他资源

- [Windows Server 2016 中已删除或弃用的功能](deprecated-features.md)
- 有关常规服务器升级和迁移选项，请访问 [Windows Server 2016 升级和转换选项](Supported-Upgrade-Paths.md)。
- 有关产品生命周期和支持级别的详细信息，请参阅[支持生命周期策略常见问题](https://support.microsoft.com/help/17140/support-lifecycle-policy-faq)。

