---
title: Tipos de dados
ms.date: 07/20/2015
helpviewer_keywords:
- data types [Visual Basic], declaring
- typing
- data types [Visual Basic]
- Visual Basic code, data types
- data types [Visual Basic], improving speed with
ms.assetid: 5e1b9aaf-c7ca-4b29-9b22-0e82ed8e85e2
ms.openlocfilehash: 928d7fb6a40873641ae3e91e057c272b946a0091
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393709"
---
# <a name="data-types-in-visual-basic"></a>Tipos de dados no Visual Basic
O *tipo de dados* de um elemento de programação se refere a que tipo de dados ele pode armazenar e como ele armazena esses dados. Os tipos de dados se aplicam a todos os valores que podem ser armazenados na memória do computador ou participam da avaliação de uma expressão. Cada variável, literal, constante, enumeração, propriedade, parâmetro de procedimento, argumento de procedimento e valor retornado do procedimento tem um tipo de dados.  
  
## <a name="declared-data-types"></a>Tipos de dados declarados  
 Você define um elemento de programação com uma instrução de declaração, e você especifica o tipo de dados com a cláusula `As`. A tabela a seguir mostra as instruções que você usa para declarar vários elementos.  
  
|Elemento de programação|Declaração de tipo de dados|  
|-------------------------|---------------------------|  
|Variável|Em uma [instrução Dim](../../../language-reference/statements/dim-statement.md)<br /><br /> `Dim`   `amount As Double`<br /><br /> `Static`   `yourName As String`<br /><br /> `Public`   `billsPaid As Decimal = 0`|  
|Literal|Com um caractere de tipo literal. Consulte "Caracteres de tipo Literal" em [Caracteres de tipo](type-characters.md)<br /><br /> `Dim searchChar As Char = "."`  `C`|  
|Constante|Em uma [instrução Const](../../../language-reference/statements/const-statement.md)<br /><br /> `Const`   `modulus As Single = 4.17825F`|  
|Enumeração|Em uma [instrução Enum](../../../language-reference/statements/enum-statement.md)<br /><br /> `Public`   `Enum`   `colors`|  
|Propriedade|Em uma [instrução Property](../../../language-reference/statements/property-statement.md)<br /><br /> `Property`   `region() As String`|  
|Parâmetro de procedimento|Em uma [instrução Sub](../../../language-reference/statements/sub-statement.md), [instrução Function](../../../language-reference/statements/function-statement.md) ou [instrução Operator](../../../language-reference/statements/operator-statement.md)<br /><br /> `Sub addSale(ByVal`   `amount`   `As Double)`|  
|Argumento de procedimento|No código de chamada. Cada argumento é um elemento de programação que já foi declarado ou uma expressão que contém elementos declarados<br /><br /> `subString = Left(`  `inputString`  `,`   `5`  `)`|  
|Valor retornado do procedimento|Em uma [instrução Function](../../../language-reference/statements/function-statement.md) ou [instrução Operator](../../../language-reference/statements/operator-statement.md)<br /><br /> `Function convert(ByVal b As Byte)`   `As String`|  
  
 Para obter uma lista dos tipos de dados do Visual Basic, consulte [Tipos de dados](../../../language-reference/data-types/index.md).  
  
## <a name="see-also"></a>Confira também

- [Caracteres de tipo](type-characters.md)
- [Tipos de dados elementares](elementary-data-types.md)
- [Tipos de dados compostos](composite-data-types.md)
- [Tipos genéricos no Visual Basic](generic-types.md)
- [Tipos de valor e referência](value-types-and-reference-types.md)
- [Conversões de tipo no Visual Basic](type-conversions.md)
- [Estruturas](structures.md)
- [Tuplas](tuples.md)
- [Solução de problemas de tipos de dados](troubleshooting-data-types.md)
- [Tipos de dados](../../../language-reference/data-types/index.md)
- [Uso eficiente de tipos de dados](efficient-use-of-data-types.md)
