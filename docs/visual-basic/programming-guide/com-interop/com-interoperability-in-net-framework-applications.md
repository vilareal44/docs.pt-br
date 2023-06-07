---
title: Interoperabilidade COM em aplicativos .NET Framework
ms.date: 07/20/2015
helpviewer_keywords:
- interoperability, COM and .NET Framework objects
- COM interop [Visual Basic]
- shared components
ms.assetid: f5a72143-c268-4dff-a019-974ad940e17d
ms.openlocfilehash: 377958a21886fe0257633ea19417f9a19bd51ff3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396863"
---
# <a name="com-interoperability-in-net-framework-applications-visual-basic"></a>Interoperabilidade COM em aplicativos .NET Framework (Visual Basic)

Quando você quiser usar objetos COM e .NET Framework objetos no mesmo aplicativo, precisará abordar as diferenças em como os objetos existem na memória. Um objeto .NET Framework está localizado na memória gerenciada — a memória controlada pelo Common Language Runtime — e pode ser movida pelo tempo de execução conforme necessário. Um objeto COM está localizado em memória não gerenciada e não deve ser movido para outro local de memória. O Visual Studio e o .NET Framework fornecem ferramentas para controlar a interação desses componentes gerenciados e não gerenciados. Para obter mais informações sobre o código gerenciado, consulte [Common Language Runtime](../../../standard/clr.md).

Além de usar objetos COM em aplicativos .NET, você também pode querer usar Visual Basic para desenvolver objetos acessíveis por meio de código não gerenciado por meio de COM.

Os links nesta página fornecem detalhes sobre as interações entre objetos COM e .NET Framework.

## <a name="related-sections"></a>Seções relacionadas

| | |
|---------|---------|
| [Interoperabilidade COM](index.md) | Fornece links para tópicos que abrangem a interoperabilidade COM no Visual Basic, incluindo objetos COM, controles ActiveX, DLLs do Win32, objetos gerenciados e herança de objetos COM. |
| [Interoperação com código não gerenciado](../../../framework/interop/index.md) | Descreve brevemente alguns dos problemas de interação entre códigos gerenciados e não gerenciados e fornece links para outros estudos. |
| [Wrappers COM](../../../standard/native-interop/com-wrappers.md) | Discute wrappers callable em tempo de execução, que permitem que o código gerenciado Chame métodos COM e wrappers COM callable, que permitem que clientes COM chamem métodos de objeto .NET. |
| [Interoperabilidade COM avançada](../../../framework/interop/index.md) | Fornece links para tópicos que abrangem a interoperabilidade com em relação a wrappers, exceções, herança, Threading, eventos, conversões e marshaling. |
| [Tlbimp. exe (tipo de importador de biblioteca de tipos)](../../../framework/tools/tlbimp-exe-type-library-importer.md) | Discute a ferramenta que você pode usar para converter as definições de tipo encontradas em uma biblioteca de tipos COM em definições equivalentes em um assembly Common Language Runtime. |
