---
title: Resolução de sobrecarga
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- overload resolution
- procedures [Visual Basic], overloading
- procedures [Visual Basic], calling
- procedure overloading [Visual Basic], overload resolution
- signatures [Visual Basic], procedure
- overloads [Visual Basic], resolution
ms.assetid: 766115d1-4352-45fb-859f-6063e0de0ec0
ms.openlocfilehash: bcb99ef3845c1ce3998dc9dc8d9f1d335515c0a9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364364"
---
# <a name="overload-resolution-visual-basic"></a>Resolução de sobrecarga (Visual Basic)
Quando o compilador de Visual Basic encontra uma chamada para um procedimento que é definido em várias versões sobrecarregadas, o compilador deve decidir qual das sobrecargas a chamar. Ele faz isso executando as seguintes etapas:  
  
1. **Acessibilidade.** Ele elimina qualquer sobrecarga com um nível de acesso que impede o código de chamada de chamá-lo.  
  
2. **Número de parâmetros.** Ele elimina qualquer sobrecarga que define um número diferente de parâmetros que são fornecidos na chamada.  
  
3. **Tipos de dados de parâmetro.** O compilador fornece preferência de métodos de instância sobre métodos de extensão. Se qualquer método de instância for encontrado e exigir apenas conversões ampliadas para corresponder à chamada de procedimento, todos os métodos de extensão serão descartados e o compilador continuará com apenas os candidatos do método de instância. Se nenhum método de instância desse tipo for encontrado, ele continuará com os métodos de instância e extensão.  
  
     Nesta etapa, ele elimina qualquer sobrecarga para a qual os tipos de dados dos argumentos de chamada não podem ser convertidos nos tipos de parâmetro definidos na sobrecarga.  
  
4. **Refinando conversões.** Ele elimina qualquer sobrecarga que exija uma conversão de restrição dos tipos de argumento de chamada para os tipos de parâmetro definidos. Isso é verdadeiro se a opção de verificação de tipo ([instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)) é `On` ou `Off` .  
  
5. **Menos alargamento.** O compilador considera as sobrecargas restantes em pares. Para cada par, ele compara os tipos de dados dos parâmetros definidos. Se os tipos em uma das sobrecargas forem ampliados para os tipos correspondentes no outro, o compilador eliminará o último. Ou seja, ele retém a sobrecarga que requer a menor quantidade de alargamento.  
  
6. **Único candidato.** Ele continua considerando sobrecargas em pares até que apenas uma sobrecarga permaneça e resolva a chamada para essa sobrecarga. Se o compilador não puder reduzir as sobrecargas para um único candidato, ele gerará um erro.  
  
 A ilustração a seguir mostra o processo que determina qual de um conjunto de versões sobrecarregadas chamar.  
  
 ![Diagrama de fluxo do processo de resolução de sobrecarga](./media/overload-resolution/determine-overloaded-version.gif "Resolvendo entre versões sobrecarregadas")
  
 O exemplo a seguir ilustra esse processo de resolução de sobrecarga.  
  
 [!code-vb[VbVbcnProcedures#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#62)]  
  
 [!code-vb[VbVbcnProcedures#63](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#63)]  
  
 Na primeira chamada, o compilador elimina a primeira sobrecarga porque o tipo do primeiro argumento () é `Short` limitado ao tipo do parâmetro correspondente ( `Byte` ). Em seguida, ele elimina a terceira sobrecarga porque cada tipo de argumento na segunda sobrecarga ( `Short` e `Single` ) se amplia ao tipo correspondente na terceira sobrecarga ( `Integer` e `Single` ). A segunda sobrecarga requer menos alargamento, portanto o compilador a utiliza para a chamada.  
  
 Na segunda chamada, o compilador não pode eliminar nenhuma das sobrecargas com base na restrição. Ele elimina a terceira sobrecarga pelo mesmo motivo que na primeira chamada, porque ela pode chamar a segunda sobrecarga com menos alargamento dos tipos de argumento. No entanto, o compilador não pode resolver entre a primeira e a segunda sobrecargas. Cada um tem um tipo de parâmetro definido que amplia o tipo correspondente no outro ( `Byte` para `Short` , mas `Single` para `Double` ). O compilador, portanto, gera um erro de resolução de sobrecarga.  
  
## <a name="overloaded-optional-and-paramarray-arguments"></a>Argumentos optional e ParamArray sobrecarregados  
 Se duas sobrecargas de um procedimento tiverem assinaturas idênticas, exceto que o último parâmetro é declarado [opcional](../../../language-reference/modifiers/optional.md) em um e [ParamArray](../../../language-reference/modifiers/paramarray.md) no outro, o compilador resolverá uma chamada para esse procedimento da seguinte maneira:  
  
|Se a chamada fornecer o último argumento como|O compilador resolve a chamada para a sobrecarga declarando o último argumento como|  
|---|---|  
|Nenhum valor (argumento omitido)|`Optional`|  
|Um único valor|`Optional`|  
|Dois ou mais valores em uma lista separada por vírgulas|`ParamArray`|  
|Uma matriz de qualquer comprimento (incluindo uma matriz vazia)|`ParamArray`|  
  
## <a name="see-also"></a>Confira também

- [Parâmetros Opcionais](./optional-parameters.md)
- [Matrizes de parâmetros](./parameter-arrays.md)
- [Sobrecarga de procedimento](./procedure-overloading.md)
- [Solucionando problemas de procedimentos](./troubleshooting-procedures.md)
- [Como definir várias versões de um procedimento](./how-to-define-multiple-versions-of-a-procedure.md)
- [Como chamar um procedimento sobrecarregado](./how-to-call-an-overloaded-procedure.md)
- [Como sobrecarregar um procedimento que usa parâmetros opcionais](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [Como sobrecarregar um procedimento que usa um número indefinido de parâmetros](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [Considerações sobre procedimentos de sobrecarga](./considerations-in-overloading-procedures.md)
- [Sobrecargas](../../../language-reference/modifiers/overloads.md)
- [Métodos de Extensão](./extension-methods.md)
