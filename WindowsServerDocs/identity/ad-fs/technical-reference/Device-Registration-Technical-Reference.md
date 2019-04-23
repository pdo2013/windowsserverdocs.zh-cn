---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: 设备注册技术参考
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fac6437e9b6c3893064769a8279c2cf96cbc47d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833778"
---
>适用于：Windows Server 2016, Windows Server 2012 R2

# <a name="device-registration-technical-reference"></a>设备注册技术参考
设备注册服务\(DRS\)是一种新的 Windows 服务，随 Windows Server 2012 R2 上的 Active Directory 联合身份验证服务角色。  DRS 必须在 AD FS 场中的所有联合服务器上进行安装和配置。  有关部署 DRS 的信息，请参阅 [使用设备注册服务配置联合务器](https://technet.microsoft.com/library/dn486831.aspx)。  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>注册设备时创建的 Active Directory 对象  
以下 Active Directory 对象作为设备注册服务的一部分进行创建。  
  
### <a name="device-registration-configuration"></a>设备注册配置  
设备注册配置存储在 Active Directory 林的配置命名上下文中。 \(例如， **CN\=Device Registration Configuration，CN\=服务，< 配置\-命名\-上下文 >**\)。 此对象在为设备注册启动 Active Directory 林时进行创建。  
  
设备注册配置包括以下元素：  
  
-   **颁发者密钥**  
  
    用于颁发与注册的设备关联的 X.509 证书的公钥和私钥。  私钥受 DKM 保护。  
  
-   **设备注册服务配置**  
  
    与设备注册服务相关的策略。  
  
### <a name="registered-devices-container"></a>注册的设备容器  
设备对象容器在 Active Directory 林中的一个域下创建。  此对象容器会包含 Active Directory 林的所有设备对象。  
  
默认情况下，该容器在与 AD FS 相同的域中创建。  \(例如， **CN\=RegisteredDevices，DC\=< 默认\-命名\-上下文 >**\)。此对象被创建时为设备注册启动 Active Directory 林。  
  
### <a name="registered-devices"></a>注册的设备  
设备对象是 Active Directory 中新的轻型对象。  它们用于表示用户、设备和公司之间的关系。  设备对象使用由 AD FS 签名的证书将物理设备定位到 Active Directory 中的逻辑设备对象。  
  
注册的设备包括以下元素：  
  
-   **显示名称**  
  
    设备的友好名称。  对于 Windows 设备，这是计算机的主机名。  
  
-   **设备 Id**  
  
    由设备注册服务器生成的 GUID。  
  
-   **证书指纹**  
  
    用于注册的设备的 X.509 证书的证书指纹。  
  
-   **OS 类型**  
  
    设备上的操作系统类型。  
  
-   **OS 版本**  
  
    设备上的操作系统版本。  
  
-   **已启用**  
  
    一个布尔值，指示是否在 Active Directory 中启用了设备。  仅允许启用的设备访问服务。  
  
-   **近似的上次使用时间**  
  
    设备用于访问资源的近似时间。  为了限制复制流量，这仅仅每 14 天更新一次。  
  
-   **已注册的所有者**  
  
    安全标识\(SID\)此设备加入工作区的用户。  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>AD FS\/DRS 服务器 SSL 证书吊销检查  
工作区加入客户端会检查 AD FS 服务器 SSL 证书的有效性。  如果 AD FS 服务器 SSL 证书包含证书吊销列表\(CRL\)终结点，客户端必须能够访问指定用于验证证书的终结点。  
  
如果您将测试环境和测试证书颁发机构\(CA\)来颁发服务器 SSL 证书，则可以选择不由 CA 颁发的服务器证书中包含 CRL 终结点。  这样做会允许工作区加入客户端绕过 CRL 检查。  
  
> [!CAUTION]  
> 绝不建议对生产系统这样做  
  

