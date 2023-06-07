---
title: Tipos de Dados de Resultados do Operador
ms.date: 07/20/2015
helpviewer_keywords:
- data types [Visual Basic], operator result data types
- result data types [Visual Basic]
- operator result data types [Visual Basic]
- operators [Visual Basic], data types
- data types [Visual Basic], ranges
- operators [Visual Basic], result data types
ms.assetid: 9d524533-e1a1-4aa8-b1b8-622068173d06
ms.openlocfilehash: b80508c5619770da0c7dc78003ff9d4847dd94d8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371421"
---
# <a name="data-types-of-operator-results-visual-basic"></a>Tipos de dados de resultados do operador (Visual Basic)
Visual Basic determina o tipo de dados de resultado de uma operação com base nos tipos de dados dos operandos. Em alguns casos, isso pode ser um tipo de dados com um intervalo maior do que o de um dos operandos.  
  
## <a name="data-type-ranges"></a>Intervalos de tipos de dados  
 Os intervalos dos tipos de dados relevantes, na ordem do menor para o maior, são os seguintes:  
  
- [Booliano](../data-types/boolean-data-type.md) — dois valores possíveis  
  
- [SByte](../data-types/sbyte-data-type.md), [byte](../data-types/byte-data-type.md) — 256 possíveis valores integrais  
  
- [Short](../data-types/short-data-type.md), [UShort](../data-types/ushort-data-type.md) — 65.536 (6.5... E + 4) valores integrais possíveis  
  
- [Integer](../data-types/integer-data-type.md), [UInteger](../data-types/uinteger-data-type.md) — 4.294.967.296 (4.2... E + 9) valores integrais possíveis  
  
- [Long](../data-types/long-data-type.md), [ULONG](../data-types/ulong-data-type.md) — 18446744073709551615 (1.8... E + 19) valores integrais possíveis  
  
- [Decimal](../data-types/decimal-data-type.md) – 1,5... e + 29 valores integrais possíveis, intervalo máximo de 7.9... e + 28 (valor absoluto)  
  
- [Único](../data-types/single-data-type.md) – intervalo máximo de 3.4... E + 38 (valor absoluto)  
  
- [Double](../data-types/double-data-type.md) — intervalo máximo 1.7... E + 308 (valor absoluto)  
  
 Para obter mais informações sobre tipos de dados Visual Basic, consulte [tipos de dados](../data-types/index.md).  
  
 Se um operando for avaliado como [Nothing](../nothing.md), o Visual Basic operadores aritméticos o tratará como zero.  
  
## <a name="decimal-arithmetic"></a>Aritmética decimal  
 Observe que o tipo de dados [decimal](../data-types/decimal-data-type.md) não é ponto flutuante nem inteiro.  
  
 Se um dos operandos de uma `+` operação,, `–` `*` `/` ou `Mod` for `Decimal` e o outro não for `Single` ou `Double` , Visual Basic ampliará o outro operando para `Decimal` . Ele executa a operação em `Decimal` e o tipo de dados de resultado é `Decimal` .  
  
## <a name="floating-point-arithmetic"></a>Aritmética de ponto flutuante  
 Visual Basic executa a maioria das aritméticas de ponto flutuante em [Double](../data-types/double-data-type.md), que é o tipo de dados mais eficiente para essas operações. No entanto, se um operando for [único](../data-types/single-data-type.md) e o outro não for `Double` , Visual Basic executará a operação no `Single` . Ele amplia cada operando conforme necessário para o tipo de dados apropriado antes da operação, e o resultado tem esse tipo de dados.  
  
### <a name="-and--operators"></a>Operadores de ^/e  
 O `/` operador é definido somente para os tipos de dados [decimal](../data-types/decimal-data-type.md), [único](../data-types/single-data-type.md)e [duplo](../data-types/double-data-type.md) . Visual Basic amplia cada operando conforme necessário para o tipo de dados apropriado antes da operação, e o resultado tem esse tipo de dados.  
  
 A tabela a seguir mostra os tipos de dados de resultado para o `/` operador. Observe que essa tabela é simétrica; para uma determinada combinação de tipos de dados de operando, o tipo de dados de resultado é o mesmo, independentemente da ordem dos operandos.  
  
||||||  
|---|---|---|---|---|  
||`Decimal`|`Single`|`Double`|Qualquer tipo de inteiro|  
|`Decimal`|Decimal|Single|Double|Decimal|  
|`Single`|Single|Single|Double|Single|  
|`Double`|Double|Double|Double|Double|  
|Qualquer tipo de inteiro|Decimal|Single|Double|Double|  
  
 O `^` operador é definido somente para o `Double` tipo de dados. Visual Basic amplia cada operando conforme necessário `Double` antes da operação, e o tipo de dados de resultado é sempre `Double` .  
  
## <a name="integer-arithmetic"></a>Aritmética de inteiro  
 O tipo de dados de resultado de uma operação de inteiro depende dos tipos de dados dos operandos. Em geral, Visual Basic usa as seguintes políticas para determinar o tipo de dados de resultado:  
  
- Se ambos os operandos de um operador binário tiverem o mesmo tipo de dados, o resultado terá esse tipo de dados. Uma exceção é `Boolean` , que é forçada para `Short` .  
  
- Se um operando não assinado participar de um operando assinado, o resultado terá um tipo assinado com pelo menos um intervalo grande como um dos operandos.  
  
- Caso contrário, o resultado geralmente tem o maior dos dois tipos de dados de operando.  
  
 Observe que o tipo de dados de resultado pode não ser o mesmo que o tipo de dados operando.  
  
> [!NOTE]
> O tipo de dados de resultado nem sempre é grande o suficiente para conter todos os valores possíveis resultantes da operação. Uma <xref:System.OverflowException> exceção pode ocorrer se o valor for muito grande para o tipo de dados de resultado.  
  
### <a name="unary--and--operators"></a>Operadores unários + e –  
 A tabela a seguir mostra os tipos de dados de resultado para os dois operadores unários `+` e `–` .  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|Unário`+`|Short|SByte|Byte|Short|UShort|Integer|UInteger|long|ULong|  
|Unário`–`|Short|SByte|Short|Short|Integer|Integer|long|long|Decimal|  
  
### <a name="-and--operators"></a><\< and >Operadores de>  
 A tabela a seguir mostra os tipos de dados de resultado para os dois operadores de deslocamento de bits `<<` e `>>` . Visual Basic trata cada operador bit-Shift como um operador unário em seu operando esquerdo (o padrão de bit a ser deslocado).  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`<<`, `>>`|Short|SByte|Byte|Short|UShort|Integer|UInteger|long|ULong|  
  
 Se o operando esquerdo for `Decimal` , `Single` , `Double` ou `String` , Visual Basic tentar convertê-lo `Long` antes da operação e o tipo de dados de resultado for `Long` . O operando à direita (o número de posições de bit a ser deslocado) deve ser `Integer` ou um tipo que amplia para `Integer` .  
  
### <a name="binary----and-mod-operators"></a>Operadores +, –, \* e mod binários  
 A tabela a seguir mostra os tipos de dados de resultado para o binário `+` e `–` os operadores e os `*` `Mod` operadores e. Observe que essa tabela é simétrica; para uma determinada combinação de tipos de dados de operando, o tipo de dados de resultado é o mesmo, independentemente da ordem dos operandos.  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|Short|SByte|Short|Short|Integer|Integer|long|long|Decimal|  
|`SByte`|SByte|SByte|Short|Short|Integer|Integer|long|long|Decimal|  
|`Byte`|Short|Short|Byte|Short|UShort|Integer|UInteger|long|ULong|  
|`Short`|Short|Short|Short|Short|Integer|Integer|long|long|Decimal|  
|`UShort`|Integer|Integer|UShort|Integer|UShort|Integer|UInteger|long|ULong|  
|`Integer`|Integer|Integer|Integer|Integer|Integer|Integer|long|long|Decimal|  
|`UInteger`|long|long|UInteger|long|UInteger|long|UInteger|long|ULong|  
|`Long`|long|long|long|long|long|long|long|long|Decimal|  
|`ULong`|Decimal|Decimal|ULong|Decimal|ULong|Decimal|ULong|Decimal|ULong|  
  
### <a name="-operator"></a>Operador \\  
 A tabela a seguir mostra os tipos de dados de resultado para o `\` operador. Observe que essa tabela é simétrica; para uma determinada combinação de tipos de dados de operando, o tipo de dados de resultado é o mesmo, independentemente da ordem dos operandos.  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|Short|SByte|Short|Short|Integer|Integer|long|long|long|  
|`SByte`|SByte|SByte|Short|Short|Integer|Integer|long|long|long|  
|`Byte`|Short|Short|Byte|Short|UShort|Integer|UInteger|long|ULong|  
|`Short`|Short|Short|Short|Short|Integer|Integer|long|long|long|  
|`UShort`|Integer|Integer|UShort|Integer|UShort|Integer|UInteger|long|ULong|  
|`Integer`|Integer|Integer|Integer|Integer|Integer|Integer|long|long|long|  
|`UInteger`|long|long|UInteger|long|UInteger|long|UInteger|long|ULong|  
|`Long`|long|long|long|long|long|long|long|long|long|  
|`ULong`|long|long|ULong|long|ULong|long|ULong|long|ULong|  
  
 Se qualquer operando do `\` operador for [decimal](../data-types/decimal-data-type.md), [Single](../data-types/single-data-type.md)ou [Double](../data-types/double-data-type.md), Visual Basic tentar convertê-lo para [Long](../data-types/long-data-type.md) antes da operação e o tipo de dados de resultado for `Long` .  
  
## <a name="relational-and-bitwise-comparisons"></a>Comparações relacionais e de bits  
 O tipo de dados de resultado de uma operação relacional ( `=` ,,,, `<>` `<` `>` `<=` , `>=` ) é sempre `Boolean` [tipo de dados booliano](../data-types/boolean-data-type.md). O mesmo é verdadeiro para operações lógicas (,,,, `And` `AndAlso` `Not` `Or` `OrElse` , `Xor` ) em `Boolean` operandos.  
  
 O tipo de dados de resultado de uma operação lógica bit a bit depende dos tipos de dados dos operandos. Observe que `AndAlso` e `OrElse` são definidos somente para `Boolean` , e Visual Basic converte cada operando conforme necessário `Boolean` antes de executar a operação.  
  
### <a name="-----and--operators"></a>=,  <>, \<, > , \<=, and > = operadores  
 Se ambos os operandos forem `Boolean` , Visual Basic considerará `True` ser menor que `False` . Se um tipo numérico for comparado com um `String` , Visual Basic tentará converter o `String` para `Double` antes da operação. Um `Char` `Date` operando or só pode ser comparado com outro operando do mesmo tipo de dados. O tipo de dados de resultado é sempre `Boolean` .  
  
### <a name="bitwise-not-operator"></a>Operador NOT bit a bit  
 A tabela a seguir mostra os tipos de dados de resultado para o operador bit a bit `Not` .  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Not`|Boolean|SByte|Byte|Short|UShort|Integer|UInteger|long|ULong|  
  
 Se o operando for `Decimal` , `Single` , `Double` ou `String` , Visual Basic tentar convertê-lo `Long` antes da operação e o tipo de dados de resultado for `Long` .  
  
### <a name="bitwise-and-or-and-xor-operators"></a>Operadores and, or e XOR  
 A tabela a seguir mostra os tipos de dados de resultado para os operadores bit a bit `And` , `Or` e `Xor` . Observe que essa tabela é simétrica; para uma determinada combinação de tipos de dados de operando, o tipo de dados de resultado é o mesmo, independentemente da ordem dos operandos.  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|Boolean|SByte|Short|Short|Integer|Integer|long|long|long|  
|`SByte`|SByte|SByte|Short|Short|Integer|Integer|long|long|long|  
|`Byte`|Short|Short|Byte|Short|UShort|Integer|UInteger|long|ULong|  
|`Short`|Short|Short|Short|Short|Integer|Integer|long|long|long|  
|`UShort`|Integer|Integer|UShort|Integer|UShort|Integer|UInteger|long|ULong|  
|`Integer`|Integer|Integer|Integer|Integer|Integer|Integer|long|long|long|  
|`UInteger`|long|long|UInteger|long|UInteger|long|UInteger|long|ULong|  
|`Long`|long|long|long|long|long|long|long|long|long|  
|`ULong`|long|long|ULong|long|ULong|long|ULong|long|ULong|  
  
 Se um operando for `Decimal` , `Single` , `Double` ou `String` , Visual Basic tentar convertê-lo `Long` antes da operação, e o tipo de dados de resultado será o mesmo que se esse operando já tivesse sido `Long` .  
  
## <a name="miscellaneous-operators"></a>Operadores diversos  
 O `&` operador é definido somente para concatenação de `String` operandos. Visual Basic converte cada operando conforme necessário `String` antes da operação, e o tipo de dados de resultado é sempre `String` . Para os fins do `&` operador, todas as conversões para `String` são consideradas como sendo ampliadas, mesmo se `Option Strict` for `On` .  
  
 Os `Is` `IsNot` operadores e exigem que ambos os operandos sejam de um tipo de referência. A `TypeOf` expressão... `Is` requer que o primeiro operando seja de um tipo de referência e o segundo operando como o nome de um tipo de dados. Em todos esses casos, o tipo de dados de resultado é `Boolean` .  
  
 O `Like` operador é definido somente para correspondência de padrões de `String` operandos. Visual Basic tenta converter cada operando conforme necessário `String` antes da operação. O tipo de dados de resultado é sempre `Boolean` .  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados](../data-types/index.md)
- [Operadores e expressões](../../programming-guide/language-features/operators-and-expressions/index.md)
- [Operadores aritméticos no Visual Basic](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [Operadores de comparação no Visual Basic](../../programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Operadores](index.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Operadores de comparação](comparison-operators.md)
- [Instrução Option Strict](../statements/option-strict-statement.md)
