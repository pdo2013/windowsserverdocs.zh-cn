---
title: 注销 NPS 服务器 Active Directory 域中
description: 你可以使用本主题注册运行 Windows Server 2016 NPS 服务器默认域中，或者在另一台域网络策略服务器的服务器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55c3b00146706831351ce63d1e5b74f45d7b9be1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="unregister-an-nps-server-from-an-active-directory-domain"></a>注销 NPS 服务器 Active Directory 域中

>适用于：Windows Server（半年通道），Windows Server 2016

处于过程 NPS 服务器部署管理，你可能会发现移到另一个域，替换 NPS 服务器，或者停用 NPS 服务器 NPS 服务器有用。 

当你移动或停止使用 NPS 服务器时，你可以取消注册 NPS 服务器 NPS 服务器其中有权读取 Active Directory 中的用户帐户属性 Active Directory 域中。

在会员**管理员**，或等效的最低要求执行这些过程。

## <a name="to-unregister-an-nps-server"></a>若要注销 NPS 服务器

1. 在服务器管理器中，域控制器上单击**工具**，然后单击**Active Directory 用户和计算机**。 打开 Active Directory 用户和计算机主机。

2. 单击**用户**，然后双击**RAS 和 IAS 服务器**。

3. 单击**成员**选项卡，然后依次选择你想要注销 NPS 服务器。

4. 单击**删除**，单击**是**，然后单击**确定**。

