---
title: 视频流扩展标头
description: 视频流扩展标头
ms.assetid: 6540026c-a41a-49e2-a41f-fe64106408f5
keywords:
- 视频捕获 WDK AVStream，扩展标头
- 捕获视频 WDK AVStream，扩展标头
- 扩展的标头 WDK 视频捕获
- 标题 WDK 视频捕获
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 80ccc8d328ee007d901a5ec3bd155b42e3fc9f02
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72843664"
---
# <a name="video-stream-extended-headers"></a>视频流扩展标头


视频捕获微型驱动程序在其输出流中使用扩展的标头来提供有关流和当前帧内容的辅助信息。 例如，图像流标头提供有关当前帧号、丢弃的帧数和字段极性标志的信息。 每个帧完成后，微型驱动程序会在扩展的标头中填充有关捕获的帧的辅助信息。

Stream 类视频捕获微型驱动程序通过将[**HW\_STREAM\_对象**](https://docs.microsoft.com/windows-hardware/drivers/ddi/strmini/ns-strmini-_hw_stream_object)结构的**StreamHeaderMediaSpecific**成员设置为**sizeof** ，来指示其为 pin 提供此附加信息的能力以下两个结构中的。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>结构名称</th>
<th>用途</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-tagks_frame_info" data-raw-source="[&lt;strong&gt;KS_FRAME_INFO&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-tagks_frame_info)"><strong>KS_FRAME_INFO</strong></a></p></td>
<td><p>帧计数、删除帧计数、字段极性标志和 DirectDraw 表面控点。</p></td>
</tr>
<tr class="even">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-tagks_vbi_frame_info" data-raw-source="[&lt;strong&gt;KS_VBI_FRAME_INFO&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-tagks_vbi_frame_info)"><strong>KS_VBI_FRAME_INFO</strong></a></p></td>
<td><p>VBI 格式、频道更改信息、视频标准。</p></td>
</tr>
</tbody>
</table>

 

如果 Stream 类微型驱动程序未提供此附加信息，则它应将**StreamHeaderMediaSpecific**设置为零。

有关何时在**StreamHeaderMediaSpecific**中指定值的详细信息，请参阅[流类别](stream-categories.md)。

 

 




