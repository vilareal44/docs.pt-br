---
title: Tipo de Dados SByte
ms.date: 04/20/2017
f1_keywords:
- vb.sbyte
helpviewer_keywords:
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- SByte data type
ms.assetid: 5c38374a-18a1-4cc2-b493-299e3dcaa60f
ms.openlocfilehash: e7d45c74056ce5b6aa66674c99e48b5ab60015f0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415564"
---
# <a name="sbyte-data-type-visual-basic"></a>Tipo de dados SByte (Visual Basic)

Mantém números inteiros de 8 bits assinados (1 byte) que variam em valor de-128 até 127.

## <a name="remarks"></a>Comentários

Use o `SByte` tipo de dados para conter valores inteiros que não exigem a largura total dos dados de `Integer` ou até a metade da largura de dados de `Short` . Em alguns casos, a Common Language Runtime pode ser capaz de empacotar suas `SByte` variáveis em conjunto e economizar o consumo de memória.

O valor padrão de `SByte` é 0.

## <a name="literal-assignments"></a>Atribuições de literal

Você pode declarar e inicializar uma `SByte` variável atribuindo a ela um literal decimal, um literal hexadecimal, um literal octal ou (começando com Visual Basic 2017) um literal binário.

No exemplo a seguir, números inteiros iguais a-102 que são representados como decimais, hexadecimais e literais binários são atribuídos a `SByte` valores. Este exemplo requer que você compile com a `/removeintchecks` opção do compilador.

[!code-vb[SByte](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#SByte)]

> [!NOTE]
> Você usa o prefixo `&h` ou `&H` para denotar um literal hexadecimal, o prefixo `&b` ou `&B` para indicar um literal binário, e o prefixo `&o` ou `&O` para indicar um literal octal. Literais decimais não têm nenhum prefixo.

A partir do Visual Basic 2017, você também pode usar o caractere de sublinhado, `_` , como um separador de dígitos para melhorar a legibilidade, como mostra o exemplo a seguir.

[!code-vb[SByteSeparator](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#SByteS)]

A partir do Visual Basic 15,5, você também pode usar o caractere de sublinhado ( `_` ) como um separador à esquerda entre o prefixo e os dígitos hexadecimal, binário ou octal. Por exemplo:

```vb
Dim number As SByte = &H_F9
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

Se o literal inteiro estiver fora do intervalo de `SByte` (ou seja, se for menor que <xref:System.SByte.MinValue?displayProperty=nameWithType> ou maior que <xref:System.SByte.MaxValue?displayProperty=nameWithType>, ocorrerá um erro de compilação. Quando um inteiro literal não tem nenhum sufixo, um [inteiro](integer-data-type.md) é inferido. Se o literal inteiro estiver fora do intervalo do `Integer` tipo, um [longo](long-data-type.md) será inferido. Isso significa que, nos exemplos anteriores, os literais numéricos `0x9A` e `0b10011010` são interpretados como inteiros com sinal de 32 bits com um valor de 156, que excede <xref:System.SByte.MaxValue?displayProperty=nameWithType> . Para compilar com êxito um código como esse que atribui um inteiro não decimal a um `SByte` , você pode fazer o seguinte:

- Desabilite as verificações de limites de inteiros compilando com a `/removeintchecks` opção do compilador.

- Use um [caractere de tipo](../../programming-guide/language-features/data-types/type-characters.md) para definir explicitamente o valor literal que você deseja atribuir ao `SByte` . O exemplo a seguir atribui um valor literal negativo `Short` a um `SByte` . Observe que, para números negativos, o bit de ordem superior da palavra de ordem superior do literal numérico deve ser definido. No caso de nosso exemplo, este é o bit 15 do valor literal `Short` .

   [!code-vb[SByteTypeChars](../../../../samples/snippets/visualbasic/language-reference/data-types/sbyte-assignment.vb#1)]

## <a name="programming-tips"></a>Dicas de programação

- **Conformidade com CLS.** O `SByte` tipo de dados não faz parte do [Common Language Specification](https://www.ecma-international.org/publications/standards/Ecma-335.htm) (CLS), portanto o código em conformidade com CLS não pode consumir um componente que o utiliza.

- **Ampliação.** O `SByte` tipo de dados amplia para `Short` , `Integer` ,,, `Long` `Decimal` `Single` e `Double` . Isso significa que você pode converter `SByte` para qualquer um desses tipos sem encontrar um <xref:System.OverflowException?displayProperty=nameWithType> erro.

- **Digite os caracteres.** `SByte`Não tem caractere de tipo literal ou caractere de tipo de identificador.

- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.SByte?displayProperty=nameWithType>.

## <a name="see-also"></a>Confira também

- <xref:System.SByte?displayProperty=nameWithType>
- [Tipos de dados](index.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Tipo de Dados Short](short-data-type.md)
- [Tipo de Dados Integer](integer-data-type.md)
- [Tipo de Dados Long](long-data-type.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
