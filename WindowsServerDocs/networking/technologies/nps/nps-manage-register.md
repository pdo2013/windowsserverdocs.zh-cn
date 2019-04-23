---
title: 在 Active Directory 域中注册 NPS
description: 本主题可用于注册在 Windows Server 2016 中的 NPS 默认域或另一个域中运行网络策略服务器的服务器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a289ec519e5107576becf2905cd881cf9def190
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877648"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>在 Active Directory 域中注册 NPS

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于注册在 Windows Server 2016 中的 NPS 默认域或另一个域中运行网络策略服务器的服务器。

## <a name="register-an-nps-in-its-default-domain"></a>在默认域中注册 NPS

此过程可用于在其中服务器是域成员的域中注册 NPS。 

必须在 Active Directory 中注册 NPSs，以便他们有权在授权过程中读取用户帐户的拨入属性。 注册 NPS 将添加到服务器**RAS 和 IAS 服务器**组在 Active Directory 中。

Administrators组成员或同等身份是执行这些过程的最低要求。

### <a name="to-register-an-nps-in-its-default-domain"></a>若要在其默认域中注册 NPS


1. 在 NPS 中，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 将打开网络策略服务器控制台。

2. 右键单击**NPS （本地）**，然后单击**Active Directory 中注册服务器**。 打开 **“网络策略服务器”** 对话框。

3. 在 **“网络策略服务器”** 中，单击 **“确定”**，然后再次单击 **“确定”**。

## <a name="register-an-nps-in-another-domain"></a>在另一个域中注册 NPS

若要向 NPS 提供的权限读取 Active Directory 中的用户帐户的拨入属性，必须在帐户所在的域中注册 NPS。

此过程可用于在 NPS 不是域成员的域中注册 NPS。

Administrators组成员或同等身份是执行这些过程的最低要求。

### <a name="to-register-an-nps-in-another-domain"></a>若要在另一个域中注册 NPS

1. 在域控制器上，在服务器管理器中，单击**工具**，然后单击**Active Directory 用户和计算机**。 此时将打开 Active Directory 用户和计算机控制台。

2. 在控制台树中，导航到 NPS 来读取用户帐户信息的域，然后单击**用户**文件夹。 

3. 在细节窗格中，右键单击**RAS 和 IAS 服务器**，然后单击**属性**。 **RAS 和 IAS 服务器属性**对话框随即打开。

4. 在中**RAS 和 IAS 服务器属性**对话框中，单击**成员**选项卡上，添加每个你想要在域中注册，然后单击 NPSs**确定**。


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>若要在另一个域中注册 NPS，通过用于 NPS 的 Netsh 命令

1. 打开命令提示符或 windows PowerShell。 

2. 在命令提示符下键入以下内容： **netsh nps 添加 registeredserver** &nbsp;*域* &nbsp; *server*，然后按 ENTER。

>[!NOTE]
>在上述命令中，*域*是你想要注册 NPS，域的 DNS 域名和*server*是 NPS 计算机的名称。

