---
title: 远程笔记本电脑从无线网络断开连接
description: 排查远程笔记本电脑从无线网络断开连接的问题。
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 70aaad61cf068fe38fa95127700655db2a186dd1
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529867"
---
# <a name="remote-laptop-disconnects-from-wireless-network"></a>远程笔记本电脑从无线网络断开连接

当远程桌面客户端使用 802.1x 无线网络连接到笔记本电脑时，可能会出现此问题。 笔记本电脑间歇性地从无线网络断开连接，且不会自动重新连接。

当无线网络连接的网络身份验证设置为“用户身份验证”时，会发生这种已知问题。 

若要解决此问题，请将网络身份验证设置指定为“用户或计算机身份验证”或者“计算机身份验证”。  

 > [!NOTE]  
> 若要更改一台计算机上的网络身份验证设置，可能需要使用“网络和共享中心”控制面板来以新的设置创建新的无线连接。

有关如何使用 GPO 配置无线网络设置的完整说明，请参阅[配置无线网络 (IEEE 802.11) 策略](../../../networking/core-network-guide/cncg/wireless/e-wireless-access-deployment.md#bkmk_policies)。
