---
title: 排查激活错误代码问题
description: 了解如何排查激活错误代码问题
ms.topic: article
ms.date: 03/21/2019
ms.technology: server-general
ms.assetid: ''
author: kaushika-msft
ms.author: kaushika-msft
ms.localizationpriority: medium
ms.openlocfilehash: 076fc3408810ba7b0f51c6a428955f99a66dd394
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "64880875"
---
# <a name="troubleshooting-activation-error-codes"></a>排查激活错误代码问题

**家庭用户** 本文主要面向支持专员和 IT 专业人员。 如果你在寻找有关 Windows 激活错误消息的详细信息，请转到以下 Microsoft 网站：

[获取有关 Windows 激活错误的帮助](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors) 

当你尝试使用多次激活密钥 (MAK) 或密钥管理服务 (KMS) 在一台或多台基于 Windows 的计算机上执行批量激活时，你会收到包含特定错误代码的错误消息。 本文讨论如何排查这些错误。

**错误代码和描述**

|错误代码 |错误消息 |激活类型 |可能的原因 |疑难解答步骤 |
|-----------|--------------|----------------|---------------|----------------------|
|    0xC004C001    |    激活服务器确定指定的产品密钥无效    |    MAK    |    输入了无效的 MAK。    |    确认该密钥是 Microsoft 提供的 MAK。 有关激活受阻的问题，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)     |
|    0xC004C003     |    激活服务器确定指定的产品密钥被阻止    |    MAK    |    MAK 在激活服务器上被阻止。    |    联系：[Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)以获取新的 MAK，然后安装/激活系统。    |
|    0xC004C008     |    激活服务器确定无法使用指定的产品密钥。    |    KMS    |    KMS 密钥已超过激活限制。    |    KMS 主机密钥最多可以在 6 台不同的计算机上激活 10 次。 如果需要激活更多次，请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004B100    |    激活服务器确定无法激活计算机。    |    MAK    |    如果 MAK 不受支持，则可能发生此问题。    |    若要排查此问题，请确认使用的 MAK 是 Microsoft 提供的 MAK。 若要验证该 MAK 是否有效，请联系：[Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C020     |    激活服务器报告多次激活密钥已超过其限制。    |    MAK    |    MAK 已超过激活限制。    |    MAK 在设计上只能激活有限的次数。   请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C021     |    激活服务器报告已超过多次激活密钥扩展限制。    |    MAK    |    MAK 已超过激活限制。    |    MAK 在设计上只能激活有限的次数。   请联系 [Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)    |
|    0xC004F009     |    软件保护服务报告宽限期已过期。    |    MAK    |    在激活系统之前宽限期已过期。 现在，系统处于“通知”状态。    |    请参阅“用户体验”一节。    |
|    0xC004F00F     |    软件授权服务器报告硬件 ID 绑定已超出容限级别。    |    MAK/KMS 客户端/KMS 主机    |    系统上的硬件已更换，或者驱动程序已更新。    |    MAK：在 OOT 宽限期使用联机激活或电话激活来重新激活系统。      KMS：重新启动，或运行 slmgr.vbs /ato。    |
|    0xC004F014     |    软件保护服务报告产品密钥不可用    |    MAK/KMS 客户端    |    系统上未安装产品密钥。    |    安装 MAK 产品密钥，或者装上安装介质上的 \sources\pid.txt 中的 KMS 安装密钥。    |
|    0xC004F02C     |    软件保护服务报告脱机激活数据的格式不正确。    |    MAK/KMS 客户端    |    系统检测到电话激活期间输入的数据无效。    |    确认已正确输入 CID。    |
|    0xC004F035     |    此错误代码等同于“软件保护服务报告无法使用批量许可证产品密钥激活该计算机...”错误文本是正确的，但不明确。 此错误表示计算机缺少 BIOS 中的 Windows 标记 - 该标记在 OEM 系统上提供，表示计算机附带合格的 Windows 版本，这是 KMS 客户端激活的一项要求。 错误： 无效的批量许可证密钥 若要激活，你需要将产品密钥更改为有效的多次激活密钥(MAK)或有效的零售密钥。 你必须具有合格的操作系统许可证和批量许可 Windows 7 升级许可证，或者由零售商提供的 Windows 7 完全许可证。 其他任何安装此软件的行为均违反你的协议和适用的著作权法。    |    KMS 客户端/KMS 主机    |    Windows 7 批量版仅获得升级许可。 不支持在未安装合格操作系统的计算机上安装批量版操作系统。    |    安装合格的 Microsoft 操作系统版本，然后使用 MAK 激活。    |
|    0xC004F038     |    软件保护服务报告无法激活计算机。 你的密钥管理服务(KMS)报告的计数不足。 请联系你的系统管理员。    |    KMS 客户端    |    KMS 主机上的计数不够大。 对于 Windows Server，KMS 计数必须 ≥5；对于 Windows 客户端，该计数必须 ≥25。    |    若要激活 KMS 客户端，需要在 KMS 池中添加更多计算机。 运行 Slmgr.vbs /dli 以获取 KMS 主机上的当前计数。    |
|    0xC004F039     |    软件保护服务报告无法激活计算机。 未启用密钥管理服务(KMS)。    |    KMS 客户端    |    未应答某个 KMS 请求时会出现此错误。    |    排查 KMS 主机与客户端之间的网络连接问题。 确保 TCP 端口 1688（默认）未被防火墙阻止，也没有被其他方式筛选掉。    |
|    0xC004F041     |    软件保护服务确定未激活密钥管理服务器(KMS)。 需要激活 KMS。    |    KMS 客户端    |    未激活 KMS 主机。    |    使用联机激活或电话激活来激活 KMS 主机。    |
|    0xC004F042     |    软件保护服务确定无法使用指定的密钥管理服务(KMS)。    |    KMS 客户端    |    KMS 客户端与 KMS 主机之间不匹配。    |    当 KMS 客户端联系一台无法激活客户端软件的 KMS 主机时，就会出现此错误。 例如，在包含特定于应用程序和操作系统的 KMS 主机的混合环境中，这种情况可能很常见。    |
|    0xC004F050     |    软件保护服务报告产品密钥无效。    |    KMS、KMS 客户端、MAK    |    这可能是由于 KMS 密钥拼写错误，或者在正式发行版操作系统中键入 Beta 密钥而造成的。    |    请在相应版本的 Windows 上安装相应的 KMS 密钥。 检查拼写。 如果密钥是复制后粘贴过来的，请确保不要将密钥中的短划线替换为长破折号。    |
|    0xC004F051     |    软件保护服务报告产品密钥被阻止。    |    MAK/KMS    |    激活服务器上的产品密钥已被 Microsoft 阻止。    |    获取新的 MAK/KMS 密钥，将它安装在系统上，然后激活。    |
|    0xC004F064     |    软件保护服务报告非正版宽限期已过期。    |    MAK    |    Windows 激活工具 (WAT) 已确定系统不是正版。    |    请参阅批量激活操作指南。    |
|    0xC004F065     |    软件保护服务报告该应用程序正在有效的非正版期间内运行。    |    MAK/KMS 客户端    |    Windows 激活工具已确定系统不是正版。 系统将在非正版宽限期内继续运行。    |    获取并安装正版产品密钥，并在宽限期内激活系统。 否则，系统将在宽限期结束时进入通知状态。    |
|    0xC004F06C     |    软件保护服务报告无法激活计算机。 密钥管理服务(KMS)确定请求时间戳无效。    |    KMS 客户端    |    客户端计算机上的系统时间与 KMS 主机上的时间有很大的差异。    |    由于多种原因，时间同步对于系统和网络的安全非常重要。 请更改客户端上的系统时间，使其与 KMS 同步，以便解决此问题。 建议使用网络时间协议 (NTP) 时间源或 Active Directory 域服务进行时间同步。 出现当前的问题是因为你使用了 UTP 时间，而这种时间与选择的时区无关。    |
|    0x80070005     |    拒绝访问。 所请求的操作需要提升特权。    |    KMS 客户端/MAK/KMS 主机    |    用户帐户控制 (UAC) 阻止激活进程在非提升的命令提示符下运行。    |    在提升的命令提示符下运行 slmgr.vbs。   右键单击 cmd.exe，然后单击“以管理员身份运行”。    |
|    0x8007232A     |    DNS 服务器失败。    |    KMS 主机    |    系统出现网络或 DNS 问题。    |    排查网络和 DNS 的问题。    |
|    0x8007232B     |    DNS 名称不存在。    |    KMS 客户端    |    KMS 客户端在 DNS 中找不到 KMS SRV RR。 如果网络上不存在 KMS 主机，则应该安装 MAK。    |    确认已安装 KMS 主机，并且已启用 DNS 发布（默认设置）。 如果 DNS 不可用，请使用 slmgr.vbs /skms <kms_host_name> 将 KMS 客户端指向 KMS 主机。或者，获取并安装 MAK，然后激活系统。 最后，排查 DNS 问题。    |
|    0x800706BA     |    RPC 服务器不可用。    |    KMS 客户端    |    未在 KMS 主机上配置防火墙设置，或者 DNS SRV 记录已过时。    |    确保已在 KMS 主机上启用密钥管理服务防火墙例外。 确保 SRV 记录指向有效的 KMS 主机。 排查网络连接问题。    |
|    0x8007251D     |    找不到 DNS 查询的记录。    |    KMS 客户端    |    KMS 客户端在 DNS 中找不到 KMS SRV RR。    |    排查网络连接和 DNS 的问题。    |
|    0xC004F074     |    软件保护服务报告无法激活计算机。 无法联系任何密钥管理服务(KMS)。 有关其他信息，请参阅应用程序事件日志。    |    KMS 客户端    |    所有 KMS 主机系统都返回了错误。    |    根据每次尝试激活时返回的相关事件 ID 12288 排查错误。    |
|    0x8004FE21     |    此计算机未运行正版 Windows。    |    MAK/KMS 客户端    |    多种原因可能导致此问题。 最可能的原因是在运行 Windows 版本且这些版本未获许可安装其他语言包的计算机上安装了语言包 (MUI)。 （请注意，这并不一定表示发生了篡改。   某些应用程序可以安装多语言支持，即使该 Windows 版本未获许可安装这些语言包。）如果 Windows 已被恶意软件修改为允许安装其他功能，也可能出现此问题。 如果某些系统文件已损坏，也可能出现此问题。    |    若要解决此问题，必须重新安装操作系统。    |
|    0x80092328     |    0x80092328 DNS 名称不存在。    |    KMS 客户端    |    如果 KMS 客户端在 DNS 中找不到 KMS SRV 资源记录，则可能出现此问题。    |    若要解决此问题，请按照以下 Microsoft 知识库文章中的步骤操作：929826 尝试激活 Windows Vista 企业版、Windows Vista 商用版、Windows 7 或 Windows Server 2008 时出现错误消息：“代码 0x8007232b”    |
|    0x8007007b    |    0x8007007b DNS 名称不存在。    |    KMS 客户端    |    如果 KMS 客户端在 DNS 中找不到 KMS SRV 资源记录，则可能出现此问题。    |    若要解决此问题，请按照以下 Microsoft 知识库文章中的步骤操作：929826 尝试激活 Windows Vista 企业版、Windows Vista 商用版、Windows 7 或 Windows Server 2008 时出现错误消息：“代码 0x8007232b”    |
|    0x80070490    |你输入的产品密钥无效。  请核对该产品密钥后再试一次，或输入其他产品密钥。 |MAK |输入无效的 MAK 或 Windows Server 2019 中的已知问题会导致此问题。 |若要解决此问题，请使用命令行 **slmgr -ipk \<5x5 key\>** 激活计算机|
