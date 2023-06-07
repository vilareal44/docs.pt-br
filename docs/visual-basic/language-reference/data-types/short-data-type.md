---
title: Tipo de Dados Short
ms.date: 01/31/2018
f1_keywords:
- vb.Short
helpviewer_keywords:
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- S literal type character [Visual Basic]
- Short data type
- literal type characters [Visual Basic], S
ms.assetid: 65fcbcf3-a841-400e-885e-301497729a8b
ms.openlocfilehash: 176d27c86127dac1d9c9c0231790f7a5c2a2fefc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415551"
---
# <a name="short-data-type-visual-basic"></a>Tipo de dados Short (Visual Basic)

Mantém números inteiros de 16 bits (2 bytes) assinados que variam em valor de-32.768 até 32.767.  
  
## <a name="remarks"></a>Comentários  

 Use o `Short` tipo de dados para conter valores inteiros que não exigem a largura total dos dados de `Integer` . Em alguns casos, o Common Language Runtime pode empacotar suas `Short` variáveis em conjunto e economizar o consumo de memória.  
  
 O valor padrão de `Short` é 0.  
  
## <a name="literal-assignments"></a>Atribuições de literal

Você pode declarar e inicializar uma `Short` variável atribuindo-lhe um literal decimal, um literal hexadecimal, um literal octal ou (começando com Visual Basic 2017) um literal binário. Se o literal inteiro estiver fora do intervalo de `Short` (ou seja, se for menor que <xref:System.Int16.MinValue?displayProperty=nameWithType> ou maior que <xref:System.Int16.MaxValue?displayProperty=nameWithType>, ocorrerá um erro de compilação.

No exemplo a seguir, números inteiros iguais a 1.034 que são representados como decimais, hexadecimais e literais binários são convertidos implicitamente de [inteiro](integer-data-type.md) em `Short` valores.

[!code-vb[Short](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#Short)]

> [!NOTE]
> Você usa o prefixo `&h` ou `&H` para denotar um literal hexadecimal, o prefixo `&b` ou `&B` para indicar um literal binário, e o prefixo `&o` ou `&O` para indicar um literal octal. Literais decimais não têm nenhum prefixo.

A partir do Visual Basic 2017, você também pode usar o caractere de sublinhado, `_` , como um separador de dígitos para melhorar a legibilidade, como mostra o exemplo a seguir.

[!code-vb[Short](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#ShortS)]

A partir do Visual Basic 15,5, você também pode usar o caractere de sublinhado ( `_` ) como um separador à esquerda entre o prefixo e os dígitos hexadecimal, binário ou octal. Por exemplo:

```vb
Dim number As Short = &H_3264
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

Os literais numéricos também podem incluir o `S` [caractere de tipo](../../programming-guide/language-features/data-types/type-characters.md) para denotar o `Short` tipo de dados, como mostra o exemplo a seguir.

```vb
Dim number = &H_3264S
```

## <a name="programming-tips"></a>Dicas de programação

- **Ampliação.** O `Short` tipo de dados amplia para `Integer` , `Long` , `Decimal` , `Single` ou `Double` . Isso significa que você pode converter `Short` em qualquer um desses tipos sem a ocorrência de um erro <xref:System.OverflowException?displayProperty=nameWithType>.  
  
- **Digite os caracteres.** Acrescentar o caractere de tipo literal `S` a um literal o força ao tipo de dados `Short`. `Short`Não tem um caractere de tipo de identificador.  
  
- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.Int16?displayProperty=nameWithType>.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Int16?displayProperty=nameWithType>
- [Tipos de dados](index.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Tipo de Dados Integer](integer-data-type.md)
- [Tipo de Dados Long](long-data-type.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
