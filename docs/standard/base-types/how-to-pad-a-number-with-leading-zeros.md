---
title: 'Como: Preencher um número com zeros à esquerda'
description: Aprenda a preencher um número com zeros à esquerda. Adicione zeros à esquerda a inteiros ou valores numéricos a um comprimento total específico ou a um número específico de zeros à esquerda.
ms.date: 02/25/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- numeric format strings [.NET Framework]
- formatting [.NET Framework], numbers
- number formatting [.NET Framework]
- numbers [.NET Framework], format strings
ms.assetid: 0b2c2cb5-c580-4891-8d81-cb632f5ec384
ms.openlocfilehash: 6ef0ddb37f1bc73254aa639d7c018ec6a01abd9b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447180"
---
# <a name="how-to-pad-a-number-with-leading-zeros"></a>Como: Preencher um número com zeros à esquerda

É possível adicionar zeros à esquerda de um inteiro usando a [cadeia de caracteres de formato numérico padrão](standard-numeric-format-strings.md) “D” com um especificador de precisão. Você pode adicionar zeros à esquerda de inteiros e pontos flutuantes usando uma [cadeia de caracteres de formato numérico personalizado](custom-numeric-format-strings.md). Este artigo mostra como usar os dois métodos para preencher um número com zeros à esquerda.

## <a name="to-pad-an-integer-with-leading-zeros-to-a-specific-length"></a>Para acrescentar um número inteiro com zeros à esquerda com um comprimento específico

1. Determine o número mínimo de dígitos que o valor inteiro deve exibir. Inclua os dígitos à esquerda nesse número.

1. Determine se quer exibir o inteiro como valor decimal ou hexadecimal.

    - Para exibir o inteiro como um valor decimal, chame seu método `ToString(String)` e passe a cadeia de caracteres "D*n*" como o valor do parâmetro `format`, em que *n* representa o tamanho mínimo da cadeia de caracteres.

    - Para exibir o número inteiro como hexadecimal, chame seu método `ToString(String)` e informe a cadeia de caracteres “X*n*” como valor do parâmetro format, em que *n* representa o comprimento mínimo da cadeia de caracteres.

Você também pode usar a cadeia de caracteres de formato em uma cadeia de caracteres interpolada em [C#](../../csharp/language-reference/tokens/interpolated.md) e em [Visual Basic](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md) ou você pode chamar um método, como <xref:System.String.Format%2A?displayProperty=nameWithType> ou <xref:System.Console.WriteLine%2A?displayProperty=nameWithType>, que usa a [formatação de composição](composite-formatting.md).

O exemplo a seguir formata diversos valores inteiros com zeros à esquerda para que o comprimento total do número formatado tenha pelo menos oito caracteres.

[!code-csharp[Formatting.HowTo.PadNumber#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#1)]
[!code-vb[Formatting.HowTo.PadNumber#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#1)]

## <a name="to-pad-an-integer-with-a-specific-number-of-leading-zeros"></a>Para acrescentar um número inteiro com uma determinada quantidade de zeros à esquerda

1. Determine quantos zeros à esquerda o valor inteiro deve exibir.

1. Determine se quer exibir o inteiro como valor decimal ou hexadecimal.

    - A formatação como um valor decimal requer que você use o especificador de formato padrão "D".

    - A formatação como um valor hexadecimal requer que você use o especificador de formato padrão "X".

1. Determine o comprimento da cadeia de caracteres numérica não acrescentada. Para isso, chame o método `ToString("D").Length` ou `ToString("X").Length` do valor inteiro.

1. Adicione quantos dígitos zero à esquerda quer incluir na cadeia de caracteres formatada com comprimento da cadeia de caracteres numérica não acrescentada. A adição do número de zeros à esquerda define o comprimento total da cadeia de caracteres preenchida.

1. Chame o método `ToString(String)` do valor inteiro e informe a cadeia de caracteres “D*n*” para cadeias de caracteres decimais e “X*n*” para cadeias de caracteres hexadecimais, em que *n* representa o comprimento total da cadeia de caracteres acrescentada. Você também pode usar uma cadeia de caracteres de formato “D*n*” ou “X*n*” em um método com suporte para formatação de composição.

O exemplo a seguir acrescenta um valor inteiro com cinco dígitos zero à esquerda.

[!code-csharp[Formatting.HowTo.PadNumber#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#2)]
[!code-vb[Formatting.HowTo.PadNumber#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#2)]

## <a name="to-pad-a-numeric-value-with-leading-zeros-to-a-specific-length"></a>Para acrescentar um valor numérico com zeros à esquerda com comprimento específico

1. Determine quantos dígitos a representação da cadeia de caracteres de números deve ter à esquerda dos decimais. Inclua os dígitos zero à esquerda no número total de dígitos.

1. Defina uma cadeia de caracteres de formato numérico personalizado que usa o espaço reservado para zero “0” para representar a quantidade mínima de dígitos zero.

1. Chame o método `ToString(String)` do número e apresente-o à cadeia de caracteres de formato personalizado. Você também pode usar uma cadeia de caracteres de formato personalizado com interpolação de cadeia de caracteres ou um método com suporte para formatação de composição.

O exemplo a seguir formata diversos valores numéricos com zeros à esquerda. Como resultado, o comprimento total do número formatado é pelo menos oito dígitos à esquerda do separador decimal.

[!code-csharp[Formatting.HowTo.PadNumber#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#3)]
[!code-vb[Formatting.HowTo.PadNumber#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#3)]

## <a name="to-pad-a-numeric-value-with-a-specific-number-of-leading-zeros"></a>Para acrescentar um valor numérico com uma determinada quantidade de zeros à esquerda

1. Determine quantos zeros à esquerda o valor numérico deve ter.

1. Determine a quantidade de dígitos à esquerda do decimal na cadeia de caracteres numéricos não preenchida:

    1. Determine se a representação da cadeia de caracteres de um número inclui um símbolo de ponto decimal.

    1. Caso o símbolo esteja presente, determine a quantidade de caracteres à esquerda do ponto decimal.

         -ou-

         Caso o símbolo de decimal não esteja presente, determine o comprimento da cadeia de caracteres.

1. Crie uma cadeia de caracteres de formato personalizado que use:

    - O espaço reservado para zero “0” para cada um dos zeros à esquerda que aparecem na cadeia de caracteres.

    - O espaço reservado para zero ou o espaço reservado de dígito “#” para representar cada dígito na cadeia de caracteres padrão.

1. Forneça a cadeia de caracteres de formato personalizado como parâmetro para o método `ToString(String)` do número ou para um método que use a formatação composta.

O exemplo a seguir acrescenta dois valores <xref:System.Double> com cinco dígitos zero à esquerda.

[!code-csharp[Formatting.HowTo.PadNumber#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#4)]
[!code-vb[Formatting.HowTo.PadNumber#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#4)]

## <a name="see-also"></a>Confira também

- [Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md)
- [Cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md)
- [Formatação composta](composite-formatting.md)
