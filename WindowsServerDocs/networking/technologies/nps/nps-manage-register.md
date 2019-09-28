---
title: 在 Active Directory 域中注册 NPS
description: 你可以使用本主题来注册运行网络策略服务器的服务器，该服务器在 NPS 默认域或另一个域中的 Windows Server 2016 中运行。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6b72624f5817d2da5d2fb4e8622883e1ef4559cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396183"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>在 Active Directory 域中注册 NPS

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来注册运行网络策略服务器的服务器，该服务器在 NPS 默认域或另一个域中的 Windows Server 2016 中运行。

## <a name="register-an-nps-in-its-default-domain"></a>在其默认域中注册 NPS

您可以使用此过程在服务器是域成员的域中注册 NPS。 

必须在 Active Directory 中注册 NPSs，使其在授权过程中有权读取用户帐户的拨入属性。 注册 NPS 会将服务器添加到 Active Directory 中的 " **RAS 和 IAS 服务器**" 组。

Administrators组成员或同等身份是执行这些过程的最低要求。

### <a name="to-register-an-nps-in-its-default-domain"></a>在其默认域中注册 NPS


1. 在 NPS 上服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 此时将打开 "网络策略服务器" 控制台。

2. 右键单击 " **NPS （本地）** "，然后单击 " **Active Directory 中的" 注册服务器**"。 打开 **“网络策略服务器”** 对话框。

3. 在 **“网络策略服务器”** 中，单击 **“确定”** ，然后再次单击 **“确定”** 。

## <a name="register-an-nps-in-another-domain"></a>在另一域中注册 NPS

若要为 NPS 提供读取 Active Directory 中用户帐户的拨入属性的权限，必须在帐户所在的域中注册 NPS。

您可以使用此过程在 NPS 不是域成员的域中注册 NPS。

Administrators组成员或同等身份是执行这些过程的最低要求。

### <a name="to-register-an-nps-in-another-domain"></a>在另一域中注册 NPS

1. 在域控制器上的 "服务器管理器中，单击"**工具**"，然后单击" **Active Directory 用户和计算机**"。 此时将打开 Active Directory 用户和计算机 "控制台。

2. 在控制台树中，导航到想要 NPS 读取其用户帐户信息的域，然后单击 "**用户**" 文件夹。 

3. 在详细信息窗格中，右键单击 " **RAS 和 IAS 服务器**"，然后单击 "**属性**"。 " **RAS 和 IAS 服务器属性**" 对话框将打开。

4. 在 " **RAS 和 IAS 服务器属性**" 对话框中，单击 "**成员**" 选项卡，添加要在域中注册的每个 NPSs，然后单击 **"确定"** 。


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>使用适用于 NPS 的 Netsh 命令在另一域中注册 NPS

1. 打开 "命令提示符" 或 "windows PowerShell"。 

2. 在命令提示符下键入以下内容： **netsh nps add registeredserver** &nbsp;*domain* &nbsp;*server*，然后按 enter。

>[!NOTE]
>在上述命令中， *domain*是要在其中注册 nps 的域的 DNS 域名，而*server*是 nps 计算机的名称。

