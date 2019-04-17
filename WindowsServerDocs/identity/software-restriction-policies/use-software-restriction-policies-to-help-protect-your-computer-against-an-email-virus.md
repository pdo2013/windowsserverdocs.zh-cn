---
title: "使用限制策略的软件来帮助保护你的计算机免受电子邮件病毒"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 41b4c2399a86ef96d34b62295eda4a1ce9300609
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>使用限制策略的软件来帮助保护你的计算机免受电子邮件病毒

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题提供如何设置控制应用程序策略使用软件限制策略 (SRP) 来帮助保护你的计算机免受与 Windows Server 2008 和 Windows Vista 的电子邮件病毒开头的信息。

## <a name="introduction"></a>简介
软件限制策略 (SRP) 是基于组策略的功能，以标识某个域中，在计算机上运行的软件程序，并控制这些程序能够运行。 你可以使用软件限制策略若要创建受限高度对于你在其中允许仅明确识别运行的应用程序的计算机，配置。 这些组策略与 Microsoft Active Directory 域服务集成，但也可单独的计算机上配置。 有关 SRP 的起始点，请参阅[软件限制策略](software-restriction-policies.md)。

开始与 Windows Server 2008 R2 和 Windows 7、 Windows AppLocker 可用于或配合 SRP 控制策略应用程序的一部分。 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>配置 SRP 来帮助防御来自电子邮件病毒的攻击

1.  查看软件限制的策略以了解 SRP 的工作原理的最佳实践。

    -   [最佳做法](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [软件限制策略的工作原理](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  打开限制策略的软件。

    -   [有关本地计算机](administer-software-restriction-policies.md#BKMK_1)

    -   [对于域，站点或单位，以及你是成员服务器或已加入域的工作站上](administer-software-restriction-policies.md#BKMK_2)

3.  如果你尚未之前定义软件限制策略，创建新的软件限制策略。

    -   [若要创建新的软件限制策略](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  创建路径规则电子邮件程序用于运行电子邮件附件的文件夹，然后将安全级别设置为**不允许**。

    -   [使用规则路径](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  指定规则适用的文件类型。

    -   [若要添加或删除指定的文件类型](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  修改策略设置，以使它们适用于用户和组所需：

    -   指定用户或组你不希望组策略对象的 (GPO) 应用的策略设置。

    -   本地管理员排除特定策略设置在组策略中软件限制策略和还有组策略适用于管理员的其余。

        -   [若要防止应用于管理员的本地软件限制策略](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  测试的策略。


