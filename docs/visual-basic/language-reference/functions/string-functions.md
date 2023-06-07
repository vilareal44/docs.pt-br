---
title: Funções de Cadeia de Caracteres
ms.date: 07/20/2015
helpviewer_keywords:
- string functions
ms.assetid: f1bf9ac2-cbcf-4298-ae51-53182076bdc8
ms.openlocfilehash: 778e57eadd75baf1aabd100f9d8d41a490f79a04
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406282"
---
# <a name="string-functions-visual-basic"></a>Funções da cadeia de caracteres (Visual Basic)

A tabela a seguir lista as funções que Visual Basic fornece na <xref:Microsoft.VisualBasic.Strings?displayProperty=nameWithType> classe para pesquisar e manipular cadeias de caracteres. Eles podem ser considerados como Visual Basic funções intrínsecas; ou seja, você não precisa chamá-los como membros explícitos de uma classe, como mostram os exemplos. Métodos adicionais e, em alguns casos, métodos complementares, estão disponíveis na <xref:System.String?displayProperty=nameWithType> classe.

|Método de .NET Framework|Descrição|
|---------------------------|-----------------|
|<xref:Microsoft.VisualBasic.Strings.Asc%2A>, <xref:Microsoft.VisualBasic.Strings.AscW%2A>|Retorna um valor de `Integer` que representa o código de caractere correspondente a um caractere.|
|<xref:Microsoft.VisualBasic.Strings.Chr%2A>, <xref:Microsoft.VisualBasic.Strings.ChrW%2A>|Retorna o caractere associado ao código de caractere especificado.|
|<xref:Microsoft.VisualBasic.Strings.Filter%2A>|Retorna uma matriz baseada em zero contendo um subconjunto de uma matriz `String` com base em critérios de filtro especificados.|
|<xref:Microsoft.VisualBasic.Strings.Format%2A>|Retorna uma cadeia de caracteres formatada de acordo com as instruções contidas em uma expressão `String` de formato.|
|<xref:Microsoft.VisualBasic.Strings.FormatCurrency%2A>|Retorna uma expressão formatada como um valor de moeda usando o símbolo da moeda definido no painel de controle do sistema.|
|<xref:Microsoft.VisualBasic.Strings.FormatDateTime%2A>|Retorna uma expressão de cadeia de caracteres que representa um valor de data/hora.|
|<xref:Microsoft.VisualBasic.Strings.FormatNumber%2A>|Retorna uma expressão formatada como um número.|
|<xref:Microsoft.VisualBasic.Strings.FormatPercent%2A>|Retorna uma expressão formatada como um percentual (isto é, multiplicada por 100) com um caractere % à direita.|
|<xref:Microsoft.VisualBasic.Strings.InStr%2A>|Retorna um inteiro que especifica a posição inicial da primeira ocorrência de uma cadeia de caracteres dentro de outra.|
|<xref:Microsoft.VisualBasic.Strings.InStrRev%2A>|Retorna a posição da primeira ocorrência de uma cadeia de caracteres em outra, começando do lado direito da cadeia de caracteres.|
|<xref:Microsoft.VisualBasic.Strings.Join%2A>|Retorna uma cadeia de caracteres criada unindo um número de subcadeias contidas em uma matriz.|
|<xref:Microsoft.VisualBasic.Strings.LCase%2A>|Retorna uma cadeia de caracteres ou um caractere convertido em minúsculas.|
|<xref:Microsoft.VisualBasic.Strings.Left%2A>|Retorna uma cadeia de caracteres que contém um número especificado de caracteres do lado esquerdo de uma cadeia de caracteres.|
|<xref:Microsoft.VisualBasic.Strings.Len%2A>|Retorna um número inteiro que contém o número de caracteres em uma cadeia.|
|<xref:Microsoft.VisualBasic.Strings.LSet%2A>|Retorna uma cadeia de caracteres alinhada à esquerda que contém a cadeia especificada ajustada no tamanho especificado.|
|<xref:Microsoft.VisualBasic.Strings.LTrim%2A>|Retorna uma cadeia de caracteres que contém uma cópia de uma cadeia de caracteres especificada sem espaços à esquerda.|
|<xref:Microsoft.VisualBasic.Strings.Mid%2A>|Retorna uma cadeia de caracteres que contém um número especificado de caracteres de uma cadeia.|
|<xref:Microsoft.VisualBasic.Strings.Replace%2A>|Retorna uma cadeia de caracteres na qual uma subcadeia de caracteres especificada foi substituída por outra subcadeia de caracteres um número especificado de vezes.|
|<xref:Microsoft.VisualBasic.Strings.Right%2A>|Retorna uma cadeia de caracteres que contém um número especificado de caracteres do lado direito de uma cadeia de caracteres.|
|<xref:Microsoft.VisualBasic.Strings.RSet%2A>|Retorna uma cadeia de caracteres alinhada à direita que contém a cadeia especificada ajustada no tamanho especificado.|
|<xref:Microsoft.VisualBasic.Strings.RTrim%2A>|Retorna uma cadeia de caracteres que contém uma cópia de uma cadeia de caracteres especificada sem espaços à direita.|
|<xref:Microsoft.VisualBasic.Strings.Space%2A>|Retorna uma cadeia de caracteres que consiste no número especificado de espaços.|
|<xref:Microsoft.VisualBasic.Strings.Split%2A>|Retorna uma matriz unidimensional baseada em zero que contém um número especificado de subcadeias de caracteres.|
|<xref:Microsoft.VisualBasic.Strings.StrComp%2A>|Retorna -1, 0 ou 1, com base no resultado de uma comparação de cadeia de caracteres.|
|<xref:Microsoft.VisualBasic.Strings.StrConv%2A>|Retorna uma cadeia de caracteres convertida, conforme especificado.|
|<xref:Microsoft.VisualBasic.Strings.StrDup%2A>|Retorna uma cadeia de caracteres ou um objeto que consiste no caractere especificado repetido no número de vezes especificado.|
|<xref:Microsoft.VisualBasic.Strings.StrReverse%2A>|Retorna uma cadeia de caracteres na qual a ordem dos caracteres de uma cadeia de caracteres especificada é invertida.|
|<xref:Microsoft.VisualBasic.Strings.Trim%2A>|Retorna uma cadeia de caracteres que contém uma cópia de uma cadeia de caracteres especificada sem espaços à esquerda ou à direita.|
|<xref:Microsoft.VisualBasic.Strings.UCase%2A>|Retorna uma cadeia de caracteres ou um caractere que contém a cadeia de caracteres especificada, convertida em maiúsculas.|

Você pode usar a instrução [Option Compare](../statements/option-compare-statement.md) para definir se as cadeias de caracteres são comparadas usando uma ordem de classificação de texto que não diferencia maiúsculas de minúsculas determinada pela localidade do seu sistema ( `Text` ) ou pelas representações binárias internas dos caracteres ( `Binary` ). O método de comparação de texto padrão é `Binary`.

## <a name="example-ucase"></a>Exemplo: UCase

Este exemplo usa a função `UCase` para retornar uma versão de uma cadeia de caracteres em letras minúsculas.
[!code-vb[VbVbalrStrings#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#31)]

## <a name="example-ltrim"></a>Exemplo: LTrim

Este exemplo usa a função `LTrim` para retirar espaços à esquerda e a função `RTrim` para retirar espaços à direita de um variável de cadeia de caracteres. Usa a função de `Trim` para retirar ambos os tipos de espaços.

[!code-vb[VbVbalrStrings#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#25)]

## <a name="example-mid"></a>Exemplo: mid

Este exemplo usa a `Mid` função para retornar um número especificado de caracteres a partir de uma String.

[!code-vb[VbVbalrStrings#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#17)]

## <a name="example-len"></a>Exemplo: Len

Este exemplo usa `Len` para retornar o número especificado de caracteres em uma cadeia de caracteres.

[!code-vb[VbVbalrStrings#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#33)]

## <a name="example-instr"></a>Exemplo: InStr

Este exemplo usa a função `InStr` para retornar a posição da primeira ocorrência de uma cadeia de caracteres dentro da outra.

[!code-vb[VbVbalrStrings#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#8)]

## <a name="example-format"></a>Exemplo: formato

Este exemplo mostra vários usos da função `Format` para formatar valores usando os formatos `String` e os formatos definidos pelo usuário. Para o separador de data (`/`), separador de hora (`:`) e indicadores AM/PM (`t` e `tt`), a saída formatada real exibida pelo seu sistema depende das configurações de localidade que o código está usando. Quando horas e datas são exibidas no ambiente de desenvolvimento, o formato abreviado de tempo e o formato abreviado de data do local do código são usados.

> [!NOTE]
> Para localidades que usam um relógio de 24 horas, os indicadores AM/PM (`t` e `tt`) não exibem nada.

[!code-vb[VbVbalrStrings#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#27)]

## <a name="see-also"></a>Confira também

- [Palavras-chave](../keywords/index.md)
- [Membros da Biblioteca de Runtime do Visual Basic](../runtime-library-members.md)
- [Resumo de Manipulação da Cadeia de Caracteres](../keywords/string-manipulation-summary.md)
- [Métodos de classe System. String](xref:System.String#methods)
