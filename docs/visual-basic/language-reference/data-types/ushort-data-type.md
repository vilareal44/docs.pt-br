---
title: Tipo de Dados UShort
ms.date: 01/31/2018
f1_keywords:
- vb.ushort
helpviewer_keywords:
- numbers [Visual Basic], whole
- literal type characters [Visual Basic], US
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- UShort data type
- US literal type characters [Visual Basic]
ms.assetid: 138db892-665d-4ba8-9cae-d8d91c4a8f39
ms.openlocfilehash: ee31156e00059699125fd72a7f091afbb21beab0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415473"
---
# <a name="ushort-data-type-visual-basic"></a>Tipo de dados UShort (Visual Basic)

Mantém inteiros de 16 bits (2 bytes) não assinados, variando no valor de 0 a 65.535.  
  
## <a name="remarks"></a>Comentários

 Use o `UShort` tipo de dados para conter dados binários muito grandes para o `Byte` .  
  
 O valor padrão de `UShort` é 0.  

## <a name="literal-assignments"></a>Atribuições de literal

Você pode declarar e inicializar uma `UShort` variável atribuindo-lhe um literal decimal, um literal hexadecimal, um literal octal ou (começando com Visual Basic 2017) um literal binário. Se o literal inteiro estiver fora do intervalo de `UShort` (ou seja, se for menor que <xref:System.UInt16.MinValue?displayProperty=nameWithType> ou maior que <xref:System.UInt16.MaxValue?displayProperty=nameWithType>, ocorrerá um erro de compilação.

No exemplo a seguir, números inteiros iguais a 65.034 que são representados como decimais, hexadecimais e literais binários são atribuídos a `UShort` valores.
  
[!code-vb[UShort](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UShort)]

> [!NOTE]
> Você usa o prefixo `&h` ou `&H` para denotar um literal hexadecimal, o prefixo `&b` ou `&B` para indicar um literal binário, e o prefixo `&o` ou `&O` para indicar um literal octal. Literais decimais não têm nenhum prefixo.

A partir do Visual Basic 2017, você também pode usar o caractere de sublinhado, `_` , como um separador de dígitos para melhorar a legibilidade, como mostra o exemplo a seguir.

[!code-vb[UShort](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UShortS)]

A partir do Visual Basic 15,5, você também pode usar o caractere de sublinhado ( `_` ) como um separador à esquerda entre o prefixo e os dígitos hexadecimal, binário ou octal. Por exemplo:

```vb
Dim number As UShort = &H_FF8C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

Os literais numéricos também podem incluir o `US` `us` [caractere de tipo](../../programming-guide/language-features/data-types/type-characters.md) ou para denotar o `UShort` tipo de dados, como mostra o exemplo a seguir.

```vb
Dim number = &H_5826us
```

## <a name="programming-tips"></a>Dicas de programação
  
- **Números negativos.** Como `UShort` é um tipo não assinado, ele não pode representar um número negativo. Se você usar o operador menos ( `-` ) unário em uma expressão avaliada como tipo `UShort` , Visual Basic converterá a expressão em `Integer` primeiro.  
  
- **Conformidade com CLS.** O `UShort` tipo de dados não faz parte do [Common Language Specification](https://www.ecma-international.org/publications/standards/Ecma-335.htm) (CLS), portanto o código em conformidade com CLS não pode consumir um componente que o utiliza.
  
- **Ampliação.** O `UShort` tipo de dados amplia para `Integer` ,,,,, `UInteger` `Long` `ULong` `Decimal` `Single` e `Double` . Isso significa que você pode converter `UShort` para qualquer um desses tipos sem encontrar um <xref:System.OverflowException?displayProperty=nameWithType> erro.  
  
- **Digite os caracteres.** Acrescentar os caracteres do tipo literal `US` a um literal força-o ao `UShort` tipo de dados. `UShort`Não tem um caractere de tipo de identificador.  
  
- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.UInt16?displayProperty=nameWithType>.  
  
## <a name="see-also"></a>Confira também

- <xref:System.UInt16>
- [Tipos de dados](index.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Como: Chamar uma função do Windows que use tipos não assinados](../../programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
