---
title: 使用 Serial.sys 和 Serenum.sys
description: 使用 Serial.sys 和 Serenum.sys
ms.assetid: 2dcf22c8-0666-4b58-8fd3-97a4d17eaa2a
keywords:
- 串行端口 WDK
- 串行设备 WDK
- 端口 WDK、串行
- 通用异步接收器-发射器 WDK 串行设备
- UART WDK 串行设备
- 函数驱动程序 WDK 串行端口
- 串行驱动程序 WDK
- 16550 UART 兼容的接口 WDK 串行设备
- 较低级别的设备筛选器驱动程序 WDK 串行设备
- 更高级的设备筛选器驱动程序 WDK 串行设备
- 筛选器驱动程序 WDK 串行设备
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: cad3d6fc38a5d257cadcebfbf49de09901e72aba
ms.sourcegitcommit: 6b09412f7bf562f7c01ffa94ac44a3d0ea895e3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086715"
---
# <a name="using-serialsys-and-serenumsys"></a>使用 Serial.sys 和 Serenum.sys

以下系统组件可与串行控制器设备一起使用，这些设备具有与16550通用异步接收方（UART）兼容的硬件接口：

-   串行和 Serenum 驱动程序

    Serial （串行）是系统提供的串行设备函数驱动程序。 对于需要 16550 UART 兼容接口的任何类型的即插即用设备，你还可以使用串行作为较低级别的设备筛选器驱动程序。

    Serenum （Serenum）是系统提供的一级设备筛选器驱动程序，可与串行（或供应商提供的函数驱动程序）结合使用，为 RS-232 端口提供即插即用总线驱动程序的功能。

    有关串行和 Serenum 操作的详细信息，请参阅以下主题：

    - [串行控制器驱动程序概述](serial-drivers-overview.md)
    - [串行和 Serenum 的功能](features-of-serial-and-serenum.md)
    - [串行设备和驱动程序的配置](configuration-of-serial-devices-and-drivers.md)
    - [Serenum 和串行的操作](operation-of-serenum-and-serial.md)
    - [用于串行的注册表设置](registry-settings-for-serial.md)
    - [用于 Serenum 的注册表设置](registry-settings-for-serenum.md)
    - [串行驱动程序参考](https://docs.microsoft.com/windows-hardware/drivers/ddi/index)
    - [Serenum 驱动程序参考](https://docs.microsoft.com/windows-hardware/drivers/ddi/index)
    - WDK 中 Ntddser 标头文件中的数据定义。

<!-- -->

- 端口[设备安装程序类](https://docs.microsoft.com/windows-hardware/drivers/install/device-setup-classes)

    端口类包括*串行端口*和*COM 端口*。 串行端口是 16550 UART 或兼容设备上的串行通信硬件接口。 计算机上的 RS-232 端口通常是一个 DB-9 或 DB-25 连接器，它在一个 UART 上通过电连接到串行端口。 COM 端口是符合其他特定于 Windows 的要求的串行端口。 有关详细信息，请参阅[COM 端口的配置](configuration-of-com-ports.md)。

- COM 端口[设备接口类](https://docs.microsoft.com/windows-hardware/drivers/install/device-interface-classes)

    必须使用 COM 端口设备接口访问 COM 端口。 （COM 端口设备接口类的 GUID 是[**\_guid DEVINTERFACE\_COMPORT**](https://docs.microsoft.com/windows-hardware/drivers/install/guid-devinterface-comport)。）

- [Com 端口数据库](com-port-database.md)和[com 端口数据库支持例程](https://docs.microsoft.com/windows-hardware/drivers/ddi/index)

    Com 端口数据库仲裁 com 端口使用 COM 端口号。

有关安装串行设备的信息，请参阅[安装串行设备](installing-serial-devices.md)。

有关串行设备的高级操作的常规信息，请参阅 Microsoft Windows SDK 中 Windows 基础服务支持的通信资源的相关信息。

## <a name="serial-driver-samples"></a>串行驱动程序示例

这些示例演示了串行驱动程序。

- [串行](https://github.com/Microsoft/Windows-driver-samples/tree/master/serial/serial)示例为串行设备生成函数驱动程序。
- [Serenum](https://github.com/Microsoft/Windows-driver-samples/tree/master/serial/serenum)示例为 RS-232 端口提供了总线驱动程序的即插即用功能。
- 简单的虚拟串行驱动程序（ComPort）和无控制器的调制解调器驱动程序（FakeModem）。
    -   [虚拟串行驱动程序示例（UMDF 1.0）](https://github.com/Microsoft/Windows-driver-samples/tree/master/serial/VirtualSerial)
    -   [Virtual serial2 driver 示例（KMDF）](https://github.com/Microsoft/Windows-driver-samples/tree/master/serial/VirtualSerial2)
