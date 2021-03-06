---
title: WindowsUri 协议
description: WindowsUri 协议
ms.assetid: 79589ECE-9DF9-40C8-897D-95B432C4C9C8
keywords:
- NFC
- 近场通信
- proximity
- 近场邻近感应
- NFP
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 8b8a7391cb55374115038552cfef7748b1d5a085
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72843463"
---
# <a name="windowsuri-protocol"></a>WindowsUri 协议


"WindowsUri" 协议是一种抽象用于简单 URI 字符串的订阅的方法。 Windows 将订阅此类型，以便向 Windows 对接收用户可能想要启动的 Uri 的驱动程序进行注册。

### <a name="required-actions"></a>必需的措施

-   驱动程序必须将 URI 字符串作为 "WindowsUri" 订阅服务器的以 NULL 结尾的 UTF-16LE 编码字符串返回。
-   驱动程序必须将 "WindowsUri" 发布的输入负载视为 UTF-16LE 编码的字符串。 驱动程序必须安全地接受以 NULL 结尾或非 NULL 终止的输入。
-   如果近程技术被播发为 NFC，则驱动程序必须将对 "WindowsUri" 类型的订阅视为等效于 "NDEF： wkt" 中 URI 有效负载的订阅。U "或" NDEF： wkt "。Sp "消息。
    -   驱动程序还必须在 "NDEF： wkt" 中将 "WindowsUri" 订阅与 URI 有效负载相匹配。Sp "消息。 对 "NDEF： wkt" 的所有订阅。Sp "必须使用" NDEF： wkt "的完整有效负载填充。Sp "消息。 如果 NDEF 消息同时包含智能海报和非嵌套 URI 记录，则必须忽略 URI 记录。
    -   驱动程序必须仅将此消息的 URI 字符串负载返回到此类型的订阅服务器。 驱动程序不得将完整的 NDEF 消息返回到此类型的订阅服务器。
-   如果近程技术被播发为 NFC，则驱动程序必须在 \[NFC URI\]中指定的 NDEF 消息中封装每个 "WindowsUri" 发布的负载。
-   提供程序还可以支持其他兼容方案。

## <a name="publications-for-windowsuriwritetag"></a>"WindowsUri： WriteTag" 的发布


这是一种特殊类型的 WindowsUri 发布，它允许将 URI 写入任何可写标记。

### <a name="required-actions"></a>必需的措施

-   其他地方所述的常见 "\*： WriteTag" 要求适用。
-   上述 "WindowsUri" 发布要求也适用于 "WindowsUri： WriteTag" 发布。

 

 
## <a name="related-topics"></a>相关主题
[NFC 设备驱动程序接口（DDI）概述](https://docs.microsoft.com/windows-hardware/drivers/ddi/index)  
[近字段邻近 DDI 引用](https://docs.microsoft.com/windows-hardware/drivers/ddi/index)  

