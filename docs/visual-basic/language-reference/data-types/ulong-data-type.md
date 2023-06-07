---
title: Tipo de Dados ULong
ms.date: 01/31/2018
f1_keywords:
- vb.ulong
helpviewer_keywords:
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- literal type characters [Visual Basic], UL
- ULong data type
- UL literal type characters [Visual Basic]
ms.assetid: 017e0702-774e-44ae-bedc-786b424ca84e
ms.openlocfilehash: ee9297ae917345d44d8e630bd09beea2245b56da
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415512"
---
# <a name="ulong-data-type-visual-basic"></a>Tipo de dados ULong (Visual Basic)

Mantém inteiros de 64 bits (8 bytes) não assinados, variando no valor de 0 a 18446744073709551615 (mais de 1,84 vezes 10 ^ 19).

## <a name="remarks"></a>Comentários

Use o `ULong` tipo de dados para conter dados binários muito grandes para `UInteger` o ou os maiores valores inteiros não assinados possíveis.

O valor padrão de `ULong` é 0.

## <a name="literal-assignments"></a>Atribuições de literal

Você pode declarar e inicializar uma `ULong` variável atribuindo-lhe um literal decimal, um literal hexadecimal, um literal octal ou (começando com Visual Basic 2017) um literal binário. Se o literal inteiro estiver fora do intervalo de `ULong` (ou seja, se for menor que <xref:System.UInt64.MinValue?displayProperty=nameWithType> ou maior que <xref:System.UInt64.MaxValue?displayProperty=nameWithType>, ocorrerá um erro de compilação.

No exemplo a seguir, inteiros iguais a 7.934.076.125 representados como literais decimais, hexadecimais e binários são atribuídos a valores `ULong`.

[!code-vb[ULong](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#ULong)]

> [!NOTE]
> Você usa o prefixo `&h` ou `&H` para denotar um literal hexadecimal, o prefixo `&b` ou `&B` para indicar um literal binário, e o prefixo `&o` ou `&O` para indicar um literal octal. Literais decimais não têm nenhum prefixo.

A partir do Visual Basic 2017, você também pode usar o caractere de sublinhado, `_` , como um separador de dígitos para melhorar a legibilidade, como mostra o exemplo a seguir.

[!code-vb[ULong](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#LongS)]

A partir do Visual Basic 15,5, você também pode usar o caractere de sublinhado ( `_` ) como um separador à esquerda entre o prefixo e os dígitos hexadecimal, binário ou octal. Por exemplo:

```vb
Dim number As ULong = &H_F9AC_0326_1489_D68C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

Os literais numéricos também podem incluir o `UL` `ul` [caractere de tipo](../../programming-guide/language-features/data-types/type-characters.md) ou para denotar o `ULong` tipo de dados, como mostra o exemplo a seguir.

```vb
Dim number = &H_00_00_0A_96_2F_AC_14_D7ul
```

## <a name="programming-tips"></a>Dicas de programação

- **Números negativos.** Como `ULong` é um tipo não assinado, ele não pode representar um número negativo. Se você usar o operador menos ( `-` ) unário em uma expressão avaliada como tipo `ULong` , Visual Basic converterá a expressão em `Decimal` primeiro.

- **Conformidade com CLS.** O `ULong` tipo de dados não faz parte do [Common Language Specification](https://www.ecma-international.org/publications/standards/Ecma-335.htm) (CLS), portanto o código em conformidade com CLS não pode consumir um componente que o utiliza.

- **Considerações sobre interoperabilidade.** Se você estiver fazendo a interface com componentes não escritos para o .NET Framework, por exemplo, automação ou objetos COM, tenha em mente que tipos como o `ulong` podem ter uma largura de dados diferente (32 bits) em outros ambientes. Se você estiver passando um argumento de 32 bits para esse componente, declare-o como `UInteger` em vez de `ULong` em seu código de Visual Basic gerenciado.

  Além disso, a automação não dá suporte a inteiros de 64 bits no Windows 95, Windows 98, Windows ME ou Windows 2000. Não é possível passar um `ULong` argumento Visual Basic para um componente de automação nessas plataformas.

- **Ampliação.** O `ULong` tipo de dados amplia para `Decimal` , `Single` e `Double` . Isso significa que você pode converter `ULong` para qualquer um desses tipos sem encontrar um <xref:System.OverflowException?displayProperty=nameWithType> erro.

- **Digite os caracteres.** Acrescentar os caracteres do tipo literal `UL` a um literal força-o ao `ULong` tipo de dados. `ULong`Não tem um caractere de tipo de identificador.

- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.UInt64?displayProperty=nameWithType>.

## <a name="see-also"></a>Confira também

- <xref:System.UInt64>
- [Tipos de dados](index.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Como: Chamar uma função do Windows que use tipos não assinados](../../programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
