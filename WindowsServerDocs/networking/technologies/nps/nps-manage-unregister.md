---
title: 从 Active Directory 域中注销 NPS
description: 你可以使用本主题来注册运行网络策略服务器的服务器，该服务器在 NPS 默认域或另一个域中的 Windows Server 2016 中运行。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3225c42ab14e2ea1bc283f520b14c09ebc2254c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396079"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>从 Active Directory 域中注销 NPS

>适用于：Windows Server（半年频道）、Windows Server 2016

在管理 NPS 部署的过程中，你可能会发现将 NPS 移动到另一个域、替换 NPS 或停用 NPS 很有用。 

在移动或解除授权 NPS 时，可以在 Active Directory 域中取消注册 nps，其中 NPS 在 Active Directory 中有权读取用户帐户的属性。

Administrators组成员或同等身份是执行这些过程的最低要求。

## <a name="to-unregister-an-nps"></a>取消注册 NPS

1. 在域控制器上的 "服务器管理器中，单击"**工具**"，然后单击" **Active Directory 用户和计算机**"。 此时将打开 Active Directory 用户和计算机 "控制台。

2. 单击 "**用户**"，然后双击 " **RAS 和 IAS 服务器**"。

3. 单击 "**成员**" 选项卡，然后选择要取消注册的 NPS。

4. 单击 "**删除**"，单击 **"是**"，然后单击 **"确定"** 。

