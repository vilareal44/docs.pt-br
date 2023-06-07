---
title: Tipos de dados numéricos
ms.date: 07/20/2015
helpviewer_keywords:
- integral types [Visual Basic], Visual Basic
- Short data type [Visual Basic], numeric data types
- Double data type [Visual Basic], numeric data types
- Long data type [Visual Basic], Visual Basic numeric data types
- numbers [Visual Basic], whole
- fractions
- numbers
- whole numbers
- integer numbers
- numbers [Visual Basic], integer
- fractional data types [Visual Basic]
- mantissas, of fractional numbers
- mantissas
- data types [Visual Basic], numeric
- Integer data type [Visual Basic], numeric data types
- exponent, of fractional numbers
- integers [Visual Basic]
- numeric data types [Visual Basic], Visual Basic
- Single data type [Visual Basic], numeric types
- Decimal data type [Visual Basic], numeric data types
ms.assetid: a27bd4d0-7e14-43eb-9cc4-b42eaab323c9
ms.openlocfilehash: 72cdca295e209935687f67ad290e816775d9fe13
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393670"
---
# <a name="numeric-data-types-visual-basic"></a>Tipos de dados numéricos (Visual Basic)
O Visual Basic fornece vários *tipos de dados numéricos* para lidar com números em várias representações. Tipos *integrais* representam apenas números inteiros (positivos, negativos e zero) e tipos não *integrais* representam números com partes inteiras e fracionárias.  
  
 Para uma tabela que mostra uma comparação lado a lado do Visual Basic tipos de dados, consulte [tipos de dados](../../../language-reference/data-types/index.md).  
  
## <a name="integral-numeric-types"></a>Tipos numéricos inteiros  
 Os *tipos de dados integral* são aqueles que representam apenas números sem partes fracionárias.  
  
 Os tipos de dados integrais *assinados* são [tipo de dados SByte](../../../language-reference/data-types/sbyte-data-type.md) (8 bits), tipo de dados [curto](../../../language-reference/data-types/short-data-type.md) (16 bits), [tipo de dados Integer](../../../language-reference/data-types/integer-data-type.md) (32 bits) e [tipo de dados Long](../../../language-reference/data-types/long-data-type.md) (64 bits). Se uma variável sempre armazenar inteiros em vez de números fracionários, declare-o como um desses tipos.  
  
 Os tipos integral *não assinados* são [tipo de dados de byte](../../../language-reference/data-types/byte-data-type.md) (8 bits), [tipo de dados UShort](../../../language-reference/data-types/ushort-data-type.md) (16 bits), [tipo de dados UInteger](../../../language-reference/data-types/uinteger-data-type.md) (32 bits) e [tipo de dados ULong](../../../language-reference/data-types/ulong-data-type.md) (64 bits). Se uma variável contiver dados binários, ou dados de natureza desconhecida, declare-a como um desses tipos.  
  
### <a name="performance"></a>Desempenho  
 As operações aritméticas são mais rápidas com tipos integrais do que com outros tipos de dados. Eles são mais rápidos com os `Integer` `UInteger` tipos e em Visual Basic.  
  
### <a name="large-integers"></a>Inteiros grandes  
 Se você precisar manter um inteiro maior do que o `Integer` tipo de dados pode conter, poderá usar o `Long` tipo de dados em vez disso. `Long`as variáveis podem conter números de-9.223.372.036.854.775.808 até 9.223.372.036.854.775.807. As operações com `Long` são um pouco mais lentas do que com `Integer` .  
  
 Se você precisar de valores ainda maiores, poderá usar o [tipo de dados decimal](../../../language-reference/data-types/decimal-data-type.md). Você pode armazenar números de-79228162514264337593543950335 por meio de 79228162514264337593543950335 em uma `Decimal` variável se não usar nenhuma casa decimal. No entanto, as operações com `Decimal` números são consideravelmente mais lentas do que com qualquer outro tipo de dados numérico.  
  
### <a name="small-integers"></a>Inteiros pequenos  
 Se você não precisar do intervalo completo do tipo de `Integer` dados, poderá usar o tipo de `Short` dados, que pode conter números inteiros de-32.768 a 32.767. Para o menor intervalo inteiro, o `SByte` tipo de dados contém números inteiros de-128 a 127. Se você tiver um número muito grande de variáveis que contenham inteiros pequenos, a Common Language Runtime pode, às vezes, armazenar suas `Short` variáveis e com `SByte` mais eficiência e economizar o consumo de memória. No entanto, operações com `Short` e `SByte` são um pouco mais lentas do que com `Integer` .  
  
### <a name="unsigned-integers"></a>Inteiros não assinados  
 Se você sabe que a variável nunca precisa conter um número negativo, você pode usar os *tipos não assinados* `Byte` , `UShort` , `UInteger` e `ULong` . Cada um desses tipos de dados pode conter um inteiro positivo duas vezes maior que o tipo assinado correspondente ( `SByte` ,, `Short` `Integer` e `Long` ). Em termos de desempenho, cada tipo não assinado é exatamente tão eficiente quanto seu tipo assinado correspondente. Em particular, `UInteger` o compartilha com `Integer` a distinção de ser a mais eficiente de todos os tipos de dados numéricos elementares.  
  
## <a name="nonintegral-numeric-types"></a>Tipos numéricos não integrais  
 Os *tipos de dados não integral* são aqueles que representam números com partes inteiras e fracionárias.  
  
 Os tipos de dados numéricos não integral são `Decimal` (ponto fixo de 128 bits), [tipo de dados único](../../../language-reference/data-types/single-data-type.md) (ponto flutuante de 32 bits) e [tipo de dados duplo](../../../language-reference/data-types/double-data-type.md) (ponto flutuante de 64 bits). Eles são todos os tipos assinados. Se uma variável puder conter uma fração, declare-a como um desses tipos.  
  
 `Decimal`Não é um tipo de dados de ponto flutuante. `Decimal`os números têm um valor inteiro binário e um fator de dimensionamento inteiro que especifica qual parte do valor é uma fração decimal.  
  
 Você pode usar `Decimal` variáveis para valores monetários. A vantagem é a precisão dos valores. O `Double` tipo de dados é mais rápido e requer menos memória, mas está sujeito a erros de arredondamento. O `Decimal` tipo de dados retém a precisão completa para 28 casas decimais.  
  
 Os números de ponto flutuante ( `Single` e `Double` ) têm intervalos maiores do que `Decimal` números, mas podem estar sujeitos a erros de arredondamento. Os tipos de ponto flutuante dão suporte a menos dígitos significativos do que `Decimal` , mas podem representar valores de magnitude maior.  
  
 Valores de número não integral podem ser expressos como mmmEeee, em que Mmm é o *mantissa* (os dígitos significativos) e EEE é o *expoente* (uma potência de 10). Os valores positivos mais altos dos tipos não integrais são 7.9228162514264337593543950335 E + 28 para `Decimal` , 3.4028235 e + 38 para `Single` E 1.79769313486231570 e + 308 para `Double` .  
  
### <a name="performance"></a>Desempenho  
 `Double`é o mais eficiente dos tipos de dados fracionários, porque os processadores em plataformas atuais executam operações de ponto flutuante em precisão dupla. No entanto, as operações com `Double` não são tão rápidas quanto com os tipos integrais, como `Integer` .  
  
### <a name="small-magnitudes"></a>Pequenas magnitudes  
 Para números com a menor magnitude possível (mais próxima de 0), as `Double` variáveis podem conter números tão pequenos quanto-4.94065645841246544 e-324 para valores negativos e 4.94065645841246544 e-324 para valores positivos.  
  
### <a name="small-fractional-numbers"></a>Números fracionários pequenos  
 Se você não precisar do intervalo completo do tipo de `Double` dados, poderá usar o tipo de `Single` dados, que pode conter números de ponto flutuante de-3.4028235 e + 38 a 3.4028235 e + 38. As menores magnitudes das `Single` variáveis são-1.401298 e-45 para valores negativos e 1.401298 e-45 para valores positivos. Se você tiver um número muito grande de variáveis que contenham números pequenos de ponto flutuante, a Common Language Runtime poderá, às vezes, armazenar suas `Single` variáveis com mais eficiência e economizar o consumo de memória.  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados elementares](elementary-data-types.md)
- [Tipos de dados de caractere](character-data-types.md)
- [Tipos de dados diversos](miscellaneous-data-types.md)
- [Solução de problemas de tipos de dados](troubleshooting-data-types.md)
- [Como: Chamar uma função do Windows que use tipos não assinados](../../com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
