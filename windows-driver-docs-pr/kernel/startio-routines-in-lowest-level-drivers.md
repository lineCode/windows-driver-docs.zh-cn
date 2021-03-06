---
title: 最低级驱动程序中的 StartIo 例程
description: 最低级驱动程序中的 StartIo 例程
ms.assetid: f79f8929-bcf4-46a2-bf0e-0f8fb0720dd9
keywords:
- StartIo 例程，最低级别驱动程序
- I/o 控制请求 WDK 内核
- 缓冲 i/o WDK 内核
- 直接 i/o WDK 内核
- 同步 WDK Irp
ms.date: 06/16/2017
ms.localizationpriority: medium
ms.openlocfilehash: db7a9cd9c287fa5e45cfbd9f884da4bb827ac549
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72836234"
---
# <a name="startio-routines-in-lowest-level-drivers"></a>最低级驱动程序中的 StartIo 例程





I/o 管理器对驱动程序的调度例程的调用是满足设备 i/o 请求的第一阶段。 [*StartIo*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_startio)例程为第二个阶段。 每个使用*StartIo*例程的设备驱动程序都可能从其[*DispatchRead*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)和[*DispatchWrite*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)例程调用[**IoStartPacket**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-iostartpacket) ，并且通常用于其[*DispatchDeviceControl*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)例程中支持的 i/o 控制代码子集。 **IoStartPacket**例程将 IRP 添加到设备的系统提供的设备队列中; 如果队列为空，则立即调用驱动程序的*StartIo*例程来处理 IRP。

可以假设在调用驱动程序的*StartIo*例程时，目标设备不忙。 这是因为，i/o 管理器在两个情况下调用*StartIo* ;其中一个驱动程序的调度例程刚刚称为**IoStartPacket** ，而设备队列为空，或者该驱动程序的[*DpcForIsr*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-io_dpc_routine)例程正在完成另一个请求，并且刚刚调用[**IOSTARTNEXTPACKET**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-iostartnextpacket)来取消下一个 IRP 的排队。

在调用最高级的设备驱动程序中的*StartIo*例程之前，该驱动程序的调度例程应已探测并锁定用户缓冲区（如有必要），以将 IRP 中的有效映射缓冲区地址设置为排入队列的*StartIo*例程. 如果最高级别的设备驱动程序为直接 i/o （或非缓冲和直接 i/o）设置其设备对象，则驱动程序无法将用户缓冲区锁定到其*StartIo*例程;每个*StartIo*例程都在任意线程上下文中以 IRQL = 调度\_级别进行调用。

**请注意**   要由驱动程序的*StartIo*例程访问的任何缓冲内存都必须锁定或从常驻系统空间内存中分配，并且必须可在任意线程上下文中访问。

 

通常，任何较低级别的设备驱动程序的*StartIo*例程都负责将[**IOGETCURRENTIRPSTACKLOCATION**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iogetcurrentirpstacklocation)与输入 IRP 一起调用，然后执行任何特定于请求的处理，以便在其上启动 i/o 操作装置. 特定于请求的处理可以包括以下内容：

-   设置或更新有关驱动程序所维护的当前请求的任何状态信息。 状态信息可以存储在目标设备对象的设备扩展或驱动程序分配的非分页池中。

    例如，如果设备驱动程序为当前传输操作维护 InterruptExpected 的布尔值，则其*StartIo*例程可能将此变量设置为**TRUE**。 如果驱动程序为当前操作维护超时计数器，则其*StartIo*例程可能会设置此值，否则*StartIo*例程可能会对驱动程序的[*CustomTimerDpc*](https://msdn.microsoft.com/library/windows/hardware/ff542983)例程进行排队。

    如果*StartIo*例程与其他驱动程序例程共享对状态信息或[硬件资源](hardware-resources.md)的访问，则必须通过旋转锁保护状态信息或资源。 （请参阅[自旋锁](spin-locks.md)。）

    如果*StartIo*例程使用驱动程序的[*InterruptService*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-kservice_routine)例程共享对状态信息或资源的访问权限，则*StartIo*必须使用[**KeSynchronizeExecution**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-kesynchronizeexecution)调用访问状态或资源信息的[*SynchCritSection*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-ksynchronize_routine)例程。 （请参阅[使用关键部分](using-critical-sections.md)。）

-   将序列号分配给 IRP，以防驱动程序在处理 IRP 时必须记录设备 i/o 错误。

    有关详细信息，请参阅[日志记录错误](logging-errors.md)。

-   如有必要，将驱动程序的 i/o 堆栈位置中的参数转换为特定于设备的值。

    例如，磁盘驱动程序可能需要计算传输操作的物理磁盘地址的起始扇区或字节偏移量，以及请求的传输长度是否跨越特定扇区边界，或超出其物理设备。

-   如果驱动程序控制可移动媒体设备，则在对设备进行 i/o 编程之前检查媒体更改，并在媒体发生更改时通知其过量文件系统。

    有关详细信息，请参阅[支持可移动介质](supporting-removable-media.md)。

-   如果设备使用 DMA，请检查是否应将请求的**长度**（在驱动程序的 i/o 堆栈位置中找到的字节数）拆分为部分传输操作，如[输入/输出技术](i-o-programming-techniques.md)中所述。假设一个紧密耦合的高级驱动程序不会 presplit 设备驱动程序的大型传输。

    此类设备驱动程序的*StartIo*例程还可以负责调用[**KeFlushIoBuffers**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-keflushiobuffers) ，如果驱动程序使用基于数据包的 DMA，则使用驱动程序的[*AdapterControl*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_control)例程调用[**AllocateAdapterChannel**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-pallocate_adapter_channel) 。

    有关更多详细信息，请参阅[适配器对象和 DMA](adapter-objects-and-dma.md)并[维护缓存一致性](maintaining-cache-coherency.md)。

-   如果设备使用 PIO，请将 irp 的基本虚拟地址（irp 中的 IRP **&gt;MdlAddress**）映射到带有[**MmGetSystemAddressForMdlSafe**](https://docs.microsoft.com/windows-hardware/drivers/kernel/mm-bad-pointer)的系统空间地址。

    对于读取请求，设备驱动程序的*StartIo*例程可负责在 PIO 操作开始之前调用[**KeFlushIoBuffers**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-keflushiobuffers) 。 有关详细信息，请参阅[维护缓存一致性](maintaining-cache-coherency.md)。

-   如果非 WDM 驱动程序使用控制器对象，请调用[**IoAllocateController**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddk/nf-ntddk-ioallocatecontroller)来注册其[*ControllerControl*](https://msdn.microsoft.com/library/windows/hardware/ff542049)例程。

-   如果驱动程序处理可取消的 Irp，请检查输入 IRP 是否已被取消。

-   如果在将输入 IRP 处理为完成前可以将其取消，则*StartIo*例程必须使用 IRP 和驱动程序[*取消*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_cancel)例程的入口点调用[**IoSetCancelRoutine**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iosetcancelroutine) 。 *StartIo*例程必须获取取消自旋锁，才能调用**IoSetCancelRoutine**。 或者，驱动程序可以使用[**IoSetStartIoAttributes**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-iosetstartioattributes)将*StartIo*例程的*NonCancelable*属性设置为**TRUE**。 这会阻止系统尝试取消通过调用[**IoStartPacket**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-iostartpacket)传递给*StartIo*的 IRP。

作为一般规则，使用缓冲 i/o 的驱动程序比使用直接 i/o 的驱动*程序更简单*。 使用缓冲 i/o 的驱动程序传输少量数据用于每个传输请求，而使用直接 i/o 的驱动程序（无论是 DMA 还是 PIO）会将大量数据传输到可跨系统内存中的物理页边界的锁定缓冲区。

上层于物理设备驱动程序的较高级别的驱动程序通常会设置其设备对象，使其与各自的设备驱动程序的设备对象相匹配。 然而，最高级别的驱动程序（特别是文件系统驱动程序）可为直接或非缓冲 i/o 设置设备对象。

为缓冲 i/o 设置其设备对象的驱动程序可以依赖于 i/o 管理器在它发送到驱动程序的所有 Irp 中传递有效缓冲区。 为直接 i/o 设置设备对象的较低级驱动程序可以依赖于其链中的最高级别的驱动程序，以将通过任何中间驱动程序发送的所有 Irp 中的有效缓冲区传递到底层的低级设备驱动程序。

### <a name="using-buffered-io-in-startio-routines"></a>在 StartIo 例程中使用缓冲 i/o

如果驱动程序的[*DispatchRead*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)、 [*DispatchWrite*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)或[*DispatchDeviceControl*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)例程确定请求有效并且调用[**IoStartPacket**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-iostartpacket)，则 I/o 管理器会调用驱动程序的[*StartIo*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_startio)例程来处理 IRP如果设备队列为空，则为。 如果队列不为空，则**IoStartPacket**会将 IRP 排队。 最终，从驱动程序的[*DpcForIsr*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-io_dpc_routine)或[*CustomDpc*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-kdeferred_routine)例程调用[**IoStartNextPacket**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-iostartnextpacket)会导致 i/o 管理器取消对 IRP 的排队并调用驱动程序的*StartIo*例程。

*StartIo*例程调用[**IoGetCurrentIrpStackLocation**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iogetcurrentirpstacklocation) ，并确定必须执行哪个操作才能满足请求。 它在对物理设备进行编程以执行 i/o 请求之前，以任何必要的方式预处理 IRP。

如果对物理设备（或设备扩展）的访问必须与[*InterruptService*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-kservice_routine)例程同步，则*StartIo*例程必须调用[*SynchCritSection*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-ksynchronize_routine)例程来执行必要的设备编程。 有关详细信息，请参阅[使用关键部分](using-critical-sections.md)。

使用缓冲 i/o 的物理设备驱动程序会将数据传入或传出由 i/o 管理器分配的系统空间缓冲区，驱动程序将在&gt;每个 irp 中的**AssociatedIrp SystemBuffer**中找到。

### <a name="using-direct-io-in-startio-routines"></a>在 StartIo 例程中使用直接 i/o

如果驱动程序的[*DispatchRead*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)、 [*DispatchWrite*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)或[*DispatchDeviceControl*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)例程确定请求有效并且调用[**IoStartPacket**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-iostartpacket)，则 I/o 管理器会调用驱动程序的*StartIo*例程来处理 IRP如果设备队列为空，则为。 如果队列不为空，则**IoStartPacket**会将 IRP 排队。 最终，从驱动程序的[*DpcForIsr*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-io_dpc_routine)或[*CustomDpc*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-kdeferred_routine)例程调用[**IoStartNextPacket**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-iostartnextpacket)会导致 i/o 管理器取消对 IRP 的排队并调用驱动程序的*StartIo*例程。

*StartIo*例程调用[**IoGetCurrentIrpStackLocation**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iogetcurrentirpstacklocation) ，并确定必须执行哪个操作才能满足请求。 它以任何必要的方式预处理 IRP，如将大型 DMA 传输请求拆分为部分传输范围，并保存与必须拆分的传入传输请求的**长度**有关的状态。 然后，它会对物理设备进行计划，以执行 i/o 请求。

如果对物理设备（或设备扩展）的访问必须与驱动程序的 ISR 同步，则*StartIo*例程必须使用驱动程序提供的*SynchCritSection*例程来执行必要的编程。 有关详细信息，请参阅[使用关键部分](using-critical-sections.md)。

使用直接 i/o 的任何驱动程序都可以将数据读入或写入锁定的缓冲区中的数据，这些数据由内存描述符列表（MDL）描述，驱动程序将在**irp 的 irp&gt;MdlAddress**中查找。 此类驱动程序通常使用缓冲 i/o 处理设备控制请求。 有关详细信息，请参阅[处理 StartIo 例程中的 I/o 控制请求](#ddk-handling-i-o-control-requests-in-startio-routines-kg)。

MDL 类型是驱动程序无法直接访问的不透明类型。 相反，使用 PIO 的驱动程序通过使用**Irp&gt;MdlAddress**作为参数调用[**MmGetSystemAddressForMdlSafe**](https://docs.microsoft.com/windows-hardware/drivers/kernel/mm-bad-pointer)来重新映射用户空间缓冲区。 使用 DMA 的驱动程序还会通过**Irp&gt;MdlAddress**来支持在其传输操作中使用例程，从而将缓冲区地址重新映射到其设备的逻辑范围。

除非紧密耦合的高级驱动程序拆分了对基础设备驱动程序的大型 DMA 传输请求，否则最低级别的设备驱动程序的*StartIo*例程必须拆分大于其设备可以在单个传输操作。 需要使用系统 DMA 的驱动程序拆分传输请求，这些请求对于系统 DMA 控制器而言太大或其设备无法在单个传输操作中进行处理。

如果设备是从属 DMA 设备，则其驱动程序必须通过系统 DMA 控制器同步传输，其中包含驱动程序分配的适配器对象（表示 DMA 通道）和驱动程序提供的[*AdapterControl*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_control)例程。 总线主机 DMA 设备的驱动程序还必须使用驱动程序分配的适配器对象来同步其传输，如果它使用系统的基于数据包的 DMA 支持，则必须提供*AdapterControl* [*例程;* ](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_list_control)如果它使用系统的散播/聚集支持。

根据驱动程序的设计，它可能会在具有控制器对象的物理设备上同步传输和设备控制操作，并提供[*ControllerControl*](https://msdn.microsoft.com/library/windows/hardware/ff542049)例程。

有关详细信息，请参阅[适配器对象、DMA](adapter-objects-and-dma.md)和[控制器对象](using-controller-objects.md)。

### <a href="" id="ddk-handling-i-o-control-requests-in-startio-routines-kg"></a>处理 StartIo 例程中的 i/o 控制请求

通常，仅从驱动程序的[*DispatchDeviceControl*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)或[*DispatchInternalDeviceControl*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)例程传递设备 i/o 控制请求的一个子集，以便驱动程序的*StartIo*例程进一步进行处理。 驱动程序的*StartIo*例程仅需处理要求设备状态更改或返回有关当前设备状态的可变信息的有效设备控制请求。

对于同一种类的设备，每个新驱动程序必须支持同一组公共 i/o 控制代码，作为其他所有驱动程序。 系统为 IRP\_MJ 定义特定于设备类型的特定 i/o 控制代码， [ **\_设备\_控制**](https://docs.microsoft.com/windows-hardware/drivers/kernel/irp-mj-device-control)请求作为缓冲的请求。

因此，物理设备驱动程序会将数据传入或传出系统空间缓冲区，每个驱动程序都在 irp 中的 irp **-&gt;AssociatedIrp SystemBuffer**请求设备控制请求。 即使是为直接 i/o 设置设备对象的驱动程序，也会使用缓冲 i/o 来满足使用公共 i/o 控制代码的设备控制请求。

每个 i/o 控制代码的定义确定是否对为该请求传输的数据进行缓冲处理。 针对特定于驱动程序的[**IRP\_MJ\_内部\_设备\_控制**](https://docs.microsoft.com/windows-hardware/drivers/kernel/irp-mj-internal-device-control)成对驱动程序之间的所有特定于驱动程序的 i/o 控制代码，可以使用缓存方法、方法直接方法或方法来定义代码。 作为一般规则，如果紧耦合的更高级别驱动程序必须为该请求分配一个缓冲区，则应使用方法定义任何私下定义的 i/o 控制代码。

### <a name="programming-the-device-for-io-operations"></a>为 i/o 操作编程设备

通常，最低级别设备驱动程序中的*StartIo*例程必须使用[**KeSynchronizeExecution**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-kesynchronizeexecution)调用驱动程序提供的[*SynchCritSection*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-ksynchronize_routine) ，同步对其与驱动程序 ISR 共享的任何内存或设备寄存器的访问权限例程. 驱动程序的*StartIo*例程使用*SYNCHCRITSECTION*例程在 DIRQL 处实际对物理设备进行 i/o 编程。 有关详细信息，请参阅[使用关键部分](using-critical-sections.md)。

在调用**KeSynchronizeExecution**之前， *StartIo*例程必须执行请求所需的任何预处理。 预处理可能包括计算初始部分传输范围并保存与其他驱动程序例程的原始请求有关的任何状态信息。

如果设备驱动程序使用 DMA，则其*StartIo*例程通常使用驱动程序提供的[*AdapterControl*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_control)例程调用[**AllocateAdapterChannel**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-pallocate_adapter_channel) 。 在这些情况下， *StartIo*例程推迟了将物理设备编程到*AdapterControl*例程的责任。 然后，它可以调用**KeSynchronizeExecution** ，使驱动程序提供的*SynchCritSection*例程对设备进行 DMA 传输。

 

 




