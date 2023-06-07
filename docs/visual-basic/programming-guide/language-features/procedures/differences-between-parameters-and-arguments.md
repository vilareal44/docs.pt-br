---
title: Diferenças entre parâmetros e argumentos
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- parameters [Visual Basic], and arguments
- procedure arguments
- Visual Basic code, procedures
- arguments [Visual Basic], and parameters
- procedure parameters
- parameters [Visual Basic], definition
ms.assetid: c237c056-74f4-4749-9f2c-15864f139a31
ms.openlocfilehash: dd0a62b6567f3e74763b7f2e9b96803c193c7976
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403350"
---
# <a name="differences-between-parameters-and-arguments-visual-basic"></a>Diferenças entre parâmetros e argumentos (Visual Basic)
Na maioria dos casos, um procedimento deve ter algumas informações sobre as circunstâncias em que ele foi chamado. Um procedimento que executa tarefas repetidas ou compartilhadas usa informações diferentes para cada chamada. Essas informações consistem em variáveis, constantes e expressões que você passa para o procedimento ao chamá-lo.  
  
 Para comunicar essas informações ao procedimento, o procedimento define um *parâmetro*e o código de chamada passa um *argumento* para esse parâmetro. Você pode considerar o parâmetro como um espaço de estacionamento e o argumento como um automóvel. Assim como os automóveis diferentes podem estacionar em um espaço de estacionamento em momentos diferentes, o código de chamada pode passar um argumento diferente para o mesmo parâmetro toda vez que ele chama o procedimento.  
  
## <a name="parameters"></a>Parâmetros  
 Um *parâmetro* representa um valor que o procedimento espera que você passe ao chamá-lo. A declaração do procedimento define seus parâmetros.  
  
 Ao definir um `Function` procedimento ou `Sub` , você especifica uma *lista de parâmetros* entre parênteses imediatamente após o nome do procedimento. Para cada parâmetro, você especifica um nome, um tipo de dados e um mecanismo de passagem ([ByVal](../../../language-reference/modifiers/byval.md) ou [ByRef](../../../language-reference/modifiers/byref.md)). Você também pode indicar que um parâmetro é opcional. Isso significa que o código de chamada não precisa passar um valor para ele.  
  
 O nome de cada parâmetro serve como uma *variável local* no procedimento. Você usa o nome do parâmetro da mesma maneira que usa qualquer outra variável.  
  
## <a name="arguments"></a>Argumentos  
 Um *argumento* representa o valor que você passa para um parâmetro de procedimento quando chama o procedimento. O código de chamada fornece os argumentos ao chamar o procedimento.  
  
 Ao chamar um `Function` procedimento ou `Sub` , você inclui uma *lista de argumentos* entre parênteses imediatamente após o nome do procedimento. Cada argumento corresponde ao parâmetro na mesma posição na lista.  
  
 Ao contrário da definição de parâmetro, os argumentos não têm nomes. Cada argumento é uma expressão, que pode conter zero ou mais variáveis, constantes e literais. O tipo de dados da expressão avaliada normalmente deve corresponder ao tipo de dados definido para o parâmetro correspondente e, em qualquer caso, ele deve ser conversível para o tipo de parâmetro.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Subprocedimentos](./sub-procedures.md)
- [Procedimentos de função](./function-procedures.md)
- [Procedimentos de propriedade](./property-procedures.md)
- [Procedimentos do operador](./operator-procedures.md)
- [Como definir um parâmetro para um procedimento](./how-to-define-a-parameter-for-a-procedure.md)
- [Como passar argumentos para um procedimento](./how-to-pass-arguments-to-a-procedure.md)
- [Passar argumentos por valor e por referência](./passing-arguments-by-value-and-by-reference.md)
- [Procedimentos recursivos](./recursive-procedures.md)
- [Sobrecarga de procedimento](./procedure-overloading.md)
