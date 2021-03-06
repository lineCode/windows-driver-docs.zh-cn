---
title: SCSI 端口的可以与存储类驱动程序交互的 SRB 接口
description: SCSI 端口的可以与存储类驱动程序交互的 SRB 接口
ms.assetid: ca30bf9b-6d76-4160-8a4e-54c681dfc843
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 10fc2d1bbe9fca50459c7aa8e38b849b5de66817
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72842653"
---
# <a name="scsi-ports-srb-interface-with-the-storage-class-driver"></a>SCSI 端口的可以与存储类驱动程序交互的 SRB 接口


## <span id="ddk_scsi_ports_srb_interface_with_the_storage_class_driver_kg"></span><span id="DDK_SCSI_PORTS_SRB_INTERFACE_WITH_THE_STORAGE_CLASS_DRIVER_KG"></span>


存储类驱动程序和其他较高级别的组件通过构建 SCSI 请求块（SRBs）与 SCSI 端口驱动程序通信。 有关 SRBs 的详细信息，请参阅[**SCSI\_REQUEST\_BLOCK**](https://docs.microsoft.com/windows-hardware/drivers/ddi/srb/ns-srb-_scsi_request_block)。 存储类驱动程序将其创建的 SRBs 传递给 IRP 中的 SCSI 端口，并将**MajorFunction**成员设置为 IRP\_MJ\_SCSI。 有关存储类驱动程序在将 SRB 传递到端口驱动程序之前必须执行的步骤的说明，请参阅[存储类驱动程序的 BuildRequest 例程](storage-class-driver-s-buildrequest-routine.md)。

在堆栈中转发 SRB 之前，SCSI 端口会在 SRB 中设置某些值，例如端口号、路径、目标号码和目标设备的逻辑单元号。

与其他端口驱动程序不同，如系统提供的用于 IDE/ATAPI 和 IEEE 1394 总线的端口驱动程序，SCSI 端口无需将其接收的 SRBs 中的命令描述符块（CDB）转换为不同的格式，然后将其转发到基础适配器。 SCSI 端口只是将一些特定于目标的信息添加到 SRB，并将其传递到具有更改的 CDB 的微型端口驱动程序。 因此，SCSI 端口只是传递包含 CDBs 的 SRBs 的信使。

出于此原因，存储类驱动程序和 SCSI 端口之间的 SRB 接口的大多数方面都涵盖在存储类和存储微型端口驱动程序的一般文档及其随附的参考资料中。 有关存储类驱动程序与 SCSI 端口-微型端口驱动程序对之间的 SRB 接口相关的章节列表，请参阅[使用 scsi 端口微型端口驱动程序的 Scsi 端口接口](scsi-port-s-interface-with-scsi-port-miniport-drivers.md)。

 

 




