---
title: Considerações sobre procedimentos de sobrecarga
ms.date: 07/20/2015
helpviewer_keywords:
- signatures [Visual Basic], ParamArray arguments
- ParamArray keyword [Visual Basic], parameter arrays
- ParamArray keyword [Visual Basic], arguments and signatures
- function overloading [Visual Basic], implicit overloads for ParamArray
- ParamArray keyword [Visual Basic], signatures
- Visual Basic code, procedures
- arguments [Visual Basic], parameter arrays
- procedures [Visual Basic], overloading
- parameters [Visual Basic], lists
- function overloading [Visual Basic], typeless programming
- typeless programming
- function overloading [Visual Basic], restrictions
- arguments [Visual Basic], optional
- optional arguments [Visual Basic], overloading
- signatures [Visual Basic], procedure
- parameter lists [Visual Basic]
- parameter arrays [Visual Basic], overloading arguments
- Visual Basic code, parameter lists
- procedure overloading [Visual Basic], considerations
- Option Explicit statement [Visual Basic]
- restrictions [Visual Basic], overloading procedures
- procedures [Visual Basic], parameter lists
ms.assetid: a2001248-10d0-42c5-b0ce-eeedc987319f
ms.openlocfilehash: c4075c87df8b9daa56ca1d35e0d6661598895fdc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403363"
---
# <a name="considerations-in-overloading-procedures-visual-basic"></a>Considerações sobre procedimentos de sobrecarga (Visual Basic)
Ao sobrecarregar um procedimento, você deve usar uma *assinatura* diferente para cada versão sobrecarregada. Isso geralmente significa que cada versão deve especificar uma lista de parâmetros diferente. Para obter mais informações, consulte "assinatura diferente" em [sobrecarga de procedimento](./procedure-overloading.md).  
  
 Você pode sobrecarregar um `Function` procedimento com um `Sub` procedimento e vice-versa, desde que eles tenham assinaturas diferentes. Duas sobrecargas não podem diferir apenas se uma tiver um valor de retorno e a outra não.  
  
 Você pode sobrecarregar uma propriedade da mesma maneira que sobrecarrega um procedimento e com as mesmas restrições. No entanto, você não pode sobrecarregar um procedimento com uma propriedade ou vice-versa.  
  
## <a name="alternatives-to-overloaded-versions"></a>Alternativas para versões sobrecarregadas  
 Às vezes, você tem alternativas para versões sobrecarregadas, especialmente quando a presença de argumentos é opcional ou seu número é variável.  
  
 Tenha em mente que os argumentos opcionais não são necessariamente suportados por todas as linguagens, e as matrizes de parâmetros são limitadas a Visual Basic. Se você estiver escrevendo um procedimento que provavelmente será chamado a partir de um código escrito em qualquer uma das várias linguagens diferentes, as versões sobrecarregadas oferecem a maior flexibilidade.  
  
### <a name="overloads-and-optional-arguments"></a>Sobrecargas e argumentos opcionais  
 Quando o código de chamada pode opcionalmente fornecer ou omitir um ou mais argumentos, você pode definir várias versões sobrecarregadas ou usar parâmetros opcionais.  
  
#### <a name="when-to-use-overloaded-versions"></a>Quando usar versões sobrecarregadas  
 Você pode considerar a definição de uma série de versões sobrecarregadas nos seguintes casos:  
  
- A lógica no código do procedimento é significativamente diferente dependendo se o código de chamada fornece um argumento opcional ou não.  
  
- O código do procedimento não pode testar de maneira confiável se o código de chamada forneceu um argumento opcional. Esse é o caso, por exemplo, se não houver nenhum candidato possível para um valor padrão que o código de chamada não tenha se esperado de fornecer.  
  
#### <a name="when-to-use-optional-parameters"></a>Quando usar parâmetros opcionais  
 Você pode preferir um ou mais parâmetros opcionais nos seguintes casos:  
  
- A única ação necessária quando o código de chamada não fornece um argumento opcional é definir o parâmetro como um valor padrão. Nesse caso, o código do procedimento pode ser menos complicado se você definir uma única versão com um ou mais `Optional` parâmetros.  
  
 Para obter mais informações, consulte [parâmetros opcionais](./optional-parameters.md).  
  
### <a name="overloads-and-paramarrays"></a>Sobrecargas e ParamArrays  
 Quando o código de chamada pode passar um número variável de argumentos, você pode definir várias versões sobrecarregadas ou usar uma matriz de parâmetros.  
  
#### <a name="when-to-use-overloaded-versions"></a>Quando usar versões sobrecarregadas  
 Você pode considerar a definição de uma série de versões sobrecarregadas nos seguintes casos:  
  
- Você sabe que o código de chamada nunca passa mais do que um pequeno número de valores para a matriz de parâmetros.  
  
- A lógica no código do procedimento é significativamente diferente dependendo de quantos valores o código de chamada passa.  
  
- O código de chamada pode passar valores de tipos de dados diferentes.  
  
#### <a name="when-to-use-a-parameter-array"></a>Quando usar uma matriz de parâmetros  
 Você é mais bem atendido por um `ParamArray` parâmetro nos seguintes casos:  
  
- Você não pode prever quantos valores o código de chamada pode passar para a matriz de parâmetros e pode ser um grande número.  
  
- A lógica do procedimento se presta para iterar em todos os valores que o código de chamada passa, executando essencialmente as mesmas operações em cada valor.  
  
 Para obter mais informações, consulte [matrizes de parâmetros](./parameter-arrays.md).  
  
## <a name="implicit-overloads-for-optional-parameters"></a>Sobrecargas implícitas para parâmetros opcionais  
 Um procedimento com um parâmetro [opcional](../../../language-reference/modifiers/optional.md) é equivalente a dois procedimentos sobrecarregados, um com o parâmetro opcional e outro sem ele. Não é possível sobrecarregar esse procedimento com uma lista de parâmetros correspondente a um desses. As declarações a seguir ilustram isso.  
  
 [!code-vb[VbVbcnProcedures#58](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#58)]  
  
 [!code-vb[VbVbcnProcedures#60](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#60)]  
  
 [!code-vb[VbVbcnProcedures#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#61)]  
  
 Para um procedimento com mais de um parâmetro opcional, há um conjunto de sobrecargas implícitas, recebidos pela lógica semelhante àquela no exemplo anterior.  
  
## <a name="implicit-overloads-for-a-paramarray-parameter"></a>Sobrecargas implícitas para um parâmetro ParamArray  
 O compilador considera um procedimento com um parâmetro [ParamArray](../../../language-reference/modifiers/paramarray.md) para ter um número infinito de sobrecargas, diferente do outro no que o código de chamada passa para a matriz de parâmetros, da seguinte maneira:  
  
- Uma sobrecarga para quando o código de chamada não fornece um argumento para o`ParamArray`  
  
- Uma sobrecarga para quando o código de chamada fornece uma matriz unidimensional do `ParamArray` tipo de elemento  
  
- Para cada inteiro positivo, uma sobrecarga para quando o código de chamada fornece esse número de argumentos, cada um dos `ParamArray` tipos de elemento  
  
 As declarações a seguir ilustram essas sobrecargas implícitas.  
  
 [!code-vb[VbVbcnProcedures#68](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#68)]  
  
 [!code-vb[VbVbcnProcedures#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#70)]  
  
 Não é possível sobrecarregar esse procedimento com uma lista de parâmetros que usa uma matriz unidimensional para a matriz de parâmetros. No entanto, você pode usar as assinaturas das outras sobrecargas implícitas. As declarações a seguir ilustram isso.  
  
 [!code-vb[VbVbcnProcedures#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#71)]  
  
## <a name="typeless-programming-as-an-alternative-to-overloading"></a>Programação não tipada como uma alternativa à sobrecarga  
 Se você quiser permitir que o código de chamada transmita tipos de dados diferentes para um parâmetro, uma abordagem alternativa será a programação sem tipo. Você pode definir a opção de verificação de tipo como `Off` com a [instrução Option Strict](../../../language-reference/statements/option-strict-statement.md) ou a opção de compilador [-optionstrict](../../../reference/command-line-compiler/optionstrict.md) . Em seguida, você não precisa declarar o tipo de dados do parâmetro. No entanto, essa abordagem tem as seguintes desvantagens em comparação com a sobrecarga:  
  
- A programação sem tipo produz um código de execução menos eficiente.  
  
- O procedimento deve testar para cada tipo de dados que ele prevê que está sendo passado.  
  
- O compilador não poderá sinalizar um erro se o código de chamada passar um tipo de dados ao qual o procedimento não dá suporte.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Solucionando problemas de procedimentos](./troubleshooting-procedures.md)
- [Como definir várias versões de um procedimento](./how-to-define-multiple-versions-of-a-procedure.md)
- [Como chamar um procedimento sobrecarregado](./how-to-call-an-overloaded-procedure.md)
- [Como sobrecarregar um procedimento que usa parâmetros opcionais](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [Como sobrecarregar um procedimento que usa um número indefinido de parâmetros](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [Resolução de sobrecarga](./overload-resolution.md)
- [Sobrecargas](../../../language-reference/modifiers/overloads.md)
