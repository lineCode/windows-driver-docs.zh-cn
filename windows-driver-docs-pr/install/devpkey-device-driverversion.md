---
title: DEVPKEY_Device_DriverVersion
description: DEVPKEY_Device_DriverVersion
ms.assetid: 68df1313-e948-4aea-9b90-c838f7bf228d
keywords:
- DEVPKEY_Device_DriverVersion 设备和驱动程序安装
topic_type:
- apiref
api_name:
- DEVPKEY_Device_DriverVersion
api_location:
- Devpkey.h
api_type:
- HeaderDef
ms.localizationpriority: medium
ms.date: 10/17/2018
ms.openlocfilehash: f87da401a83b912678cf068447a6a82351897a08
ms.sourcegitcommit: fb7d95c7a5d47860918cd3602efdd33b69dcf2da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67378254"
---
# <a name="devpkeydevicedriverversion"></a>DEVPKEY_Device_DriverVersion


PKEY_Device_DriverVersion 设备属性表示版本的设备实例当前安装的驱动程序。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>属性键</strong></p></td>
<td align="left"><p>DEVPKEY_Device_DriverVersion</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>属性数据类型标识符</strong></p></td>
<td align="left"><p><a href="devprop-type-string.md" data-raw-source="[&lt;strong&gt;DEVPROP_TYPE_STRING&lt;/strong&gt;](devprop-type-string.md)"><strong>DEVPROP_TYPE_STRING</strong></a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>属性访问</strong></p></td>
<td align="left"><p>通过安装应用程序和安装程序的只读访问权限</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>对应的注册表值标识符和注册表值名称</strong></p></td>
<td align="left"><p>REGSTR_VAL_DRIVERVERSION</p>
<p><strong>DriverVersion</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>本地化？</strong></p></td>
<td align="left"><p>否</p></td>
</tr>
</tbody>
</table>

 

<a name="remarks"></a>备注
-------

由提供的值 DEVPKEY_Device_DriverVersion [ **INF DriverVer 指令**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-driverver-directive)包含在[ **INF 版本部分**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-version-section)INF 文件的安装在设备或提供的特定于设备 INF **DriverVer**中包含的指令[ **INF *DDInstall*部分**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-ddinstall-section)用于安装设备。

您可以调用[ **SetupDiGetDeviceProperty** ](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupdigetdevicepropertyw)检索 PKEY_Device_DriverVersion 值。

Windows Server 2003、 Windows XP 和 Windows 2000 支持此属性，但不是支持 DEVPKEY_Device_DriverVersion 属性键。 在这些早期版本的 Windows 中，您可以访问此属性的值，通过访问对应**DriverVersion**设备实例软件项下的注册表值。 有关如何访问这些早期版本的 Windows 上此属性的值的信息，请参阅[访问设备驱动程序属性](https://docs.microsoft.com/windows-hardware/drivers/install/accessing-device-driver-properties)。

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
<td align="left"><p>在 Windows Vista 和更高版本的 Windows 中可用。</p></td>
</tr>
<tr class="even">
<td align="left"><p>Header</p></td>
<td align="left">Devpkey.h （包括 Devpkey.h）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>请参阅


[**INF *DDInstall*部分**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-ddinstall-section)

[**INF DriverVer Directive**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-driverver-directive)

[**INF 版本部分**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-version-section)

[**SetupDiGetDeviceProperty**](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupdigetdevicepropertyw)

 

 






