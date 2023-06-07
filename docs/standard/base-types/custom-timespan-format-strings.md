---
title: Cadeias de caracteres de formato TimeSpan personalizado
description: Entender cadeias de caracteres de formato de TimeSpan personalizadas no .NET. Uma cadeia de caracteres de formato personalizado contém um ou mais especificadores de formato TimeSpan & qualquer número de caracteres literais.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- format specifiers, custom time interval
- format strings
- formatting [.NET Framework], time interval
- custom time interval format strings
- formatting [.NET Framework], time
- custom TimeSpan format strings
ms.assetid: a63ebf55-7269-416b-b4f5-286f6c03bf0e
ms.openlocfilehash: 54079975b9b73844f598a7c7a7fea1a64bd6450c
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768554"
---
# <a name="custom-timespan-format-strings"></a>Cadeias de caracteres de formato TimeSpan personalizado

Uma cadeia de caracteres de formato <xref:System.TimeSpan> define a representação de cadeia de caracteres de um valor de <xref:System.TimeSpan> que é resultante de uma operação de formatação. Uma cadeia de caracteres de formato personalizado consiste em um ou mais O especificadores de formato <xref:System.TimeSpan> em conjunto com qualquer número de caracteres literais. Qualquer cadeia de caracteres que não seja uma [cadeia de caracteres de formato TimeSpan padrão](standard-timespan-format-strings.md) é interpretada como uma <xref:System.TimeSpan> cadeia de formato personalizado.

> [!IMPORTANT]
> Os especificadores de formato <xref:System.TimeSpan> personalizados não incluem símbolos de separador de espaço reservado, como os símbolos que separam dias de horas, horas de minutos ou segundos de frações de segundo. Em vez disso, esses símbolos devem ser incluídos na cadeia de caracteres de formato personalizado como literais de cadeia de caracteres. Por exemplo, `"dd\.hh\:mm"` define um ponto (.) como o separador entre dias e horas e dois-pontos (:) como o separador entre horas e minutos.
>
> Os especificadores de formato <xref:System.TimeSpan> personalizado também não incluem um símbolo de sinal que permite diferenciar entre intervalos de tempo positivos e negativos. Para incluir um símbolo de sinal, você precisa construir uma cadeia de caracteres de formato usando lógica condicional. A seção [outros caracteres](#other-characters) inclui um exemplo.

As representações de sequência de valores <xref:System.TimeSpan> são produzidas por chamadas para as sobrecargas do método <xref:System.TimeSpan.ToString%2A?displayProperty=nameWithType> e por métodos que oferecem suporte à formatação composta, como <xref:System.String.Format%2A?displayProperty=nameWithType>. Para obter mais informações, consulte [Tipos de formatação](formatting-types.md) e [Formatação de composição](composite-formatting.md). O exemplo a seguir ilustra o uso de sequências de formato personalizado em operações de formatação.

[!code-csharp[Conceptual.TimeSpan.Custom#1](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customformatexample1.cs#1)]
[!code-vb[Conceptual.TimeSpan.Custom#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customformatexample1.vb#1)]

As sequências de formato <xref:System.TimeSpan> personalizado são usadas também pelos métodos <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> e <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> para definir o formato necessário de sequências de caracteres de entrada para análise de operações. (A análise converte a representação de cadeia de caracteres de um valor para esse valor.) O exemplo a seguir ilustra o uso de cadeias de caracteres de formato padrão em operações de análise.

[!code-csharp[Conceptual.TimeSpan.Custom#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customparseexample1.cs#2)]
[!code-vb[Conceptual.TimeSpan.Custom#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customparseexample1.vb#2)]

<a name="table"></a>A tabela a seguir descreve os especificadores de formato de data e hora personalizados.

| Especificador de formato | Descrição | Exemplo |
|----------------------|-----------------|-------------|
|"d", "%d"|O número de dias inteiros no intervalo de tempo.<br /><br /> Mais informações: [o especificador de formato personalizado "d"](#dSpecifier).|`new TimeSpan(6, 14, 32, 17, 685):`<br /><br /> `%d` --> "6"<br /><br /> `d\.hh\:mm` --> "6.14:32"|
|"dd"-"dddddddd"|O número de dias inteiros no intervalo de tempo, preenchido com zeros à esquerda, conforme necessário.<br /><br /> Mais informações: [os especificadores de formato personalizado "dd"-"dddddddd"](#ddSpecifier).|`new TimeSpan(6, 14, 32, 17, 685):`<br /><br /> `ddd` --> "006"<br /><br /> `dd\.hh\:mm` --> "06.14:32"|
|"h", "%h"|O número de horas inteiras no intervalo de tempo que não são contadas como parte dos dias. Horas de dígito único não têm um zero à esquerda.<br /><br /> Mais informações: [o especificador de formato personalizado "h"](#hSpecifier).|`new TimeSpan(6, 14, 32, 17, 685):`<br /><br /> `%h` --> "14"<br /><br /> `hh\:mm` --> "14:32"|
|"hh"|O número de horas inteiras no intervalo de tempo que não são contadas como parte dos dias. Horas de dígito único têm um zero à esquerda.<br /><br /> Mais informações: [o especificador de formato personalizado "HH"](#hhSpecifier).|`new TimeSpan(6, 14, 32, 17, 685):`<br /><br /> `hh` --> "14"<br /><br /> `new TimeSpan(6, 8, 32, 17, 685):`<br /><br /> `hh` --> 08|
|"m", "%m"|O número de minutos inteiros no intervalo de tempo que não são incluídos como parte das horas ou dias. Minutos de dígito único não têm um zero à esquerda.<br /><br /> Mais informações: [o especificador de formato personalizado "m"](#mSpecifier).|`new TimeSpan(6, 14, 8, 17, 685):`<br /><br /> `%m` --> "8"<br /><br /> `h\:m` --> "14:8"|
|"mm"|O número de minutos inteiros no intervalo de tempo que não são incluídos como parte das horas ou dias. Minutos de dígito único têm um zero à esquerda.<br /><br /> Mais informações: [o especificador de formato personalizado "mm"](#mmSpecifier).|`new TimeSpan(6, 14, 8, 17, 685):`<br /><br /> `mm` --> "08"<br /><br /> `new TimeSpan(6, 8, 5, 17, 685):`<br /><br /> `d\.hh\:mm\:ss` --> 6.08:05:17|
|"s", "%s"|O número de segundos inteiros no intervalo de tempo que não são incluídos como parte das horas, dias ou minutos. Segundos de dígito único não têm um zero à esquerda.<br /><br /> Mais informações: [o especificador de formato personalizado "s"](#sSpecifier).|`TimeSpan.FromSeconds(12.965)`:<br /><br /> `%s` --> 12<br /><br /> `s\.fff` --> 12.965|
|"ss"|O número de segundos inteiros no intervalo de tempo que não são incluídos como parte das horas, dias ou minutos.  Segundos de dígito único têm um zero à esquerda.<br /><br /> Mais informações: [o especificador de formato personalizado "SS"](#ssSpecifier).|`TimeSpan.FromSeconds(6.965)`:<br /><br /> `ss` --> 06<br /><br /> `ss\.fff` --> 06.965|
|"f", "%f"|Os décimos de segundo em um intervalo de tempo.<br /><br /> Mais informações: [o especificador de formato personalizado "f"](#fSpecifier).|`TimeSpan.FromSeconds(6.895)`:<br /><br /> `f` --> 8<br /><br /> `ss\.f` --> 06.8|
|"ff"|Os centésimos de segundo em um intervalo de tempo.<br /><br /> Mais informações: [o especificador de formato personalizado "FF"](#ffSpecifier).|`TimeSpan.FromSeconds(6.895)`:<br /><br /> `ff` --> 89<br /><br /> `ss\.ff` --> 06.89|
|"fff"|Os milissegundos em um intervalo de tempo.<br /><br /> Mais informações: [o especificador de formato personalizado "fff"](#f3Specifier).|`TimeSpan.FromSeconds(6.895)`:<br /><br /> `fff` --> 895<br /><br /> `ss\.fff` --> 06.895|
|"ffff"|Os décimos de milésimos de segundo em um intervalo de tempo.<br /><br /> Mais informações: [o especificador de formato personalizado "ffff"](#f4Specifier).|`TimeSpan.Parse("0:0:6.8954321")`:<br /><br /> `ffff` --> 8954<br /><br /> `ss\.ffff` --> 06.8954|
|"fffff"|As centenas de milésimos de segundo em um intervalo de tempo.<br /><br /> Mais informações: [o especificador de formato personalizado "fffff"](#f5Specifier).|`TimeSpan.Parse("0:0:6.8954321")`:<br /><br /> `fffff` --> 89543<br /><br /> `ss\.fffff` --> 06.89543|
|"ffffff"|Os milionésimos de segundo em um intervalo de tempo.<br /><br /> Mais informações: [o especificador de formato personalizado "FFFFFF"](#f6Specifier).|`TimeSpan.Parse("0:0:6.8954321")`:<br /><br /> `ffffff` --> 895432<br /><br /> `ss\.ffffff` --> 06.895432|
|"fffffff"|Os dez milionésimos de segundo (ou as marcas fracionárias) em um intervalo de tempo.<br /><br /> Mais informações: [o especificador de formato personalizado "fffffff"](#f7Specifier).|`TimeSpan.Parse("0:0:6.8954321")`:<br /><br /> `fffffff` --> 8954321<br /><br /> `ss\.fffffff` --> 06.8954321|
|"F", "%F"|Os décimos de segundo em um intervalo de tempo. Nada será exibido se o dígito for zero.<br /><br /> Mais informações: [o especificador de formato personalizado "F"](#F_Specifier).|`TimeSpan.Parse("00:00:06.32")`:<br /><br /> `%F`: 3<br /><br /> `TimeSpan.Parse("0:0:3.091")`:<br /><br /> `ss\.F`: 03.|
|"FF"|Os centésimos de segundo em um intervalo de tempo. Zeros à direita fracionais ou dois dígitos zero não são incluídos.<br /><br /> Mais informações: [o especificador de formato personalizado "FF"](#FF_Specifier).|`TimeSpan.Parse("00:00:06.329")`:<br /><br /> `FF`: 32<br /><br /> `TimeSpan.Parse("0:0:3.101")`:<br /><br /> `ss\.FF`: 03.1|
|"FFF"|Os milissegundos em um intervalo de tempo. Zeros à direita fracionais não são incluídos.<br /><br /> Mais informações:|`TimeSpan.Parse("00:00:06.3291")`:<br /><br /> `FFF`: 329<br /><br /> `TimeSpan.Parse("0:0:3.1009")`:<br /><br /> `ss\.FFF`: 03.1|
|"FFFF"|Os décimos de milésimos de segundo em um intervalo de tempo. Zeros à direita fracionais não são incluídos.<br /><br /> Mais informações: [o especificador de formato personalizado "ffff"](#F4_Specifier).|`TimeSpan.Parse("00:00:06.32917")`:<br /><br /> `FFFFF`: 3291<br /><br /> `TimeSpan.Parse("0:0:3.10009")`:<br /><br /> `ss\.FFFF`: 03.1|
|"FFFFF"|As centenas de milésimos de segundo em um intervalo de tempo. Zeros à direita fracionais não são incluídos.<br /><br /> Mais informações: [o especificador de formato personalizado "fffff"](#F5_Specifier).|`TimeSpan.Parse("00:00:06.329179")`:<br /><br /> `FFFFF`: 32917<br /><br /> `TimeSpan.Parse("0:0:3.100009")`:<br /><br /> `ss\.FFFFF`: 03.1|
|"FFFFFF"|Os milionésimos de segundo em um intervalo de tempo. Zeros à direita fracionais não são exibidos.<br /><br /> Mais informações: [o especificador de formato personalizado "FFFFFF"](#F6_Specifier).|`TimeSpan.Parse("00:00:06.3291791")`:<br /><br /> `FFFFFF`: 329179<br /><br /> `TimeSpan.Parse("0:0:3.1000009")`:<br /><br /> `ss\.FFFFFF`: 03.1|
|"FFFFFFF"|Os décimos de milionésimos de segundo em um intervalo de tempo. Zeros à direita fracionais ou sete dígitos zero não são exibidos.<br /><br /> Mais informações: [o especificador de formato personalizado "fffffff"](#F7_Specifier).|`TimeSpan.Parse("00:00:06.3291791")`:<br /><br /> `FFFFFF`: 3291791<br /><br /> `TimeSpan.Parse("0:0:3.1900000")`:<br /><br /> `ss\.FFFFFF`: 03.19|
|'*cadeia de caracteres*'|Delimitador de cadeia de caracteres literal.<br /><br /> Mais informações: [outros caracteres](#other-characters).|`new TimeSpan(14, 32, 17):`<br /><br /> `hh':'mm':'ss` --> "14:32:17"|
|&#92;|O caractere de escape.<br /><br /> Mais informações: [outros caracteres](#other-characters).|`new TimeSpan(14, 32, 17):`<br /><br /> `hh\:mm\:ss` --> "14:32:17"|
|Qualquer outro caractere|Qualquer outro caractere sem escape é interpretado como um especificador de formato personalizado.<br /><br /> Mais informações: [outros caracteres](#other-characters).|`new TimeSpan(14, 32, 17):`<br /><br /> `hh\:mm\:ss` --> "14:32:17"|

## <a name="the-d-custom-format-specifier"></a><a name="dSpecifier"></a>O especificador de formato personalizado "d"

O especificador de formato personalizado "d" fornece o valor da propriedade <xref:System.TimeSpan.Days%2A?displayProperty=nameWithType>, que representa o número de dias inteiros no intervalo de tempo. Ele gera o número total de dias em um valor de <xref:System.TimeSpan>, mesmo se o valor tiver mais de um dígito. Se o valor da propriedade <xref:System.TimeSpan.Days%2A?displayProperty=nameWithType> for zero, o especificador gerará "0".

Se o especificador de formato personalizado "d" for usado sozinho, especifique "%d" para que ele não seja interpretado incorretamente como uma cadeia de caracteres de formato padrão. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#3)]
[!code-vb[Conceptual.TimeSpan.Custom#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#3)]

O exemplo a seguir ilustra o uso do especificador de formato personalizado “d”.

[!code-csharp[Conceptual.TimeSpan.Custom#4](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#4)]
[!code-vb[Conceptual.TimeSpan.Custom#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#4)]

[Voltar à tabela](#table)

## <a name="the-dd-dddddddd-custom-format-specifiers"></a><a name="ddSpecifier"></a>Os especificadores de formato personalizado "dd"-"dddddddd"

Os especificadores de formato personalizados "dd", "ddd", "dddd", "ddddd", "dddddd", "ddddddd" e "dddddddd" fornecem o valor da propriedade <xref:System.TimeSpan.Days%2A?displayProperty=nameWithType>, que representa o número de dias inteiros no intervalo de tempo.

A cadeia de caracteres de saída inclui um número mínimo de dígitos especificado pelo número de caracteres "d" no especificador de formato e é preenchido com zeros à esquerda conforme necessário. Se os dígitos do número de dias ultrapassarem o número de caracteres "d" no especificador de formato, o número total de dias será fornecido na cadeia de caracteres de resultado.

O exemplo a seguir usa esses O especificadores de formato para exibir a representação de cadeia de caracteres de dois valores de <xref:System.TimeSpan>. O valor do componente de dias do primeiro intervalo de tempo é zero; o valor do componente de dias do segundo é 365.

[!code-csharp[Conceptual.TimeSpan.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#5)]
[!code-vb[Conceptual.TimeSpan.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#5)]

[Voltar à tabela](#table)

## <a name="the-h-custom-format-specifier"></a><a name="hSpecifier"></a>O especificador de formato personalizado "h"

O especificador de formato personalizado "h" fornece o valor da propriedade <xref:System.TimeSpan.Hours%2A?displayProperty=nameWithType>, que representa o número de horas inteiras no intervalo de tempo que não são contadas como parte do componente de dia. Ele retorna um valor de cadeia de caracteres de um dígito se o valor da propriedade <xref:System.TimeSpan.Hours%2A?displayProperty=nameWithType> for de 0 a 9 e retorna um valor de cadeia de caracteres de dois dígitos se o valor da propriedade <xref:System.TimeSpan.Hours%2A?displayProperty=nameWithType> for de 10 a 23.

Se o especificador de formato personalizado "h" for usado sozinho, especifique "%h" para que ele não seja interpretado incorretamente como uma cadeia de caracteres de formato padrão. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#6)]
[!code-vb[Conceptual.TimeSpan.Custom#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#6)]

Normalmente, em uma operação de análise, uma cadeia de caracteres de entrada que inclui apenas um único número é interpretada como o número de dias. Você pode usar o especificador de formato personalizado "%h" em vez disso para interpretar a cadeia de caracteres numérica como o número de horas. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#8)]
[!code-vb[Conceptual.TimeSpan.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#8)]

O exemplo a seguir ilustra o uso do especificador de formato personalizado “h”.

[!code-csharp[Conceptual.TimeSpan.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#7)]
[!code-vb[Conceptual.TimeSpan.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#7)]

[Voltar à tabela](#table)

## <a name="the-hh-custom-format-specifier"></a><a name="hhSpecifier"></a>O especificador de formato personalizado "HH"

O especificador de formato personalizado "hh" fornece o valor da propriedade <xref:System.TimeSpan.Hours%2A?displayProperty=nameWithType>, que representa o número de horas inteiras no intervalo de tempo que não são contadas como parte do componente de dia. Para valores de 0 a 9, a cadeia de caracteres de saída inclui um zero à esquerda.

Normalmente, em uma operação de análise, uma cadeia de caracteres de entrada que inclui apenas um único número é interpretada como o número de dias. Você pode usar o especificador de formato personalizado "hh" em vez disso para interpretar a cadeia de caracteres numérica como o número de horas. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#9](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#9)]
[!code-vb[Conceptual.TimeSpan.Custom#9](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#9)]

O exemplo a seguir ilustra o uso do especificador de formato personalizado “hh”.

[!code-csharp[Conceptual.TimeSpan.Custom#10](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#10)]
[!code-vb[Conceptual.TimeSpan.Custom#10](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#10)]

[Voltar à tabela](#table)

## <a name="the-m-custom-format-specifier"></a><a name="mSpecifier"></a>O especificador de formato personalizado "m"

O especificador de formato personalizado "m" fornece o valor da propriedade <xref:System.TimeSpan.Minutes%2A?displayProperty=nameWithType>, que representa o número de minutos inteiros no intervalo de tempo que não são contados como parte do componente de dia. Ele retorna um valor de cadeia de caracteres de um dígito se o valor da propriedade <xref:System.TimeSpan.Minutes%2A?displayProperty=nameWithType> for de 0 a 9 e retorna um valor de cadeia de caracteres de dois dígitos se o valor da propriedade <xref:System.TimeSpan.Minutes%2A?displayProperty=nameWithType> for de 10 a 59.

Se o especificador de formato personalizado "m" for usado sozinho, especifique "%m" para que ele não seja interpretado incorretamente como uma cadeia de caracteres de formato padrão. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#6)]
[!code-vb[Conceptual.TimeSpan.Custom#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#6)]

Normalmente, em uma operação de análise, uma cadeia de caracteres de entrada que inclui apenas um único número é interpretada como o número de dias. Você pode usar o especificador de formato personalizado "%m" em vez disso para interpretar a cadeia de caracteres numérica como o número de minutos. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#11](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#11)]
[!code-vb[Conceptual.TimeSpan.Custom#11](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#11)]

O exemplo a seguir ilustra o uso do especificador de formato personalizado “m”.

[!code-csharp[Conceptual.TimeSpan.Custom#12](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#12)]
[!code-vb[Conceptual.TimeSpan.Custom#12](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#12)]

[Voltar à tabela](#table)

## <a name="the-mm-custom-format-specifier"></a><a name="mmSpecifier"></a>O especificador de formato personalizado "mm"

O especificador de formato personalizado "mm" fornece o valor da propriedade <xref:System.TimeSpan.Minutes%2A?displayProperty=nameWithType>, que representa o número de minutos inteiros no intervalo de tempo que não está incluído como parte do componente de dia. Para valores de 0 a 9, a cadeia de caracteres de saída inclui um zero à esquerda.

Normalmente, em uma operação de análise, uma cadeia de caracteres de entrada que inclui apenas um único número é interpretada como o número de dias. Você pode usar o especificador de formato personalizado "mm" em vez disso para interpretar a cadeia de caracteres numérica como o número de minutos. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#13)]
[!code-vb[Conceptual.TimeSpan.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#13)]

O exemplo a seguir ilustra o uso do especificador de formato personalizado “mm”.

[!code-csharp[Conceptual.TimeSpan.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#14)]
[!code-vb[Conceptual.TimeSpan.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#14)]

[Voltar à tabela](#table)

## <a name="the-s-custom-format-specifier"></a><a name="sSpecifier"></a>O especificador de formato personalizado "s"

O especificador de formato personalizado "s" fornece o valor da propriedade <xref:System.TimeSpan.Seconds%2A?displayProperty=nameWithType>, que representa o número de segundos inteiros no intervalo de tempo que não são incluídos como parte do componente de horas, dias ou minutos. Ele retorna um valor de cadeia de caracteres de um dígito se o valor da propriedade <xref:System.TimeSpan.Seconds%2A?displayProperty=nameWithType> for de 0 a 9 e retorna um valor de cadeia de caracteres de dois dígitos se o valor da propriedade <xref:System.TimeSpan.Seconds%2A?displayProperty=nameWithType> for de 10 a 59.

Se o especificador de formato personalizado "s" for usado sozinho, especifique "%s" para que ele não seja interpretado incorretamente como uma cadeia de caracteres de formato padrão. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#15](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#15)]
[!code-vb[Conceptual.TimeSpan.Custom#15](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#15)]

Normalmente, em uma operação de análise, uma cadeia de caracteres de entrada que inclui apenas um único número é interpretada como o número de dias. Você pode usar o especificador de formato personalizado "%s" em vez disso para interpretar a cadeia de caracteres numérica como o número de segundos. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#17](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#17)]
[!code-vb[Conceptual.TimeSpan.Custom#17](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#17)]

O exemplo a seguir ilustra o uso do especificador de formato personalizado “s”.

[!code-csharp[Conceptual.TimeSpan.Custom#16](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#16)]
[!code-vb[Conceptual.TimeSpan.Custom#16](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#16)]

[Voltar à tabela](#table)

## <a name="the-ss-custom-format-specifier"></a><a name="ssSpecifier"></a>O especificador de formato personalizado "SS"

O especificador de formato personalizado "ss" fornece o valor da propriedade <xref:System.TimeSpan.Seconds%2A?displayProperty=nameWithType>, que representa o número de segundos inteiros no intervalo de tempo que não são incluídos como parte do componente de horas, dias ou minutos. Para valores de 0 a 9, a cadeia de caracteres de saída inclui um zero à esquerda.

Normalmente, em uma operação de análise, uma cadeia de caracteres de entrada que inclui apenas um único número é interpretada como o número de dias. Você pode usar o especificador de formato personalizado "ss" em vez disso para interpretar a cadeia de caracteres numérica como o número de segundos. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.TimeSpan.Custom#18](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#18)]
[!code-vb[Conceptual.TimeSpan.Custom#18](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#18)]

O exemplo a seguir ilustra o uso do especificador de formato personalizado “ss”.

[!code-csharp[Conceptual.TimeSpan.Custom#19](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#19)]
[!code-vb[Conceptual.TimeSpan.Custom#19](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#19)]

[Voltar à tabela](#table)

## <a name="the-f-custom-format-specifier"></a><a name="fSpecifier"></a>O especificador de formato personalizado "f"

O especificador de formato personalizado "f" gera os décimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a cadeia de caracteres de entrada deve conter exatamente um dígitos fracionários.

Se o especificador de formato personalizado "f" for usado sozinho, especifique "%F" para que ele não seja interpretado incorretamente como uma cadeia de caracteres de formato padrão.

O exemplo a seguir usa o especificador de formato personalizado "F" para exibir os décimos de segundo em um valor de <xref:System.TimeSpan>. "f" é usado primeiro como o especificador de formato único e, depois, combinado com o especificador "s" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[Voltar à tabela](#table)

## <a name="the-ff-custom-format-specifier"></a><a name="ffSpecifier"></a>O especificador de formato personalizado "FF"

O especificador de formato personalizado "ff" gera os centésimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a cadeia de caracteres de entrada deve conter exatamente dois dígitos fracionários.

O exemplo a seguir usa o especificador de formato personalizado "FF" para exibir os centésimos de segundo em um valor de <xref:System.TimeSpan>. "ff" é usado primeiro como o especificador de formato único e, depois, combinado com o especificador "s" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[Voltar à tabela](#table)

## <a name="the-fff-custom-format-specifier"></a><a name="f3Specifier"></a>O especificador de formato personalizado "fff"

O especificador de formato personalizado "fff" (com três caracteres "f") gera os milissegundos em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a cadeia de caracteres de entrada deve conter exatamente três dígitos fracionários.

O exemplo a seguir usa o especificador de formato personalizado "fff" para exibir os milissegundos em um valor de <xref:System.TimeSpan>. "fff" é usado primeiro como o especificador de formato único e, depois, combinado com o especificador "s" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[Voltar à tabela](#table)

## <a name="the-ffff-custom-format-specifier"></a><a name="f4Specifier"></a>O especificador de formato personalizado "ffff"

O especificador de formato personalizado "ffff" (com quatro caracteres "f") gera os décimos de milésimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a cadeia de caracteres de entrada deve conter exatamente quatro dígitos fracionários.

O exemplo a seguir usa o especificador de formato personalizado "ffff" para exibir os décimos de milésimos de segundo em um valor de <xref:System.TimeSpan>. "ffff" é usado primeiro como o especificador de formato único e, depois, combinado com o especificador "s" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[Voltar à tabela](#table)

## <a name="the-fffff-custom-format-specifier"></a><a name="f5Specifier"></a>O especificador de formato personalizado "fffff"

O especificador de formato personalizado "fffff" (com cinco caracteres "f") gera os centésimos de milésimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a cadeia de caracteres de entrada deve conter exatamente cinco dígitos fracionários.

O exemplo a seguir usa o especificador de formato personalizado "fffff" para exibir os centésimos de milésimos de segundo em um valor de <xref:System.TimeSpan>. "fffff" é usado primeiro como o especificador de formato único e, depois, combinado com o especificador "s" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[Voltar à tabela](#table)

## <a name="the-ffffff-custom-format-specifier"></a><a name="f6Specifier"></a>O especificador de formato personalizado "FFFFFF"

O especificador de formato personalizado "ffffff" (com seis caracteres "f") gera os milionésimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a cadeia de caracteres de entrada deve conter exatamente seis dígitos fracionários.

O exemplo a seguir usa o especificador de formato personalizado "ffffff" para exibir os milionésimos de segundo em um valor de <xref:System.TimeSpan>. Ele é usado primeiro como o especificador de formato único e, depois, combinado com o especificador "s" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[Voltar à tabela](#table)

## <a name="the-fffffff-custom-format-specifier"></a><a name="f7Specifier"></a>O especificador de formato personalizado "fffffff"

O especificador de formato personalizado "fffffff" (com sete caracteres "f") gera os décimos de milionésimos de segundo (ou o número fracionário de marcadores) em um intervalo de tempo. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a cadeia de caracteres de entrada deve conter exatamente sete dígitos fracionários.

O exemplo a seguir usa o especificador de formato personalizado "fffffff" para exibir o número fracionário de marcadores em um valor de <xref:System.TimeSpan>. Ele é usado primeiro como o especificador de formato único e, depois, combinado com o especificador "s" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[Voltar à tabela](#table)

## <a name="the-f-custom-format-specifier"></a><a name="F_Specifier"></a>O especificador de formato personalizado "F"

O especificador de formato personalizado "F" gera os décimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Se o valor dos décimos de segundo do intervalo de tempo for zero, ele não será incluído na cadeia de caracteres de resultado. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a presença de décimos de um segundo dígito é opcional.

Se o especificador de formato personalizado "F" for usado sozinho, especifique "%F" para que ele não seja interpretado incorretamente como uma cadeia de caracteres de formato padrão.

O exemplo a seguir usa o especificador de formato personalizado "F" para exibir os décimos de segundo em um valor de <xref:System.TimeSpan>. Ele também usa este especificador de formato personalizado em uma operação de análise.

[!code-csharp[Conceptual.TimeSpan.Custom#21](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#21)]
[!code-vb[Conceptual.TimeSpan.Custom#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#21)]

[Voltar à tabela](#table)

## <a name="the-ff-custom-format-specifier"></a><a name="FF_Specifier"></a>O especificador de formato personalizado "FF"

O especificador de formato personalizado "FF" gera os centésimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Se houver zeros fracionários à direita, eles não serão incluídos na cadeia de caracteres de resultado. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a presença de décimos e centésimos de um segundo dígito é opcional.

O exemplo a seguir usa o especificador de formato personalizado "FF" para exibir os centésimos de segundo em um valor de <xref:System.TimeSpan>. Ele também usa este especificador de formato personalizado em uma operação de análise.

[!code-csharp[Conceptual.TimeSpan.Custom#22](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#22)]
[!code-vb[Conceptual.TimeSpan.Custom#22](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#22)]

[Voltar à tabela](#table)

## <a name="the-fff-custom-format-specifier"></a><a name="F3_Specifier"></a>O especificador de formato personalizado "FFF"

O especificador de formato personalizado "FFF" (com três caracteres "F") gera os milissegundos em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Se houver zeros fracionários à direita, eles não serão incluídos na cadeia de caracteres de resultado. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a presença do dígito de décimos, centésimos e milésimos de segundo é opcional.

O exemplo a seguir usa o especificador de formato personalizado "FFF" para exibir os milésimos de segundo em um valor de <xref:System.TimeSpan>. Ele também usa este especificador de formato personalizado em uma operação de análise.

[!code-csharp[Conceptual.TimeSpan.Custom#23](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#23)]
[!code-vb[Conceptual.TimeSpan.Custom#23](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#23)]

[Voltar à tabela](#table)

## <a name="the-ffff-custom-format-specifier"></a><a name="F4_Specifier"></a>O especificador de formato personalizado "FFFF"

O especificador de formato personalizado "FFFF" (com quatro caracteres "f") gera os décimos de milésimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Se houver zeros fracionários à direita, eles não serão incluídos na cadeia de caracteres de resultado. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a presença do dígito de décimos, centésimos, milésimos e décimos de milésimos de segundo é opcional.

O exemplo a seguir usa o especificador de formato personalizado "FFFF" para exibir os décimos de milésimos de segundo em um valor de <xref:System.TimeSpan>. Ele também usa o especificador de formato personalizado "FFFF" em uma operação de análise.

[!code-csharp[Conceptual.TimeSpan.Custom#24](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#24)]
[!code-vb[Conceptual.TimeSpan.Custom#24](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#24)]

[Voltar à tabela](#table)

## <a name="the-fffff-custom-format-specifier"></a><a name="F5_Specifier"></a>O especificador de formato personalizado "FFFFF"

O especificador de formato personalizado "FFFFF" (com cinco caracteres "F") gera os centésimos de milésimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Se houver zeros fracionários à direita, eles não serão incluídos na cadeia de caracteres de resultado. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a presença do dígito de décimos, centésimos, milésimos e décimos de milésimos e centésimos de milésimos de segundo é opcional.

O exemplo a seguir usa o especificador de formato personalizado "FFFFF" para exibir os centésimos de milésimos de segundo em um valor de <xref:System.TimeSpan>. Ele também usa o especificador de formato personalizado "FFFFF" em uma operação de análise.

[!code-csharp[Conceptual.TimeSpan.Custom#25](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#25)]
[!code-vb[Conceptual.TimeSpan.Custom#25](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#25)]

[Voltar à tabela](#table)

## <a name="the-ffffff-custom-format-specifier"></a><a name="F6_Specifier"></a>O especificador de formato personalizado "FFFFFF"

O especificador de formato personalizado "FFFFFF" (com seis caracteres "F") gera os milionésimos de segundo em um intervalo de tempo. Em uma operação de formatação, os dígitos fracionários restantes são truncados. Se houver zeros fracionários à direita, eles não serão incluídos na cadeia de caracteres de resultado. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a presença do dígito de décimos, centésimos, milésimos, décimos de milésimos, centésimos de milésimos e milionésimos de segundo é opcional.

O exemplo a seguir usa o especificador de formato personalizado "FFFFFF" para exibir os milionésimos de segundo em um valor de <xref:System.TimeSpan>. Ele também usa este especificador de formato personalizado em uma operação de análise.

[!code-csharp[Conceptual.TimeSpan.Custom#26](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#26)]
[!code-vb[Conceptual.TimeSpan.Custom#26](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#26)]

[Voltar à tabela](#table)

## <a name="the-fffffff-custom-format-specifier"></a><a name="F7_Specifier"></a>O especificador de formato personalizado "FFFFFFF"

O especificador de formato personalizado "FFFFFFF" (com sete caracteres "F") gera os décimos de milionésimos de segundo (ou o número fracionário de marcadores) em um intervalo de tempo. Se houver zeros fracionários à direita, eles não serão incluídos na cadeia de caracteres de resultado. Em uma operação de análise que chama o método <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType>, a presença dos sete dígitos fracionários na cadeia de entrada é opcional.

O exemplo a seguir usa o especificador de formato personalizado "FFFFFFF" para exibir as partes fracionárias de um segundo em um valor de <xref:System.TimeSpan>. Ele também usa este especificador de formato personalizado em uma operação de análise.

[!code-csharp[Conceptual.TimeSpan.Custom#27](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#27)]
[!code-vb[Conceptual.TimeSpan.Custom#27](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#27)]

[Voltar à tabela](#table)

## <a name="other-characters"></a>Outros caracteres

Qualquer outro caractere sem escape em uma cadeia de caracteres de formato, incluindo um caractere de espaço em branco, é interpretado como um especificador de formato personalizado. Na maioria dos casos, a presença de qualquer outro caractere sem escape resulta em uma <xref:System.FormatException>.

Há duas maneiras de incluir um caractere literal em uma cadeia de caracteres de formato:

- Coloque-o entre aspas simples (o delimitador de cadeia de caracteres literal).

- Preceda-o com uma barra invertida ("\\"), que é interpretada como um caractere de escape. Isso significa que, em C#, a cadeia de caracteres de formato deve demarcado por @-quoted, ou o caractere literal deve ser precedido por uma barra invertida adicional.

  Em alguns casos, talvez você precise usar lógica condicional para incluir um literal de escape em uma cadeia de caracteres de formato. O exemplo a seguir usa lógica condicional para incluir um símbolo de sinal para intervalos de tempo negativos.

  [!code-csharp[Conceptual.TimeSpan.Custom#29](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/negativevalues1.cs#29)]
  [!code-vb[Conceptual.TimeSpan.Custom#29](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/negativevalues1.vb#29)]

O .NET não define uma gramática para separadores em intervalos de tempo. Isso significa que os separadores entre dias e horas, horas e minutos, minutos e segundos, e segundos e frações de segundo devem ser tratados como literais de caracteres em uma cadeia de caracteres de formato.

O exemplo a seguir usa o caractere de escape e a aspa simples para definir uma cadeia de caracteres de formato personalizado que inclui a palavra "minutos" na cadeia de caracteres de saída.

[!code-csharp[Conceptual.TimeSpan.Custom#28](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/literal1.cs#28)]
[!code-vb[Conceptual.TimeSpan.Custom#28](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/literal1.vb#28)]

[Voltar à tabela](#table)

## <a name="see-also"></a>Veja também

- [Formatar tipos](formatting-types.md)
- [Cadeias de caracteres de formato standard TimeSpan](standard-timespan-format-strings.md)
