---
title: 关闭通信服务器端口
description: 关闭通信服务器端口
ms.assetid: 43dfa162-0098-4a9b-9272-9da429cb0108
keywords:
- 通信服务器端口 WDK 文件系统微筛选器
- 端口 WDK，文件系统微筛选器
- 关闭通信服务器端口
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 500988fc7b6ea75e4454231b5b3bc6bd2adab225
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72841475"
---
# <a name="closing-the-communication-server-port"></a>关闭通信服务器端口


## <span id="ddk_closing_the_communication_server_port_if"></span><span id="DDK_CLOSING_THE_COMMUNICATION_SERVER_PORT_IF"></span>


如果微筛选器驱动程序以前通过调用[**FltCreateCommunicationPort**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nf-fltkernel-fltcreatecommunicationport)打开了内核模式通信服务器端口，则它必须通过调用[**FltCloseCommunicationPort**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nf-fltkernel-fltclosecommunicationport)来关闭端口。 若要防止系统在卸载过程中挂起，微筛选器驱动程序的[**FilterUnloadCallback**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nc-fltkernel-pflt_filter_unload_callback)例程必须在调用[**FltUnregisterFilter**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nf-fltkernel-fltunregisterfilter)之前关闭此端口。

如果用户模式应用程序打开了到通信服务器端口的连接，则在[**FltCloseCommunicationPort**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nf-fltkernel-fltclosecommunicationport)返回后，该连接的任何客户端端口都将保持打开状态。 不过，筛选器管理器将在卸载微筛选器驱动程序时关闭任何客户端端口。

 

 




