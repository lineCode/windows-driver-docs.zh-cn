---
title: DiskClassGuid
description: DiskClassGuid
ms.assetid: eeefc86c-caff-4d1c-b2c1-aefde3bd21ed
keywords:
- DiskClassGuid 设备和驱动程序安装
topic_type:
- apiref
api_name:
- DiskClassGuid
api_location:
- Ntddstor.h
api_type:
- HeaderDef
ms.localizationpriority: medium
ms.date: 10/17/2018
ms.openlocfilehash: 367437200cfa99ac950e99de587b6a39cdacecb7
ms.sourcegitcommit: fb7d95c7a5d47860918cd3602efdd33b69dcf2da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67375291"
---
# <a name="diskclassguid"></a>DiskClassGuid


DiskClassGuid 是硬盘的设备接口类的已过时标识符[存储设备](https://docs.microsoft.com/windows-hardware/drivers/storage/index)。 从 Microsoft Windows 2000 开始，使用[ **GUID_DEVINTERFACE_DISK** ](guid-devinterface-disk.md)此类的新实例的类标识符。

<a name="remarks"></a>备注
-------

存储[示例](https://go.microsoft.com/fwlink/p/?LinkId=618052)WDK 中包括[磁盘类驱动程序](https://go.microsoft.com/fwlink/p/?linkid=256103)示例并[Addfilter 存储筛选器工具](https://go.microsoft.com/fwlink/p/?linkid=256076)。 磁盘类驱动程序示例使用 DiskClassGuid 注册 GUID_DEVINTERFACE_DISK 设备接口类的实例。 示例 Addfilter 应用程序使用 DiskClassGuid 枚举 GUID_DEVINTERFACE_DISK 设备接口类的实例。

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Version</p></td>
<td align="left"><p>已过时。 从 Windows 2000 开始，请改用 GUID_DEVINTERFACE_DISK。</p></td>
</tr>
<tr class="even">
<td align="left"><p>Header</p></td>
<td align="left">Ntddstor.h （包括 Ntddstor.h）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>请参阅


[**GUID_DEVINTERFACE_DISK**](guid-devinterface-disk.md)

 

 






