---
title: 将中间驱动程序注册为协议
description: 将中间驱动程序注册为协议
ms.assetid: 79707f6b-0e31-46a8-a763-fa2669ce9635
keywords:
- 注册中间驱动程序
- 中间驱动程序 WDK 网络，注册
- NDIS 中间驱动程序 WDK，注册
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 991abf74ad9edc31d0e8c04317ba4a7066fbd0de
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72842093"
---
# <a name="registering-an-intermediate-driver-as-a-protocol"></a>将中间驱动程序注册为协议





中间驱动程序通过调用[**NdisRegisterProtocolDriver**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/nf-ndis-ndisregisterprotocoldriver)在其[**DriverEntry**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_initialize)函数的上下文中向 NDIS 注册其*ProtocolXxx*函数。

将中间驱动程序注册为协议与注册为协议驱动程序几乎完全相同。 有关详细信息，请参阅[初始化协议驱动程序](initializing-a-protocol-driver.md)。

具有面向连接的下边缘的中间驱动程序必须注册为面向连接的客户端。 面向连接的客户端使用呼叫管理器或集成微型端口调用管理器（MCM）的调用设置和拆卸服务。 面向连接的客户端还使用面向连接的微型端口驱动程序或 MCM 的发送和接收功能来发送和接收数据。 有关详细信息，请参阅[客户端执行的面向连接的操作](connection-oriented-operations-performed-by-clients.md)。

中间驱动程序可能需要其他特定于实现的*ProtocolXxx*函数。 有关注册可选的*ProtocolXxx*函数的信息，请参阅[配置可选的协议驱动程序服务](configuring-optional-protocol-driver-services.md)。

 

 





