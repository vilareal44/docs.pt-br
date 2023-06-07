---
title: Conversões de Widening e Narrowing
ms.date: 07/20/2015
helpviewer_keywords:
- widening conversions [Visual Basic]
- narrowing conversions [Visual Basic]
- conversions [Visual Basic], type
- data types [Visual Basic], changing
- variables [Visual Basic], changing data type
- conversions [Visual Basic], exceptions during conversion
- type conversion [Visual Basic], exceptions during conversion
- conversions [Visual Basic], data type
- conversions [Visual Basic], narrowing
- type conversion [Visual Basic], narrowing
- data type conversion [Visual Basic], widening
- data type conversion [Visual Basic], narrowing
- changing data types [Visual Basic]
- type conversion [Visual Basic], widening
- data type conversion [Visual Basic], exceptions during conversion
- conversions [Visual Basic], widening
ms.assetid: 058c3152-6c28-4268-af44-2209e774f0bd
ms.openlocfilehash: 177ff6c6fe15c57563d2f62ed8927f9ee975be48
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392994"
---
# <a name="widening-and-narrowing-conversions-visual-basic"></a>Conversões de Widening e Narrowing (Visual Basic)
Uma consideração importante com uma conversão de tipo é se o resultado da conversão está dentro do intervalo do tipo de dados de destino.  
  
 Uma *conversão de ampliação* altera um valor para um tipo de dados que pode permitir qualquer valor possível dos dados originais.  As conversões de alargamento preservam o valor de origem, mas podem alterar sua representação. Isso ocorre se você converter de um tipo integral para `Decimal` , ou de `Char` para `String` .  
  
 Uma *conversão de restrição* altera um valor para um tipo de dados que pode não ser capaz de manter alguns dos valores possíveis. Por exemplo, um valor fracionário é arredondado quando é convertido em um tipo integral, e um tipo numérico sendo convertido em `Boolean` é reduzido para `True` ou `False` .  
  
## <a name="widening-conversions"></a>Conversões de expansão  
 A tabela a seguir mostra as conversões de alargamento padrão.  
  
|Tipo de dados|Amplia os tipos de dados <sup>1</sup>|  
|---|---|  
|[SByte](../../../language-reference/data-types/sbyte-data-type.md)|`SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, `Double`|  
|[Minuciosa](../../../language-reference/data-types/byte-data-type.md)|`Byte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`|  
|[Baixo](../../../language-reference/data-types/short-data-type.md)|`Short`, `Integer`, `Long`, `Decimal`, `Single`, `Double`|  
|[Num](../../../language-reference/data-types/ushort-data-type.md)|`UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`|  
|[Integer](../../../language-reference/data-types/integer-data-type.md)|`Integer`, `Long` , `Decimal` , `Single` , `Double` <sup>2</sup>|  
|[UInteger](../../../language-reference/data-types/uinteger-data-type.md)|`UInteger`, `Long` , `ULong` , `Decimal` , `Single` , `Double` <sup>2</sup>|  
|[Long](../../../language-reference/data-types/long-data-type.md)|`Long`, `Decimal` , `Single` , `Double` <sup>2</sup>|  
|[ULong](../../../language-reference/data-types/ulong-data-type.md)|`ULong`, `Decimal` , `Single` , `Double` <sup>2</sup>|  
|[Decimal](../../../language-reference/data-types/decimal-data-type.md)|`Decimal`, `Single` , `Double` <sup>2</sup>|  
|[Exclusivo](../../../language-reference/data-types/single-data-type.md)|`Single`, `Double`|  
|[Clique](../../../language-reference/data-types/double-data-type.md)|`Double`|  
|Qualquer tipo enumerado ([enum](../../../language-reference/statements/enum-statement.md))|Seu tipo integral subjacente e qualquer tipo ao qual o tipo subjacente amplia.|  
|[º](../../../language-reference/data-types/char-data-type.md)|`Char`, `String`|  
|Matriz `Char`|`Char`variedade`String`|  
|Qualquer tipo|[Objeto](../../../language-reference/data-types/object-data-type.md)|  
|Qualquer tipo derivado|Qualquer tipo base do qual é derivado <sup>3</sup>.|  
|Qualquer tipo|Qualquer interface que ele implementa.|  
|[Nada](../../../language-reference/nothing.md)|Qualquer tipo de dados ou tipo de objeto.|  
  
 <sup>1</sup> por definição, cada tipo de dados amplia-se para si mesmo.  
  
 <sup>2</sup> conversões de `Integer` , `UInteger` , `Long` , `ULong` ou `Decimal` para `Single` ou `Double` podem resultar em perda de precisão, mas nunca em perda de magnitude. Nesse sentido, eles não incorrem em perda de informações.  
  
 <sup>3</sup> pode parecer surpreendente que uma conversão de um tipo derivado para um de seus tipos base está ampliando. A justificativa é que o tipo derivado contém todos os membros do tipo base, portanto, ele é qualificado como uma instância do tipo base. Na direção oposta, o tipo base não contém novos membros definidos pelo tipo derivado.  
  
 Conversões de alargamento sempre são bem sucedidos em tempo de execução e nunca incorrem em perda de dados. Você sempre pode executá-las implicitamente, se a [instrução Option Strict](../../../language-reference/statements/option-strict-statement.md) define a opção de verificação de tipo para `On` ou para `Off` .  
  
## <a name="narrowing-conversions"></a>Conversões de redução  
 As conversões redutoras padrão incluem o seguinte:  
  
- As direções inversas das conversões de ampliação na tabela anterior (exceto que cada tipo se amplia para si mesmo)  
  
- Conversões em qualquer direção entre [booliano](../../../language-reference/data-types/boolean-data-type.md) e qualquer tipo numérico  
  
- Conversões de qualquer tipo numérico para qualquer tipo enumerado ( `Enum` )  
  
- Conversões em qualquer direção entre a [cadeia de caracteres](../../../language-reference/data-types/string-data-type.md) e qualquer tipo numérico, `Boolean` ou [Data](../../../language-reference/data-types/date-data-type.md)  
  
- Conversões de um tipo de dados ou tipo de objeto para um tipo derivado dele  
  
 As conversões redutoras nem sempre são bem sucedidas em tempo de execução e podem falhar ou incorrer em perda de dados. Ocorrerá um erro se o tipo de dados de destino não puder receber o valor que está sendo convertido. Por exemplo, uma conversão numérica pode resultar em um estouro. O compilador não permite que você execute conversões estreitas implicitamente, a menos que a [instrução Option Strict](../../../language-reference/statements/option-strict-statement.md) defina a opção de verificação de tipo como `Off` .  
  
> [!NOTE]
> O erro de conversão de restrição é suprimido para conversões dos elementos em uma `For Each…Next` coleção para a variável de controle de loop. Para obter mais informações e exemplos, consulte a seção "conversões redutoras" em [para cada... Próxima instrução](../../../language-reference/statements/for-each-next-statement.md).  
  
### <a name="when-to-use-narrowing-conversions"></a>Quando usar conversões redutoras  
 Você usa uma conversão de restrição quando sabe que o valor de origem pode ser convertido para o tipo de dados de destino sem perda de dados ou erro. Por exemplo, se você tiver um `String` que você sabe que contém "true" ou "false", poderá usar a `CBool` palavra-chave para convertê-lo em `Boolean` .  
  
## <a name="exceptions-during-conversion"></a>Exceções durante a conversão  
 Como as conversões de alargamento sempre têm sucesso, elas não geram exceções. Restringir conversões, quando elas falham, geralmente lançam as seguintes exceções:  
  
- <xref:System.InvalidCastException>— Se nenhuma conversão for definida entre os dois tipos  
  
- <xref:System.OverflowException>— (somente tipos integrais) se o valor convertido for muito grande para o tipo de destino  
  
 Se uma classe ou estrutura definir uma [função CType](../../../language-reference/functions/ctype-function.md) para servir como um operador de conversão para ou dessa classe ou estrutura, isso `CType` poderá gerar qualquer exceção que considerar apropriada. Além disso, isso `CType` pode chamar funções de Visual Basic ou .NET Framework métodos que, por sua vez, podem lançar uma variedade de exceções.  
  
## <a name="changes-during-reference-type-conversions"></a>Alterações durante conversões de tipo de referência  
 Uma conversão de um *tipo de referência* copia apenas o ponteiro para o valor. O valor em si não é copiado nem alterado de nenhuma forma. A única coisa que pode ser alterada é o tipo de dados da variável que contém o ponteiro. No exemplo a seguir, o tipo de dados é convertido da classe derivada para sua classe base, mas o objeto que as duas variáveis apontam agora é inalterado.  
  
```vb  
' Assume class cSquare inherits from class cShape.  
Dim shape As cShape  
Dim square As cSquare = New cSquare  
' The following statement performs a widening  
' conversion from a derived class to its base class.  
shape = square  
```  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados](index.md)
- [Conversões de tipo no Visual Basic](type-conversions.md)
- [Conversões implícitas e explícitas](implicit-and-explicit-conversions.md)
- [Conversões entre cadeias de caracteres e outros tipos](conversions-between-strings-and-other-types.md)
- [Como converter um objeto em outro tipo no Visual Basic](how-to-convert-an-object-to-another-type.md)
- [Conversões de matriz](array-conversions.md)
- [Tipos de dados](../../../language-reference/data-types/index.md)
- [Funções de conversão do tipo](../../../language-reference/functions/type-conversion-functions.md)
