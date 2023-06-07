---
title: Funções anônimas – Guia de Programação em C#
description: Saiba mais sobre funções anônimas. Você pode usar uma expressão lambda ou um método anônimo para criar uma função anônima.
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [C#], as anonymous functions
- anonymous functions [C#]
- anonymous methods [C#]
ms.assetid: 6ce3f04d-0c71-4728-9127-634c7e9a8365
ms.openlocfilehash: 1fde7d535054f09d55018a010468776622ebfba7
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063257"
---
# <a name="anonymous-functions-c-programming-guide"></a>Funções anônimas (Guia de Programação em C#)

Uma função anônima é uma instrução ou expressão "embutida" que pode ser usada em qualquer local em que um tipo delegado é esperado. Você pode usá-la para inicializar um delegado nomeado ou passá-la em vez de um tipo delegado nomeado como um parâmetro de método.

Você pode usar uma [expressão lambda](../../language-reference/operators/lambda-expressions.md) ou um [método anônimo](../../language-reference/operators/delegate-operator.md) para criar uma função anônima. Recomendamos o uso de expressões lambda já que elas fornecem uma maneira mais concisa e expressiva de gravar código embutido. Ao contrário dos métodos anônimos, alguns tipos de expressões lambda podem ser convertidos nos tipos de árvore de expressão.

## <a name="the-evolution-of-delegates-in-c"></a>A evolução de delegados em C\#

 No C# 1.0, você criava uma instância de um delegado, inicializando-a explicitamente com um método que era definido em outro local no código. O C# 2.0 introduziu o conceito de métodos anônimos como uma maneira de escrever blocos de instrução sem nome embutidos, que podem ser executados em uma invocação de delegado. O C# 3.0 introduziu as expressões lambda, que são semelhantes ao conceito dos métodos anônimos, mas mais expressivas e concisas. Essas duas funcionalidades são conhecidas coletivamente como *funções anônimas*. Em geral, os aplicativos que se destinam .NET Framework 3,5 ou posterior devem usar expressões lambda.  
  
 O exemplo a seguir demonstra a evolução da criação de delegado, desde o C# 1.0 até o C# 3.0:  
  
 [!code-csharp[csProgGuideLINQ#65](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#65)]  
  
## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações, confira a seção [Expressões de função anônima](~/_csharplang/spec/expressions.md#anonymous-function-expressions) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).
  
## <a name="see-also"></a>Consulte também

- [Instruções, expressões e operadores](./index.md)
- [Expressões lambda](../../language-reference/operators/lambda-expressions.md)
- [Representantes](../delegates/index.md)
- [Árvores de expressão (C#)](../concepts/expression-trees/index.md)
