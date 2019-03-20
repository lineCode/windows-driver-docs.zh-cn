---
title: 电视连接器和视频微型端口驱动程序中的复制保护
description: 电视连接器和视频微型端口驱动程序中的复制保护支持
ms.assetid: 7d7d44b5-3248-4bee-bc4d-e02fd3c606a7
keywords:
- 微型端口驱动程序 WDK Windows 2000，电视连接器
- 微型端口驱动程序 WDK Windows 2000 中，复制保护支持
- 电视连接器 WDK 微型端口
- 复制保护 WDK 视频微型端口
- IOCTL_VIDEO_HANDLE_VIDEOPARAMETERS
- 复制保护 WDK 微型端口，有关复制保护的支持
- 硬件 WDK 复制保护
ms.date: 12/06/2018
ms.localizationpriority: medium
ms.custom: seodec18
ms.openlocfilehash: 07a3751eaa0e47f25cb997ed32dacb577a25385d
ms.sourcegitcommit: a33b7978e22d5bb9f65ca7056f955319049a2e4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "56547630"
---
# <a name="tv-connector-and-copy-protection-support-in-video-miniport-drivers"></a>电视连接器和视频微型端口驱动程序中的复制保护支持

有一个电视连接器的适配器的微型端口驱动程序必须处理[ **VRPs** ](https://msdn.microsoft.com/library/windows/hardware/ff570547)与[ **IOCTL\_视频\_句柄\_VIDEOPARAMETERS** ](https://msdn.microsoft.com/library/windows/hardware/ff567805) I/O 控制代码。 此 IOCTL 发送到要查询的功能和电视连接器和复制保护硬件的当前设置，或者设置的电视连接器和复制保护的硬件功能的微型端口驱动程序。 微型端口驱动程序确定要通过检查来执行的操作**dwCommand**字段[ **VIDEOPARAMETERS** ](https://msdn.microsoft.com/library/windows/hardware/ff570173) VRP 的中传递的结构**InputBuffer**。 系统将不允许微型端口驱动程序不处理此 VRP 如果 Rovi (以前称为 Macrovision) 播放受保护的 Dvd。

如果**dwCommand**设置为副总裁\_命令\_GET，以及设备*不*支持电视输出，然后微型端口驱动程序不应返回任何\_中的错误**状态**隶属 VRP **StatusBlock**。 它还应设置**信息**VRP 成员**StatusBlock**大小，以字节为单位，VIDEOPARAMETERS 的结构。 应设置**dwFlags**为零，设置**dwTVStandard**到副总裁\_电视\_标准\_赢取\_VGA，并设置**dwAvailableTVStandard**到副总裁\_电视\_标准\_赢取\_VGA。

如果**dwCommand**设置为副总裁\_命令\_GET，以及设备*does*支持电视扩展微型端口驱动程序应指示这一点 VIDEOPARAMETERS 结构中设置相应的标志**dwFlags**成员并通过将值分配给其他设置标志与对应的结构成员。

以下各节提供了电视连接器的设备的微型端口驱动程序的实现详细信息：

[查询电视连接器和复制保护硬件](querying-tv-connector-and-copy-protection-hardware.md)

[设置复制保护硬件](setting-copy-protection-hardware.md)

 

 




