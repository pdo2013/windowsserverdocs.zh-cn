---
title: 使用软件限制策略来帮助保护计算机免受电子邮件病毒攻击
description: Windows Server 安全
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850668"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>使用软件限制策略来帮助保护计算机免受电子邮件病毒攻击

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题提供使用软件限制策略 (SRP) 来帮助保护计算机免受电子邮件病毒开始使用 Windows Server 2008 和 Windows Vista 如何设置应用程序控制策略的信息。

## <a name="introduction"></a>简介
软件限制策略 (SRP) 是基于组策略的功能，用于标识在域中的计算机上运行的软件程序，以及控制这些程序的运行能力。 你可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 这些与 Microsoft Active Directory 域服务和组策略集成，但也可以配置在独立计算机上。 有关 SRP 的起始点，请参阅[软件限制策略](software-restriction-policies.md)。

从 Windows Server 2008 R2 和 Windows 7 开始，Windows AppLocker 可以用于或 SRP 配合应用程序控制策略的一部分。 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>配置 SRP 来帮助保护对电子邮件病毒

1.  查看软件限制策略，以了解 SRP 的工作原理的最佳实践。

    -   [最佳做法](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [软件限制策略的工作原理](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  打开“软件限制策略”。

    -   [为本地计算机](administer-software-restriction-policies.md#BKMK_1)

    -   [对于域，站点或组织单位，并且是成员服务器上还是在上加入到域的工作站](administer-software-restriction-policies.md#BKMK_2)

3.  如果以前未定义软件限制策略，创建新的软件限制策略。

    -   [若要创建新的软件限制策略](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  创建电子邮件程序用来运行电子邮件附件的文件夹的路径规则和设置的安全性级别**不允许**。

    -   [使用路径规则](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  指定应用规则的文件类型。

    -   [若要添加或删除指定的文件类型](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  修改策略设置，使它们适用于用户和所需的组：

    -   指定用户或组你不希望组策略对象的 (GPO) 应用策略设置。

    -   从组策略中的特定策略设置的软件限制策略中排除本地管理员，并且仍有适用于管理员的组策略的其余部分。

        -   [若要防止向本地管理员应用软件限制策略](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  测试策略。


