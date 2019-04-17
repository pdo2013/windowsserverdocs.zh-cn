---
title: 注册 Active Directory 域中 NPS 服务器
description: 你可以使用本主题注册运行 Windows Server 2016 NPS 服务器默认域中，或者在另一台域网络策略服务器的服务器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ba18f6c994e8b15da3a07a3e37550d5dbff24af
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="register-an-nps-server-in-an-active-directory-domain"></a>注册 Active Directory 域中 NPS 服务器

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题注册运行 Windows Server 2016 NPS 服务器默认域中，或者在另一台域网络策略服务器的服务器。

## <a name="register-an-nps-server-in-its-default-domain"></a>在其默认域中注册 NPS 服务器

你可以使用此过程在其中服务器域加入的域中注册 NPS 服务器。 

在 Active Directory 必须注册 NPS 服务器，以便并且有权授权过程阅读拨号中的用户帐户的属性。 注册 NPS 服务器添加到服务器**RAS 和 IAS 服务器**组中的 Active Directory。

在会员**管理员**，或等效的最低要求执行这些过程。

### <a name="to-register-an-nps-server-in-its-default-domain"></a>若要在其默认域中注册 NPS 服务器


1. 在 NPS 服务器，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 打开网络策略服务器主机。

2. 右键单击**NPS（本地）**，然后单击**Active Directory 中注册服务器**。 **网络策略服务器**对话框中打开。

3. 在**网络策略服务器**，单击**确定**，然后单击**确定**再次。

## <a name="register-an-nps-server-in-another-domain"></a>在另一台域中注册 NPS 服务器

有权读取 Active Directory 中的用户帐户的拨属性提供 NPS 服务器，在域帐户所在必须注册 NPS 服务器。

你可以使用此过程 NPS 服务器不可域成员某个域中注册 NPS 服务器。

在会员**管理员**，或等效的最低要求执行这些过程。

### <a name="to-register-an-nps-server-in-another-domain"></a>在另一台域中注册 NPS 服务器

1. 在服务器管理器中，域控制器上单击**工具**，然后单击**Active Directory 用户和计算机**。 打开 Active Directory 用户和计算机主机。

2. 控制台树中，导航到你希望 NPS 服务器读取用户的帐户信息的位置的域，然后单击**用户**文件夹。 

3. 在详细信息窗格中，右键单击**RAS 和 IAS 服务器**，然后单击**属性**。 **RAS 和 IAS 服务器属性**对话框中打开。

4. 在**RAS 和 IAS 服务器属性**对话框中，单击**成员**选项卡上，每个 NPS 服务器注册在域中，然后单击你要添加**确定**。


### <a name="to-register-an-nps-server-in-another-domain-by-using-netsh-commands-for-nps"></a>在另一台域中注册 NPS 服务器，方法是使用适用于 NPS Netsh 命令

1. 打开命令提示符或 windows PowerShell。 

2. 在命令提示符中键入以下命令：**netsh nps 添加**&nbsp;*域*&nbsp;*服务器*，然后按 ENTER。

>[!NOTE]
>在上述的命令，*域*是你想要注册 NPS 服务器的域的 DNS 域名称和*服务器*是 NPS 服务器计算机的名称。

