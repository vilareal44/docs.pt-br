---
title: Operador TryCast
ms.date: 07/20/2015
f1_keywords:
- vb.trycast
- trycast
helpviewer_keywords:
- TryCast keyword [Visual Basic]
ms.assetid: d1ef5d47-fef4-491e-b014-1d910628f65c
ms.openlocfilehash: 8f0d4f612cd5a96e0b0ab7305c41e00790613cc8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406369"
---
# <a name="trycast-operator-visual-basic"></a>Operador TryCast (Visual Basic)
Apresenta uma operação de conversão de tipo que não gera uma exceção.  
  
## <a name="remarks"></a>Comentários  
 Se uma tentativa de conversão falhar, `CType` e `DirectCast` os dois geram um <xref:System.InvalidCastException> erro. Isso pode afetar negativamente o desempenho do seu aplicativo. `TryCast`Não retorna [nada](../nothing.md), de modo que, em vez de ter que lidar com uma possível exceção, você só precisa testar o resultado retornado `Nothing` .  
  
 Você usa a `TryCast` palavra-chave da mesma maneira que usa a [função CType](../functions/ctype-function.md) e a palavra-chave [DirectCast Operator](directcast-operator.md) . Você fornece uma expressão como o primeiro argumento e um tipo para convertê-lo como o segundo argumento. `TryCast`funciona somente em tipos de referência, como classes e interfaces. Ele requer uma relação de herança ou de implementação entre os dois tipos. Isso significa que um tipo deve herdar ou implementar o outro.  
  
## <a name="errors-and-failures"></a>Erros e falhas  
 `TryCast`gera um erro do compilador se detectar que não existe nenhuma relação de herança ou implementação. Mas a falta de um erro do compilador não garante uma conversão bem-sucedida. Se a conversão desejada for restrita, ela poderá falhar em tempo de execução. Se isso acontecer, o não `TryCast` retornará [nada](../nothing.md).  
  
## <a name="conversion-keywords"></a>Palavras-chave de conversão  
 Uma comparação das palavras-chave de conversão de tipo é a seguinte:  
  
|Palavra-chave|Tipos de dados|Relação de argumento|Falha de tempo de execução|  
|---|---|---|---|  
|[Função CType](../functions/ctype-function.md)|Quaisquer tipos de dados|A conversão de alargamento ou de estreitamento deve ser definida entre os dois tipos de dados|Emite<xref:System.InvalidCastException>|  
|[Operador DirectCast](directcast-operator.md)|Quaisquer tipos de dados|Um tipo deve herdar ou implementar o outro tipo|Emite<xref:System.InvalidCastException>|  
|`TryCast`|Somente tipos de referência|Um tipo deve herdar ou implementar o outro tipo|Não retorna [nada](../nothing.md)|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar `TryCast`.  
  
 [!code-vb[VbVbalrKeywords#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>Confira também

- [Conversões de Widening e Narrowing](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [Conversões implícitas e explícitas](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
