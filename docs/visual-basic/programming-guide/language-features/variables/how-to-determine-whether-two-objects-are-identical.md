---
title: Como determinar se dois objetos são idênticos
ms.date: 07/20/2015
helpviewer_keywords:
- testing [Visual Basic], objects
- objects [Visual Basic], comparing
- object variables [Visual Basic], determining identity
ms.assetid: 7829f817-0d1f-4749-a707-de0b95e0cf5c
ms.openlocfilehash: 67c3af8b7bdac3ad1c7e4908f1ac2684df7a87aa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410471"
---
# <a name="how-to-determine-whether-two-objects-are-identical-visual-basic"></a>Como determinar se dois objetos são idênticos (Visual Basic)
Em Visual Basic, duas referências de variáveis são consideradas idênticas se seus ponteiros forem iguais, ou seja, se ambas as variáveis apontarem para a mesma instância de classe na memória. Por exemplo, em um aplicativo Windows Forms, talvez você queira fazer uma comparação para determinar se a instância atual ( `Me` ) é igual a uma instância específica, como `Form2` .  
  
 Visual Basic fornece dois operadores para comparar ponteiros. O [operador is](../../../language-reference/operators/is-operator.md) retorna `True` se os objetos são idênticos e o [operador IsNot](../../../language-reference/operators/isnot-operator.md) retorna `True` se eles não forem.  
  
## <a name="determining-if-two-objects-are-identical"></a>Determinando se dois objetos são idênticos  
  
#### <a name="to-determine-if-two-objects-are-identical"></a>Para determinar se dois objetos são idênticos  
  
1. Configure uma `Boolean` expressão para testar os dois objetos.  
  
2. Em sua expressão de teste, use o `Is` operador com os dois objetos como operandos.  
  
     `Is`retorna `True` se os objetos apontam para a mesma instância de classe.  
  
## <a name="determining-if-two-objects-are-not-identical"></a>Determinando se dois objetos não são idênticos  
 Às vezes, você deseja executar uma ação se os dois objetos não forem idênticos, e pode ser estranho combinar `Not` e `Is` , por exemplo `If Not obj1 Is obj2` . Nesse caso, você pode usar o `IsNot` operador.  
  
#### <a name="to-determine-if-two-objects-are-not-identical"></a>Para determinar se dois objetos não são idênticos  
  
1. Configure uma `Boolean` expressão para testar os dois objetos.  
  
2. Em sua expressão de teste, use o `IsNot` operador com os dois objetos como operandos.  
  
     `IsNot`retorna `True` se os objetos não apontam para a mesma instância de classe.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir testa pares de `Object` variáveis para ver se elas apontam para a mesma instância de classe.  
  
 [!code-vb[VbVbalrKeywords#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/class7.vb#14)]  
  
 O exemplo anterior exibe a saída a seguir.  
  
 `objA different from objB? True`  
  
 `objA identical to objC? True`  
  
## <a name="see-also"></a>Confira também

- [Tipo de dados Object](../../../language-reference/data-types/object-data-type.md)
- [Variáveis de Objeto](object-variables.md)
- [Valores de Variável de Objeto](object-variable-values.md)
- [Operador Is](../../../language-reference/operators/is-operator.md)
- [Operador IsNot](../../../language-reference/operators/isnot-operator.md)
- [Como determinar se dois objetos estão relacionados](how-to-determine-whether-two-objects-are-related.md)
- [Me, My, MyBase e MyClass](../../program-structure/me-my-mybase-and-myclass.md)
