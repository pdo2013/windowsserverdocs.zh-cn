---
title: 根据 Windows 激活错误代码解决问题
description: 了解如何排查激活错误代码问题
ms.topic: article
ms.date: 9/18/2019
ms.technology: server-general
ms.assetid: ''
author: kaushika-msft
ms.author: kaushika-msft; v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 26b107264c9dfaca16ef445760089b8ac0ae8e22
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960972"
---
# <a name="resolve-windows-activation-error-codes"></a>根据 Windows 激活错误代码解决问题

> **家庭用户**  
> 本文主要面向支持专员和 IT 专业人员。 如果你在寻找有关 Windows 激活错误消息的详细信息，请参阅[获取有关 Windows 激活错误的帮助](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors)。  

本文提供的故障排除信息有助于你响应错误消息，这些消息是当你尝试使用多次激活密钥 (MAK) 或密钥管理服务 (KMS) 在一台或多台基于 Windows 的计算机上执行批量激活时可能会收到的。 请查看下表中的错误代码，然后选择相应的链接来查看有关该错误代码的详细信息及其解决方法。

有关批量激活的详细信息，请参阅[规划批量激活](https://docs.microsoft.com/en-us/windows/deployment/volume-activation/plan-for-volume-activation-client)。

有关 Windows 当前版本和最新版本的批量激活的详细信息，请参阅[批量激活 [客户端]](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-windows-10)。

若要详细了解旧版 Windows 的批量激活，请参阅 KB 929712： [Volume Activation information for Windows Vista, Windows Server 2008, Windows Server 2008 R2 and Windows 7](https://support.microsoft.com/en-us/help/929712/volume-activation-information-for-windows-vista-windows-server-2008-wi)（Windows Vista、Windows Server 2008、Windows Server 2008 R2 和 Windows 7 的批量激活信息）。

## <a name="summary-of-error-codes"></a>错误代码摘要

|错误代码 |错误消息 |激活&nbsp;类型|
|-----------|--------------|----------------|
|[0x8004FE21](#0x8004fe21-this-computer-is-not-running-genuine-windows) |此计算机未运行正版 Windows。  |MAK<br />KMS 客户端 |
|[0x80070005](#0x80070005-access-denied) |拒绝访问。 所请求的操作需要提升特权。 |MAK<br />KMS 客户端<br />KMS 主机 |
|[0x8007007b](#0x8007007b-dns-name-does-not-exist) |0x8007007b DNS 名称不存在。 |KMS 客户端 |
|[0x80070490](#0x80070490-the-product-key-you-entered-didnt-work) |你输入的产品密钥无效。 请核对该产品密钥后再试一次，或输入其他产品密钥。 |MAK |
|[0x800706BA](#0x800706ba-the-rpc-server-is-unavailable) |RPC 服务器不可用。 |KMS 客户端 |
|[0x8007232A](#0x8007232a-dns-server-failure) |DNS 服务器失败。  |KMS 主机  |
|[0x8007232B](#0x8007232b-dns-name-does-not-exist) |DNS 名称不存在。 |KMS 客户端 |
|[0x8007251D](#0x8007251d-no-records-found-for-dns-query) |找不到 DNS 查询的记录。 |KMS 客户端 |
|[0x80092328](#0x80092328-dns-name-does-not-exist) |DNS 名称不存在。  |KMS 客户端 |
|[0xC004B100](#0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated) |激活服务器确定无法激活计算机。 |MAK |
|[0xC004C001](#0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid) |激活服务器确定指定的产品密钥无效 |MAK|
|[0xC004C003](#0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked) |激活服务器确定指定的产品密钥被阻止。 |MAK |
|[0xC004C008](#0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used) |激活服务器确定无法使用指定的产品密钥。 |KMS |
|[0xC004C020](#0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit) |激活服务器报告多次激活密钥已超过其限制。 |MAK |
|[0xC004C021](#0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded) |激活服务器报告已超过多次激活密钥扩展限制。 |MAK |
|[0xC004F009](#0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired) |软件保护服务报告宽限期已过期。 |MAK |
|[0xC004F00F](#0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance) |软件授权服务器报告硬件 ID 绑定已超出容限级别。 |MAK<br />KMS 客户端<br />KMS 主机 |
|[0xC004F014](#0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available) |软件保护服务报告产品密钥不可用 |MAK<br />KMS 客户端 |
|[0xC004F02C](#0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect) |软件保护服务报告脱机激活数据的格式不正确。 |MAK<br />KMS 客户端 |
|[0xC004F035](#0xc004f035-invalid-volume-license-key) |软件保护服务报告无法使用批量许可证产品密钥激活该计算机。 |KMS 客户端<br />KMS 主机 |
|[0xC004F038](#0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient) |软件保护服务报告无法激活计算机。 你的密钥管理服务(KMS)报告的计数不足。 请联系你的系统管理员。 |KMS 客户端 |
|[0xC004F039](#0xc004f039-the-key-management-service-kms-is-not-enabled) |软件保护服务报告无法激活计算机。 未启用密钥管理服务(KMS)。 |KMS 客户端 |
|[0xC004F041](#0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated) |软件保护服务确定未激活密钥管理服务器(KMS)。 需要激活 KMS。  |KMS 客户端 |
|[0xC004F042](#0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used) |软件保护服务确定无法使用指定的密钥管理服务(KMS)。 |KMS 客户端 |
|[0xC004F050](#0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid) |软件保护服务报告产品密钥无效。 |MAK<br />KMS<br />KMS 客户端 |
|[0xC004F051](#0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked) |软件保护服务报告产品密钥被阻止。 |MAK<br />KMS |
|[0xC004F064](#0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired) |软件保护服务报告非正版宽限期已过。 |MAK |
|[0xC004F065](#0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period) |软件保护服务报告该应用程序正在有效的非正版宽限期内运行。 |MAK<br />KMS 客户端 |
|[0xC004F06C](#0xc004f06c-the-request-timestamp-is-invalid) |软件保护服务报告无法激活计算机。 密钥管理服务(KMS)确定请求时间戳无效。  |KMS 客户端 |
|[0xC004F074](#0xc004f074-no-key-management-service-kms-could-be-contacted) |软件保护服务报告无法激活计算机。 无法联系任何密钥管理服务(KMS)。 有关其他信息，请参阅应用程序事件日志。  |KMS 客户端 |

## <a name="causes-and-resolutions"></a>原因和解决方法

### <a name="0x8004fe21-this-computer-is-not-running-genuine-windows"></a>0x8004FE21 此计算机未运行正版 Windows  

#### <a name="possible-cause"></a>可能的原因

多种原因可能导致此问题。 最可能的原因是安装语言包 (MUI) 的计算机运行的 Windows 版本未获得安装其他语言包的许可。  

> [!NOTE]
> 此问题不一定表示发生了篡改。 某些应用程序可以安装多语言支持，即使该 Windows 版本未获安装这些语言包的许可。  

如果 Windows 已被恶意软件修改为允许安装其他功能，也可能出现此问题。 如果某些系统文件已损坏，也可能出现此问题。  

#### <a name="resolution"></a>分辨率

若要解决此问题，必须重新安装操作系统。  

### <a name="0x80070005-access-denied"></a>0x80070005 访问被拒绝

此错误消息的完整文本如下所示：

> 拒绝访问。 所请求的操作需要提升特权。

#### <a name="possible-cause"></a>可能的原因

用户帐户控制 (UAC) 阻止激活进程在非提升的命令提示符窗口中运行。  

#### <a name="resolution"></a>分辨率

在提升的命令提示符窗口中运行 **slmgr.vbs**。 为此，请在“开始”  菜单中右键单击“cmd.exe”  ，然后选择“以管理员身份运行”  。  

### <a name="0x8007007b-dns-name-does-not-exist"></a>0x8007007b DNS 名称不存在

#### <a name="possible-cause"></a>可能的原因

如果 KMS 客户端在 DNS 中找不到 KMS SRV 资源记录，则可能出现此问题。  

#### <a name="resolution"></a>分辨率

若要详细了解如何排查此类与 DNS 相关的问题，请参阅 [Common troubleshooting procedures for KMS and DNS issues](common-troubleshooting-procedures-kms-dns.md)（KMS 和 DNS 问题的常见排查过程）。  

### <a name="0x80070490-the-product-key-you-entered-didnt-work"></a>0x80070490 你输入的产品密钥无效

此错误的完整文本如下所示：
> 你输入的产品密钥无效。 请核对该产品密钥后再试一次，或输入其他产品密钥。  

#### <a name="possible-cause"></a>可能的原因

输入的 MAK 无效或 Windows Server 2019 中存在已知问题会导致此问题。  

#### <a name="resolution"></a>分辨率

若要解决此问题并激活计算机，请在提升的命令提示符处运行 **slmgr -ipk <5x5 key>** 。

### <a name="0x800706ba-the-rpc-server-is-unavailable"></a>0x800706BA RPC 服务器不可用

#### <a name="possible-cause"></a>可能的原因

未在 KMS 主机上配置防火墙设置，或者 DNS SRV 记录已过时。  

#### <a name="resolution"></a>分辨率

在 KMS 主机上，确保已启用密钥管理服务（TCP 端口 1688）防火墙例外。

确保 DNS SRV 记录指向有效的 KMS 主机。 

排查网络连接问题。  

若要详细了解如何排查此类与 DNS 相关的问题，请参阅 [Common troubleshooting procedures for KMS and DNS issues](common-troubleshooting-procedures-kms-dns.md)（KMS 和 DNS 问题的常见排查过程）。  

### <a name="0x8007232a-dns-server-failure"></a>0x8007232A DNS 服务器故障

#### <a name="possible-cause"></a>可能的原因

系统出现网络或 DNS 问题。

#### <a name="resolution"></a>分辨率

排查网络和 DNS 的问题。  

### <a name="0x8007232b-dns-name-does-not-exist"></a>0x8007232B DNS 名称不存在

#### <a name="possible-cause"></a>可能的原因

KMS 客户端在 DNS 中找不到 KMS 服务器资源记录 (SRV RR)。  

#### <a name="resolution"></a>分辨率

确认已安装 KMS 主机，并且已启用 DNS 发布（默认设置）。 如果 DNS 不可用，请使用 **slmgr.vbs /skms <*kms_host_name*>** 将 KMS 客户端指向 KMS 主机。  

如果没有 KMS 主机，请获取并安装 MAK。 然后，激活系统。

若要详细了解如何排查此类与 DNS 相关的问题，请参阅 [Common troubleshooting procedures for KMS and DNS issues](common-troubleshooting-procedures-kms-dns.md)（KMS 和 DNS 问题的常见排查过程）。  

### <a name="0x8007251d-no-records-found-for-dns-query"></a>0x8007251D 找不到 DNS 查询的记录

#### <a name="possible-cause"></a>可能的原因

KMS 客户端在 DNS 中找不到 KMS SRV 记录。

#### <a name="resolution"></a>分辨率

排查网络连接和 DNS 的问题。 若要详细了解如何排查此类与 DNS 相关的问题，请参阅 [Common troubleshooting procedures for KMS and DNS issues](common-troubleshooting-procedures-kms-dns.md)（KMS 和 DNS 问题的常见排查过程）。  

### <a name="0x80092328-dns-name-does-not-exist"></a>0x80092328 DNS 名称不存在

#### <a name="possible-cause"></a>可能的原因

如果 KMS 客户端在 DNS 中找不到 KMS SRV 资源记录，则可能出现此问题。

#### <a name="resolution"></a>分辨率

若要详细了解如何排查此类与 DNS 相关的问题，请参阅 [Common troubleshooting procedures for KMS and DNS issues](common-troubleshooting-procedures-kms-dns.md)（KMS 和 DNS 问题的常见排查过程）。  

### <a name="0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated"></a>0xC004B100 激活服务器确定无法激活计算机

#### <a name="possible-cause"></a>可能的原因

不支持 MAK。  

#### <a name="resolution"></a>分辨率

若要排查此问题，请确认使用的 MAK 是 Microsoft 提供的 MAK。 若要确认 MAK 有效，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。

### <a name="0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid"></a>0xC004C001 激活服务器确定指定的产品密钥无效

#### <a name="possible-cause"></a>可能的原因

输入的 MAK 无效。

#### <a name="resolution"></a>分辨率

确认该密钥是 Microsoft 提供的 MAK。 如需其他帮助，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。

### <a name="0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked"></a>0xC004C003 激活服务器确定指定的产品密钥被阻止

#### <a name="possible-cause"></a>可能的原因

MAK 在激活服务器上被阻止。

#### <a name="resolution"></a>分辨率

若要获取新 MAK，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。 获取新 MAK 后，请再次尝试安装并激活 Windows。  

### <a name="0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used"></a>0xC004C008 激活服务器确定无法使用指定的产品密钥

#### <a name="possible-cause"></a>可能的原因

KMS 密钥已超过其激活限制。 KMS 主机密钥最多可以在 6 台不同的计算机上激活 10 次。  

#### <a name="resolution"></a>分辨率

如果需要激活更多次，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。  

### <a name="0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit"></a>0xC004C020 激活服务器报告多次激活密钥已超过其限制

#### <a name="possible-cause"></a>可能的原因

MAK 已超过其激活限制。 根据设计，MAK 可以激活的次数有限。

#### <a name="resolution"></a>分辨率

如果需要激活更多次，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。

### <a name="0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded"></a>0xC004C021 激活服务器报告已超过多次激活密钥扩展限制

#### <a name="possible-cause"></a>可能的原因

MAK 已超过其激活限制。 根据设计，MAK 的激活次数有限。

#### <a name="resolution"></a>分辨率

如果需要激活更多次，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。

### <a name="0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired"></a>0xC004F009 软件保护服务报告宽限期已过

#### <a name="possible-cause"></a>可能的原因

在激活系统之前宽限期已过期。 现在，系统处于“通知”状态。  

#### <a name="resolution"></a>分辨率

如需帮助，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。

### <a name="0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance"></a>0xC004F00F 软件授权服务器报告硬件 ID 绑定已超出容限级别

#### <a name="possible-cause"></a>可能的原因

系统上的硬件已更换，或者驱动程序已更新。  

#### <a name="resolution"></a>分辨率

如果使用 MAK 激活，请在 OOT 宽限期内使用联机激活或电话激活来重新激活系统。  

如果使用 KMS 激活，请重启 Windows 或运行 **slmgr.vbs /ato**。

### <a name="0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available"></a>0xC004F014 软件保护服务报告产品密钥不可用

#### <a name="possible-cause"></a>可能的原因

系统上未安装产品密钥。  

#### <a name="resolution"></a>分辨率

如果使用 MAK 激活，请安装 MAK 产品密钥。 

如果使用 KMS 激活，请查看 Pid.txt 文件（位于安装介质的 \sources 文件夹中）中是否有 KMS 安装密钥。 安装该密钥。

### <a name="0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect"></a>0xC004F02C 软件保护服务报告脱机激活数据的格式不正确

#### <a name="possible-cause"></a>可能的原因

系统检测到电话激活期间输入的数据无效。  

#### <a name="resolution"></a>分辨率

确认已正确输入 CID。  

### <a name="0xc004f035-invalid-volume-license-key"></a>0xC004F035 批量许可证密钥无效

此错误消息的完整文本如下所示：

> 错误：批量许可证密钥无效。 若要激活，需要将产品密钥更改为有效的多次激活密钥(MAK)或零售密钥。 你必须有合格的操作系统许可证和批量许可 Windows 7 升级许可证，或者由零售商提供的 Windows 7 完全许可证。 其他任何安装此软件的行为均违反你的协议和适用的著作权法。  

错误文本是正确的，但不明确。 此错误表明计算机的 BIOS 中缺少 Windows 标记，该标记将其标识为运行符合条件版本的 Windows 的 OEM 系统。 该信息是 KMS 客户端激活所需的。 此代码的更具体含义是“错误：批量许可证密钥无效”

#### <a name="possible-cause"></a>可能的原因

Windows 7 批量版仅获得升级许可。 Microsoft 不支持在未安装合格操作系统的计算机上安装批量版操作系统。  

#### <a name="resolution"></a>分辨率

若要激活，需要执行下列操作之一：

- 将产品密钥更改为有效的多次激活密钥(MAK)或零售密钥。 你必须有合格的操作系统许可证和批量许可 Windows 7 升级许可证，或者由零售商提供的 Windows 7 完全许可证。
  > [!NOTE]
  > 如果在尝试激活时收到错误 0x80072ee2，请改用后续的电话激活方法。
- 按照这些步骤操作，通过电话激活：
   1. 运行 slmgr /dti，然后记录安装 ID 的值  。 </li>
   1. 请联系 [Microsoft 许可证激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)并提供安装 ID 以接收确认 ID。</li>
   1. 若要通过使用确认 ID 激活，请运行 slmgr /atp &lt;Confirmation ID&gt;  。

### <a name="0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient"></a>0xC004F038 你的密钥管理服务(KMS)报告的计数不足

此错误消息的完整文本如下所示：

> 软件保护服务报告无法激活计算机。 你的密钥管理服务(KMS)报告的计数不足。 请联系你的系统管理员。  

#### <a name="possible-cause"></a>可能的原因

KMS 主机上的计数不够大。 对于 Windows Server，KMS 计数必须大于或等于 5。 对于 Windows（客户端），KMS 计数必须大于或等于 25。  

#### <a name="resolution"></a>分辨率
使用 KMS 激活 Windows 之前，必须在 KMS 池中有更多的计算机。 若要获取 KMS 主机上的当前计数，请运行 **Slmgr.vbs /dli**。  

### <a name="0xc004f039-the-key-management-service-kms-is-not-enabled"></a>0xC004F039 未启用密钥管理服务(KMS)

此错误消息的完整文本如下所示：

> 软件保护服务报告无法激活计算机。 未启用密钥管理服务(KMS)。  

#### <a name="possible-cause"></a>可能的原因

KMS 未响应 KMS 请求。

#### <a name="resolution"></a>分辨率

排查 KMS 主机与客户端之间的网络连接问题。 确保 TCP 端口 1688（默认）未被防火墙阻止，也没有被其他方式筛选掉。

### <a name="0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated"></a>0xC004F041 软件保护服务确定未激活密钥管理服务器(KMS)

此错误消息的完整文本如下所示：

> 软件保护服务确定未激活密钥管理服务器(KMS)。 需要激活 KMS。  

#### <a name="possible-cause"></a>可能的原因

未激活 KMS 主机。  

#### <a name="resolution"></a>分辨率

使用联机激活或电话激活来激活 KMS 主机。  

### <a name="0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used"></a>0xC004F042 软件保护服务确定无法使用指定的密钥管理服务(KMS)

#### <a name="possible-cause"></a>可能的原因

如果 KMS 客户端联系一台无法激活客户端软件的 KMS 主机，则会出现此错误。 例如，在包含特定于应用程序和特定于操作系统的 KMS 主机的混合环境中，这种情况可能很常见。  

#### <a name="resolution"></a>分辨率

确保使用特定的 KMS 主机来激活特定的应用程序或操作系统，且 KMS 客户端连接到正确的主机。

### <a name="0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid"></a>0xC004F050 软件保护服务报告产品密钥无效

#### <a name="possible-cause"></a>可能的原因

这可能是由于 KMS 密钥拼写错误，或者在正式发行版操作系统中键入 Beta 密钥而造成的。  

#### <a name="resolution"></a>分辨率

请在相应版本的 Windows 上安装相应的 KMS 密钥。 检查拼写。 如果密钥是复制后粘贴过来的，请确保不要将密钥中的连字符取代为长破折号线。  

### <a name="0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked"></a>0xC004F051 软件保护服务报告产品密钥被阻止

#### <a name="possible-cause"></a>可能的原因

激活服务器确定 Microsoft 已阻止此产品密钥。  

#### <a name="resolution"></a>分辨率

获取新的 MAK 或 KMS 密钥，将它安装在系统上，然后激活。

### <a name="0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired"></a>0xC004F064 软件保护服务报告非正版宽限期已过

#### <a name="possible-cause"></a>可能的原因

Windows 激活工具 (WAT) 已确定系统不是正版。  

#### <a name="resolution"></a>分辨率

如需帮助，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。

### <a name="0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period"></a>0xC004F065 软件保护服务报告该应用程序正在有效的非正版宽限期内运行

#### <a name="possible-cause"></a>可能的原因

Windows 激活工具已确定系统不是正版。 系统将在非正版宽限期内继续运行。  

#### <a name="resolution"></a>分辨率

获取并安装正版产品密钥，并在宽限期内激活系统。 否则，系统将在宽限期结束时进入通知状态。

### <a name="0xc004f06c-the-request-timestamp-is-invalid"></a>0xC004F06C 请求时间戳无效

此错误消息的完整文本如下所示：

> 软件保护服务报告无法激活计算机。 密钥管理服务(KMS)确定请求时间戳无效。  

#### <a name="possible-cause"></a>可能的原因

客户端计算机上的系统时间与 KMS 主机上的时间有很大的差异。 由于多种原因，时间同步对于系统和网络的安全非常重要。  

#### <a name="resolution"></a>分辨率

请更改客户端上的系统时间，使其与 KMS 主机同步，以便解决此问题。 建议使用网络时间协议 (NTP) 时间源或 Active Directory 域服务进行时间同步。 出现当前的问题是因为你使用了 UTP 时间，而这种时间与选择的时区无关。  

### <a name="0xc004f074-no-key-management-service-kms-could-be-contacted"></a>0xC004F074 无法联系任何密钥管理服务(KMS)

此错误消息的完整文本如下所示：

> 软件保护服务报告无法激活计算机。 无法联系任何密钥管理服务(KMS)。 有关其他信息，请参阅应用程序事件日志。  

#### <a name="possible-cause"></a>可能的原因

所有 KMS 主机系统都返回了错误。  

#### <a name="resolution"></a>分辨率

在应用程序事件日志中，确定事件 ID 为 12288 且与激活尝试相关联的每个事件。 排查这些事件中的错误。

若要详细了解如何排查与 DNS 相关的问题，请参阅 [Common troubleshooting procedures for KMS and DNS issues](common-troubleshooting-procedures-kms-dns.md)（KMS 和 DNS 问题的常见排查过程）。  
