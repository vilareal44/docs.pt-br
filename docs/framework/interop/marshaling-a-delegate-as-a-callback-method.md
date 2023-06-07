---
title: Realizando marshaling de um delegado como um método de retorno de chamada
description: Aprenda a realizar marshaling de um delegado como um método de retorno de chamada. Veja um exemplo de como passar delegados para uma função não gerenciada que é esperada ponteiros de função.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data marshaling, Callback sample
- marshaling, Callback sample
ms.assetid: 6ddd7866-9804-4571-84de-83f5cc017a5a
ms.openlocfilehash: bf9ef3b9d48c0869dcc96820c3a2fb6fb608479e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618942"
---
# <a name="marshaling-a-delegate-as-a-callback-method"></a>Realizando marshaling de um delegado como um método de retorno de chamada
Este exemplo demonstra como passar delegados para uma função não gerenciada esperando ponteiros de função. Um delegado é uma classe que pode conter uma referência a um método e é equivalente a um ponteiro de função fortemente tipada ou a uma função de retorno de chamada.

> [!NOTE]
> Quando você usa um delegado dentro de uma chamada, o Common Language Runtime protege o delegado de sofrer coleta de lixo pela duração da chamada. No entanto, se a função não gerenciada armazena o delegado para uso após a conclusão da chamada, você deve evitar manualmente a coleta de lixo até que a função não gerenciada termine de usar o delegado. Para obter mais informações, consulte a [Amostra de HandleRef](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/hc662t8k(v=vs.100)) e [Amostra de GCHandle](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/44ey4b32(v=vs.100)).

A amostra de Callback usa as seguintes funções não gerenciadas, mostradas com a respectiva declaração de função original:

- `TestCallBack` exportado de PinvokeLib.dll.

    ```cpp
    void TestCallBack(FPTR pf, int value);
    ```

- `TestCallBack2` exportado de PinvokeLib.dll.

    ```cpp
    void TestCallBack2(FPTR2 pf2, char* value);
    ```

[PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) é uma biblioteca personalizada não gerenciada que contém uma implementação para as funções listadas anteriormente.

Neste exemplo, a classe `NativeMethods` contém protótipos gerenciados para os métodos `TestCallBack` e `TestCallBack2`. Ambos os métodos passam um delegado para uma função de retorno de chamada como um parâmetro. A assinatura do delegado deve corresponder à assinatura do método ao qual ele faz referência. Por exemplo, os delegados `FPtr` e `FPtr2` têm assinaturas que são idênticas aos métodos `DoSomething` e `DoSomething2`.

## <a name="declaring-prototypes"></a>Declarando Protótipos
[!code-cpp[Conceptual.Interop.Marshaling#37](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/callback.cpp#37)]
[!code-csharp[Conceptual.Interop.Marshaling#37](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/callback.cs#37)]
[!code-vb[Conceptual.Interop.Marshaling#37](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/callback.vb#37)]

## <a name="calling-functions"></a>Chamando Funções
[!code-cpp[Conceptual.Interop.Marshaling#38](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/callback.cpp#38)]
[!code-csharp[Conceptual.Interop.Marshaling#38](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/callback.cs#38)]
[!code-vb[Conceptual.Interop.Marshaling#38](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/callback.vb#38)]

## <a name="see-also"></a>Consulte também

- [Exemplos diversos de marshaling](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ss9sb93t(v=vs.100))
- [Tipos de dados de invocação de plataforma](marshaling-data-with-platform-invoke.md#platform-invoke-data-types)
- [Criando protótipos em código gerenciado](creating-prototypes-in-managed-code.md)
