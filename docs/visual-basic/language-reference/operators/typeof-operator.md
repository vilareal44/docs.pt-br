---
title: Operador TypeOf
ms.date: 07/20/2015
f1_keywords:
- TypeOf
- vb.TypeOf
helpviewer_keywords:
- types [Visual Basic], compatible
- comparison operators [Visual Basic]
- TypeOf...Is expression
- data types [Visual Basic], compatible
- TypeOf operator [Visual Basic]
- compatible data types [Visual Basic]
ms.assetid: 33f65296-659a-4b9a-9a29-c2a91cff68b2
ms.openlocfilehash: 0cce36073b53442bce63f966f3bd94bd5d70d2a8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406313"
---
# <a name="typeof-operator-visual-basic"></a>Operador TypeOf (Visual Basic)
Verifica se o tipo de tempo de execução do resultado de uma expressão é compatível com tipo com o tipo especificado.
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
result = TypeOf objectexpression Is typename  
```  
  
```vb  
result = TypeOf objectexpression IsNot typename  
```  
  
## <a name="parts"></a>Partes  
 `result`  
 Exibido. Um valor `Boolean`.  
  
 `objectexpression`  
 Obrigatórios. Qualquer expressão que seja avaliada como um tipo de referência.  
  
 `typename`  
 Obrigatórios. Qualquer nome de tipo de dados.  
  
## <a name="remarks"></a>Comentários  
 O `TypeOf` operador determina se o tipo de tempo de execução do `objectexpression` é compatível com `typename` . A compatibilidade depende da categoria de tipo de `typename` . A tabela a seguir mostra como a compatibilidade é determinada.  
  
|Categoria de tipo de`typename`|Critério de compatibilidade|  
|---------------------------------|-----------------------------|  
|Classe|`objectexpression`é do tipo `typename` ou herda de`typename`|  
|Estrutura|`objectexpression`é do tipo`typename`|  
|Interface|`objectexpression`implementa `typename` ou herda de uma classe que implementa`typename`|  
  
 Se o tipo de tempo de execução `objectexpression` atende ao critério de compatibilidade, `result` é `True` . Caso contrário, `result` é `False`.  Se `objectexpression` for NULL, então `TypeOf` ... `Is` retorna `False` e... `IsNot` retorna `True` .  
  
 `TypeOf`é sempre usado com a `Is` palavra-chave para construir uma `TypeOf` expressão... `Is` ou com a `IsNot` palavra-chave para construir uma `TypeOf` expressão... `IsNot` .  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa `TypeOf` expressões... `Is` para testar a compatibilidade de tipo de duas variáveis de referência de objeto com vários tipos de dados.  
  
 [!code-vb[VbVbalrOperators#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#39)]  
  
 A variável `refInteger` tem um tipo de tempo de execução de `Integer` . Ele é compatível com `Integer` , mas não com `Double` . A variável `refForm` tem um tipo de tempo de execução de <xref:System.Windows.Forms.Form> . Ele é compatível com <xref:System.Windows.Forms.Form> porque é seu tipo, com <xref:System.Windows.Forms.Control> porque <xref:System.Windows.Forms.Form> herda de <xref:System.Windows.Forms.Control> e com <xref:System.ComponentModel.IComponent> porque <xref:System.Windows.Forms.Form> herda de <xref:System.ComponentModel.Component> , que implementa <xref:System.ComponentModel.IComponent> . No entanto, o `refForm` não é compatível com o <xref:System.Windows.Forms.Label> .  
  
## <a name="see-also"></a>Confira também

- [Operador Is](is-operator.md)
- [Operador IsNot](isnot-operator.md)
- [Operadores de comparação no Visual Basic](../../programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operadores e expressões](../../programming-guide/language-features/operators-and-expressions/index.md)
