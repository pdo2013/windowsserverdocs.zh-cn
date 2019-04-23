---
title: 从 Active Directory 域中注销 NPS
description: 本主题可用于注册在 Windows Server 2016 中的 NPS 默认域或另一个域中运行网络策略服务器的服务器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe4773efd89aeb413b3793f874ad6a1b030294a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864348"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>从 Active Directory 域中注销 NPS

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在过程中管理 NPS 部署，你可能会发现可以将 NPS 移动到另一个域，以替换 NPS 或停用 NPS。 

当您移动或取消配置 NPS 时，可以取消注册 NPS 其中有权读取用户帐户在 Active Directory 中的属性的 Active Directory 域中的 NPS。

Administrators组成员或同等身份是执行这些过程的最低要求。

## <a name="to-unregister-an-nps"></a>若要取消注册 NPS

1. 在域控制器上，在服务器管理器中，单击**工具**，然后单击**Active Directory 用户和计算机**。 此时将打开 Active Directory 用户和计算机控制台。

2. 单击**用户**，然后双击**RAS 和 IAS 服务器**。

3. 单击**成员**卡，并选择你想要注销的 NPS。

4. 单击**删除**，单击**是**，然后单击**确定**。

