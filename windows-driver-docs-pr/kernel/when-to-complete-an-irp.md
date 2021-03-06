---
title: 何时完成 IRP
description: 何时完成 IRP
ms.assetid: 6986b24c-e7e5-43f2-861d-b84e4c131a8a
keywords:
- 完成 Irp WDK 内核，何时完成
ms.date: 06/16/2017
ms.localizationpriority: medium
ms.openlocfilehash: 91dda582bc5e149acaecf379710269f286f67d86
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72835730"
---
# <a name="when-to-complete-an-irp"></a>何时完成 IRP





满足以下任一条件时，驱动程序应启动 IRP 完成：

-   驱动程序确定 IRP 处理由于无效参数或其他条件而无法进行。

-   驱动程序能够处理请求的 i/o 操作，而不会将 IRP 向下传递到驱动程序堆栈，并且操作已完成。

-   IRP 正在取消。 （请参阅[取消 irp](canceling-irps.md)。）

如果未满足这些条件，则驱动程序的调度例程必须将 IRP 向下传递到下一个较低版本的驱动程序，或者必须处理 i/o 请求的处理。 如果满足其中一个条件，则驱动程序必须调用[**IoCompleteRequest**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iocompleterequest)。

如果驱动程序完成了请求，因为处理不能进行，或者它通过处理请求的操作（而不实际访问设备）来完成请求，则通常从其一个调度例程调用**IoCompleteRequest** 。 有关详细信息，请参阅[在调度例程中完成 irp](completing-irps-in-dispatch-routines.md)。

如果驱动程序必须访问设备以满足请求，则通常从[*DpcForIsr*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-io_dpc_routine)例程调用**IoCompleteRequest** 。 [服务中断](servicing-interrupts.md)中广泛讨论了这些例程。

 

 




