---
title: Como sobrecarregar um procedimento que usa parâmetros opcionais
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], parameters
- procedure overloading [Visual Basic], optional parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
ms.assetid: 825f9d56-4cde-43fd-993a-b9171717e2eb
ms.openlocfilehash: 9ae6818b1e03ccd00ed554e98690e02ffa45de99
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387836"
---
# <a name="how-to-overload-a-procedure-that-takes-optional-parameters-visual-basic"></a>Como sobrecarregar um procedimento que usa parâmetros opcionais (Visual Basic)
Se um procedimento tiver um ou mais parâmetros [opcionais](../../../language-reference/modifiers/optional.md) , você não poderá definir uma versão sobrecarregada que corresponda a qualquer uma de suas sobrecargas implícitas. Para obter mais informações, consulte "sobrecargas implícitas para parâmetros opcionais" em [Considerações sobre procedimentos de sobrecarga](./considerations-in-overloading-procedures.md).  
  
## <a name="one-optional-parameter"></a>Um parâmetro opcional  
  
#### <a name="to-overload-a-procedure-that-takes-one-optional-parameter"></a>Para sobrecarregar um procedimento que recebe um parâmetro opcional  
  
1. Escreva uma `Sub` instrução ou declaração `Function` que inclua o parâmetro opcional na lista de parâmetros. Não use a `Optional` palavra-chave nesta versão sobrecarregada.  
  
2. Preceda a `Sub` `Function` palavra-chave ou com as [sobrecargas](../../../language-reference/modifiers/overloads.md) palavra-chave.  
  
3. Escreva o código de procedimento que deve ser executado quando o código de chamada fornecer o argumento opcional.  
  
4. Encerre o procedimento com a `End Sub` `End Function` instrução ou conforme apropriado.  
  
5. Escreva uma segunda declaração de declaração que seja idêntica à primeira declaração, exceto que ela não inclui o parâmetro opcional na lista de parâmetros.  
  
6. Escreva o código de procedimento que deve ser executado quando o código de chamada não fornecer o argumento opcional. Encerre o procedimento com a `End Sub` `End Function` instrução ou conforme apropriado.  
  
     O exemplo a seguir mostra um procedimento definido com um parâmetro opcional, um conjunto equivalente de dois procedimentos sobrecarregados e, por fim, exemplos de versões sobrecarregadas válidas e válidas.  
  
     [!code-vb[VbVbcnProcedures#59](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#59)]  
  
     [!code-vb[VbVbcnProcedures#60](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#60)]  
  
     [!code-vb[VbVbcnProcedures#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#61)]  
  
## <a name="multiple-optional-parameters"></a>Vários parâmetros opcionais  
 Para um procedimento com mais de um parâmetro opcional, normalmente você precisa de mais de duas versões sobrecarregadas. Por exemplo, se houver dois parâmetros opcionais e o código de chamada puder fornecer ou omitir cada um independente do outro, você precisará de quatro versões sobrecarregadas, uma para cada combinação possível de argumentos fornecidos.  
  
 À medida que aumenta o número de parâmetros opcionais, a complexidade da sobrecarga aumenta. A menos que algumas combinações de argumentos fornecidos não sejam aceitáveis, para N parâmetros opcionais, você precisará de 2 ^ N versões sobrecarregadas. Dependendo da natureza do procedimento, você pode achar que a clareza da lógica justifica o esforço extra de definir todas as versões sobrecarregadas.  
  
#### <a name="to-overload-a-procedure-that-takes-more-than-one-optional-parameter"></a>Para sobrecarregar um procedimento que usa mais de um parâmetro opcional  
  
1. Determine quais combinações de argumentos opcionais fornecidos são aceitáveis para a lógica do procedimento. Uma combinação inaceitável pode surgir se um parâmetro opcional depender de outro. Por exemplo, se um parâmetro aceitar o nome de uma pessoa e outro aceitar a idade da pessoa, uma combinação de argumentos que fornece a idade, mas omitir o nome, será inaceitável.  
  
2. Para cada combinação aceitável de argumentos opcionais fornecidos, escreva uma declaração de `Sub` `Function` declaração ou que defina a lista de parâmetros correspondente. Não use a `Optional` palavra-chave.  
  
3. Em cada declaração, preceda a `Sub` `Function` palavra-chave ou com as [sobrecargas](../../../language-reference/modifiers/overloads.md) palavra-chave.  
  
4. Após cada declaração, escreva o código do procedimento que deve ser executado quando o código de chamada fornecer uma lista de argumentos correspondente à lista de parâmetros da declaração.  
  
5. Encerre cada procedimento com a `End Sub` `End Function` instrução ou conforme apropriado.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Parâmetros Opcionais](./optional-parameters.md)
- [Matrizes de parâmetros](./parameter-arrays.md)
- [Sobrecarga de procedimento](./procedure-overloading.md)
- [Solucionando problemas de procedimentos](./troubleshooting-procedures.md)
- [Como definir várias versões de um procedimento](./how-to-define-multiple-versions-of-a-procedure.md)
- [Como chamar um procedimento sobrecarregado](./how-to-call-an-overloaded-procedure.md)
- [Como sobrecarregar um procedimento que usa um número indefinido de parâmetros](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [Resolução de sobrecarga](./overload-resolution.md)
