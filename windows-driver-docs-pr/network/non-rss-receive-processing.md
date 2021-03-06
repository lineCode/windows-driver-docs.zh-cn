---
title: 非 RSS 接收处理
description: 非 RSS 接收处理
ms.assetid: 9fe262c3-9ce5-4625-8d29-ff7dc4ccb90a
keywords:
- 接收方缩放 WDK 网络，非 RSS 接收处理
- RSS WDK 网络，非 RSS 接收处理
- 非 RSS 接收处理 WDK RSS
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 176e41c7c2df6e5f68e7a6e7dd1799bd5120b40a
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72842768"
---
# <a name="non-rss-receive-processing"></a>非 RSS 接收处理





不支持 RSS 的微型端口驱动程序接收处理，如本主题中所述。

下图说明了非 RSS 接收处理。

![阐释无需 rss 的发送和接收处理的示意图](images/rsslessstack.png)

在图中，虚线路径表示发送和接收处理的备用路径。 由于系统控制缩放，因此处理并不总是在提供最佳性能的 CPU 上发生。 仅可以通过连续中断在同一 CPU 上处理连接。

对于每个非 RSS 中断周期，将重复以下过程：

1.  NIC 使用 DMA 来填充包含已接收数据的缓冲区，并使系统中断。

    小型端口驱动程序在初始化期间将接收缓冲区分配到共享内存中。

2.  在此中断周期内，NIC 可随时继续填充额外的接收缓冲区。 但是，NIC 不会再次中断，直到微型端口驱动程序启用中断。

    系统在一个中断周期中处理的接收的缓冲区可以与多个不同的网络连接关联。

3.  NDIS 在系统确定的 CPU 上调用微型端口驱动程序的[*MiniportInterrupt*](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/nc-ndis-miniport_isr)函数（ISR）。

    理想情况下，ISR 应到繁忙 CPU 最忙。 但是，在某些系统中，系统会将 ISR 分配给可用 CPU 或与 NIC 关联的 CPU。

4.  ISR 禁用中断，并请求 NDIS 对延迟的过程调用（DPC）进行排队以处理接收的数据。

5.  NDIS 在当前 CPU 上调用[*MiniportInterruptDPC*](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/nc-ndis-miniport_interrupt_dpc)函数（DPC）。

6.  DPC 为所有接收的缓冲区生成接收描述符，并指示驱动程序堆栈上的数据。 有关详细信息，请参阅[接收网络数据](receiving-network-data.md)。

    许多不同的连接可能有很多缓冲区，并且可能需要大量的处理才能完成。 可在其他 Cpu 上处理与后续中断周期关联的接收数据。 给定网络连接的发送处理也可以在不同的 CPU 上运行。

7.  DPC 启用中断。 此中断周期已完成，进程将再次启动。

 

 





