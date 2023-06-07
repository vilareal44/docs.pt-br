---
title: Sobrecargas de operador
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- operators [.NET Framework], overloads
- names [.NET Framework], overloaded operators
- member design guidelines, operators
- overloaded operators
ms.assetid: 37585bf2-4c27-4dee-849a-af70e3338cc1
ms.openlocfilehash: 893b7d1f76dfb059a0ddca77dfd8654812e9ae12
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289727"
---
# <a name="operator-overloads"></a>Sobrecargas de operador
As sobrecargas de operador permitem que os tipos de estrutura apareçam como se fossem primitivos de linguagem internos.

 Embora seja permitido e útil em algumas situações, as sobrecargas de operador devem ser usadas com cuidado. Há muitos casos em que o sobrecarga de operador foi feito com abuso, como quando designers de estrutura começaram a usar operadores para operações que devem ser métodos simples. As diretrizes a seguir devem ajudá-lo a decidir quando e como usar a sobrecarga de operador.

 ❌Evite definir sobrecargas de operador, exceto em tipos que devem se sentir como tipos primitivos (internos).

 ✔️ Considere definir sobrecargas de operador em um tipo que deve parecer um tipo primitivo.

 Por exemplo, <xref:System.String?displayProperty=nameWithType> tem `operator==` e `operator!=` definido.

 ✔️ definem sobrecargas de operador em structs que representam números (como <xref:System.Decimal?displayProperty=nameWithType> ).

 ❌Não seja graciosos ao definir sobrecargas de operador.

 A sobrecarga de operador é útil em casos nos quais é imediatamente óbvio qual será o resultado da operação. Por exemplo, faz sentido ser capaz de subtrair um <xref:System.DateTime> de outro `DateTime` e obter um <xref:System.TimeSpan> . No entanto, não é apropriado usar o operador lógico Union para unir duas consultas de banco de dados ou usar o operador SHIFT para gravar em um fluxo.

 ❌Não forneça sobrecargas de operador, a menos que pelo menos um dos operandos seja do tipo que define a sobrecarga.

 ✔️ os operadores de sobrecarga de maneira simétrica.

 Por exemplo, se você sobrecarregar o `operator==` , também deverá sobrecarregar o `operator!=` . Da mesma forma, se você sobrecarregar o `operator<` , também deverá sobrecarregar o `operator>` e assim por diante.

 ✔️ Considere fornecer métodos com nomes amigáveis que correspondam a cada operador sobrecarregado.

 Muitas linguagens não dão suporte à sobrecarga de operador. Por esse motivo, é recomendável que os tipos que sobrecarregam operadores incluam um método secundário com um nome específico de domínio apropriado que forneça funcionalidade equivalente.

 A tabela a seguir contém uma lista de operadores e os nomes de métodos amigáveis correspondentes.

|Símbolo de operador C#|Nome de metadados|Nome amigável|
|-------------------------|-------------------|-------------------|
|`N/A`|`op_Implicit`|`To<TypeName>/From<TypeName>`|
|`N/A`|`op_Explicit`|`To<TypeName>/From<TypeName>`|
|`+ (binary)`|`op_Addition`|`Add`|
|`- (binary)`|`op_Subtraction`|`Subtract`|
|`* (binary)`|`op_Multiply`|`Multiply`|
|`/`|`op_Division`|`Divide`|
|`%`|`op_Modulus`|`Mod or Remainder`|
|`^`|`op_ExclusiveOr`|`Xor`|
|`& (binary)`|`op_BitwiseAnd`|`BitwiseAnd`|
|<code>&#124;</code>|`op_BitwiseOr`|`BitwiseOr`|
|`&&`|`op_LogicalAnd`|`And`|
|<code>&#124;&#124;</code>|`op_LogicalOr`|`Or`|
|`=`|`op_Assign`|`Assign`|
|`<<`|`op_LeftShift`|`LeftShift`|
|`>>`|`op_RightShift`|`RightShift`|
|`N/A`|`op_SignedRightShift`|`SignedRightShift`|
|`N/A`|`op_UnsignedRightShift`|`UnsignedRightShift`|
|`==`|`op_Equality`|`Equals`|
|`!=`|`op_Inequality`|`Equals`|
|`>`|`op_GreaterThan`|`CompareTo`|
|`<`|`op_LessThan`|`CompareTo`|
|`>=`|`op_GreaterThanOrEqual`|`CompareTo`|
|`<=`|`op_LessThanOrEqual`|`CompareTo`|
|`*=`|`op_MultiplicationAssignment`|`Multiply`|
|`-=`|`op_SubtractionAssignment`|`Subtract`|
|`^=`|`op_ExclusiveOrAssignment`|`Xor`|
|`<<=`|`op_LeftShiftAssignment`|`LeftShift`|
|`%=`|`op_ModulusAssignment`|`Mod`|
|`+=`|`op_AdditionAssignment`|`Add`|
|`&=`|`op_BitwiseAndAssignment`|`BitwiseAnd`|
|<code>&#124;=</code>|`op_BitwiseOrAssignment`|`BitwiseOr`|
|`,`|`op_Comma`|`Comma`|
|`/=`|`op_DivisionAssignment`|`Divide`|
|`--`|`op_Decrement`|`Decrement`|
|`++`|`op_Increment`|`Increment`|
|`- (unary)`|`op_UnaryNegation`|`Negate`|
|`+ (unary)`|`op_UnaryPlus`|`Plus`|
|`~`|`op_OnesComplement`|`OnesComplement`|

### <a name="overloading-operator-"></a>Operador de sobrecarga = =
 A sobrecarga `operator ==` é bastante complicada. A semântica do operador precisa ser compatível com vários outros membros, como <xref:System.Object.Equals%2A?displayProperty=nameWithType> .

### <a name="conversion-operators"></a>Operadores de conversão
 Os operadores de conversão são operadores unários que permitem a conversão de um tipo para outro. Os operadores devem ser definidos como membros estáticos no operando ou no tipo de retorno. Há dois tipos de operadores de conversão: implícito e explícito.

 ❌Não forneça um operador de conversão se tal conversão não for claramente esperada pelos usuários finais.

 ❌Não defina operadores de conversão fora do domínio de um tipo.

 Por exemplo, <xref:System.Int32> , <xref:System.Double> e <xref:System.Decimal> são todos os tipos numéricos, enquanto <xref:System.DateTime> não é. Portanto, não deve haver nenhum operador de conversão para converter um `Double(long)` para um `DateTime` . Um construtor é preferencial nesse caso.

 ❌Não forneça um operador de conversão implícita se a conversão tiver potencialmente perda.

 Por exemplo, não deve haver uma conversão implícita de `Double` para `Int32` porque `Double` o tem um intervalo maior do que `Int32` . Um operador de conversão explícita pode ser fornecido mesmo que a conversão tenha potencialmente perda.

 ❌Não lance exceções de conversões implícitas.

 É muito difícil para os usuários finais entenderem o que está acontecendo, pois eles podem não estar cientes de que uma conversão está ocorrendo.

 ✔️ gerar <xref:System.InvalidCastException?displayProperty=nameWithType> se uma chamada para um operador cast resultar em uma conversão com perdas e o contrato do operador não permitir conversões com perdas.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de membro](member.md)
- [Diretrizes de design de estrutura](index.md)
