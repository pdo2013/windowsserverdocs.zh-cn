---
title: QoS 策略错误和事件消息
description: 本主题提供 Windows Server 2016 中的服务质量（QoS）策略的错误和事件消息列表。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 935bda5ab47f3e9a362c81a8aeb99ebf22095725
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405327"
---
# <a name="qos-policy-error-and-event-messages"></a>QoS 策略错误和事件消息

>适用于：Windows Server（半年频道）、Windows Server 2016

下面是与 QoS 策略相关联的错误和事件消息。  
  
## <a name="informational-messages"></a>信息性消息  

下面是 QoS 策略信息性消息的列表。

|||  
|-|-|  
|**MessageId**|16500|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**语言**|英语|  
|**Message**|已成功刷新计算机 QoS 策略。 未检测到任何更改。|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**语言**|英语|  
|**Message**|已成功刷新计算机 QoS 策略。 检测到策略更改。|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**语言**|英语|  
|**Message**|已成功刷新用户 QoS 策略。 未检测到任何更改。|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**语言**|英语|  
|**Message**|已成功刷新用户 QoS 策略。 检测到策略更改。|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**语言**|英语|  
|**Message**|已成功刷新入站 TCP 吞吐量级别的高级 QoS 设置。 设置值不由任何 QoS 策略指定。 将应用本地计算机默认设置。|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**语言**|英语|  
|**Message**|已成功刷新入站 TCP 吞吐量级别的高级 QoS 设置。 设置值为级别0（最小吞吐量）。|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**语言**|英语|  
|**Message**|已成功刷新入站 TCP 吞吐量级别的高级 QoS 设置。 设置值为级别1。|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**语言**|英语|  
|**Message**|已成功刷新入站 TCP 吞吐量级别的高级 QoS 设置。 设置值为级别2。|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**语言**|英语|  
|**Message**|已成功刷新入站 TCP 吞吐量级别的高级 QoS 设置。 设置值为 Level 3 （最大吞吐量）。|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**语言**|英语|  
|**Message**|DSCP 标记替代的高级 QoS 设置已成功刷新。 未指定设置值。 应用程序可以独立于 QoS 策略设置 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**语言**|英语|  
|**Message**|DSCP 标记替代的高级 QoS 设置已成功刷新。 将忽略应用程序 DSCP 标记请求。 只有 QoS 策略才能设置 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**语言**|英语|  
|**Message**|DSCP 标记替代的高级 QoS 设置已成功刷新。 应用程序可以独立于 QoS 策略设置 DSCP 值。|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Severity**|信息性|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**语言**|英语|  
|**Message**|基于域网络类别的 QoS 策略的选择性应用已禁用。 QoS 策略将应用于所有网络接口。|  
  
## <a name="warning-messages"></a>警告消息

下面是 QoS 策略警告消息列表。

|||  
|-|-|  
|**MessageId**|16600|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**语言**|英语|  
|**Message**|EQOS： * * * 测试 @ no__t-0 @ no__t-1 @ no__t [，其中包含一个字符串] "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**语言**|英语|  
|**Message**|EQOS： * * * 测试 @ no__t-0 @ no__t-1 @ no__t [，其中包含两个字符串，string1 为] "% 2" [，string2 为] "% 3"。|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**语言**|英语|  
|**Message**|计算机 QoS 策略 "% 2" 的版本号无效。 不会应用此策略。|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**语言**|英语|  
|**Message**|用户 QoS 策略 "% 2" 的版本号无效。 不会应用此策略。|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**语言**|英语|  
|**Message**|计算机 QoS 策略 "% 2" 没有指定 DSCP 值或限制速率。 不会应用此策略。|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**语言**|英语|  
|**Message**|用户 QoS 策略 "% 2" 没有指定 DSCP 值或限制速率。 不会应用此策略。|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**语言**|英语|  
|**Message**|超出了计算机 QoS 策略的最大数目。 QoS 策略 "% 2" 和后续计算机 QoS 策略将不会应用。|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**语言**|英语|  
|**Message**|超过用户 QoS 策略的最大数目。 QoS 策略 "% 2" 和后续用户 QoS 策略将不会应用。|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**语言**|英语|  
|**Message**|计算机 QoS 策略 "% 2" 可能与其他 QoS 策略冲突。 有关将应用的策略的规则，请参阅文档。|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**语言**|英语|  
|**Message**|用户 QoS 策略 "% 2" 可能与其他 QoS 策略冲突。 有关将应用的策略的规则，请参阅文档。|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**语言**|英语|  
|**Message**|由于无法处理应用程序路径，计算机 QoS 策略 "% 2" 被忽略。 应用程序路径可能无效、包含无效驱动器号或包含网络映射驱动器。|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Severity**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**语言**|英语|  
|**Message**|由于无法处理应用程序路径，用户 QoS 策略 "% 2" 被忽略。 应用程序路径可能无效、包含无效驱动器号或包含网络映射驱动器。|  
  
## <a name="error-messages"></a>错误消息  

下面是 QoS 策略错误消息的列表。

|||  
|-|-|  
|**MessageId**|16700|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**语言**|英语|  
|**Message**|计算机 QoS 策略无法刷新。 错误代码： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**语言**|英语|  
|**Message**|用户 QoS 策略无法刷新。 错误代码： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**语言**|英语|  
|**Message**|QoS 无法打开计算机级的 QoS 策略根密钥。 错误代码： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**语言**|英语|  
|**Message**|QoS 无法为 QoS 策略打开用户级根密钥。 错误代码： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**语言**|英语|  
|**Message**|计算机 QoS 策略超出了允许的最大名称长度。 出现问题的策略在计算机级别的 QoS 策略根密钥下列出，索引为 "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**语言**|英语|  
|**Message**|用户 QoS 策略超出了允许的最大名称长度。 有问题的策略列在用户级 QoS 策略根密钥下，索引为 "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**语言**|英语|  
|**Message**|计算机 QoS 策略的名称长度为零。 出现问题的策略在计算机级别的 QoS 策略根密钥下列出，索引为 "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**语言**|英语|  
|**Message**|用户 QoS 策略的名称长度为零。 有问题的策略列在用户级 QoS 策略根密钥下，索引为 "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**语言**|英语|  
|**Message**|QoS 无法打开计算机 QoS 策略的注册表子项。 此策略在计算机级 QoS 策略根密钥下列出，索引为 "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**语言**|英语|  
|**Message**|QoS 无法打开用户 QoS 策略的注册表子项。 该策略列在用户级 QoS 策略根密钥下，索引为 "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**语言**|英语|  
|**Message**|QoS 无法读取或验证计算机 QoS 策略 "% 3" 的 "% 2" 字段。|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**语言**|英语|  
|**Message**|QoS 无法读取或验证用户 QoS 策略 "% 3" 的 "% 2" 字段。|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**语言**|英语|  
|**Message**|QoS 无法读取或设置入站 TCP 吞吐量级别，错误代码： "% 2"。|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**语言**|英语|  
|**Message**|QoS 无法读取或设置 DSCP 标记替代设置，错误代码： "% 2"。|  

有关本指南的下一个主题，请参阅[QoS 策略常见问题](qos-policy-faq.md)。

有关本指南的第一个主题，请参阅[服务质量（QoS）策略](qos-policy-top.md)。
