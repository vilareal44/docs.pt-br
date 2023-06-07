---
title: Como sobrecarregar um procedimento que usa um número indefinido de parâmetros
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], parameters
- procedure overloading [Visual Basic], indefinite number of parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
ms.assetid: c7042de2-2422-4039-94e8-ac298896af69
ms.openlocfilehash: ddff8c8cd82593b7d89fb0847e56123c287e364b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387875"
---
# <a name="how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters-visual-basic"></a>Como sobrecarregar um procedimento que use um número indefinido de parâmetros (Visual Basic)
Se um procedimento tiver um parâmetro [ParamArray](../../../language-reference/modifiers/paramarray.md) , você não poderá definir uma versão sobrecarregada usando uma matriz unidimensional para a matriz de parâmetros. Para obter mais informações, consulte "sobrecargas implícitas para um parâmetro ParamArray" em [Considerações sobre procedimentos de sobrecarga](./considerations-in-overloading-procedures.md).  
  
### <a name="to-overload-a-procedure-that-takes-a-variable-number-of-parameters"></a>Para sobrecarregar um procedimento que usa um número variável de parâmetros  
  
1. Verifique se o procedimento e a chamada lógica de código se beneficia de versões sobrecarregadas mais do que de um `ParamArray` parâmetro. Consulte "sobrecargas e ParamArrays" em [Considerações sobre os procedimentos de sobrecarga](./considerations-in-overloading-procedures.md).  
  
2. Determine quais números de valores fornecidos o procedimento deve aceitar na parte variável da lista de parâmetros. Isso pode incluir o caso de nenhum valor e pode incluir o caso de uma única matriz unidimensional.  
  
3. Para cada número aceitável de valores fornecidos, grave uma `Sub` `Function` instrução de declaração ou que define a lista de parâmetros correspondente. Não use o `Optional` ou a `ParamArray` palavra-chave nessa versão sobrecarregada.  
  
4. Em cada declaração, preceda a `Sub` `Function` palavra-chave ou com as [sobrecargas](../../../language-reference/modifiers/overloads.md) palavra-chave.  
  
5. Após cada declaração, escreva o código do procedimento que deve ser executado quando o código de chamada fornecer valores correspondentes à lista de parâmetros da declaração.  
  
6. Encerre cada procedimento com a `End Sub` `End Function` instrução ou conforme apropriado.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra um procedimento definido com um parâmetro [ParamArray](../../../language-reference/modifiers/paramarray.md) e, em seguida, um conjunto equivalente de procedimentos sobrecarregados.  
  
 [!code-vb[VbVbcnProcedures#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#69)]  
  
 [!code-vb[VbVbcnProcedures#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#70)]  
  
 Não é possível sobrecarregar esse procedimento com uma lista de parâmetros que usa uma matriz unidimensional para a matriz de parâmetros. No entanto, você pode usar as assinaturas das outras sobrecargas implícitas. As declarações a seguir ilustram isso.  
  
 [!code-vb[VbVbcnProcedures#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#71)]  
  
 O código nas versões sobrecarregadas não tem que testar se o código de chamada forneceu um ou mais valores para o `ParamArray` parâmetro, ou se for, quantos. Visual Basic passa o controle para a versão que corresponde à lista de argumentos de chamada.  
  
## <a name="compile-the-code"></a>Compilar o código  
 Como um procedimento com um `ParamArray` parâmetro é equivalente a um conjunto de versões sobrecarregadas, não é possível sobrecarregar esse procedimento com uma lista de parâmetros correspondente a qualquer uma dessas sobrecargas implícitas. Para obter mais informações, consulte [Considerações sobre sobrecarga de procedimentos](./considerations-in-overloading-procedures.md).  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Sempre que você lida com uma matriz que pode ser indefinidamente grande, há um risco de sobreexecutar alguma capacidade interna de seu aplicativo. Se você aceitar uma matriz de parâmetros, deverá testar o comprimento da matriz que o código de chamada passou e tomar as medidas apropriadas se for muito grande para seu aplicativo.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Parâmetros Opcionais](./optional-parameters.md)
- [Matrizes de parâmetros](./parameter-arrays.md)
- [Sobrecarga de procedimento](./procedure-overloading.md)
- [Solucionando problemas de procedimentos](./troubleshooting-procedures.md)
- [Como definir várias versões de um procedimento](./how-to-define-multiple-versions-of-a-procedure.md)
- [Como chamar um procedimento sobrecarregado](./how-to-call-an-overloaded-procedure.md)
- [Como sobrecarregar um procedimento que usa parâmetros opcionais](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [Resolução de sobrecarga](./overload-resolution.md)
