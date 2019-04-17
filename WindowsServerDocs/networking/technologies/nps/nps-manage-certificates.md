---
title: 管理证书与 NPS 一起使用
description: 本主题提供了有关与 Windows Server 2016 中的网络策略服务器使用 server 证书的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2044cf30cc90c1673e05a1948ac9196d05940d1f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-used-with-nps"></a>管理证书与 NPS 一起使用

>适用于：Windows Server（半年通道），Windows Server 2016

如果你将其部署证书基于身份验证方法，如可扩展身份验证 Protocol\ 传输层安全性 \(EAP\-TLS\)、保护可扩展身份验证 Protocol\ 传输层安全性 \(PEAP\-TLS\) 和 PEAP\ Microsoft 质询握手身份验证协议版本 2 \ (MS\ CHAP v2\)，必须对所有 NPS 服务器都登记服务器证书。 必须服务器证书：

- 满足最低服务器证书要求，述[配置 PEAP 和 EAP 要求的证书模板](nps-manage-cert-requirements.md)

- 向证书颁发机构颁发 \(CA\) 受信任的客户端计算机。 当它证书存在当前用户，本机的受信任的根证书颁发机构证书应用商店中，CA 是受信任。

以下说明进行操作帮助管理 NPS server 证书部署所在的位置根受信任的第三方 CA，如 Verisign，或者是你已将其部署公钥基础结构的 CA 在使用 Active Directory 证书服务 \(AD CS\) \(PKI\)。

## <a name="change-the-cached-tls-handle-expiry"></a>更改缓存的 TLS 句柄到期

在适用于 EAP\ TLS、PEAP\ TLS 和 PEAP\ MS\ CHAP v2 初始身份验证过程中，NPS 服务器缓存连接的客户端 TLS 连接属性的一部分。 客户端还缓存 NPS 服务器 TLS 连接属性的一部分。

这些 TLS 连接属性每个单独集锦称为 TLS 句柄。

尽管 NPS 服务器可以缓存的许多客户端计算机 TLS 手柄客户端计算机可以缓存多个身份 TLS 手柄。

客户端和服务器上的缓存的 TLS 手柄允许更快地出现重新进行身份验证过程。 例如，当无线计算机 reauthenticates NPS 服务器，NPS 服务器 TLS 句柄检查无线客户端，并快速确定客户端连接为重新连接。 NPS 服务器连接授权无需执行完整身份验证。

相应地，的客户端检查 NPS 服务器 TLS 句柄，确定为重新连接，并且无需执行服务器身份验证。

在运行 Windows 10 和 Windows Server 2016 的计算机，默认 TLS 句柄到期位于 10 小时。

在某些情况下，你可能想要加快或减慢 TLS 句柄的过期时间。

例如，你可能想要降低在其中的用户已被吊销管理员，证书已过期的情况下 TLS 句柄过期时间。 在此情况下，用户仍然可以连接到该网络 NPS 服务器是否未到期的缓存的 TLS 句柄。 减少 TLS 句柄到期可能有助于防止吊销证书具有此类用户重新连接。

>[!NOTE]
>此项 scenario 最佳解决方法是要禁用的用户帐户在 Active Directory，或删除 Active Directory 组被授予权限，连接到网络策略中的网络的用户帐户。 这些更改所有域控制器传播可能还会遭遇延迟，但是，由于复制延迟。 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>在客户端计算机上配置 TLS 句柄的过期时间

你可以使用此过程若要更改的客户端计算机缓存的 NPS 服务器 TLS 句柄的时间。 后成功进行身份验证 NPS 服务器的客户端计算机作为 TLS 句柄缓存 NPS 服务器 TLS 连接属性。 TLS 句柄有 10 个小时 \(36,000,000 milliseconds\) 默认持续时间。 你可以增加或减少 TLS 句柄的过期时间使用下面的过程。

在会员**管理员**，或等效的最低要求完成此过程。

>[!IMPORTANT]
>必须 NPS 在服务器上，不在客户端计算机上执行此过程。

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>若要配置 TLS 处理客户端计算机上的过期时间

1. 在 NPS 服务器上，打开注册表编辑器中。

2. 浏览到的注册表项**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在**编辑**菜单上，单击**新建**，然后单击**键**。

4. 键入**ClientCacheTime**，然后按 ENTER。

5. 右键单击**ClientCacheTime**，单击**新建**，然后单击**DWORD（32 位）值**。

6. 键入你想要后首次成功身份验证尝试通过 NPS 服务器缓存的 NPS 服务器 TLS 句柄的客户端计算机毫秒为单位一段时间。

## <a name="configure-the-tls-handle-expiry-time-on-nps-servers"></a>配置 TLS 句柄的过期时间 NPS 服务器

使用此过程，若要更改的时间 NPS 服务器缓存的客户端计算机 TLS 句柄。 后成功进行身份验证的访问权限的客户端，服务器 NPS 缓存作为 TLS 句柄 TLS 连接属性的客户端计算机。 TLS 句柄有 10 个小时 \(36,000,000 milliseconds\) 默认持续时间。 你可以增加或减少 TLS 句柄的过期时间使用下面的过程。

在会员**管理员**，或等效的最低要求完成此过程。

>[!IMPORTANT]
>必须 NPS 在服务器上，不在客户端计算机上执行此过程。

### <a name="to-configure-the-tls-handle-expiry-time-on-nps-servers"></a>若要配置 TLS 处理 NPS 服务器上的过期时间

1. 在 NPS 服务器上，打开注册表编辑器中。

2. 浏览到的注册表项**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在**编辑**菜单上，单击**新建**，然后单击**键**。

4. 键入**ServerCacheTime**，然后按 ENTER。

5. 右键单击**ServerCacheTime**，单击**新建**，然后单击**DWORD（32 位）值**。

6. 键入的时间，，以毫秒为单位，要 NPS 缓存的客户端计算机 TLS 句柄后客户端尝试首次成功身份验证的服务器。

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>获取受信任的根 CA 证书的希 SHA-1

使用此过程从本地计算机安装的证书获取安全哈希算法 (sha-1) 的的受信任的根证书颁发机构（加拿大）希。 在某些情况下，如部署组策略，时需指定由使用证书 SHA-1 哈希证书。

当使用组策略，你可以指定以进行 NPS 服务器身份验证 EAP 或 PEAP 相互身份验证过程的客户端必须使用的一个或多个受信任的根加拿大证书。 若要指定客户必须用来验证的服务器证书受信任的根加拿大证书，你可以输入证书 SHA-1 哈希。

此过程演示如何获取的受信任的根 SHA-1 希由使用证书 Microsoft 管理控制台 (MMC) 管理单元。 

若要完成此过程，你必须**用户**组本地计算机上。

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>若要获取的受信任的根证书希 SHA-1

1. 在运行对话框或 Windows PowerShell 中，键入**mmc**，然后按 ENTER。 打开 Microsoft 管理控制台 \(MMC\)。 在 MMC 中，单击**文件**，然后单击**添加/删除 Snap\in**。 **添加或删除管理单元**对话框中打开。

2. 在**添加或删除管理单元**中**可用管理单元**，双击**证书**。 证书贴靠向导将打开。 单击**计算机帐户**，然后单击**下一步**。

3. 在**选择计算机**，确保**本地计算机（此控制台运行的计算机）**是处于选中状态，单击**完成**，然后单击**确定**。

4. 在左侧窗格中，双击**证书（本地计算机）**，然后双击**信任根证书颁发机构**文件夹。

5. **证书**文件夹是子文件夹的**信任根证书颁发机构**文件夹。 单击**证书**文件夹。

6. 在详细信息窗格中，浏览到你信任根证书。 双击该证书。 **证书**对话框中打开。

7. 在**证书**对话框中，单击**详细信息**选项卡。

8. 在列表中的字段，向滚动并选择**指纹**。

9. 在较低窗格中，将显示为你证书的 SHA-1 哈希十六进制字符串。 选择 SHA-1 希并按 Windows 键盘快捷方式副本命令 \(CTRL\+C\) 将哈希复制到剪贴板 Windows。

10. 打开该位置的命令 \(CTRL\+V\) 到要粘贴 SHA-1 哈希、正确地找到光标和粘贴按 Windows 键盘快捷方式。 

有关证书和 NPS 的详细信息，请参阅[配置 PEAP 和 EAP 要求的证书模板](nps-manage-cert-requirements.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。
