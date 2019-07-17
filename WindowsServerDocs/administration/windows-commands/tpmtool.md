---
title: tpmtool
description: Windows 命令 tpmtool-主题获取有关受信任的平台模块的信息。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: e125dbf6127b92c91e041c431f1e462e1f884168
ms.sourcegitcommit: 0ff812a80f654fa2c35b1632524e27841eca75c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68230857"
---
# <a name="tpmtool"></a>tpmtool

此实用程序可用于获取其相关信息[受信任的平台模块 (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview)。

>[!IMPORTANT]
>有些信息与预发布产品相关，这些产品在商业发行之前可能发生重大更改。 Microsoft 不对此处提供的信息作任何明示或默示的担保。

有关如何使用此命令的示例，请参阅[示例](#tpmtool_examples)。

## <a name="syntax"></a>语法

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|getdeviceinformation|显示 TPM 的基本信息。 可找到的信息的标志值的含义[此处](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters)。|
|gatherlogs [输出目录路径]|收集的 TPM 日志并将其置于指定目录中。 如果该目录不存在，则创建它。 默认情况下，将它们放在当前目录中。 生成的可能文件包括： </br>-TpmEvents.evtx</br>-TpmInformation.txt</br>-SRTMBoot.dat</br>-SRTMResume.dat</br>-DRTMBoot.dat</br>-DRTMResume.dat</br>|
|drivertracing [开始 / 停止]|启动/停止收集 TPM 驱动程序跟踪。 跟踪日志，TPMTRACE.etl，将生成并放置在当前目录中。|
|parsetcglogs [-验证 (-v)]|显示已分析的 TCG 日志，也称为 Windows 启动配置日志 (WBCL)。 可以上找到最新的事件描述[TCG 网站](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/)下**事件描述**。 如果`-validate`标志设置，验证是否在 TPM 上的平台配置注册 (PCR) 值与在日志中的值匹配。|
|/?|在命令提示符下显示帮助。|

## <a name="tpmtool_examples"></a>示例

若要显示的 TPM 的基本信息，请键入：
```
tpmtool getdeviceinformation
```
若要收集 TPM 日志并将其放在当前目录中，键入：
```
tpmtool gatherlogs
```
若要收集 TPM 日志，并将其放置在`C:\Users\Public`，类型：
```
tpmtool gatherlogs C:\Users\Public
```
若要收集 TPM 驱动程序跟踪，请键入：
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
若要分析的 TCG 日志：
```
tpmtool parsetcglogs
```
若要分析的 TCG 日志和验证 PCRs:
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>对错误代码解码

特定于 TPM 的错误代码[此处](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6)。
