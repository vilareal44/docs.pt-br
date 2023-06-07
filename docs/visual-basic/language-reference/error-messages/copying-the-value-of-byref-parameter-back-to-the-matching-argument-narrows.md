---
title: A cópia do parâmetro '<parametername>' do valor 'ByRef' de volta para o argumento correspondente é restrita do tipo '<typename1>' para o tipo '<typename2>'
ms.date: 07/20/2015
f1_keywords:
- bc32053
- vbc32053
helpviewer_keywords:
- BC32053
ms.assetid: 281564b7-99f7-451f-b10d-f985e831bb25
ms.openlocfilehash: bac5f9a88df719bc64a8b0541f65e5912275866e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409745"
---
# <a name="copying-the-value-of-byref-parameter-parametername-back-to-the-matching-argument-narrows-from-type-typename1-to-type-typename2"></a>A cópia do parâmetro '\<parametername>' do valor 'ByRef' de volta para o argumento correspondente é restrita do tipo '\<typename1>' para o tipo '\<typename2>'
Um procedimento é chamado com um argumento que amplia o tipo de parâmetro correspondente e a conversão do parâmetro para o argumento é restrita.  
  
 Quando você define uma classe ou estrutura, pode definir um ou mais operadores de conversão para converter essa classe ou tipo de estrutura em outros tipos. Você também pode definir operadores de conversão reverso para converter esses outros tipos de volta para o tipo de classe ou estrutura. Quando você usa seu tipo de classe ou estrutura em uma chamada de procedimento, Visual Basic pode usar esses operadores de conversão para converter o tipo de um argumento para o tipo de seu parâmetro correspondente.  
  
 Se você passar o argumento [ByRef](../modifiers/byref.md), o Visual Basic às vezes copiará o valor do argumento em uma variável local no procedimento em vez de passar uma referência. Nesse caso, quando o procedimento retorna, Visual Basic deve copiar o valor da variável local de volta para o argumento no código de chamada.  
  
 Se um `ByRef` valor de argumento for copiado para o procedimento e o argumento e o parâmetro forem do mesmo tipo, nenhuma conversão será necessária. Mas se os tipos forem diferentes, Visual Basic deverá converter em ambas as direções. Se um dos tipos for seu tipo de classe ou estrutura, Visual Basic deverá convertê-lo de e para o outro tipo. Se uma dessas conversões estiver ampliando, a conversão reversa poderá ser restrita.  
  
 **ID do erro:** BC32053  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se possível, use um argumento de chamada do mesmo tipo que o parâmetro de procedimento, portanto Visual Basic não precisa fazer nenhuma conversão.  
  
- Se você precisar chamar o procedimento com um tipo de argumento diferente do tipo de parâmetro, mas não precisar retornar um valor para o argumento de chamada, defina o parâmetro como [ByVal](../modifiers/byval.md) em vez de `ByRef` .  
  
- Se você precisar retornar um valor para o argumento de chamada, defina o operador de conversão reversa como [ampliação](../modifiers/widening.md), se possível.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](../../programming-guide/language-features/procedures/index.md)
- [Parâmetros e Argumentos de Procedimento](../../programming-guide/language-features/procedures/procedure-parameters-and-arguments.md)
- [Passar argumentos por valor e por referência](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
- [Procedimentos do operador](../../programming-guide/language-features/procedures/operator-procedures.md)
- [Instrução Operator](../statements/operator-statement.md)
- [Como definir um operador](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [Como definir um operador de conversão](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [Conversões de tipo no Visual Basic](../../programming-guide/language-features/data-types/type-conversions.md)
- [Conversões de Widening e Narrowing](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
