---
title: 单个二进制选择加入 POOL_NX_OPTIN
description: 若要生成在 Windows 8 和更早版本的 Windows 中运行的单个驱动程序二进制文件，请使用 POOL_NX_OPTIN 选择加入机制。
ms.assetid: BE9D3C85-0212-4206-A59B-4D53FB842C39
ms.localizationpriority: medium
ms.date: 10/17/2018
ms.openlocfilehash: 1a8fd9b15bda28ae3e78d9b5aea08c72d349a724
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72838419"
---
# <a name="single-binary-opt-in-pool_nx_optin"></a>单个二进制选择加入：池\_NX\_OPTIN


若要生成在 Windows 8 和更早版本的 Windows 中运行的单个驱动程序二进制文件，请使用\_NX 的池\_OPTIN 选择加入机制。 这是为第三方硬件供应商提供的一项移植辅助，它们提供单个驱动程序二进制文件以支持多个 Windows 版本。

若要使用此选择机制，请执行以下操作：

-   为要选择的所有源文件定义\_NX\_OPTIN = 1 的池。 为此，请在驱动程序项目的相应属性页中包含以下预处理器定义：

    `C_DEFINES=$(C_DEFINES) -DPOOL_NX_OPTIN=1`

-   在[**DriverEntry**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_initialize) （或等效的）例程中，包括以下函数调用：

    `ExInitializeDriverRuntime(DrvRtPoolNxOptIn);`

    此调用必须在驱动程序进行任何使用**非分页池**池类型或调用[**ExInitializeNPagedLookasideList**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-exinitializenpagedlookasidelist)例程的分配之前发生。 **ExInitializeDriverRuntime**是强制内联函数，可在 windows 8 或更高版本的 windows 上调用。

对于大多数驱动程序，这两项任务足以为单一驱动程序二进制文件启用选择加入机制。

## <a name="implementation-details"></a>实现详细信息


池\_NX\_OPTIN 的工作原理是：将**非分页池**替换为全局[**池\_类型**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/ne-wdm-_pool_type)变量 `ExDefaultNonPagedPoolType`，该全局池会初始化为**NonPagedPoolNx** （适用于 windows 8 和更高版本的 windows）或**NonPagedPoolExecute** （适用于 Windows 的早期版本）。 此选择机制使你的内核模式驱动程序能够在 Windows 8 上运行，同时增强了 NX 池的保护，并且在 Windows 的早期版本上运行，而后者不支持 NX 池。 将**非分页池**常量名称的实例转换为**NonPagedPoolNx**的宏也会将**NonPagedPoolCacheAligned**的实例转换为**NonPagedPoolNxCacheAligned**。

## <a name="support-for-static-libraries-lib-projects"></a>支持静态库（.lib 项目）


可以为 .lib 项目使用池\_NX\_OPTIN 选择加入机制，但是链接到 .lib 的项目通常还必须使用 POOL\_NX\_OPTIN。 实现[**DriverEntry**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_initialize)例程的项目至少必须包含以下函数调用：

`ExInitializeDriverRuntime(DrvRtPoolNxOptIn);`

 

 




