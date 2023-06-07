---
title: Caracteres especiais no código
ms.date: 07/20/2015
f1_keywords:
- vb.)
- vb.(
- vb.colon
- vb.!
- vb..
- 'vb.:'
helpviewer_keywords:
- special characters [Visual Basic], in code
- parentheses [Visual Basic], using in code
- colons (:)
- period character in code
- dot operator (.)
- dictionary access operator [Visual Basic]
- concatenation operators [Visual Basic], special characters in code
- concatenation operators [Visual Basic], vs. addition operator
- '! operator'
- separators [Visual Basic], using in code
- operators [Visual Basic], dictionary access
- ': separator character'
- member access operator [Visual Basic]
- addition operator [Visual Basic]
- operators [Visual Basic], member access
- . operator
- exclamation points
- separators
- exclamation point operator (!)
- Visual Basic code, special characters
ms.assetid: 310dce0c-45b5-4e0d-83e9-32df258d2a3e
ms.openlocfilehash: c9b170ed812474cdeee100f1dc388d5c7e85f2cc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400585"
---
# <a name="special-characters-in-code-visual-basic"></a>Caracteres especiais no código (Visual Basic)
Às vezes, você precisa usar caracteres especiais em seu código, ou seja, caracteres que não são alfabéticos ou numéricos. A pontuação e os caracteres especiais no conjunto de caracteres Visual Basic têm vários usos, da organização do texto do programa para definir as tarefas que o compilador ou o programa compilado executa. Eles não especificam uma operação a ser executada.  
  
## <a name="parentheses"></a>Parênteses  
 Use parênteses ao definir um procedimento, como um `Sub` ou `Function` . Você deve colocar todas as listas de argumentos de procedimento entre parênteses. Você também usa parênteses para colocar variáveis ou argumentos em grupos lógicos, especialmente para substituir a ordem padrão de precedência de operador em uma expressão complexa. O exemplo a seguir ilustra isto.  
  
 [!code-vb[VbVbcnConventions#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#11)]  
  
 Após a execução do código anterior, o valor de `d` é 8,225 e o valor de `e` é 3. O cálculo para `d` usa a precedência padrão `/` de `+` e é equivalente a `d = b + (c / a)` . Os parênteses no cálculo para `e` substituir a precedência padrão.  
  
## <a name="separators"></a>Separadores  
 Os separadores fazem o que seu nome sugere: Eles separam seções de código. Em Visual Basic, o caractere separador é o dois-pontos ( `:` ). Use separadores quando desejar incluir várias instruções em uma única linha em vez de linhas separadas. Isso economiza espaço e melhora a legibilidade do seu código. O exemplo a seguir mostra três instruções separadas por dois-pontos.  
  
 [!code-vb[VbVbcnConventions#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#12)]  
  
 Para obter mais informações, consulte [como: quebrar e combinar instruções no código](how-to-break-and-combine-statements-in-code.md).  
  
 O caractere de dois-pontos ( `:` ) também é usado para identificar um rótulo de instrução. Para obter mais informações, consulte [instruções de rótulo](how-to-label-statements.md).  
  
## <a name="concatenation"></a>Concatenação  
 Use o `&` operador para *concatenação*ou vinculação de cadeias de caracteres. Não o confunda com o `+` operador, que adiciona valores numéricos. Se você usar o `+` operador para concatenar ao operar em valores numéricos, poderá obter resultados incorretos. O exemplo a seguir demonstra isso.  
  
 [!code-vb[VbVbcnConventions#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#13)]  
  
 Após a execução do código anterior, o valor de `resultA` é 21, 1 e o valor de `resultB` é "10, 111".  
  
## <a name="member-access-operators"></a>Operadores de acesso de membro  
 Para acessar um membro de um tipo, use o operador ponto ( `.` ) ou ponto de exclamação ( `!` ) entre o nome do tipo e o nome do membro.  
  
### <a name="dot--operator"></a>Ponto (.) Operador  
 Use o `.` operador em uma classe, estrutura, interface ou enumeração como um operador de acesso de membro. O membro pode ser um campo, propriedade, evento ou método. O exemplo a seguir ilustra isto.  
  
 [!code-vb[VbVbcnConventions#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#14)]  
  
### <a name="exclamation-point--operator"></a>Ponto de exclamação (!) Operador  
 Use o `!` operador somente em uma classe ou interface como um operador de acesso de dicionário. A classe ou interface deve ter uma propriedade padrão que aceite um único `String` argumento. O identificador imediatamente após o `!` operador torna-se o valor do argumento passado para a propriedade padrão como uma cadeia de caracteres. O exemplo a seguir demonstra isso.  
  
 [!code-vb[VbVbcnConventions#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#15)]  
  
 As três linhas de saída de `MsgBox` todos exibem o valor `32856` . A primeira linha usa o acesso tradicional à propriedade `index` , o segundo utiliza o fato que `index` é a propriedade padrão da classe `hasDefault` e o terceiro usa o acesso de dicionário à classe.  
  
 Observe que o segundo operando do `!` operador deve ser um identificador de Visual Basic válido não entre aspas duplas ( `" "` ). Em outras palavras, você não pode usar um literal de cadeia de caracteres ou uma variável de cadeia de caracteres. A seguinte alteração na última linha da `MsgBox` chamada gera um erro porque `"X"` é um literal de cadeia de caracteres incluído.  
  
 `"Dictionary access returns " & hD!"X")`  
  
> [!NOTE]
> Referências a coleções padrão devem ser explícitas. Em particular, você não pode usar o `!` operador em uma variável de associação tardia.  
  
 O `!` caractere também é usado como o `Single` caractere de tipo.  
  
## <a name="see-also"></a>Confira também

- [Estrutura do programa e convenções de código](program-structure-and-code-conventions.md)
- [Caracteres de tipo](../language-features/data-types/type-characters.md)
