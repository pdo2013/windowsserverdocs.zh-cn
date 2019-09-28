---
title: 使用软件限制策略来帮助保护计算机免受电子邮件病毒攻击
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4c691255683eb37eecdbeaa55c094b7ce5c4e26d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357648"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>使用软件限制策略来帮助保护计算机免受电子邮件病毒攻击

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍如何使用软件限制策略（SRP）设置应用程序控制策略，以帮助保护计算机免受 Windows Server 2008 和 Windows Vista 中的电子邮件病毒的攻击。

## <a name="introduction"></a>介绍
软件限制策略 (SRP) 是基于组策略的功能，用于标识在域中的计算机上运行的软件程序，以及控制这些程序的运行能力。 你可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 它们与 Microsoft Active Directory 域服务和组策略集成在一起，但也可以在独立计算机上进行配置。 有关 SRP 的起点，请参阅[软件限制策略](software-restriction-policies.md)。

从 Windows Server 2008 R2 和 Windows 7 开始，可以使用 Windows AppLocker，而不是与 SRP 一起使用，以获得部分应用程序控制策略。 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>配置 SRP 以帮助防止电子邮件病毒

1.  查看软件限制策略的最佳实践以了解 SRP 的工作方式。

    -   [最佳做法](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [软件限制策略的工作方式](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  打开“软件限制策略”。

    -   [对于本地计算机](administer-software-restriction-policies.md#BKMK_1)

    -   [对于域、站点或组织单位，并且位于加入到域中的成员服务器或工作站上](administer-software-restriction-policies.md#BKMK_2)

3.  如果你之前未定义软件限制策略，请创建新的软件限制策略。

    -   [创建新的软件限制策略](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  为电子邮件程序用来运行电子邮件附件的文件夹创建路径规则，然后将安全级别设置为 "不**允许**"。

    -   [使用路径规则](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  指定规则应用到的文件类型。

    -   [添加或删除指定的文件类型](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  修改策略设置，以便将其应用于所需的用户和组：

    -   指定你不希望应用组策略对象的（GPO）策略设置的用户或组。

    -   从组策略中的特定策略设置的软件限制策略中排除本地管理员，并且仍将组策略应用到管理员。

        -   [防止软件限制策略应用于本地管理员](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  测试策略。


