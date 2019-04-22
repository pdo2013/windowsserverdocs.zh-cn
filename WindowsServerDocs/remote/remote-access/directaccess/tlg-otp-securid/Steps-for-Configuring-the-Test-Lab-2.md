---
title: 配置测试实验室的步骤
description: 本主题是一部分的测试实验室指南-使用 OTP 身份验证和 Windows Server 2016 的 RSA SecurID 演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 991f06568a16f7e8bcce3bc6c86eef3467ea31e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826288"
---
# <a name="steps-for-configuring-the-test-lab"></a>配置测试实验室的步骤

>适用于：Windows 服务器 （半年频道），Windows Server 2016

以下步骤介绍如何配置远程访问基础结构、 配置远程访问服务器和客户端，并测试 Homenet 和 Internet 子网中的 DirectAccess 连接。  
  
在本测试实验室指南中，你将生成 OTP 环境提供的远程访问，通过执行以下步骤：  
  
-   [步骤 1:完成 DirectAccess 配置](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6)。 完成中的所有步骤[测试实验室指南：演示混合使用 IPv4 和 IPv6 的 DirectAccess 单服务器安装](https://go.microsoft.com/fwlink/p/?LinkId=237004)。  
  
-   [步骤 2:配置 APP1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff)。 使用 OTP 证书模板以供 EDGE1 配置 APP1。  
  
-   [步骤 3:配置 DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522)。 验证在 DC1 上定义的用户主体名称。  
  
-   [步骤 7:安装和配置 RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a)。 安装和配置 RSA、 RADIUS 和 OTP 服务器，并为 OTP 配置 EDGE1。  
  
-   [步骤 11:验证 OTP EDGE1 上的运行状况](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba)。 确保状态 OTP 的远程访问服务器上正常运行。  
  
-   [步骤 8:测试 Homenet 子网中的 DirectAccess 连接](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14)。 测试从 NAT 设备后面的 DirectAccess OTP 功能。  
  
-   [步骤 10:测试来自 Internet 的 DirectAccess 连接](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9)。 测试来自 Internet 的 DirectAccess 客户端连接。  
  
-   [步骤 12:拍摄配置快照](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4)。 完成测试实验后，需要使用 DirectAccess OTP 配置的快照，以便可以返回到更高版本以测试其他方案。  
  


