---
title: 有关转储加密
description: 介绍如何加密转储文件以及进行故障排除加密。
ms.prod: windows-server-threshold
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: e0ca8829aa8f93e7543b4183539beade3b4aeb47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812388"
---
# <a name="about-dump-encryption"></a>有关转储加密
可以使用转储加密来加密故障转储和实时系统生成的转储。 为每个转储使用生成的对称加密密钥加密这些转储。 此密钥本身则使用由受信任的主机 （崩溃转储加密密钥保护程序） 的管理员指定的公钥进行加密。 这可确保只有具有匹配的私钥的人员可以解密并因此访问转储的内容。 受保护的构造中利用此功能。
注意：如果配置转储加密，也可以禁用 Windows 错误报告。 WER 无法读取加密的故障转储。

# <a name="configuring-dump-encryption"></a>配置转储加密
## <a name="manual-configuration"></a>手动配置
若要启用转储加密使用注册表，配置下的以下注册表值 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| 值名称 | 在任务栏的搜索框中键入 | 值 |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 表示启用转储加密，0 表示禁用转储加密 |
| EncryptionCertificates\Certificate.1::PublicKey | Binary | 公共密钥 （RSA，2048 位），应该用于加密转储。 这必须格式为[BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx)。 |
| EncryptionCertificates\Certificate.1::Thumbprint | 字符串 | 若要解密的故障转储时允许自动查找本地证书存储中的私钥的证书指纹。 |


## <a name="configuration-using-script"></a>使用脚本配置
若要简化配置，[的示例脚本](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption)是可用于启用基于证书的公钥转储加密。

1. 在受信任的环境中：使用 2048 位 RSA 密钥创建证书并导出公共证书
2. 在目标主机：公共证书导入到本地证书存储区
3. 运行示例配置脚本 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

# <a name="decrypting-encrypted-dumps"></a>解密已加密的转储
若要解密现有加密的转储文件，您需要下载并安装调试工具的 Windows。 此工具集包含 KernelDumpDecrypt.exe 用于解密已加密的转储文件。
如果存在当前用户的证书存储区中包括私钥的证书，则可以通过调用解密转储文件

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
解密之后, WinDbg 之类的工具可以打开已解密的转储文件。

# <a name="troubleshooting-dump-encryption"></a>转储加密故障排除
如果系统上启用转储加密，但没有转储正在生成，请检查系统的`System`事件日志中是否`Kernel-IO`1207年事件。 无法初始化转储加密，创建此事件和转储处于禁用状态。

| 详细的错误消息 | 缓解步骤 |
| ---------------------- | ----------------- |
| 公共密钥或指纹注册表缺少 | 检查是否在预期位置中存在这两个注册表值 |
| 公钥无效 | 请确保公钥存储在 PublicKey 注册表值存储为[BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx)。 |
| 不受支持的公钥大小 | 目前，支持仅 2048 位 RSA 密钥。 配置匹配此要求的密钥 |

另请检查是否值`GuardedHost`下`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled`设置为 0 以外的值。 这会完全禁用故障转储。 如果这种情况，则将其设置为 0。
