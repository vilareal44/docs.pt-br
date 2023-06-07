---
title: Tipo de dados booliano
ms.date: 07/20/2015
f1_keywords:
- vb.FALSE
- vb.TRUE
- vb.Boolean
helpviewer_keywords:
- Boolean data type
- Boolean values [Visual Basic], False keyword
- False keyword [Visual Basic]
- True keyword [Visual Basic]
- Boolean values [Visual Basic], True keyword
ms.assetid: 4858e630-4813-4216-a55e-f4d0feb884e4
ms.openlocfilehash: 851c524a83c5f24b77a151634a96798249c5905e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84374415"
---
# <a name="boolean-data-type-visual-basic"></a>Tipo de dados booliano (Visual Basic)

Contém valores que podem ser apenas `True` ou `False` . As palavras-chave `True` e `False` correspondem aos dois Estados de `Boolean` variáveis.  
  
## <a name="remarks"></a>Comentários  

 Use o [tipo de dados booliano (Visual Basic)](boolean-data-type.md) para conter valores de dois Estados, como true/false, yes/no ou on/off.  
  
 O valor padrão de `Boolean` é `False`.  
  
 `Boolean`os valores não são armazenados como números e os valores armazenados não devem ser equivalentes aos números. Você nunca deve escrever código que dependa de valores numéricos equivalentes para `True` e `False` . Sempre que possível, você deve restringir o uso de `Boolean` variáveis para os valores lógicos para os quais elas foram projetadas.  
  
## <a name="type-conversions"></a>Conversões de tipo  

 Quando Visual Basic converte valores de tipo de dados numéricos para `Boolean` , 0 se torna `False` e todos os outros valores se tornam `True` . Quando Visual Basic converte `Boolean` valores em tipos numéricos, `False` torna-se 0 e `True` se torna-1.  
  
 Ao converter entre `Boolean` valores e tipos de dados numéricos, tenha em mente que os métodos de conversão de .NET Framework nem sempre produzem os mesmos resultados que as palavras-chave de conversão de Visual Basic. Isso ocorre porque a conversão de Visual Basic retém o comportamento compatível com as versões anteriores. Para obter mais informações, consulte "o tipo booliano não converte em tipo numérico com precisão" em [tipos de dados de solução de problemas](../../programming-guide/language-features/data-types/troubleshooting-data-types.md).  
  
## <a name="programming-tips"></a>Dicas de programação  
  
- **Números negativos.** `Boolean`Não é um tipo numérico e não pode representar um valor negativo. Em qualquer caso, você não deve usar `Boolean` para armazenar valores numéricos.  
  
- **Digite os caracteres.** `Boolean`Não tem caractere de tipo literal ou caractere de tipo de identificador.  
  
- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.Boolean?displayProperty=nameWithType>.  
  
## <a name="example"></a>Exemplo  

 No exemplo a seguir, `runningVB` é uma `Boolean` variável, que armazena uma configuração Sim/não simples.  
  
```vb  
Dim runningVB As Boolean  
' Check to see if program is running on Visual Basic engine.  
If scriptEngine = "VB" Then  
    runningVB = True  
End If  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Boolean?displayProperty=nameWithType>
- [Tipos de dados](index.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [Solução de problemas de tipos de dados](../../programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [Função CType](../functions/ctype-function.md)
