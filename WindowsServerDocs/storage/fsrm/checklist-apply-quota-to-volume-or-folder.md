---
title: 将配额应用于卷或文件夹
description: 本文介绍如何将存储配额应用于卷或文件夹
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 62910af666fb16db5c2e7a30b49eedfa8c12cacb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860848"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>清单：将配额应用于卷或文件夹

> 适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

1. 要通过电子邮件发送阈值通知或存储报告，请配置电子邮件设置。 [配置电子邮件通知](configure-email-notifications.md)

2. 评估有关卷或文件夹的存储要求。 你可以使用**存储报告管理**节点的报告来提供数据。 （例如，文件的所有者报表按需运行以标识使用大量的磁盘空间的用户。）[按需求生成报告](generate-reports-on-demand.md)

3. 查看可用的预配置配额模板。 (在**配额管理**，单击**配额模板**节点。)[编辑配额模板属性](edit-quota-template-properties.md) 
<br />-或者- <br /> 创建新的配额模板，在你的组织中强制执行存储策略。 [创建配额模板](create-quota-template.md)

4. 基于卷或文件夹相关的模板创建配额。  
 [创建配额](create-quota.md) <br /> -或者- <br /> 创建自动应用配额，用于自动生成卷或文件夹相关的阈值配额。 [创建自动应用配额](create-auto-apply-quota.md)

6. 安排一项包含配额使用报告的报告任务，以定期监测配额使用情况。 [计划的一组报表](schedule-set-of-reports.md)

> [!Note]
> 如果想屏幕上的卷或文件夹的文件，请参阅[核对清单：将文件屏蔽应用于卷或文件夹](checklist-apply-file-screen-to-volume-or-folder.md)。











