---
title: 管理传输层安全性 (TLS)
description: Windows Server 安全
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
ms.date: 05/16/2018
ms.openlocfilehash: 872647f09898bf8ae08ee69f28b717d28abf7c78
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447303"
---
# <a name="manage-transport-layer-security-tls"></a>管理传输层安全性 (TLS)

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>配置 TLS 密码套件顺序

不同 Windows 版本支持不同的 TLS 密码套件和优先级顺序。 请参阅[TLS/SSL (Schannel SSP) 中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)有关支持的不同 Windows 版本中的 Microsoft Schannel 提供程序的默认顺序。

> [!NOTE] 
> 此外可以修改列表使用 CNG 函数的密码套件，请参阅[Prioritizing Schannel 密码套件](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)有关详细信息。

对 TLS 密码套件顺序的更改将在下一步启动生效。 直到重新启动或关机，现有的订单将生效。

> [!WARNING] 
> 更新默认优先级排序的注册表设置不受支持，可能会重置与维护服务更新。 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>通过使用组策略配置 TLS 密码套件顺序

可以使用的 SSL 密码套件顺序组策略设置来配置默认 TLS 密码套件顺序。

1. 从组策略管理控制台中，转到**计算机配置** > **管理模板** > **网络** >  **SSL 配置设置**。
2. 双击**SSL 密码套件顺序**，然后单击**已启用**选项。
3. 右键单击**SSL 密码套件**框，然后选择**全**从弹出菜单。

   ![组策略设置](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. 右键单击所选的文本，然后选择**复制**从弹出菜单。
5. 将文本粘贴到文本编辑器，如 notepad.exe 和更新与新的密码套件顺序列表。

   > [!NOTE]
   > TLS 密码套件顺序列表必须采用严格的逗号分隔格式。 每个密码套件字符串将用到它的右侧逗号 （，） 结束。 
   > 
   > 此外，密码套件列表被限制为 1023 个字符。

6. 替换中的列表**SSL 密码套件**与更新的排序列表。
7. 单击“确定”  或“应用”  。

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>通过使用 MDM 配置 TLS 密码套件顺序

Windows 10 策略 CSP 支持 TLS 密码套件的配置。 请参阅[加密/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites)有关详细信息。

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>通过使用 TLS PowerShell Cmdlet 配置 TLS 密码套件顺序

TLS PowerShell 模块支持获取 TLS 密码套件的有序列的表中，禁用密码套件，并启用密码套件。 请参阅[TLS 模块](https://technet.microsoft.com/itpro/powershell/windows/tls/tls)有关详细信息。

## <a name="configuring-tls-ecc-curve-order"></a>配置 TLS ECC 曲线顺序 

自 Windows 10 和 Windows Server 2016 起，ECC 曲线顺序可以配置独立的密码套件顺序。 如果 TLS 密码套件顺序列表了椭圆曲线后缀，它们将被重写的新的椭圆曲线优先级顺序，启用时。 这使组织能够使用组策略对象配置具有相同的密码套件顺序的不同版本的 Windows。

> [!NOTE]
> 在 Windows 10 之前已追加确定曲线优先级的椭圆曲线密码套件字符串。

### <a name="managing-windows-ecc-curves-using-certutil"></a>管理 Windows ECC 曲线使用 CertUtil

从 Windows 10 和 Windows Server 2016 开始，Windows 提供了通过命令行实用程序 certutil.exe 椭圆曲线参数管理。 椭圆曲线参数存储在 bcryptprimitives.dll。 使用 certutil.exe，管理员可以添加和删除的曲线参数与 Windows，分别。 Certutil.exe 在注册表中安全地存储的曲线参数。 Windows 可以开始使用曲线参数的曲线与关联的名称。    

#### <a name="displaying-registered-curves"></a>显示已注册的曲线

使用以下 certutil.exe 命令显示为当前计算机注册的曲线的列表。

```powershell
certutil.exe –displayEccCurve
```

![Certutil 显示曲线](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*图 1 Certutil.exe 输出以显示已注册的曲线的列表。*

#### <a name="adding-a-new-curve"></a>添加新的曲线

组织可以创建和使用由其他受信任实体研究的曲线参数。  
管理员想要在 Windows 中使用这些新曲线必须添加曲线。  
使用以下 certutil.exe 命令添加到当前计算机的一条曲线：

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- **CurveName**参数表示的曲线的曲线参数已添加在其下的名称。
- **CurveParameters**参数表示包含你想要添加的曲线参数的证书的文件名。
- **CurveOid**参数表示包含 OID 的曲线参数 （可选） 要添加的证书的文件名。
- **CurveType**自变量表示十进制值为从已命名的曲线[EC 名为曲线注册表](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8)（可选）。

![Certutil 添加曲线](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*图 2 添加使用 certutil.exe 的曲线。*

#### <a name="removing-a-previously-added-curve"></a>删除以前添加的曲线

管理员可以删除以前添加的曲线，使用以下 certutil.exe 命令：

```powershell
Certutil.exe –deleteEccCurve curveName
```

管理员从计算机中删除该曲线后，Windows 不能使用已命名的曲线。

## <a name="managing-windows-ecc-curves-using-group-policy"></a>使用组策略管理 Windows ECC 曲线

组织可以将分发到 enterprise，已加入域的计算机使用组策略和组策略首选项注册扩展的曲线参数。  
用于分发一条曲线的过程是：

1.  在 Windows 10 和 Windows Server 2016 上，使用**certutil.exe**向 Windows 添加新的已注册已命名的曲线。
2.  从该同一台计算机，打开组策略管理控制台 (GPMC)、 创建新的组策略对象，并对其进行编辑。
3.  导航到**计算机配置 |首选项 |Windows 设置 |注册表**。  右键单击**注册表**。 将鼠标悬停**新建**，然后选择**收集项**。 重命名收集项以匹配该曲线的名称。 你将创建一个注册表收集项下每个注册表项*HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*。
4.  添加一个新的配置新创建的组策略首选项注册表收集**注册表项**对于每个注册表值下列出*HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\[curveName]* 。
5.  部署包含到应接收新的已命名的曲线的 Windows 10 和 Windows Server 2016 计算机的组策略注册表收集项的组策略对象。

    ![GPP 分发曲线](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *图 3 使用组策略首选项将曲线*

## <a name="managing-tls-ecc-order"></a>管理 TLS ECC 顺序

从 Windows 10 和 Windows Server 2016 开始，可以使用设置的 ECC 曲线顺序组策略配置默认 TLS ECC 曲线顺序。 使用泛型 ECC 和此设置，组织可以添加其自己受信任的已命名曲线 （被批准用于 TLS） 的操作系统以及如何对曲线优先级组策略设置，以确保将来 TLS 中使用它们将这些已命名的曲线握手。 新的曲线优先级列表上处于活动状态后接收的策略设置的下一步重启。     

![GPP 分发曲线](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*图 4 管理 TLS 曲线优先级使用组策略*


