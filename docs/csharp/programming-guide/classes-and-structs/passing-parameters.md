---
title: Passando parâmetros – Guia de Programação em C#
description: Você pode passar um argumento para um parâmetro em C# por valor ou referência. As alterações em um argumento passado por referência são mantidas. Use ref ou out para passar por referência.
ms.date: 07/20/2015
helpviewer_keywords:
- parameters [C#], passing
- passing parameters [C#]
- arguments [C#]
- methods [C#], passing parameters
- C# language, method parameters
ms.assetid: a5c3003f-7441-4710-b8b1-c79de77e0b77
ms.openlocfilehash: 875a42aacf3d7aa4124684aefafdcb07ff4c87d6
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864729"
---
# <a name="passing-parameters-c-programming-guide"></a>Passando parâmetros (Guia de Programação em C#)
No C#, argumentos podem ser passados para parâmetros por valor ou por referência. A passagem por referência permite que métodos, propriedades, indexadores, operadores, construtores e membros da função alterem o valor dos parâmetros e façam essa alteração persistir no ambiente de chamada. Para passar um parâmetro por referência com a intenção de alterar o valor, use a palavra-chave `ref` ou `out`. Para passar por referência com a intenção de evitar a cópia, mas não alterar o valor, use o modificador `in`. Para simplificar, somente a palavra-chave `ref` é usada nos exemplos neste tópico. Para obter mais informações sobre a diferença entre `in`, `ref` e `out`, consulte [in](../../language-reference/keywords/in-parameter-modifier.md), [ref](../../language-reference/keywords/ref.md) e [out](../../language-reference/keywords/out-parameter-modifier.md).  
  
 O exemplo a seguir ilustra a diferença entre parâmetros de valor e referência.  
  
 [!code-csharp[csProgGuideParameters#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#10)]  
  
 Para obter mais informações, consulte estes tópicos:  
  
- [Passando parâmetros de tipo de valor](./passing-value-type-parameters.md)  
  
- [Passando parâmetros de tipo de referência](./passing-reference-type-parameters.md)  
  
## <a name="c-language-specification"></a>Especificação da Linguagem C#  

Para obter mais informações, veja as [listas de argumentos](~/_csharplang/spec/expressions.md#argument-lists) na [Especificação da linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Métodos](./methods.md)
