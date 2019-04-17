---
title: "创建密钥分发服务 KDS 根键"
description: "Windows Server 安全"
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
ms.openlocfilehash: 30075e56f3ca8e90a0655508efeacfcf2aaa0337
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>创建密钥分发服务 KDS 根键

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员介绍了如何创建 Microsoft 键分发服务 (kdssvc.dll) 根密钥域控制器使用 Windows PowerShell，若要在 Windows Server 2012 生成组托管服务帐户密码。

 Windows Server 2012 时域控制器 (DC) 需要根密钥开始生成 gMSA 密码。 域控制器将等待最多 10 小时从创建，以使所有的域控制器聚合他们的广告复制允许 gMSA 创建之前的时间。 10 小时是阻止发生之前环境中的所有 Dc 都都能回答 gMSA 请求密码代安全措施。  如果你尝试使用 gMSA 较短时间密钥可能未已复制到所有 Windows Server 2012 域控制器，因此密码检索可能会失败时 gMSA 主机尝试检索该密码。 使用 Dc 与受限的复制计划或是否复制的问题时，也会发生 gMSA 密码检索失败。

在会员**域管理员**或**企业管理员**组中或等效是最低要求完成此过程。 有关使用相应的帐户和组成员的详细信息，请参阅[本地和域默认组](https://technet.microsoft.com/library/dd728026(WS.10).aspx)。

> [!NOTE]
> 64 位体系结构需运行的 Windows PowerShell 命令，用于进行管理组托管服务帐户。

#### <a name="to-create-the-kds-root-key-using-the-new-kdsrootkey-cmdlet"></a>若要创建使用 New-KdsRootKey cmdlet KDS 根键

1.  在 Windows Server 2012 域控制器上，从任务栏上运行的 Windows PowerShell。

2.  在 Active Directory 的 Windows PowerShell 模块的命令提示符下，键入以下命令，然后按 ENTER:

    **Add-KdsRootKey EffectiveImmediately**

    > [!TIP]
    > 有效的时间参数可用于提供会传播到之前使用的所有 Dc 键的时间。 使用 Add-KdsRootKey EffectiveImmediately 将添加到目标直流 KDS 服务立即将使用根键。 但是，Windows Server 2012 的其他 Dc 将无法使用根键，直到复制成功。

有关测试环境中只有一个直流，你可以创建 KDS 根键，并设置过去以避免通过以下步骤等待密钥生成的时间间隔的开始时间。 验证 4004 事件已登录 kds 事件日志。

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>有关即时效果测试环境创建 KDS 根键

1.  在 Windows Server 2012 域控制器上，从任务栏上运行的 Windows PowerShell。

2.  在 Active Directory 的 Windows PowerShell 模块的命令提示符下，键入以下命令，然后按 ENTER:

    **$= 获取日期**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey EffectiveTime $b**

    或使用单个命令

    **Add-KdsRootKey EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>请参阅
[入门组托管服务帐户](getting-started-with-group-managed-service-accounts.md)


