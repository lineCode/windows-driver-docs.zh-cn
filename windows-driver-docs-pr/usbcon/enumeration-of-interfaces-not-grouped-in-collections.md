---
Description: 复合 USB 设备上的接口可分组到集合中或分别代表一个 USB 函数。
title: 枚举 USB 复合设备上的接口
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 38145018b08f977bb0aa862a2980e1b8fa3dc2c3
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72845012"
---
# <a name="enumeration-of-interfaces-on-usb-composite-devices"></a>枚举 USB 复合设备上的接口


复合 USB 设备上的接口可分组到集合中或分别代表一个 USB 函数。 如果接口未分组在集合中，则通用父驱动程序会为每个接口创建一个 PDO，并为每个 PDO 生成一组硬件 Id。

接口 PDO 的*设备 ID*具有以下格式：

`USB\VID_v(4)&PID_p(4)&MI_z(2)`

在这些 Id 中：

-   *v （4）* 是 USB 标准委员会分配给供应商的四位供应商代码。
-   *p （4）* 是供应商分配给设备的四位数产品代码。
-   *z （2）* 是从接口描述符的**bInterfaceNumber**字段中提取的接口号。

一般父驱动程序还通过使用接口描述符中的信息（[**USB\_接口\_描述符**](https://docs.microsoft.com/windows-hardware/drivers/ddi/usbspec/ns-usbspec-_usb_interface_descriptor)）生成以下兼容的 id：

`USB\CLASS_d(2)&SUBCLASS_s(2)&PROT_p(2)`

`USB\CLASS_d(2)&SUBCLASS_s(2)`

`USB\CLASS_d(2)`

在这些 Id 中：

-   *d （2）* 是类代码（**bInterfaceClass**）
-   *s （2）* 是子类代码（**bInterfaceSubClass**）
-   *p （2）* 是协议代码（**bInterfaceProtocol**）

其中每个代码均为四位数字。

## <a name="related-topics"></a>相关主题
[USB 复合设备上的接口集合枚举](support-for-interface-collections.md)  
[USB 通用父驱动程序（Usbccgp）](usb-common-class-generic-parent-driver.md)  
[Microsoft 提供的 USB 驱动程序](system-supplied-usb-drivers.md)  



