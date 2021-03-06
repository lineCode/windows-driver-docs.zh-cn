---
title: CustomTimerDpc 例程注册和排队
description: CustomTimerDpc 例程注册和排队
ms.assetid: 884bff8e-8437-44fb-acc0-f535d64ce900
keywords:
- timer 对象 WDK 内核，CustomTimerDpc 例程
- CustomTimerDpc
- 队列计时器对象
- 正在注册 timer 对象
- KeSetTimer
- KeSetTimerEx
- KeInitializeTimer
- KeInitializeTimerEx
- 重复调用 CustomTimerDpc 例程
- 重复调用 CustomTimerDpc 例程
- DueTime 值
- 计时器过期 WDK 内核
- 过期计时器内核内核
- 计时器对象 WDK 内核，队列
- 计时器对象 WDK 内核，注册
- 计时器对象 WDK 内核，过期
ms.date: 06/16/2017
ms.localizationpriority: medium
ms.openlocfilehash: 0b5486063e486fc9e3aa6be9bdb93ab47429b62e
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72838475"
---
# <a name="registering-and-queuing-a-customtimerdpc-routine"></a>CustomTimerDpc 例程注册和排队





驱动程序可以通过调用以下例程（通常来自其[*AddDevice*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_add_device)例程）来注册[*CustomTimerDpc*](https://msdn.microsoft.com/library/windows/hardware/ff542983)例程：

1.  [**KeInitializeDpc**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-keinitializedpc)注册其例程

2.  用于设置 timer 对象的[**KeInitializeTimer**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-keinitializetimer)或[**KeInitializeTimerEx**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-keinitializetimerex)

随后，驱动程序可以调用[**KeSetTimer**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-kesettimer)或[**KeSetTimerEx**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-kesettimerex)来指定过期时间，并将 timer 对象添加到系统的计时器队列。 当达到过期时间时，系统将取消排队计时器对象并调用*CustomTimerDpc*例程。 下图说明了这些调用。

![说明如何对 customtimerdpc 例程使用 timer 和 dpc 对象的关系图](images/3ketmdpc.png)

如上图所示，驱动程序必须提供 DPC 对象和 timer 对象的存储。 大多数驱动程序在[设备扩展](device-extensions.md)或其他驱动程序分配的常驻内存中提供这些对象的存储。

在对**KeSetTimer**的调用中，驱动程序将指针传递给*Dpc*和*Timer*对象，以及以100毫微秒为单位表示的*DueTime* ，如上图所示。 *DueTime*的正值指定*绝对过期时间*（自1601年1月1日起），应在该时间调用*CustomTimerDpc*例程。 *DueTime*的负值指定了*相对到期时间*。

由于绝对计时器会在特定系统时间过期，因此，如果系统时间在计时器过期之前发生更改，则不会影响绝对计时器的等待持续时间。 另一方面，相对计时器始终在指定的时间单位数后过期，而不考虑对绝对系统时间的更改。

若要重复调用*CustomTimerDpc*例程，请使用**KeSetTimerEx**设置计时器，并在*Period*参数中指定重复间隔。 除了此附加参数外， **KeSetTimerEx**与**KeSetTimer**类似。

如上图所示，对**KeSetTimer**或**KeSetTimerEx**的调用将计时器对象按指定间隔排队，如下所示：

1.  当*DueTime*过期时，将取消排队对象并将其设置为终止状态。

2.  如果计算机中的每个处理器当前正在运行的代码的 IRQL 大于或等于调度\_级别，则与计时器对象关联的 DPC 对象将被放入 DPC 队列。 否则，将调用*CustomTimerDpc*例程。

3.  如果在*DueTime*间隔过期时，DPC 对象已在队列中，则会在计算机中的任何处理器上的 IRQL 降到下面时立即调用*CUSTOMTIMERDPC*例程\_级别。

    **请注意**  *CustomTimerDpc*例程（如所有 DPC 例程）在 IRQL = 调度\_级别调用。 当 DPC 例程运行时，将阻止所有线程在同一处理器上运行。 驱动程序开发人员应认真设计其*CustomTimerDpc*例程，使其尽可能简短地运行。

     

可以指定为**KeSetTimer**和**KeSetTimerEx**的最小时间间隔大约为10毫秒，因此，当时间间隔比[*IoTimer*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-io_timer_routine)例程（每秒运行一次）可以处理时，驱动程序可以使用*CustomTimerDpc*例程。

在任何时刻，只能对特定计时器对象的一个实例化进行排队。 再次调用具有相同*计时器*对象指针的**KeSetTimer**或**KeSetTimerEx**会取消排队计时器对象并将其重置。

设置[*CustomTimerDpc*](https://msdn.microsoft.com/library/windows/hardware/ff542983)例程的方式与设置[*CustomDpc*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-kdeferred_routine)例程的方式完全相同，并附加了一个初始化 timer 对象的步骤。 事实上，它们的原型是相同的，但*CustomTimerDpc*例程不能使用在其原型中声明的两个*SystemArgument*指针。

 

 




