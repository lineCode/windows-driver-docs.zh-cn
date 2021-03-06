---
title: 在 IEEE 1394 总线上发送异步 I/O 请求数据包
description: 在 IEEE 1394 总线上发送异步 I/O 请求数据包
ms.assetid: 93ad0cdf-5ac2-4916-b90e-1e64ca4494b6
keywords:
- 发送异步 i/o 请求
- raw 模式寻址 WDK IEEE 1394 总线
- 虚拟模式寻址 WDK IEEE 1394 总线
- 正常-模式寻址 WDK IEEE 1394 总线
- 解决 WDK IEEE 1394 总线
- 数据块 WDK IEEE 1394 总线
- 连续数据块 WDK IEEE 1394 总线
- 非递增数据块 WDK IEEE 1394 总线
- 缓冲 WDK IEEE 1394 总线
- 总线重置生成 WDK IEEE 1394 总线
- 重置生成 WDK IEEE 1394 总线
- 锁定 WDK IEEE 1394 总线
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 418efa59f4c8fc3a8143510c6c0740d60d4af011
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72841526"
---
# <a name="sending-asynchronous-io-request-packets-on-the-ieee-1394-bus"></a>在 IEEE 1394 总线上发送异步 I/O 请求数据包





驱动程序使用请求\_ASYNC\_读取、请求\_异步\_写入，并请求\_ASYNC\_LOCK 向 IEEE 1394 总线上的设备发送异步读取、写入和锁定操作。 对于请求\_异步\_读取并请求\_异步\_写入，IRB 的操作特定参数存储在 IRB 的**AsyncWrite**成员中 **。**

### <a name="types-of-addressing"></a>寻址类型

发出异步 i/o 请求的驱动程序必须在 IRB 的**DestinationAddress**成员中指定类型为[**IO\_地址**](https://docs.microsoft.com/windows-hardware/drivers/ddi/1394/ns-1394-_io_address)的目标地址。 目标地址包括两个值：节点地址和地址偏移量。 总线驱动程序为这两个值提供的解释取决于启动请求的驱动程序所使用的寻址模式。

有三种寻址模式可用于发送异步1394包：*普通*、*原始*和*虚拟*。

在正常模式下，寻址启动请求的驱动程序提供了地址偏移，但未指定目标设备的节点地址。 总线驱动程序使用它在枚举设备时存储在设备的 PDO 中的信息来填充节点地址。

在 raw 模式寻址中，启动请求的驱动程序必须同时提供节点地址和地址偏移量。 另外，驱动程序必须将请求发送到主机控制器的 PDO，而不是向目标设备的 PDO 发送请求。 这会通知总线驱动程序它不应覆盖数据包中的节点地址信息。

在虚拟模式寻址中，启动请求的驱动程序必须在数据包中显式指示目标设备的节点地址，就像在 raw 模式寻址中一样。 不过，虚拟设备在1394总线上没有真实的节点地址。 虚拟设备的节点地址只是约定所建立的值，它允许虚拟设备的驱动程序标识其数据包。 正常操作时，虚拟设备驱动程序应接收在总线上广播的所有数据包，并通过在其中搜索包含其设备 "节点地址" 的预先设定值的数据包进行浏览。

启动虚拟设备请求的驱动程序无需采取任何特殊措施来阻止总线驱动程序覆盖数据包中记录的节点地址。 当总线驱动程序首次枚举虚拟设备时，它将在设备 PDO 的设备扩展中设置一个标志，指示该设备是虚拟设备。 接收到此设备的请求后，总线驱动程序能够验证它是否为虚拟设备，并且不会覆盖数据包中的节点地址。

### <a name="buffering-of-io-requests"></a>I/o 请求的缓冲

启动异步 i/o 请求的驱动程序必须提供指向指定 i/o 缓冲区的 MDL 的指针。 它将此指针放在 IRB 的**Mdl**成员中。 总线驱动程序使用此缓冲区从设备中复制其读取的数据，或将其写入设备的数据源复制到设备。

驱动程序在**AsyncXXX**的**nNumberOfBytesToRead**或**nNumberOfBytesToWrite**成员中指定数据缓冲区的大小，在**nBlockSize**成员中指定块大小。 事务实际发生时，总线驱动程序会将数据分解为在**nBlockSize**中指定的大小的数据包。 默认情况下，总线驱动程序会连续读取或写入数据：从设备的地址空间中的后续位置读取或写入数据块。

下图说明了连续的数据块。

![说明连续数据块的关系图](images/1394blkd.png)

或者，驱动程序可以为请求指定 ASYNC\_标志\_NONINCREMENTING 标志;然后，总线驱动程序将为每个块使用相同的地址集。

下图演示了异步的非递增数据块。

![阐释异步非递增数据块的关系图](images/1394blkf.png)

**警告**  总线驱动程序强制实施设备与计算机之间的当前连接速度的最大异步数据包大小，以及设备在其配置 ROM 的最大\_记录字段中报告的最大速度。 如果**nBlockSize**的值大于上述任一值，则总线驱动程序将为块大小使用强制值。 如果驱动程序将 ASYNC\_标志设置\_NONINCREMENTING 标志，则这不太可能会给出所需的行为。 如果驱动程序设置此标志，则在提交请求之前，它们应检查块大小是否小于强制限制。

 

### <a name="sending-lock-requests"></a>发送锁请求

IEEE 1394 总线协议提供了异步锁定请求，这些请求允许原子操作和设置类型操作。 除了指定目标地址（在**AsyncLock. DestinationAddress**中），驱动程序还指定事务类型（在**AsyncLock. fulTransactionType**中）。 操作的数据值和参数值（有关详细信息，请参阅 IEEE 1394 规范）会传入**AsyncLock. datavalue**和**AsyncLock**。

### <a name="bus-reset-generation"></a>总线重置生成

使用异步 i/o 的设备驱动程序将跟踪总线重置生成。 在每个异步请求中，设备驱动程序会在请求的 IRB 的**ulGeneration**成员中报告该值。 总线驱动程序将该值与实际生成计数进行比较，如果它们无法匹配，则将失败状态值为 "状态"\_无效\_生成的请求。 如果请求以这种方式失败，则驱动程序应通过使用\_请求\_生成\_计数总线请求来查询正确的生成。 但是，驱动程序不应重新发出通过此状态失败的任何请求，直到在其总线重置通知回调中检索到新代。 这可确保设备仍在总线上存在。 请注意，客户端驱动程序不应依赖于 IRP\_MN\_总线\_重置以获得总线重置通知。 在 Windows XP 及更高版本的操作系统中，IRP\_MN\_总线\_重置已过时。

 

 




