---
title: "管理传输层安全性 (TLS)"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 06/05/2017
ms.openlocfilehash: e26d1f833dbb7219251947f1cb3cb09def958aea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manage-transport-layer-security-tls"></a>管理传输层安全性 (TLS)

## <a name="configuring-tls-cipher-suite-order"></a>配置 TLS 密码 Suite 订单

不同的 Windows 版本支持不同 TLS 密码套件和优先顺序。 请参阅[TLS 月 SSL (Schannel SSP) 中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)为默认的顺序在不同的 Windows 版本中提供 Microsoft Schannel 程序支持。

> [!NOTE] 
> 你还可以修改列表密码套件使用 CNG 功能，请参阅[排列优先顺序 Schannel 密码套件](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)有关详细信息。

更改 TLS 密码 suite 订单将在下一次启动生效。 重启或关机，直到现有订单将生效。

> [!WARNING] 
> 更新的注册表设置默认优先级订购不受支持，并可能会重置与服务更新。 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>使用组策略配置 TLS 密码 Suite 订单

你可以使用 SSL 密码套件订单组策略设置配置默认 TLS 密码 suite 订单。

1.  从组策略管理控制台中，转到**计算机配置** > **管理模板** > **网络** > **SSL 配置设置**。
2.  双击**SSL 密码套件订单**，然后单击**启用**选项。
3.  右键单击**SSL 密码套件**框中，选择**选择所有**从弹出菜单。

    ![组策略设置](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4.  右键单击所选的文本，然后选择**副本**从弹出菜单。
5.  粘贴到文本编辑器 notepad.exe 和更新的新密码套件订单列表如的文本。

    > [!NOTE]
    > 严格逗号的分隔格式必须 TLS 密码套件订单列表。 每个密码套件字符串大战用到它的右侧逗号 （，）。 

    > 此外，密码套件列表限于 1023 字符。

6.  更换的列表中**SSL 密码套件**有序更新的列表。
7.  单击**确定**或**应用**。

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>通过使用 MDM 配置 TLS 密码 Suite 订单

Windows 10 策略 CSP 支持 TLS 密码套件配置。 请参阅[加密/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites)详细信息。

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>通过使用 TLS PowerShell Cmdlet 配置 TLS 密码 Suite 订单

TLS PowerShell 模块支持获取 TLS 密码套件排序的列表、 禁用编码器套件，并启用编码器套件。 请参阅[TLS 模块](https://technet.microsoft.com/itpro/powershell/windows/tls/tls)详细信息。

## <a name="configuring-tls-ecc-curve-order"></a>配置 TLS ECC 曲线订单 

开始使用 Windows 10 和 Windows Server 2016，ECC 曲线订单可以配置独立于密码套件订单。 如果 TLS 密码列表具有椭圆曲线后缀套件订单，他们将覆盖新椭圆曲线优先顺序时启用。 这允许组织使用组策略对象使用同一密码套件订单配置不同版本的 Windows。

> [!NOTE]
> 在 Windows 10 之前已附加椭圆曲线来确定曲线优先级密码套件字符串。

### <a name="managing-windows-ecc-curves-using-certutil"></a>管理 Windows ECC 曲线使用 CertUtil

从 Windows 10 和 Windows Server 2016 年开始，Windows 提供了椭圆曲线参数管理尽管命令行实用程序 certuil.exe。 在 bcryptprimitives.dll 存储椭圆曲线参数。 使用 certutil.exe，管理员可以添加和删除曲线参数与 Windows，分别。 Certutil.exe 在注册表中安全地存储曲线参数。 Windows 可以通过开始使用曲线参数与曲线关联的名称。    

#### <a name="displaying-registered-curves"></a>显示注册的曲线

使用以下 certutil.exe 命令显示当前计算机注册曲线的列表。

```powershell
certutil.exe –displayEccCurve
```

![Certutil 显示曲线](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*输出显示列表中注册曲线图 1 Certutil.exe。*

#### <a name="adding-a-new-curve"></a>添加新曲线

创建和使用通过其他受信任的实体研究曲线参数组织。  
想要在 Windows 中使用这些新曲线管理员必须添加曲线。  
使用以下 certutil.exe 命令添加到当前计算机曲线：

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- **CurveName**参数表示的曲线下，曲线参数已添加的名称。
- **CurveParameters**参数表示包含你想要添加的曲线参数证书的文件名。
- **CurveOid**参数表示包含你想要添加 （可选） 曲线参数 OID 证书文件名。
- **CurveType**参数表示小从命名曲线的数值[EC 名为曲线注册表](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8)（可选）。

![Certutil 添加曲线](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*图 2 添加曲线使用 certutil.exe。*

#### <a name="removing-a-previously-added-curve"></a>删除以前添加的曲线

管理员可以删除之前添加了的曲线使用以下 certutil.exe 命令：

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows 才能使用命名的曲线后管理员从电脑中删除曲线。

## <a name="managing-windows-ecc-curves-using-group-policy"></a>使用组策略管理 Windows ECC 曲线

组织可以分配到企业版，已加入域的计算机使用组策略和组策略偏好注册表扩展曲线参数。  
分发曲线的过程介绍：

1.  在 Windows 10 和 Windows Server 2016 上，使用**certutil.exe**向 Windows 添加新的已注册命名的曲线。
2.  从那同一台打开组策略管理控制台 (GPMC)、 创建一个新的组策略对象，并进行编辑。
3.  导航到**计算机配置 |首选项 |Windows 设置 |注册表**。  右键单击**注册表**。 将鼠标悬停在**新建**选择**集锦项目**。 重命名曲线的名称匹配的集锦项。 你将创建一个注册表集锦项目下每注册表项*HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*。
4.  通过添加了新配置组策略的偏好随意选择注册表新建的集锦**注册表项**下列出的每个注册表值*HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\ [curveName]*。
5.  部署包含组策略注册表集合项目添加到 Windows 10 和 Windows Server 2016 应将接收新的命名的曲线的计算机的组策略对象。

    ![GPP 分配曲线](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *图 3 使用组策略首选项分配曲线*

## <a name="managing-tls-ecc-order"></a>管理 TLS ECC 订单

从 Windows 10 和 Windows Server 2016 年开始，可以使用设置 ECC 曲线订单组策略配置默认 TLS ECC 曲线订单。 使用通用 ECC 此设置，组织可以添加自己受信任的名为操作系统曲线 （即经批准可用于与 TLS 使用），并将这些名为曲线对曲线优先级组策略设置以确保它们按将来 TLS 握手使用。 新曲线优先级列表后收到的策略设置的下一步重启上生效。     

![GPP 分配曲线](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*图 4 管理 TLS 曲线优先级使用组策略*


