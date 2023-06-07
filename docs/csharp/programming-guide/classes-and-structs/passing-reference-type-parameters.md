---
title: Passando parâmetros de tipo de referência – Guia de Programação em C#
description: Quando você passa um parâmetro de tipo de referência por valor em C#, os dados no objeto referenciado podem ser alterados, mas não o valor da referência em si.
ms.date: 07/20/2015
helpviewer_keywords:
- method parameters [C#], reference types
- parameters [C#], reference
ms.assetid: 9e6eb65c-942e-48ab-920a-b7ba9df4ea20
ms.openlocfilehash: 64a4735eded7a468549862b3221b4fbd0966e64d
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864703"
---
# <a name="passing-reference-type-parameters-c-programming-guide"></a>Passando parâmetros de tipo de referência (Guia de Programação em C#)
Uma variável de um [tipo de referência](../../language-reference/keywords/reference-types.md) não contém seus dados diretamente; ela contém uma referência a seus dados. Quando você passa um parâmetro de tipo de referência por valor, é possível alterar os dados que pertencem ao objeto referenciado, como o valor de um membro de classe. No entanto, não é possível alterar o valor da referência em si. Por exemplo, não é possível usar a mesma referência para alocar memória para um novo objeto e fazer com que ele persista fora do bloco. Para fazer isso, passe o parâmetro usando a palavra-chave [ref](../../language-reference/keywords/ref.md) ou [out](../../language-reference/keywords/out-parameter-modifier.md). Para simplificar, os exemplos a seguir usam `ref`.  
  
## <a name="passing-reference-types-by-value"></a>Passando tipos de referência por valor  
 O exemplo a seguir demonstra a passagem de um parâmetro de tipo de referência, `arr`, por valor, para um método, `Change`. Como o parâmetro é uma referência a `arr`, é possível alterar os valores dos elementos da matriz. No entanto, a tentativa de reatribuir o parâmetro para um local diferente de memória só funciona dentro do método e não afeta a variável original, `arr`.  
  
 [!code-csharp[csProgGuideParameters#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#7)]  
  
 No exemplo anterior, a matriz, `arr`, que é um tipo de referência, é passada para o método sem o parâmetro `ref`. Nesse caso, uma cópia da referência, que aponta para `arr`, é passada para o método. A saída mostra que é possível para o método alterar o conteúdo de um elemento de matriz, nesse caso de `1` para `888`. No entanto, alocar uma nova parte da memória usando o operador [new](../../language-reference/operators/new-operator.md) dentro do método `Change` faz a variável `pArray` referenciar uma nova matriz. Portanto, quaisquer alterações realizadas depois disso não afetarão a matriz original, `arr`, criada dentro de `Main`. Na verdade, duas matrizes são criadas neste exemplo, uma dentro de `Main` e outra dentro do método `Change`.  
  
## <a name="passing-reference-types-by-reference"></a>Passando tipos de referência por referência  
 O exemplo a seguir é o mesmo que o exemplo anterior, exceto que a palavra-chave `ref` é adicionada ao cabeçalho e à chamada do método. Quaisquer alterações que ocorrem no método afetam a variável original no programa de chamada.  
  
 [!code-csharp[csProgGuideParameters#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#8)]  
  
 Todas as alterações que ocorrem dentro do método afetam a matriz original em `Main`. Na verdade, a matriz original é realocada usando o operador `new`. Portanto, depois de chamar o método `Change`, qualquer referência a `arr` aponta para a matriz de cinco elementos, criada no método `Change`.  
  
## <a name="swapping-two-strings"></a>Trocando duas cadeias de caracteres  
 Trocar cadeias de caracteres é um bom exemplo de passar parâmetros de tipo de referência por referência. No exemplo, duas cadeias de caracteres, `str1` e `str2`, são inicializadas em `Main` e passadas para o método `SwapStrings` como parâmetros modificado pela palavra-chave `ref`. As duas cadeias de caracteres são trocadas dentro do método e dentro de `Main` também.  
  
 [!code-csharp[csProgGuideParameters#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#9)]  
  
 Neste exemplo, os parâmetros precisam ser passados por referência para afetar as variáveis no programa de chamada. Se você remover a palavra-chave `ref` do cabeçalho de método e da chamada de método, nenhuma alteração ocorrerá no programa de chamada.  
  
 Para obter mais informações sobre cadeias de caracteres, consulte [cadeia de caracteres](../../language-reference/builtin-types/reference-types.md).  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Passando parâmetros](./passing-parameters.md)
- [ref](../../language-reference/keywords/ref.md)
- [no](../../language-reference/keywords/in-parameter-modifier.md)
- [fora](../../language-reference/keywords/out.md)
- [Tipos de referência](../../language-reference/keywords/reference-types.md)
