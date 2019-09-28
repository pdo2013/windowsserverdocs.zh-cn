---
title: 管理与 NPS 配合使用的证书
description: 本主题提供有关在 Windows Server 2016 中将服务器证书用于网络策略服务器的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b83f68b52a9cceef779e5204e295bbc9e45e7a14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396194"
---
# <a name="manage-certificates-used-with-nps"></a>管理与 NPS 配合使用的证书

>适用于：Windows Server（半年频道）、Windows Server 2016

如果部署基于证书的身份验证方法，例如可扩展身份验证协议 @ no__t-0Transport Layer Security \(EAP @ no__t-2TLS @ no__t-3，受保护的可扩展身份验证协议 @ no__t-4Transport 层安全性 @no__5PEAP @ no__t-6TLS @ no__t-7 和 PEAP @ no__t-8Microsoft 质询握手身份验证协议版本 2 \(MS @ no__t-10CHAP v2 @ no__t-11，你必须将服务器证书注册到你的所有 NPSs。 服务器证书必须：

- 满足为[PEAP 和 EAP 要求配置证书模板](nps-manage-cert-requirements.md)中所述的最低服务器证书要求

- 由客户端计算机信任的证书颁发机构 \(CA @ no__t-1 颁发。 在当前用户和本地计算机的 "受信任的根证书颁发机构" 证书存储中存在其证书时，信任 CA。

以下说明有助于在受信任的根 CA 为第三方 CA （如 Verisign）的部署中管理 NPS 证书，或者通过使用 Active Directory 为公钥基础结构部署的 CA 提供 \(PKI @ no__t证书服务 \(AD CS @ no__t-3。

## <a name="change-the-cached-tls-handle-expiry"></a>更改缓存的 TLS 句柄过期

在针对 EAP @ no__t-0TLS、PEAP @ no__t-1TLS 和 PEAP @ no__t-2MS @ no__t-3CHAP v2 的初始身份验证过程中，NPS 将缓存连接客户端的 TLS 连接属性的一部分。 客户端还会缓存 NPS 的部分 TLS 连接属性。

这些 TLS 连接属性的每个单独集合称为 TLS 句柄。

客户端计算机可以缓存多个验证器的 TLS 句柄，而 NPSs 可以缓存许多客户端计算机的 TLS 句柄。

客户端和服务器上的缓存 TLS 句柄允许重新进行重新进行身份验证的过程更快。 例如，当无线计算机使用 NPS reauthenticates 时，NPS 可以检查无线客户端的 TLS 句柄，并可快速确定客户端连接是重新连接的。 NPS 无需执行完全身份验证即可授权连接。

同样，客户端检查 NPS 的 TLS 句柄，确定它是重新连接的，无需执行服务器身份验证。

在运行 Windows 10 和 Windows Server 2016 的计算机上，默认的 TLS 句柄过期时间为10小时。

在某些情况下，可能需要增加或减少 TLS 句柄到期时间。

例如，你可能想要在用户的证书被管理员吊销且证书已过期的情况下减少 TLS 处理程序到期时间。 在这种情况下，如果 NPS 具有未过期的缓存 TLS 句柄，则用户仍可以连接到网络。 降低 TLS 句柄过期时间可能有助于防止此类用户重新连接吊销的证书。

>[!NOTE]
>此方案的最佳解决方法是在 Active Directory 中禁用用户帐户，或者从授予连接网络策略中的网络权限的 Active Directory 组中删除用户帐户。 不过，由于复制延迟，对所有域控制器的这些更改的传播可能也会延迟。 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>在客户端计算机上配置 TLS 句柄过期时间

您可以使用此过程来更改客户端计算机缓存 NPS 的 TLS 句柄的时间。 成功对 NPS 进行身份验证后，客户端计算机会将 NPS 的 TLS 连接属性缓存为 TLS 句柄。 TLS 句柄的默认持续时间为10小时 \(36000000 毫秒 @ no__t。 使用以下过程可以增加或减少 TLS 句柄到期时间。

**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。

>[!IMPORTANT]
>此过程必须在 NPS 上执行，而不是在客户端计算机上执行。

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>在客户端计算机上配置 TLS 处理过期时间

1. 在 NPS 上，打开注册表编辑器。

2. 浏览到注册表项**HKEY @ no__t-1LOCAL @ no__t-2MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在 "**编辑**" 菜单上，单击 "**新建**"，然后单击 "**项**"。

4. 键入**ClientCacheTime**，然后按 enter。

5. 右键单击**ClientCacheTime**，单击 "**新建**"，然后单击 " **DWORD （32位）值**"。

6. 键入在 NPS 第一次成功进行身份验证尝试之后客户端计算机缓存 NPS TLS 句柄的时间（以毫秒为单位）。

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>在 NPSs 上配置 TLS 句柄过期时间

使用此过程来更改 NPSs 缓存客户端计算机的 TLS 句柄的时间长度。 成功对访问客户端进行身份验证后，将客户端计算机的 NPSs 缓存 TLS 连接属性作为 TLS 句柄。 TLS 句柄的默认持续时间为10小时 \(36000000 毫秒 @ no__t。 使用以下过程可以增加或减少 TLS 句柄到期时间。

**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。

>[!IMPORTANT]
>此过程必须在 NPS 上执行，而不是在客户端计算机上执行。

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>在 NPSs 上配置 TLS 句柄过期时间

1. 在 NPS 上，打开注册表编辑器。

2. 浏览到注册表项**HKEY @ no__t-1LOCAL @ no__t-2MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 在 "**编辑**" 菜单上，单击 "**新建**"，然后单击 "**项**"。

4. 键入**ServerCacheTime**，然后按 enter。

5. 右键单击**ServerCacheTime**，单击 "**新建**"，然后单击 " **DWORD （32位）值**"。

6. 键入客户端第一次成功进行身份验证尝试之后，NPSs 要缓存客户端计算机的 TLS 句柄的时间长度（以毫秒为单位）。

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>获取受信任的根 CA 证书的 SHA-1 哈希

使用此过程从本地计算机上安装的证书获取受信任的根证书颁发机构（CA）的安全哈希算法（SHA-1）哈希。 在某些情况下，例如在部署组策略时，需要使用证书的 SHA-1 哈希来指定证书。

使用组策略时，可以指定一个或多个受信任的根 CA 证书，客户端必须使用这些证书，以便在使用 EAP 或 PEAP 进行相互身份验证的过程中对 NPS 进行身份验证。 若要指定客户端必须用于验证服务器证书的受信任的根 CA 证书，可以输入证书的 SHA-1 哈希。

此过程演示如何使用 "证书" Microsoft 管理控制台（MMC）管理单元获取受信任的根 CA 证书的 SHA-1 哈希。 

若要完成此过程，您必须是本地计算机上 "**用户**" 组的成员。

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>获取受信任的根 CA 证书的 SHA-1 哈希

1. 在 "运行" 对话框或 Windows PowerShell 中，键入**mmc**，然后按 enter。 将打开 Microsoft 管理控制台 \(MMC @ no__t-1。 在 MMC 中，单击 "**文件**"，然后单击 "**添加/删除 Snap\in**"。 "**添加或删除管理单元**" 对话框将打开。

2. 在 "**添加或删除管理单元**" 的 "**可用管理单元**" 中，双击 "**证书**"。 "证书管理单元" 向导将打开。 单击 "**计算机帐户**"，然后单击 "**下一步**"。

3. 在 "**选择计算机**" 中，确保选中 "**本地计算机（运行此控制台的计算机）** "，单击 "**完成**"，然后单击 **"确定"** 。

4. 在左窗格中，双击 "**证书（本地计算机）** "，然后双击 "**受信任的根证书颁发机构**" 文件夹。

5. "**证书**" 文件夹是 "**受信任的根证书颁发机构**" 文件夹的子文件夹。 单击 "**证书**" 文件夹。

6. 在详细信息窗格中，浏览到受信任的根 CA 的证书。 双击该证书。 此时将打开 "**证书**" 对话框。

7. 在 "**证书**" 对话框中，单击 "**详细信息**" 选项卡。

8. 在字段列表中，滚动到 "指纹" 并选择 "**指纹**"。

9. 在下面的窗格中，将显示作为证书的 SHA-1 哈希的十六进制字符串。 选择 SHA-1 哈希，然后按 "复制" 命令的 Windows 键盘快捷方式 \(CTRL @ no__t-1C @ no__t，将哈希复制到 Windows 剪贴板。

10. 打开要粘贴 SHA-1 哈希的位置，正确找到光标，然后按 "粘贴" 命令的 Windows 键盘快捷方式 \(CTRL @ no__t-1V @ no__t。 

有关证书和 NPS 的详细信息，请参阅[为 PEAP 和 EAP 要求配置证书模板](nps-manage-cert-requirements.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。
