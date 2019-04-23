---
title: 适用于 Windows Server 的 Azure 混合权益
description: 使用本地 Windows Server 许可证在 Azure VM 实现节省
ms.prod: windows-server
ms.date: 11/10/2017
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.openlocfilehash: 62821abc6c9eec660fa6af832bb1aba151708021
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884078"
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>适用于 Windows Server 的 Azure 混合权益

>适用于：Windows Server

## <a name="benefit-description-rules-and-use-cases"></a>权益的描述、规则和用例

通过适用于 Windows Server 的 Azure 混合权益，你可以利用包含软件保障的本地 Windows Server 许可证，在 Azure 中的 Windows Server VM 上实现高达 40% 的节约。  凭借此权益，客户只需要为虚拟机的基础结构成本付费，因为软件保障权益涵盖 Windows Server 的许可。  该权益适用于标准版和数据中心版的 Windows Server 2008R2、2012、2012R2 和 2016。  此权益在所有地区和自主云中提供。


![图像 1](media/ahb01.png)

只需拥有有效的软件保障或订阅许可证（例如 Windows Server 许可证中的 EAS、SCE 订阅或 Open Value 订阅）即享有该权益资格。  

每个 Windows Server 双处理器许可证（包含有效的 SA/订阅），以及每组 16 个 Windows Server 核心许可证（包含 SA/订阅），使客户有资格在两个或更少 Azure 基本实例（虚拟机）上分配的多达 16 个虚拟核心上使用基于 Microsoft Azure 的 Windows Server。 其他每组 8 个核心许可证（包含 SA/订阅）使用户有资格在多达 8 个虚拟核心和一个基本实例 (VM) 上使用。

| 包含 SA/订阅的许可证            | 授权的 VM 和核心            | 使用方式                                |
|-----------------------------------------|----------------------------------|-----------------------------------------------------|
| WS Datacenter（16 个核心或双处理器 L）  | 最多两个 VM 和 16 个核心 | 在本地和 Azure 中运行虚拟机  |
| WS Standard（16 个核心或双处理器 L）    | 最多两个 VM 和 16 个核心 | 在本地或 Azure 中运行虚拟机 |

利用 Azure 混合权益的 VM 只能在 SA/订阅期内在 Azure 中运行。 在 SA/订阅即将到期时，客户可以选择续订其 SA/订阅，为该 VM 关闭混合权益功能，或使用混合权益取消预配 VM。 

### <a name="savings-examples"></a>节省示例 

![图像 2](media/ahb02.png)
 
在下面，你可以找到参考表，以帮助你更具体地了解权益规则。 绿色列显示相同类型的 VM 的数量，而蓝色行显示每个 VM 的核心密度。 黄色单元格显示双处理器许可证（或 16 个核心集）的数量，用户必须部署某一核心密度的一定数量的 VM。 

Windows Server，包含 SA 要求参考表：

![图像 3](media/ahb03.png)
 
通过适用于 Windows Server 的 Azure 混合权益，你还可以灵活地根据自己的需要并结合不同类型的 VM 运行配置。

几个许可位置的示例配置：

![图 4](media/ahb04.png)
![图像 5](media/ahb05.png)

 
如果你希望了解有关适用于 Windows Server 的 Azure 混合权益的详细信息，请转到 Azure 混合权益网站。

## <a name="how-to-maintain-compliance"></a>如何保持合规性

客户如果希望将 Azure 混合权益应用于其 Windows Server VM，则需要在激活此权益前验证合格的许可证数，以及其 SA/订阅的相应覆盖期，并应用上述指南以使用该权益部署正确数量的 VM。 如果你已使用 Azure 混合权益运行 VM，则将需要清点你正在运行的装置数量，并查看你拥有的有效 SA 许可证。  请联系你的 Microsoft 企业协议许可专家，以验证你的 SA 许可位置。
若要查看使用订阅中适用于 Windows Server 的 Azure 混合权益部署的所有虚拟机并计算其数量，你可以执行以下操作之一：

1. 配置 Microsoft Azure 门户，以显示适用于 Windows Server 的 Azure 混合权益的利用情况。将“Azure 混合权益”列添加在 Microsoft Azure 门户虚拟机部分的列表视图中。 

    ![图像 6](media/ahb06.png)

2.  使用 PowerShell 列出适用于 Windows Server 的 Azure 混合权益

    ```
    $vms = Get-AzureRMVM 
    foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
    ```

3.  查看你的 Microsoft Azure 账单，以确定你正运行多少个使用适用于 Windows Server 的 Azure 混合权益的虚拟机。 有关使用该权益的实例数的信息显示在“其他信息”下方：

    ```
    "{"ImageType":"WindowsServerBYOL","ServiceType":"Standard_A1","VMName":"","UsageType":"ComputeHR"}" 
    ```

请注意，账单并不实时计费，即从你使用混合权益激活 VM 起经过几小时的延迟后，费用才在账单上显示。
然后，你可以在下面的**适用于 Windows Server 的 Azure 混合权益 SA 计数工具**中填充结果，以获得所需的 SA 或订阅覆盖的 WS 许可证的数量。

请务必清点你拥有的每个订阅，以生成许可位置的全面视图。

[Azure 混合权益 WS SA 计数工具](http://download.microsoft.com/download/7/1/2/712FEFF0-155C-4ABF-96C0-CE4EC4DB0516/Azure_Hybrid_Benefit_Windows_Server_SA_Count_Tool.xlsx)

如果你执行了上述操作，并确认了你已针对正在运行的 Azure 混合权益实例数获得完全许可，则无需执行任何进一步的操作。 如果你发现你可以使用该权益涵盖增量 VM，则可能需要改为使用该权益而非全部成本运行实例，以进一步优化你的成本。

如果对于已部署的 VM 数，你没有足够多的合格 Windows Server 许可证，则需要通过下列频道之一购买软件保障覆盖的其他 Windows Server 本地许可证，以常规的每小时价格购买 Windows Server VM，或者关闭某些 VM 的混合权益功能。 请注意，你可以 8 个核心的增量购买核心许可证，以有资格获得其他每个 Azure 混合权益 VM。 

Windows Server 软件保障和/或订阅可通过以下 Microsoft 许可频道组合之一进行购买：

| 通道                      | 打开     | OVS      | Select/Select Plus  | MPSA       | EA/EAS   |
|------------------------------|----------|----------|-----------------------|-----------|----------|
| 典型大小（设备数）  | 5-250    | 5-250    | >250                  | >250      | >500     |
| SA/订阅            | 可选 | 已包含 | 可选              | 可选  | 已包含 |

Microsoft 保留随时审核最终客户以验证 Azure 混合权益资格的权利。 

## <a name="deployment-guidance"></a>部署指南 

我们已对拥有合格许可证的所有客户启用预建库图像可用性（与他们购买这些许可证的位置无关），以及使合作伙伴能够代表客户执行部署。 

请在[此处](https://azure.microsoft.com/pricing/hybrid-use-benefit/)查找所有可用部署选项的说明，包括： 
-   重点介绍利用预建库图像的新部署体验的详细视频
-   有关上载自定义构建的 VM 的详细说明 
-   有关使用 Azure Site Recovery 和 PowerShell 迁移现有虚拟机的详细说明。 
