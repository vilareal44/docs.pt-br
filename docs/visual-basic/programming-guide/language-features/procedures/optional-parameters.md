---
title: Parâmetros Opcionais
ms.date: 07/20/2015
helpviewer_keywords:
- parameters [Visual Basic], optional
- Visual Basic code, procedures
- procedures [Visual Basic], optional arguments
- optional arguments
- named arguments [Visual Basic], and optional arguments
- optional parameters
- Optional keyword [Visual Basic], optional arguments
- arguments [Visual Basic], optional
- optional arguments [Visual Basic], and named arguments
ms.assetid: 398d2845-1069-4e94-b934-a73b545c8b87
ms.openlocfilehash: 4e07b75c94b4aea681e6e862e161bda80b2833fc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364325"
---
# <a name="optional-parameters-visual-basic"></a>Parâmetros opcionais (Visual Basic)
Você pode especificar que um parâmetro de procedimento é opcional e nenhum argumento precisa ser fornecido para ele quando o procedimento é chamado. Os *parâmetros opcionais* são indicados pela `Optional` palavra-chave na definição do procedimento. As seguintes regras se aplicam:  
  
- Cada parâmetro opcional na definição do procedimento deve especificar um valor padrão.  
  
- O valor padrão para um parâmetro opcional deve ser uma expressão constante.  
  
- Todo parâmetro após um parâmetro opcional na definição do procedimento também deve ser opcional.  
  
 A sintaxe a seguir mostra uma declaração de procedimento com um parâmetro opcional:  
  
```vb  
Sub name(ByVal parameter1 As datatype1, Optional ByVal parameter2 As datatype2 = defaultvalue)  
```  
  
## <a name="calling-procedures-with-optional-parameters"></a>Chamando procedimentos com parâmetros opcionais  
 Ao chamar um procedimento com um parâmetro opcional, você pode escolher se deseja fornecer o argumento. Caso contrário, o procedimento usa o valor padrão declarado para esse parâmetro.  
  
 Ao omitir um ou mais argumentos opcionais no lista de argumentos, você use sucessivas vírgulas para marcar as posições. A chamada de exemplo a seguir fornece o primeiro e o quarto argumentos, mas não o segundo ou terceiro:  
  
```vb  
Sub name(argument 1, , , argument 4)  
```  
  
 O exemplo a seguir faz várias chamadas para a função `MsgBox`. `MsgBox` tem um parâmetro obrigatório e dois parâmetros opcionais.  
  
 A primeira chamada para `MsgBox` fornece todos os três argumentos na ordem que `MsgBox` os define. A segunda chamada fornece apenas o argumento necessário. A terceira e a quarta chamadas fornecem o primeiro e o terceiro argumentos. A terceira chamada faz isso por posição, e a quarta chamada faz isso por nome.  
  
 [!code-vb[VbVbcnProcedures#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#47)]  
  
## <a name="determining-whether-an-optional-argument-is-present"></a>Determinando se um argumento opcional está presente  
 Um procedimento não pode detectar no tempo de execução se um determinado argumento foi omitido ou o código de chamada forneceu explicitamente o valor padrão. Se você precisar fazer essa distinção, defina um valor improvável como o padrão. O procedimento a seguir define o parâmetro opcional `office` e testa seu valor padrão, `QJZ` , para ver se ele foi omitido na chamada:  
  
 [!code-vb[VbVbcnProcedures#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#46)]  
  
 Se o parâmetro opcional for um tipo de referência como `String`, você poderá usar `Nothing` como valor padrão, desde que não seja um valor esperado para o argumento.  
  
## <a name="optional-parameters-and-overloading"></a>Parâmetros opcionais e sobrecarga  
 Outra maneira de definir um procedimento com parâmetros opcionais é usar a sobrecarga. Se você tiver um parâmetro opcional, poderá definir duas versões sobrecarregadas do procedimento, uma aceitando o parâmetro e outra sem ele. Essa abordagem torna-se mais complicada à medida que o número de parâmetros opcionais aumenta. No entanto, sua vantagem é que você pode ter certeza absoluta se o programa de chamada forneceu cada argumento opcional.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Passar argumentos por valor e por referência](./passing-arguments-by-value-and-by-reference.md)
- [Passando argumentos por posição e nome](./passing-arguments-by-position-and-by-name.md)
- [Matrizes de parâmetros](./parameter-arrays.md)
- [Sobrecarga de procedimento](./procedure-overloading.md)
- [Opcional](../../../language-reference/modifiers/optional.md)
- [ParamArray](../../../language-reference/modifiers/paramarray.md)
