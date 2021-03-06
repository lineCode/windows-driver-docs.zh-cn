---
title: INF 文件条目
description: INF 文件条目
ms.assetid: 8af2cbe7-f249-4e2f-940f-b50bc451cabe
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 514bdab3fbd599e7c57246a27dc1ecdf7667ecf2
ms.sourcegitcommit: fb7d95c7a5d47860918cd3602efdd33b69dcf2da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67378965"
---
# <a name="inf-file-entries"></a>INF 文件条目





使用 microdriver 适用于你的设备需要的安装程序信息 (INF) 文件中的其他项。 首先，必须有一个名为"MicroDriver"条目中**DeviceData** INF 文件部分。 (请参阅[WIA 设备 INF 文件](inf-files-for-wia-devices.md)有关详细信息。)此条目应设置为实现 microdriver 的 DLL 的名称。

### <a name="windows-me-inf-file-entries"></a>Windows Me INF 文件条目

以下内容适用于 Microsoft Windows Millennium Edition （me）、 Windows XP 和更高版本的操作系统。

中的空间中[ **INF AddReg 指令**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-addreg-directive)通常将引用你 WIA 微型驱动程序，其中应列出 INF *wiafbdrv.dll*作为驱动程序。 这是实现 WIA 平板驱动程序的组件。

确保[ **INF CopyFiles 指令**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-copyfiles-directive)包括这两个您 microdriver 并*wiafbdrv.dll*，这从 Windows CAB 文件复制。 INF 文件的其余部分是与其他 WIA 的设备相同。

### <a name="windows-xp-inf-file-entries"></a>Windows XP INF 文件条目

以下信息适用于 Windows XP 和更高版本。 Windows Me INF 文件不使用**Include**并**需要**指令，并因此不能使用这种样式的 INF。

[ **INF DDInstall 部分**](https://docs.microsoft.com/windows-hardware/drivers/install/inf-ddinstall-section)应包含指令**Include**= sti.inf。 此外，**需要**指令应引用**STI。MICRODRIVERSection**，以及适当的设备类型部分。 这将提供所需的 USDClass 和 CLSID **AddReg**指令，因此它们不需要显式包含在 INF。

**请注意**  不需要包括*wiafbdrv.dll*在你**CopyFiles**指令。

 

包含的 Windows Driver Kit (WDK) CD 上 WIA microdriver INF 使用这种新方法来引用 microdriver。

 

 




