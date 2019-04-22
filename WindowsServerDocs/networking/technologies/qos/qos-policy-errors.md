---
title: QoS 策略错误和事件消息
description: 本主题提供有关 Windows Server 2016 中的服务质量 (QoS) 策略错误和事件消息的列表。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 774d9473beed1da861c6827357710133aeaca15e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824048"
---
# <a name="qos-policy-error-and-event-messages"></a>QoS 策略错误和事件消息

>适用于：Windows 服务器 （半年频道），Windows Server 2016

以下是与 QoS 策略相关联的错误和事件消息。  
  
## <a name="informational-messages"></a>信息性消息  

下面是一系列 QoS 策略信息性消息。

|||  
|-|-|  
|**MessageId**|16500|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**语言**|英语|  
|**Message**|已成功刷新的计算机 QoS 策略。 未检测到的更改。|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**语言**|英语|  
|**Message**|已成功刷新的计算机 QoS 策略。 检测到的策略更改。|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**语言**|英语|  
|**Message**|已成功刷新的用户 QoS 策略。 未检测到的更改。|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**语言**|英语|  
|**Message**|已成功刷新的用户 QoS 策略。 检测到的策略更改。|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**语言**|英语|  
|**Message**|入站 TCP 吞吐量级别的高级 QoS 设置已成功刷新。 未指定的任何 QoS 策略值设置。 将应用本地计算机默认值。|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**语言**|英语|  
|**Message**|入站 TCP 吞吐量级别的高级 QoS 设置已成功刷新。 将值设置为级别 0 （最小吞吐量）。|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**语言**|英语|  
|**Message**|入站 TCP 吞吐量级别的高级 QoS 设置已成功刷新。 将值设置为级别 1。|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**语言**|英语|  
|**Message**|入站 TCP 吞吐量级别的高级 QoS 设置已成功刷新。 将值设置为级别 2。|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**语言**|英语|  
|**Message**|入站 TCP 吞吐量级别的高级 QoS 设置已成功刷新。 将值设置为级别 3 （最大吞吐量）。|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**语言**|英语|  
|**Message**|DSCP 标记为高级 QoS 设置将替代已成功刷新。 未指定设置值。 应用程序可以设置 DSCP 值独立于 QoS 策略。|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**语言**|英语|  
|**Message**|DSCP 标记为高级 QoS 设置将替代已成功刷新。 应用程序的 DSCP 标记请求将被忽略。 仅 QoS 策略可以设置 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**语言**|英语|  
|**Message**|DSCP 标记为高级 QoS 设置将替代已成功刷新。 应用程序可以设置 DSCP 值独立于 QoS 策略。|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**语言**|英语|  
|**Message**|选择性的基于域网络类别的 QoS 策略的应用程序已被禁用。 QoS 策略将应用于所有网络接口。|  
  
## <a name="warning-messages"></a>警告消息

下面是一系列 QoS 策略警告消息。

|||  
|-|-|  
|**MessageId**|16600|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**语言**|英语|  
|**Message**|EQOS: * * * 测试\*\*\*[，带有一个字符串]"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**语言**|英语|  
|**Message**|EQOS: * * * 测试\*\*\*[，使用两个字符串 string1 为]"%2"[，string2 是]"%3"。|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**语言**|英语|  
|**Message**|计算机 QoS 策略"%2"具有无效的版本号。 将不会应用此策略。|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**语言**|英语|  
|**Message**|用户 QoS 策略"%2"具有无效的版本号。 将不会应用此策略。|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**语言**|英语|  
|**Message**|QoS 策略"%2"的计算机不会指定 DSCP 值或限制速率。 将不会应用此策略。|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**语言**|英语|  
|**Message**|QoS 策略"%2"的用户不指定 DSCP 值或限制速率。 将不会应用此策略。|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**语言**|英语|  
|**Message**|已超过计算机的 QoS 策略的最大数目。 将不应用 QoS 策略"%2"和随后的计算机的 QoS 策略。|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**语言**|英语|  
|**Message**|已超过用户 QoS 策略的最大数目。 将不应用 QoS 策略"%2"和后续用户 QoS 策略。|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**语言**|英语|  
|**Message**|QoS 策略"%2"的计算机可能与其他 QoS 策略冲突。 请参阅有关哪些应用策略的规则文档。|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**语言**|英语|  
|**Message**|QoS 策略"%2"的用户可能与其他 QoS 策略冲突。 请参阅有关哪些应用策略的规则文档。|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**语言**|英语|  
|**Message**|QoS 策略"%2"的计算机被忽略，因为无法处理的应用程序路径。 应用程序的路径可能无效，含有无效的驱动器号或包含已映射网络驱动器。|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**语言**|英语|  
|**Message**|用户 QoS 策略"%2"被忽略，因为无法处理的应用程序路径。 应用程序的路径可能无效，含有无效的驱动器号或包含已映射网络驱动器。|  
  
## <a name="error-messages"></a>错误消息  

下面是 QoS 策略错误消息的列表。

|||  
|-|-|  
|**MessageId**|16700|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**语言**|英语|  
|**Message**|计算机的 QoS 策略刷新失败。 错误代码:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**语言**|英语|  
|**Message**|用户 QoS 策略刷新失败。 错误代码:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**语言**|英语|  
|**Message**|无法打开 QoS 策略的计算机级别根密钥 QoS。 错误代码:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**语言**|英语|  
|**Message**|无法打开 QoS 策略的用户级根密钥 QoS。 错误代码:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**语言**|英语|  
|**Message**|QoS 策略的计算机超过了允许的最大名称长度。 有问题的策略列出的计算机级别 QoS 策略根项下，使用索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**语言**|英语|  
|**Message**|QoS 策略的用户超过了允许的最大名称长度。 有问题的策略被列为的用户级 QoS 策略根项下，使用索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**语言**|英语|  
|**Message**|QoS 策略的计算机具有零长度的名称。 有问题的策略列出的计算机级别 QoS 策略根项下，使用索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**语言**|英语|  
|**Message**|QoS 策略的用户具有零长度的名称。 有问题的策略被列为的用户级 QoS 策略根项下，使用索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**语言**|英语|  
|**Message**|QoS 无法打开计算机的 QoS 策略的注册表子项。 策略被列为的计算机级别 QoS 策略根项下，使用索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**语言**|英语|  
|**Message**|QoS 无法打开用户 QoS 策略的注册表子项。 策略被列为的用户级 QoS 策略根项下，使用索引"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**语言**|英语|  
|**Message**|QoS 无法读取或验证计算机 QoS 策略"%3"的"%2"字段。|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**语言**|英语|  
|**Message**|QoS 无法读取或验证用户 QoS 策略"%3"的"%2"字段。|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**语言**|英语|  
|**Message**|QoS 无法读取或设置入站的 TCP 吞吐量级别，错误代码:"%2"。|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Severity**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**语言**|英语|  
|**Message**|无法读取或设置 DSCP 标记的 QoS 重写设置，错误代码:"%2"。|  

本指南的下一个主题，请参阅[QoS 策略 Frequently Asked Questions](qos-policy-faq.md)。

本指南中的第一个主题，请参阅[服务质量 (QoS) 策略](qos-policy-top.md)。
