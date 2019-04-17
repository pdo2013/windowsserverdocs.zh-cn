---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: "设备注册技术参考"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fac6437e9b6c3893064769a8279c2cf96cbc47d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2

# <a name="device-registration-technical-reference"></a>设备注册技术参考
设备注册服务 \(DRS\) 是一种新的 Windows 服务，包含在 Windows Server 2012 R2 上 Active Directory 联合身份验证服务角色。  必须安装并配置上的所有广告 FS 场中联盟服务器 DRS。  有关部署 DRS 的信息，请参阅[与设备注册服务配置联合服务器](https://technet.microsoft.com/library/dn486831.aspx)。  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>已注册设备时创建的 Active Directory 对象  
以下 Active Directory 对象将创建作为设备注册服务的一部分。  
  
### <a name="device-registration-configuration"></a>注册设备  
注册设备配置存储在 Active Directory 森林配置命名上下文。 \ (例如，**CN\ = 设备注册配置，CN\ = 服务 < configuration\ naming\ 上下文 >**\)。 此对象时 Active Directory 森林为设备注册 initialed 创建。  
  
注册设备包括的以下元素：  
  
-   **发行商键**  
  
    公开并专用键用于发出 X.509 证书已注册设备与相关联。  专用的键可以使用 DKM 保护。  
  
-   **设备注册服务配置**  
  
    相关的设备注册服务策略。  
  
### <a name="registered-devices-container"></a>已注册的设备容器  
设备对象容器创建下其中一个域的 Active Directory 树林中。  此对象容器将包含的所有设备对象的 Active Directory 森林。  
  
默认情况下，容器广告 FS 相同的域中创建。  \ (例如，**CN\ = RegisteredDevices，DC\ = < default\ naming\ 上下文 >**\)。此对象时 Active Directory 森林为设备注册 initialed 创建。  
  
### <a name="registered-devices"></a>已注册的设备  
设备对象有新的浅色重量 Active Directory 对象。  使用它们来表示之间的关系：用户、设备和公司。  设备对象使用由广告 FS 签名证书定位物理中 Active Directory 的逻辑设备对象的设备。  
  
已注册的设备包括的以下元素：  
  
-   **显示名称**  
  
    设备的友好名称。  对于 windows 设备，这是主计算机的名称。  
  
-   **设备 Id**  
  
    通过注册设备 server 生成一个 GUID。  
  
-   **证书指纹**  
  
    证书指纹可与已注册设备的 X.509 证书。  
  
-   **操作系统类型**  
  
    在设备上操作系统类型。  
  
-   **OS 版本**  
  
    在设备上的操作系统版本。  
  
-   **启用**  
  
    指示是否设备已启用中 Active Directory 的布尔值。  允许访问的服务已启用的设备。  
  
-   **大致上次使用时间**  
  
    设备用于访问资源的大致时间。  若要限制复制通信，这将仅更新 14 天一次。  
  
-   **注册的所有者**  
  
    加入工作区此设备的用户身份安全 \(SID\)。  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>广告 FS\/DRS 服务器 SSL 证书吊销检查  
加入工作区客户端检查广告 FS 服务器 SSL 证书的有效性。  广告 FS 服务器 SSL 证书包括证书吊销列表 \(CRL\) 端点，如果客户端必须是能够访问指定以验证的证书端点。  
  
如果你使用测试环境和测试证书颁发机构 \(CA\) 处理服务器 SSL 证书，然后你可以选择不包括您 CA 颁发 server 证书 CRL 端点。  执行此操作将允许工作区加入客户端忽略 CRL 检查。  
  
> [!CAUTION]  
> 永远不建议这样做生产系统  
  

