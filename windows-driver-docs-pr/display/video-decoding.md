---
title: 视频解码
description: 视频解码
ms.assetid: 4e8bf7e9-7a3c-4732-9ac0-4f6f5ef07552
keywords:
- DirectX 视频加速 WDK Windows 2000 显示视频解码
- 视频加速 WDK DirectX，视频解码
- VA WDK DirectX，视频解码
- 有关解码视频解码视频 WDK DirectX VA，
- 有关解码视频解码 WDK DirectX va，因此视频
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: b800d011f23e0ac8e83ab8ac632eb8cb776f033d
ms.sourcegitcommit: fb7d95c7a5d47860918cd3602efdd33b69dcf2da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67365088"
---
# <a name="video-decoding"></a>视频解码


## <span id="ddk_video_decoding_gg"></span><span id="DDK_VIDEO_DECODING_GG"></span>


DirectX VA 允许一个或多个阶段的视频解码过程之间分割*承载 CPU*和视频硬件加速。 快捷键执行[经过运动补偿预测](motion-compensated-prediction.md)(MCP)，并且还可以执行的离散余弦逆变 (IDCT) 和解码过程的可变长度解码 (VLD) 阶段。

DirectX VA API 对单个视频流进行解码。 支持多个视频流的每个视频流 （例如，若要在筛选器图形操作中使用的视频解码器和加速驱动程序的输出和输入插针单独对） 需要单独的 DirectX VA 会话。 有关筛选器关系图的详细信息，请参阅[KS 微型驱动程序体系结构](https://docs.microsoft.com/windows-hardware/drivers/stream/ks-minidriver-architecture)。

 

 





