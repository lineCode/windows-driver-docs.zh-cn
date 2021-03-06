---
title: 验证工具调查
description: 验证工具调查
ms.assetid: d36e041f-efa5-450f-b4de-c84c4880e44d
keywords:
- 动态验证工具 WDK
- 静态验证工具 WDK
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 8440736eae33b3aaa3e05d25669ed1fea261b2cd
ms.sourcegitcommit: cbcb712a9f1f62c7d67e1b98097a0d8d24bd0c71
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83769693"
---
# <a name="survey-of-verification-tools"></a>验证工具调查


以下验证工具在 WDK 中进行了介绍，并建议供驱动程序开发人员和测试人员使用。 它们按通常使用的顺序列出。

### <a name="span-idas_soon_as_the_code_compilesspanspan-idas_soon_as_the_code_compilesspanas-soon-as-the-code-compiles"></a><span id="as_soon_as_the_code_compiles"></span><span id="AS_SOON_AS_THE_CODE_COMPILES"></span>代码编译后立即

-   [驱动程序的代码分析](code-analysis-for-drivers.md)是一种在编译时运行的静态验证工具。 驱动程序的 CCode 分析可以验证用 C/c + + 和托管代码编写的驱动程序。 它独立检查驱动程序的每个函数中的代码，因此你可以在构建驱动程序后立即运行它。 它的运行速度相对较快，并使用了一些资源。

    Visual Studio 中的代码分析工具的基本功能检测常规编码错误，如不检查返回值。 驱动程序特定的功能将检测更多微妙的驱动程序编码错误，如将未初始化的字段保留在复制的 IRP 中，并且无法在例程结束时还原已更改的 IRQL。

<!-- -->

-   [静态驱动程序验证程序](static-driver-verifier.md)（SDV）是一种静态验证工具，在编译时运行并验证用 c/c + + 编写的内核模式驱动程序代码。 它包含在 WDK 中，可以从 Visual Studio Ultimate 2012 或 Visual Studio 命令提示符窗口中使用 MSBuild 启动。

    静态驱动程序验证器根据一组接口规则和操作系统型号，确定驱动程序是否与 Windows 操作系统内核正确交互。 静态驱动程序验证程序非常彻底，它会探讨驱动程序源代码中所有可访问的路径，并符号执行这些路径。 因此，它会查找使用任何其他传统的驱动程序测试方法检测不到的 bug。

### <a name="span-idwhen_the_driver_runsspanspan-idwhen_the_driver_runsspanwhen-the-driver-runs"></a><span id="when_the_driver_runs"></span><span id="WHEN_THE_DRIVER_RUNS"></span>当驱动程序运行时

构建驱动程序并在运行时不出现明显错误，请使用以下动态验证工具。

-   [驱动程序验证程序](driver-verifier.md)是专门为 Windows 驱动程序编写的动态验证工具。 它包含可同时在多个驱动程序上运行的多个测试。 驱动程序验证器在发现驱动程序开发人员和测试人员在开发或测试环境中运行时，驱动程序验证程序要运行的驱动程序验证程序，因此非常有效。 驱动程序验证程序包含在 Windows 2000 和更高版本的 Windows 中。 为驱动程序启用驱动程序验证程序时，还必须对该驱动程序运行多个测试。 驱动程序验证程序可以检测到某些难以使用静态验证工具检测的驱动程序错误。 这些类型的错误的示例包括：
    -   **内核池缓冲区溢出。** 当经验证的驱动程序分配池内存缓冲区时，驱动程序验证器会使用不可访问的内存页来保护它们。 如果驱动程序尝试使用超出缓冲区末尾的内存，则驱动程序验证程序将发出 bug 检查。
    -   **在释放内存后使用内存。** 特殊池内存块使用其自己的内存页，不与其他分配共享内存页。 当驱动程序释放池内存块时，相应的内存页将不可访问。 如果驱动程序在释放该内存后尝试使用该内存，则驱动程序将立即崩溃。
    -   **在提升的 IRQL 下运行时使用可分页内存。** 当经验证的驱动程序在调度 \_ 级别或更高级别引发 IRQL 时，驱动程序验证程序将从系统工作集中修整所有可分页内存，并在内存压力下模拟系统。 如果驱动程序尝试使用这些可分页虚拟地址之一，则该驱动程序会崩溃。
    -   **低资源模拟。** 若要在资源不足的情况下模拟系统，驱动程序验证程序可能会导致驱动程序调用的各种操作系统内核 Api 失败。
    -   **内存泄露。** 驱动程序验证程序可跟踪驱动程序所执行的内存分配，并确保在卸载驱动程序之前释放内存。
    -   **需要很长时间才能完成或被取消的 i/o 操作。** 驱动程序验证程序可以测试驱动程序的逻辑，以响应 \_ [**IoCallDriver**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iocalldriver)中的状态挂起返回值。
    -   **DDI 遵从性检查。** （从 Windows 8 开始提供）驱动程序验证程序应用一组设备驱动程序接口（DDI）规则，这些规则检查操作系统的驱动程序和内核接口之间的适当交互。 这些规则对应于静态驱动程序验证器用于分析驱动程序源代码的规则。 如果在启用 DDI 相容性检查时，驱动程序验证程序发现错误，请运行 "[静态驱动程序验证程序](static-driver-verifier.md)"，然后选择导致错误的同一规则。 静态驱动程序验证程序可帮助您找到驱动程序源代码中出现缺陷的原因。
-   [应用程序验证工具](application-verifier.md)是一种动态验证工具，适用于使用 C/c + + 编写的用户模式应用程序和驱动程序。 它不验证托管代码。 
 

 





