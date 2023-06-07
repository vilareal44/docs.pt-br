---
title: Como usar variáveis locais e matrizes de tipo implícito em uma expressão de consulta – guia de programação em C#
description: Use variáveis locais digitadas implicitamente em C# para que o compilador determine o tipo de uma variável local. Você deve usá-los para armazenar tipos anônimos.
ms.date: 07/20/2015
helpviewer_keywords:
- implicitly-typed local variables [C#], how to use
ms.assetid: 6b7354d2-af79-427a-b6a8-f74eb8fd0b91
ms.openlocfilehash: b69c646aa878166cca3adc1a568ce16454fb7574
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864326"
---
# <a name="how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression-c-programming-guide"></a>Como usar variáveis locais e matrizes de tipo implícito em uma expressão de consulta (guia de programação C#)
Será possível usar variáveis locais de tipo implícito sempre que você desejar que o compilador determine o tipo de uma variável local. É necessário usar variáveis locais de tipo implícito para armazenar tipos anônimos, usados frequentemente em expressões de consulta. Os exemplos a seguir ilustram usos obrigatórios e opcionais de variáveis locais de tipo implícito em consultas.  
  
 As variáveis locais de tipo implícito são declaradas usando a palavra-chave contextual [var](../../language-reference/keywords/var.md). Para obter mais informações, consulte [Variáveis locais de tipo implícito](./implicitly-typed-local-variables.md) e [Matrizes de tipo implícito](../arrays/implicitly-typed-arrays.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra um cenário comum em que a palavra-chave `var` é necessária: uma expressão de consulta que produz uma sequência de tipos anônimos. Nesse cenário, a variável de consulta e a variável de iteração na instrução `foreach` devem ser tipadas implicitamente usando `var`, porque você não tem acesso a um nome de tipo para o tipo anônimo. Para obter mais informações sobre tipos anônimos, consulte [Tipos anônimos](./anonymous-types.md).  
  
 [!code-csharp[csProgGuideLINQ#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#32)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a palavra-chave `var` em uma situação semelhante, mas na qual o uso de `var` é opcional. Como `student.LastName` é uma cadeia de caracteres, a execução da consulta retorna uma sequência de cadeias de caracteres. Portanto, o tipo de `queryID` poderia ser declarado como `System.Collections.Generic.IEnumerable<string>` em vez de `var`. A palavra-chave `var` é usada por conveniência. No exemplo, a variável de iteração na instrução `foreach` tem tipo explícito como uma cadeia de caracteres, mas, em vez disso, poderia ser declarada usando `var`. Como o tipo da variável de iteração não é um tipo anônimo, o uso de `var` é opcional, não obrigatório. Lembre-se de que `var` por si só não é um tipo, mas uma instrução para o compilador inferir e atribuir o tipo.  
  
 [!code-csharp[csProgGuideLINQ#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#33)]  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Métodos de Extensão](./extension-methods.md)
- [LINQ (Consulta Integrada à Linguagem)](../../linq/index.md)
- [var](../../language-reference/keywords/var.md)
- [LINQ em C#](../../linq/index.md)
