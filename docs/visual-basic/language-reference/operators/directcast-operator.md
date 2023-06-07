---
title: Operador DirectCast
ms.date: 07/20/2015
f1_keywords:
- vb.directCast
- directCast
helpviewer_keywords:
- DirectCast keyword [Visual Basic]
ms.assetid: 63e5a1d0-4d9e-4732-bf8f-e90c0c8784b8
ms.openlocfilehash: 96cb2082d59373deb34d6894270205b7c1045850
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371512"
---
# <a name="directcast-operator-visual-basic"></a>Operador DirectCast (Visual Basic)
Apresenta uma operação de conversão de tipo baseada na herança ou implementação.  
  
## <a name="remarks"></a>Comentários  
 `DirectCast`o não usa as rotinas auxiliares de tempo de execução Visual Basic para conversão, para que possa fornecer um desempenho um pouco melhor do que `CType` ao converter de e para o tipo de dados `Object` .  
  
 Você usa a `DirectCast` palavra-chave semelhante à maneira como usa a [função CType](../functions/ctype-function.md) e a palavra-chave do [Operador TryCast](trycast-operator.md) . Você fornece uma expressão como o primeiro argumento e um tipo para convertê-lo como o segundo argumento. `DirectCast`requer uma relação de herança ou de implementação entre os tipos de dados dos dois argumentos. Isso significa que um tipo deve herdar ou implementar o outro.  
  
## <a name="errors-and-failures"></a>Erros e falhas  
 `DirectCast`gera um erro do compilador se detectar que não existe nenhuma relação de herança ou implementação. Mas a falta de um erro do compilador não garante uma conversão bem-sucedida. Se a conversão desejada for restrita, ela poderá falhar em tempo de execução. Se isso acontecer, o tempo de execução gera um <xref:System.InvalidCastException> erro.  
  
## <a name="conversion-keywords"></a>Palavras-chave de conversão  
 Uma comparação das palavras-chave de conversão de tipo é a seguinte:  
  
|Palavra-chave|Tipos de dados|Relação de argumento|Falha de tempo de execução|  
|---|---|---|---|  
|[Função CType](../functions/ctype-function.md)|Quaisquer tipos de dados|A conversão de alargamento ou de estreitamento deve ser definida entre os dois tipos de dados|Emite<xref:System.InvalidCastException>|  
|`DirectCast`|Quaisquer tipos de dados|Um tipo deve herdar ou implementar o outro tipo|Emite<xref:System.InvalidCastException>|  
|[Operador TryCast](trycast-operator.md)|Somente tipos de referência|Um tipo deve herdar ou implementar o outro tipo|Não retorna [nada](../nothing.md)|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra dois usos de `DirectCast` , um que falha em tempo de execução e um que é bem sucedido.  
  
 [!code-vb[VbVbalrKeywords#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#1)]  
  
 No exemplo anterior, o tipo de tempo de execução de `q` é `Double` . `CType`tem sucesso porque `Double` pode ser convertido em `Integer` . No entanto, a primeira `DirectCast` falha em tempo de execução porque o tipo de tempo de execução de `Double` não tem nenhuma relação de herança com `Integer` , embora exista uma conversão. O segundo `DirectCast` é executado com sucesso porque converte de tipo <xref:System.Windows.Forms.Form> para tipo <xref:System.Windows.Forms.Control> , do qual <xref:System.Windows.Forms.Form> herda.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Convert.ChangeType%2A?displayProperty=nameWithType>
- [Conversões de Widening e Narrowing](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [Conversões implícitas e explícitas](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
