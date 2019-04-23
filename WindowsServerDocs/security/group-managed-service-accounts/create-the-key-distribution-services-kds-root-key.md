---
title: 创建密钥分发服务 KDS 根密钥
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 3d5f7b46b28e6a2fbfafb664b69aebc8d34886fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867208"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>创建密钥分发服务 KDS 根密钥

>适用于：Windows 服务器 （半年频道），Windows Server 2016

面向 IT 专业人员的本主题介绍如何使用 Windows PowerShell 在 Windows Server 2012 中生成组托管服务帐户密码的域控制器上创建一个 Microsoft 密钥分发服务 (kdssvc.dll) 根密钥。

 Windows Server 2012 域控制器 (DC) 要求根密钥开始生成 gMSA 密码。 从创建时到允许所有域控制器聚合其 AD 复制，域控制器将最长等待 10 小时，才能允许创建 gMSA。 设置为 10 小时是一项安全措施，可防止在环境中的所有 DC 都能应答 gMSA 请求之前生成密码。  如果你尝试使用 gMSA 时间太短密钥可能尚未复制到所有 Windows Server 2012 Dc 和密码检索可能因此失败当 gMSA 主机尝试检索的密码。 如果使用具有受限复制计划的 DC 或者出现复制问题，gMSA 密码检索失败也会发生。

“域管理员”  或“企业管理员”  组中的成员身份或同等身份是完成此过程的最低要求。 有关使用适当帐户和组成员身份的详细信息，请参阅 [本地和域默认组](https://technet.microsoft.com/library/dd728026(WS.10).aspx)。

> [!NOTE]
> 要运行用于管理组托管服务帐户的 Windows PowerShell 命令，需要 64 位体系结构。

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>若要创建 KDS 根密钥，使用 Add-kdsrootkey cmdlet

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模块的命令提示符下键入以下命令，然后按 ENTER：

    **Add-KdsRootKey -EffectiveImmediately**

    > [!TIP]
    > 可以使用有效时间参数来使密钥在使用之前有时间传播到所有 DC。 使用 Add-kdsrootkey-EffectiveImmediately 将将一个根密钥添加到目标 DC 将立即由 KDS 服务使用。 但是，其他 Windows Server 2012 Dc 将不能使用根密钥，直到复制成功。

对于只带有一个 DC 的测试环境，你可以使用以下过程来创建 KDS 根密钥，并将开始时间设置为过去的时间，以避免密钥生成的时间间隔等待。 验证 4004 事件是否已记录在 kds 事件日志中。

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>在测试环境中创建即时生效的 KDS 根密钥

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模块的命令提示符下键入以下命令，然后按 ENTER：

    **$a=Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey -EffectiveTime $b**

    或使用单个命令

    **Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>请参阅
[开始使用组托管服务帐户](getting-started-with-group-managed-service-accounts.md)


