---
title: QoS 策略的错误和事件消息
description: 本主题提供适用于 Windows Server 2016 服务质量 (QoS) 策略的错误和活动的邮件列表。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e2e890a7947d4f7de09159d7de606c0542f45045
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-error-and-event-messages"></a>QoS 策略的错误和事件消息

>适用于：Windows Server（半年通道），Windows Server 2016

以下是与 QoS 策略的错误和事件消息。  
  
## <a name="informational-messages"></a>信息消息  

以下是 QoS 策略信息消息的列表。

|||  
|-|-|  
|**邮件 Id**|16500|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**语言**|英语|  
|**消息**|成功刷新的计算机 QoS 策略。 未检测到的更改。|  
  
|||  
|-|-|  
|**邮件 Id**|16501|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**语言**|英语|  
|**消息**|成功刷新的计算机 QoS 策略。 策略未检测到更改。|  
  
|||  
|-|-|  
|**邮件 Id**|16502|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**语言**|英语|  
|**消息**|成功刷新的用户 QoS 策略。 未检测到的更改。|  
  
|||  
|-|-|  
|**邮件 Id**|16503|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**语言**|英语|  
|**消息**|成功刷新的用户 QoS 策略。 策略未检测到更改。|  
  
|||  
|-|-|  
|**邮件 Id**|16504|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**语言**|英语|  
|**消息**|成功刷新的入站 TCP 吞吐量级别高级 QoS 设置。 没有任何 QoS 策略指定值设置。 将应用本地计算机默认值。|  
  
|||  
|-|-|  
|**邮件 Id**|16505|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**语言**|英语|  
|**消息**|成功刷新的入站 TCP 吞吐量级别高级 QoS 设置。 值设置为等级 0（最低吞吐量）。|  
  
|||  
|-|-|  
|**邮件 Id**|16506|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**语言**|英语|  
|**消息**|成功刷新的入站 TCP 吞吐量级别高级 QoS 设置。 值设置为级别 1。|  
  
|||  
|-|-|  
|**邮件 Id**|16507|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**语言**|英语|  
|**消息**|成功刷新的入站 TCP 吞吐量级别高级 QoS 设置。 值设置为级别 2。|  
  
|||  
|-|-|  
|**邮件 Id**|16508|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**语言**|英语|  
|**消息**|成功刷新的入站 TCP 吞吐量级别高级 QoS 设置。 值设置为级别 3（最大的吞吐量）。|  
  
|||  
|-|-|  
|**邮件 Id**|16509|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**语言**|英语|  
|**消息**|DSCP 标记为高级 QoS 设置替代成功刷新。 未指定值设置。 应用程序可以设置独立 QoS 策略于 DSCP 值。|  
  
|||  
|-|-|  
|**邮件 Id**|16510|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**语言**|英语|  
|**消息**|DSCP 标记为高级 QoS 设置替代成功刷新。 将忽略应用程序 DSCP 标记请求。 仅 QoS 策略可以设置 DSCP 值。|  
  
|||  
|-|-|  
|**邮件 Id**|16511|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**语言**|英语|  
|**消息**|DSCP 标记为高级 QoS 设置替代成功刷新。 应用程序可以设置独立 QoS 策略于 DSCP 值。|  
  
|||  
|-|-|  
|**邮件 Id**|16512|  
|**严重性**|信息|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**语言**|英语|  
|**消息**|已禁用 QoS 策略基于域网络类别选择性应用。 QoS 策略将应用于所有网络接口。|  
  
## <a name="warning-messages"></a>警告消息

以下是一份 QoS 策略警告消息。

|||  
|-|-|  
|**邮件 Id**|16600|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**语言**|英语|  
|**消息**|EQOS: *** Testing\ * \ * \ * [，带有一个字符串]"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16601|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**语言**|英语|  
|**消息**|EQOS: *** Testing\ * \ * \ * [，使用两个字符串，则字符串 1]"%2"[，字符串 2 是]"%3"。|  
  
|||  
|-|-|  
|**邮件 Id**|16602|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**语言**|英语|  
|**消息**|计算机 QoS 策略"%2"了一个无效的版本号。 此策略不会应用。|  
  
|||  
|-|-|  
|**邮件 Id**|16603|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**语言**|英语|  
|**消息**|用户 QoS 策略"%2"拥有一个无效的版本号。 此策略不会应用。|  
  
|||  
|-|-|  
|**邮件 Id**|16604|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**语言**|英语|  
|**消息**|计算机 QoS 策略"%2"不指定 DSCP 值或限制率。 此策略不会应用。|  
  
|||  
|-|-|  
|**邮件 Id**|16605|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**语言**|英语|  
|**消息**|用户 QoS 策略"%2"未指定 DSCP 值或限制率。 此策略不会应用。|  
  
|||  
|-|-|  
|**邮件 Id**|16606|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**语言**|英语|  
|**消息**|超出最大的计算机 QoS 策略。 将不会应用 QoS 策略"%2"并随后计算机 QoS 策略。|  
  
|||  
|-|-|  
|**邮件 Id**|16607|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**语言**|英语|  
|**消息**|超出最大的用户 QoS 策略。 将不会应用 QoS 策略"%2"和后续用户 QoS 策略。|  
  
|||  
|-|-|  
|**邮件 Id**|16608|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**语言**|英语|  
|**消息**|与其他 QoS 策略潜在冲突计算机 QoS 策略"%2"。 请参阅有关哪些应用策略规则文档。|  
  
|||  
|-|-|  
|**邮件 Id**|16609|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**语言**|英语|  
|**消息**|与其他 QoS 策略潜在冲突用户 QoS 策略"%2"。 请参阅有关哪些应用策略规则文档。|  
  
|||  
|-|-|  
|**邮件 Id**|16610|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**语言**|英语|  
|**消息**|因为无法处理应用程序路径忽略计算机 QoS 策略"%2"。 应用程序路径可能无效，包含无效的驱动器号，或包含映射网络驱动器。|  
  
|||  
|-|-|  
|**邮件 Id**|16611|  
|**严重性**|警告|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**语言**|英语|  
|**消息**|因为无法处理应用程序路径忽略用户 QoS 策略"%2"。 应用程序路径可能无效，包含无效的驱动器号，或包含映射网络驱动器。|  
  
## <a name="error-messages"></a>错误消息  

以下是 QoS 策略的错误消息的列表。

|||  
|-|-|  
|**邮件 Id**|16700|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**语言**|英语|  
|**消息**|计算机 QoS 策略无法恢复。 错误代码:"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16701|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**语言**|英语|  
|**消息**|用户 QoS 策略无法恢复。 错误代码:"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16702|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**语言**|英语|  
|**消息**|打开计算机级别根密钥 QoS 策略 QoS 失败。 错误代码:"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16703|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**语言**|英语|  
|**消息**|打开 QoS 策略用户级别根密钥 QoS 失败。 错误代码:"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16704|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**语言**|英语|  
|**消息**|一台计算机 QoS 策略超过最大的允许的名称长度。 有问题的策略下列出的计算机级别 QoS 策略根键，索引"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16705|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**语言**|英语|  
|**消息**|用户 QoS 策略超过最大的允许的名称长度。 有问题的策略下列出的用户级别 QoS 策略根键，索引"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16706|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**语言**|英语|  
|**消息**|一台计算机 QoS 策略具有零长度名称。 有问题的策略下列出的计算机级别 QoS 策略根键，索引"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16707|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**语言**|英语|  
|**消息**|用户 QoS 策略具有零长度名称。 有问题的策略下列出的用户级别 QoS 策略根键，索引"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16708|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**语言**|英语|  
|**消息**|打开计算机 QoS 策略的注册表子项 QoS 失败。 该策略下列出的计算机级别 QoS 策略根键，索引"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16709|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**语言**|英语|  
|**消息**|打开的用户 QoS 策略注册表子项 QoS 失败。 该策略是在用户级别 QoS 策略根密钥，索引"%2"下列出。|  
  
|||  
|-|-|  
|**邮件 Id**|16710|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**语言**|英语|  
|**消息**|QoS 无法读取或验证"%2"字段计算机 QoS 策略"%3"。|  
  
|||  
|-|-|  
|**邮件 Id**|16711|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**语言**|英语|  
|**消息**|QoS 无法读取或验证"%2"字段用户 QoS 策略"%3"。|  
  
|||  
|-|-|  
|**邮件 Id**|16712|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**语言**|英语|  
|**消息**|QoS 无法读取或设置站的 TCP 吞吐量级别，错误代码:"%2"。|  
  
|||  
|-|-|  
|**邮件 Id**|16713|  
|**严重性**|错误|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**语言**|英语|  
|**消息**|无法读取或设置 DSCP 标记 QoS 覆盖设置，错误代码:"%2"。|  

本指南中的下一步主题，请参阅[QoS 策略常见问题](qos-policy-faq.md)。

本指南中的第一个主题，请参阅[质量服务 (QoS) 策略](qos-policy-top.md)。
