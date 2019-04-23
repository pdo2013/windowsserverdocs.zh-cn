---
title: 在 vSwitch 中接收段合并 (RSC)
description: 接收段合并 (RSC) vSwitch 中是 Windows Server 2019 中的功能和 Windows 10 2018 年 10 月更新可帮助减少主机 CPU 使用率和虚拟工作负载的增加吞吐量，由合并到更少，但更大的多个 TCP 段段。 处理更少、 较大的段 （合并） 是处理比效率更高很多，小的段。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.author: dacuo
author: shortpatti
ms.date: 09/07/2018
ms.openlocfilehash: 667e795e398443cadd4c966cc31e65eeee4962f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827778"
---
# <a name="rsc-in-the-vswitch"></a>RSC vSwitch 中
>适用于：Windows Server 2019

接收段合并 (RSC) vSwitch 中是 Windows Server 2019 中的功能和 Windows 10 2018 年 10 月更新可帮助减少主机 CPU 使用率和虚拟工作负载的增加吞吐量，由合并到更少，但更大的多个 TCP 段段。 处理更少、 较大的段 （合并） 是处理比效率更高很多，小的段。

Windows Server 2012 及更高版本包括仅限硬件的卸载版本的技术也称为接收段合并 （在物理网络适配器中实现）。 RSC 此卸载的版本是在更高版本的 Windows 中仍然可用。 但是，它与虚拟工作负载不兼容，并且一旦将物理网络适配器附加到 vSwitch 已被禁用。 RSC 的仅限硬件的版本的详细信息，请参阅[接收段合并 (RSC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11))。

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>VSwitch 中受益于 RSC 的方案

其数据路径遍历虚拟交换机的工作负荷可以利用此功能。

例如：

-   包括主机虚拟 Nic:

    -   软件定义的网络

    -   Hyper-V 主机

    -   存储空间直通

-   HYPER-V 来宾虚拟 Nic

-   软件定义网络 GRE 网关

-   容器

与此功能不兼容的工作负载包括：

-   软件定义网络 IPSEC 网关

-   启用 SR-IOV 的虚拟 Nic

-   SMB 直通

## <a name="configure-rsc-in-the-vswitch"></a>在 vSwitch 中配置 RSC


默认情况下，在外部 Vswitch，设施，RSC 被启用。

**查看当前设置：**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**启用或禁用 RSC vSwitch 中**


>[!IMPORTANT]
>重要提示：可以启用和禁用与现有连接不会影响动态 RSC vSwitch 中。


**VSwitch 中禁用 RSC**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**重新启用 RSC vSwitch 中**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
有关详细信息，请参阅[Set-vmswitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps)。
