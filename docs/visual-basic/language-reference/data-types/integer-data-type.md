---
title: Tipo de Dados Integer
ms.date: 01/31/2018
f1_keywords:
- vb.Integer
- Integer
helpviewer_keywords:
- numbers [Visual Basic], whole
- enumerated values [Visual Basic]
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- literal type characters [Visual Basic], I
- integers [Visual Basic], types
- data types [Visual Basic], integral
- '% identifier type character'
- data types [Visual Basic], assigning
- identifier type characters [Visual Basic], %
- I literal type character [Visual Basic]
- Integer data type
ms.assetid: a8f233b4-4be3-455c-861b-05af2fbb6c60
ms.openlocfilehash: aa7b64162308d6af2763b29034c5a7276c973876
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415603"
---
# <a name="integer-data-type-visual-basic"></a>Tipo de dados Integer (Visual Basic)

Armazena inteiros de 32 bits (4 bytes) com sinal que variam em valor de -2.147.483.648 a 2.147.483.647.  
  
## <a name="remarks"></a>Comentários

 O tipo de dados `Integer` proporciona um desempenho ideal em um processador de 32 bits. Os outros tipos integrais são mais lentos para carregar e armazenar na memória.  
  
 O valor padrão de `Integer` é 0.  

## <a name="literal-assignments"></a>Atribuições de literal

Você pode declarar e inicializar uma `Integer` variável atribuindo a ela um literal decimal, um literal hexadecimal, um literal octal ou (começando com Visual Basic 2017) um literal binário. Se o literal inteiro estiver fora do intervalo de `Integer` (ou seja, se for menor que <xref:System.Int32.MinValue?displayProperty=nameWithType> ou maior que <xref:System.Int32.MaxValue?displayProperty=nameWithType>, ocorrerá um erro de compilação.

No exemplo a seguir, inteiros iguais a 90.946 representados como literais decimais, hexadecimais e binários são atribuídos a valores `Integer`.

[!code-vb[integer](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#Int)]  

> [!NOTE]
> Você usa o prefixo `&h` ou `&H` para denotar um literal hexadecimal, o prefixo `&b` ou `&B` para indicar um literal binário, e o prefixo `&o` ou `&O` para indicar um literal octal. Literais decimais não têm nenhum prefixo.

A partir do Visual Basic 2017, você também pode usar o caractere de sublinhado, `_` , como um separador de dígitos para melhorar a legibilidade, como mostra o exemplo a seguir.

[!code-vb[integer](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#IntS)]  

A partir do Visual Basic 15,5, você também pode usar o caractere de sublinhado ( `_` ) como um separador à esquerda entre o prefixo e os dígitos hexadecimal, binário ou octal. Por exemplo:

```vb
Dim number As Integer = &H_C305_F860
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

Os literais numéricos também podem incluir o `I` [caractere de tipo](../../programming-guide/language-features/data-types/type-characters.md) para denotar o `Integer` tipo de dados, como mostra o exemplo a seguir.

```vb
Dim number = &H_035826I
```

## <a name="programming-tips"></a>Dicas de programação

- **Considerações sobre interoperabilidade.** Se você estiver fazendo a interface com componentes não escritos para o .NET Framework, como automação ou objetos COM, lembre-se de que `Integer` tem uma largura de dados diferente (16 bits) em outros ambientes. Se você estiver passando um argumento de 16 bits para esse componente, declare-o como `Short` em vez de `Integer` no seu novo código do Visual Basic.  
  
- **Ampliação.** O tipo de dados `Integer` é ampliado para `Long`, `Decimal`, `Single` ou `Double`. Isso significa que você pode converter `Integer` em qualquer um desses tipos sem a ocorrência de um erro <xref:System.OverflowException?displayProperty=nameWithType>.  
  
- **Digite os caracteres.** Acrescentar o caractere de tipo literal `I` a um literal o força ao tipo de dados `Integer`. Acrescentar o caractere de tipo identificador `%` a qualquer identificador o força ao tipo `Integer`.  
  
- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.Int32?displayProperty=nameWithType>.  
  
## <a name="range"></a>Intervalo

Se você tentar definir uma variável de um tipo integral para um número fora do intervalo para esse tipo, ocorrerá um erro. Se você tentar defini-lo como uma fração, o número será arredondado para cima ou para baixo para o valor inteiro mais próximo. Se o número for igualmente próximo de dois valores inteiros, o valor será arredondado para o inteiro par mais próximo. Esse comportamento minimiza erros de arredondamento gerados ao arredondar consistentemente um valor de ponto médio em uma única direção. O código a seguir mostra exemplos de arredondamento.  

```vb  
' The valid range of an Integer variable is -2147483648 through +2147483647.  
Dim k As Integer  
' The following statement causes an error because the value is too large.  
k = 2147483648  
' The following statement sets k to 6.  
k = 5.9  
' The following statement sets k to 4  
k = 4.5  
' The following statement sets k to 6  
' Note, Visual Basic uses banker’s rounding (toward nearest even number)  
k = 5.5  
```

## <a name="see-also"></a>Confira também

- <xref:System.Int32?displayProperty=nameWithType>
- [Tipos de dados](index.md)
- [Tipo de Dados Long](long-data-type.md)
- [Tipo de Dados Short](short-data-type.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
