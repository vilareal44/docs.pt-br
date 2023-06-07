---
title: Palavra-chave set – Referência de C#
ms.date: 03/10/2017
f1_keywords:
- set
- set_CSharpKeyword
helpviewer_keywords:
- set keyword [C#]
ms.assetid: 30d7e4e5-cc2e-4635-a597-14a724879619
ms.openlocfilehash: cdd84c824cc4dc93f4433f07e9978d22cba3f245
ms.sourcegitcommit: de7f589de07a9979b6ac28f54c3e534a617d9425
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82795060"
---
# <a name="set-c-reference"></a>set (Referência de C#)

A palavra-chave `set` define um método *acessador* em uma propriedade ou indexador que atribui um valor ao elemento da propriedade ou do elemento. Para obter mais informações e exemplos, consulte [Propriedades](../../programming-guide/classes-and-structs/properties.md), [Propriedades autoimplementadas](../../programming-guide/classes-and-structs/auto-implemented-properties.md) e [Indexadores](../../programming-guide/indexers/index.md).

O exemplo a seguir define um acessador `get` e um acessador `set` para uma propriedade chamada `Seconds`. Ela usa um campo particular chamado `_seconds` para dar suporte ao valor da propriedade.

[!code-csharp[set#1](~/samples/snippets/csharp/language-reference/keywords/get/get-1.cs)]

Geralmente, o acessador `set` consiste em uma única instrução que retorna um valor, como no exemplo anterior. Começando com o C# 7.0, você pode implementar o acessador `set` como um membro apto para expressão. O exemplo a seguir implementa os acessadores `get` e `set` como membros com corpo de expressão.

[!code-csharp[set#3](~/samples/snippets/csharp/language-reference/keywords/get/get-3.cs)]
  
Para casos simples em que os acessadores `get` e `set` de uma propriedade não realizam nenhuma outra operação, a não ser a configuração ou a recuperação de um valor em um campo de suporte particular, você pode tirar proveito do suporte do compilador do C# para propriedades autoimplementadas. O exemplo a seguir implementa `Hours` como uma propriedade autoimplementada.

[!code-csharp[set#2](~/samples/snippets/csharp/language-reference/keywords/get/get-2.cs)]
  
## <a name="c-language-specification"></a>especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Veja também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Propriedades](../../programming-guide/classes-and-structs/properties.md)
