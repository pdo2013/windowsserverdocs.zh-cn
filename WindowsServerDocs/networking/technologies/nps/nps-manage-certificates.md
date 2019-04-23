---
title: 管理与 NPS 配合使用的证书
description: 本主题提供有关使用 Windows Server 2016 中的网络策略服务器使用服务器证书的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73f3d6a1e9dc6ae1520b1d685b6b05b5f3aed601
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864228"
---
# <a name="manage-certificates-used-with-nps"></a>管理与 NPS 配合使用的证书

>适用于：Windows 服务器 （半年频道），Windows Server 2016

如果部署基于证书的身份验证方法，如可扩展身份验证协议\-传输层安全性\(EAP\-TLS\)，受保护的可扩展身份验证协议\-传输层安全性\(PEAP\-TLS\)，和 PEAP\-Microsoft 质询握手身份验证协议版本 2 \(MS\-CHAP v2\)，必须对所有你 NPSs 注册服务器证书。 服务器证书必须：

- 满足最小服务器证书要求，如中所述[为 PEAP 和 EAP 要求配置证书模板](nps-manage-cert-requirements.md)

- 由证书颁发机构颁发\(CA\)受信任的客户端计算机。 在当前用户和本地计算机的受信任的根证书颁发机构证书存储中存在其证书时，CA 是受信任。

以下说明帮助管理 NPS 中部署受信任的根 CA 其中是第三方 CA，如 Verisign，或已部署的公钥基础结构是 ca 的证书\(PKI\)使用处于活动状态Directory 证书服务\(AD CS\)。

## <a name="change-the-cached-tls-handle-expiry"></a>更改缓存的 TLS 句柄到期

在 EAP 的初始身份验证进程期间\-TLS、 PEAP\-TLS 和 PEAP\-MS\-CHAP v2，NPS 将缓存连接的客户端的 TLS 连接属性的一部分。 客户端还会缓存 NPS 的 TLS 连接属性的一部分。

这些 TLS 连接属性的每个单个集合称为 TLS 句柄。

NPSs 可以缓存的许多客户端计算机的 TLS 句柄时，客户端计算机可以为多个身份验证器，缓存 TLS 句柄。

在客户端和服务器上的缓存的 TLS 句柄允许重新进行身份验证过程更快速地发生。 例如，当 nps reauthenticates 无线计算机，NPS 可以实现无线客户端检查 TLS 句柄，并可以快速确定客户端连接为重新连接。 NPS 连接授权而不执行完全身份验证。

相应地，客户端检查 NPS 的 TLS 句柄，确定它为重新连接，并不需要进行服务器身份验证。

在计算机上运行 Windows 10 和 Windows Server 2016，默认 TLS 句柄到期日期在 10 小时。

在某些情况下，你可能想要增加或减少的 TLS 句柄到期时间。

例如，你可能想要减少在其中用户的证书吊销由管理员和证书已过期的情况下的 TLS 句柄到期时间。 在此方案中，用户可以仍连接到网络 NPS 是否未过期的缓存的 TLS 句柄。 减少了 TLS 句柄到期可能有助于阻止此类具有吊销的证书的用户重新连接。

>[!NOTE]
>此方案的最佳解决方案是若要禁用 Active Directory 中的用户帐户或从被授予连接到网络策略中的网络权限的 Active Directory 组中删除用户帐户。 这些更改传播到所有域控制器可能还会延迟，但是，由于复制延迟。 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>在客户端计算机上配置 TLS 句柄到期时间

可以使用此过程更改客户端计算机缓存 NPS 为 TLS 句柄的时间量。 身份验证成功后 NPS，客户端计算机缓存 TLS 句柄作为 NPS 的 TLS 连接属性。 TLS 句柄具有默认持续时间为 10 小时\(36,000,000 毫秒\)。 可以增加或减少的 TLS 句柄到期时间，方法是使用以下过程。

中的成员身份**管理员**，或等效身份是完成此过程所需的最小。

>[!IMPORTANT]
>必须在 NPS 中，不在客户端计算机上执行此过程。

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>若要配置 TLS 处理客户端计算机上的到期时间

1. 在 NPS 上打开注册表编辑器。

2. 浏览到注册表项**HKEY\_本地\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 上**编辑**菜单上，单击**新建**，然后单击**密钥**。

4. 类型**ClientCacheTime**，然后按 ENTER。

5. 右键单击**ClientCacheTime**，单击**新建**，然后单击**DWORD （32 位） 值**。

6. 键入的时间，以毫秒为单位，您想要客户端计算机后由 NPS 尝试的第一个成功的身份验证缓存 TLS 句柄的 NPS。

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>在 NPSs 配置 TLS 句柄到期时间

使用此过程更改 NPSs 缓存客户端计算机的 TLS 句柄的时间量。 身份验证成功后访问客户端，NPSs TLS 句柄作为缓存客户端计算机的 TLS 连接属性。 TLS 句柄具有默认持续时间为 10 小时\(36,000,000 毫秒\)。 可以增加或减少的 TLS 句柄到期时间，方法是使用以下过程。

中的成员身份**管理员**，或等效身份是完成此过程所需的最小。

>[!IMPORTANT]
>必须在 NPS 中，不在客户端计算机上执行此过程。

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>若要配置 TLS 处理 NPSs 到期时间

1. 在 NPS 上打开注册表编辑器。

2. 浏览到注册表项**HKEY\_本地\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. 上**编辑**菜单上，单击**新建**，然后单击**密钥**。

4. 类型**ServerCacheTime**，然后按 ENTER。

5. 右键单击**ServerCacheTime**，单击**新建**，然后单击**DWORD （32 位） 值**。

6. 键入的时间，以毫秒为单位，你想 NPSs 后首次成功身份验证尝试的客户端缓存的客户端计算机的 TLS 句柄。

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>获取受信任的根 CA 证书的 sha-1 哈希

使用此过程以从本地计算机安装的证书获取安全哈希算法 (sha-1) 的受信任的根证书颁发机构 (CA) 的哈希。 在某些情况下，例如部署组策略时，必须使用证书的 sha-1 哈希指定证书。

使用组策略，可以指定一个或多个客户端必须使用以在使用 EAP 或 PEAP 的相互身份验证的过程中执行身份验证 NPS 的受信任的根 CA 证书。 若要指定客户端必须用于验证服务器证书的受信任的根 CA 证书，可以输入证书的 sha-1 哈希。

此过程说明如何通过使用证书 Microsoft 管理控制台 (MMC) 管理单元中获取 sha-1 的受信任的根 CA 证书的哈希值。 

若要完成此过程，您必须**用户**组在本地计算机上。

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>若要获取的受信任的根 CA 证书哈希的 SHA-1

1. 在运行对话框或 Windows PowerShell 中，键入**mmc**，然后按 ENTER。 Microsoft 管理控制台\(MMC\)随即打开。 在 MMC 中，单击**文件**，然后单击**添加/删除 Snap\in**。 **添加或删除管理单元**对话框随即打开。

2. 在中**添加或删除管理单元**，在**可用的管理单元**，双击**证书**。 证书管理单元中向导随即打开。 单击**计算机帐户**，然后单击**下一步**。

3. 中**选择计算机**，，请确保**本地计算机 （运行此控制台的计算机）** 为选中状态，单击**完成**，然后单击**确定**.

4. 在左窗格中，双击**证书 （本地计算机）**，然后双击**受信任的根证书颁发机构**文件夹。

5. **证书**文件夹是子文件夹的**受信任的根证书颁发机构**文件夹。 单击**证书**文件夹。

6. 在细节窗格中，浏览到受信任的根 CA 的证书。 双击该证书。 **证书**对话框随即打开。

7. 在中**证书**对话框中，单击**详细信息**选项卡。

8. 在字段列表中，向下滚动到并选择**指纹**。

9. 在下部窗格中，显示你的证书的 sha-1 哈希的十六进制字符串。 选择 sha-1 哈希，，然后复制命令按 Windows 键盘快捷方式\(CTRL\+C\)将哈希值复制到 Windows 剪贴板。

10. 打开你想要粘贴的 sha-1 哈希，正确定位光标，，然后按的粘贴命令的 Windows 键盘快捷方式的位置\(CTRL\+V\)。 

有关证书和 NPS 的详细信息，请参阅[为 PEAP 和 EAP 要求配置证书模板](nps-manage-cert-requirements.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。
