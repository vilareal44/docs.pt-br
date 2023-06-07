---
title: Cadeias de caracteres de formato de data e hora personalizado
description: Aprenda a usar cadeias de caracteres de formato de data e hora personalizadas para converter valores DateTime ou DateTimeOffset em representações de texto ou para analisar cadeias de caracteres de datas & horas.
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.topic: reference
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [.NET Framework], dates
- custom DateTime format string
- format specifiers, custom date and time
- format strings
- custom date and time format strings
- formatting [.NET Framework], time
- date and time strings
ms.assetid: 98b374e3-0cc2-4c78-ab44-efb671d71984
ms.openlocfilehash: 48e1b40ddd4bc7fae7d65660adf216756d7c83f7
ms.sourcegitcommit: 2987e241e2f76c9248d2146bf2761a33e2c7a882
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228738"
---
# <a name="custom-date-and-time-format-strings"></a>Cadeias de caracteres de formato de data e hora personalizado

Uma cadeia de caracteres de formato de data e hora define a representação de texto de um valor <xref:System.DateTime> ou <xref:System.DateTimeOffset> que é resultante de uma operação de formatação. Ela também pode definir a representação de um valor de data e hora necessário em uma operação de análise para converter com êxito a cadeia de caracteres para uma data e hora. Uma cadeia de caracteres de formato personalizado consiste em um ou mais especificadores de formato de data e hora personalizado. Qualquer cadeia de caracteres que não é uma [cadeia de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md) é interpretada como uma cadeia de caracteres de formato de data e hora personalizado.

> [!TIP]
> Baixe o **Utilitário de Formatação**, um aplicativo do Windows Forms do .NET Core que permite aplicar cadeias de caracteres de formato a valores numéricos ou de data e hora e exibir a cadeia de caracteres de resultado. O código-fonte está disponível para o [C#](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-cs) e o [Visual Basic](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-vb).

As cadeias de caracteres de formato de data e hora personalizado podem ser usadas tanto com valores <xref:System.DateTime> quanto <xref:System.DateTimeOffset>.

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-with-utc-partial-note.md)]

<a name="table"></a> Em operações de formatação, cadeias de caracteres de formato de data e hora personalizadas podem ser usadas com o `ToString` método de uma instância de data e hora ou com um método que dá suporte à formatação composta. O exemplo a seguir ilustra ambos os usos.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#17](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/custandformatting1.cs#17)]
[!code-vb[Formatting.DateAndTime.Custom#17](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/custandformatting1.vb#17)]

Em operações de análise, as cadeias de caracteres de formato de data e hora personalizado podem ser usadas com os métodos <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType>, <xref:System.DateTime.TryParseExact%2A?displayProperty=nameWithType>, <xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType> e <xref:System.DateTimeOffset.TryParseExact%2A?displayProperty=nameWithType>. Esses métodos exigem que uma cadeia de caracteres de entrada esteja exatamente de acordo com um padrão específico para que a operação de análise obtenha êxito. O exemplo a seguir ilustra uma chamada ao método <xref:System.DateTimeOffset.ParseExact%28System.String%2CSystem.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> para analisar uma data que deve incluir um dia, um mês e um ano com dois dígitos.

[!code-csharp[Formatting.DateAndTime.Custom#18](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/custandparsing1.cs#18)]
[!code-vb[Formatting.DateAndTime.Custom#18](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/custandparsing1.vb#18)]

A tabela a seguir descreve os especificadores de formato de data e hora padrão e exibe uma cadeia de caracteres de resultado produzida por cada especificador de formato. Por padrão, as cadeias de caracteres de resultado refletem as convenções de formatação da cultura en-US. Se um determinado especificador de formato produz uma cadeia de caracteres de resultado localizada, o exemplo também observa a cultura à qual a cadeia de caracteres de resultado se aplica. Para obter informações adicionais sobre como usar cadeias de caracteres de formato data e hora personalizado, confira a seção [Observações](#notes).

| Especificador de formato | Descrição | Exemplos |
|--|--|--|
| "d" | O dia do mês, de 1 a 31.<br /><br /> Mais informações: [Especificador de formato personalizado "d"](#dSpecifier). | 2009-06-01T13:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 15 |
| "dd" | O dia do mês, de 01 a 31.<br /><br /> Mais informações: [Especificador de formato personalizado "dd"](#ddSpecifier). | 2009-06-01T13:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 15 |
| "ddd" | O nome abreviado do dia da semana.<br /><br /> Mais informações: [Especificador de formato personalizado "ddd"](#dddSpecifier). | 2009-06-15T13:45:30 -> Mon (en-US)<br /><br /> 2009-06-15T13:45:30 -> Пн (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> lun. (fr-FR) |
| "dddd" | O nome completo do dia da semana.<br /><br /> Mais informações: [Especificador de formato personalizado "dddd"](#ddddSpecifier). | 2009-06-15T13:45:30 -> Monday (en-US)<br /><br /> 2009-06-15T13:45:30 -> понедельник (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> lundi (fr-FR) |
| "f" | Os décimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [Especificador de formato personalizado "F"](#fSpecifier). | 2009-06-15T13:45:30.6170000 -> 6<br /><br /> 2009-06-15T13:45:30.05 -> 0 |
| "ff" | Os centésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [o especificador de formato personalizado "FF"](#ffSpecifier). | 2009-06-15T13:45:30.6170000 -> 61<br /><br /> 2009-06-15T13:45:30.0050000 -> 00 |
| "fff" | Os milissegundos em um valor de data e hora.<br /><br /> Mais informações: [Especificador de formato personalizado "fff"](#fffSpecifier). | 6/15/2009 13:45:30.617 -> 617<br /><br /> 6/15/2009 13:45:30.0005 -> 000 |
| "ffff" | Os décimos de milésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [o especificador de formato personalizado "ffff"](#ffffSpecifier). | 2009-06-15T13:45:30.6175000 -> 6175<br /><br /> 2009-06-15T13:45:30.0000500  -> 0000 |
| "fffff" | Os centésimos de milésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [Especificador de formato personalizado "fffff"](#fffffSpecifier). | 2009-06-15T13:45:30.6175400 -> 61754<br /><br /> 6/15/2009 13:45:30.000005 -> 00000 |
| "ffffff" | Os milionésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [o especificador de formato personalizado "FFFFFF"](#ffffffSpecifier). | 2009-06-15T13:45:30.6175420 -> 617542<br /><br /> 2009-06-15T13:45:30.0000005 -> 000000 |
| "fffffff" | Os décimos de milionésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [o especificador de formato personalizado "fffffff"](#fffffffSpecifier). | 2009-06-15T13:45:30.6175425 -> 6175425<br /><br /> 2009-06-15T13:45:30.0001150 -> 0001150 |
| "F" | Se diferente de zero, os décimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [o especificador de formato personalizado "F"](#F_Specifier). | 2009-06-15T13:45:30.6170000 -> 6<br /><br /> 2009-06-15T13:45:30.0500000 -> (nenhuma saída) |
| "FF" | Se diferente de zero, os centésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [Especificador de formato personalizado "FF"](#FF_Specifier). | 2009-06-15T13:45:30.6170000 -> 61<br /><br /> 2009-06-15T13:45:30.0050000 -> (nenhuma saída) |
| "FFF" | Se diferente de zero, os milissegundos em um valor de data e hora.<br /><br /> Mais informações: [o especificador de formato personalizado "fff"](#FFF_Specifier). | 2009-06-15T13:45:30.6170000 -> 617<br /><br /> 2009-06-15T13:45:30.0005000 -> (nenhuma saída) |
| "FFFF" | Se diferente de zero, os décimos de milésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [Especificador de formato personalizado "FFFF"](#FFFF_Specifier). | 2009-06-15T13:45:30.5275000 -> 5275<br /><br /> 2009-06-15T13:45:30.0000500 -> (nenhuma saída) |
| "FFFFF" | Se diferente de zero, os centésimos de milésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [o especificador de formato personalizado "fffff"](#FFFFF_Specifier). | 2009-06-15T13:45:30.6175400 -> 61754<br /><br /> 2009-06-15T13:45:30.0000050 -> (nenhuma saída) |
| "FFFFFF" | Se diferente de zero, os milionésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [Especificador de formato personalizado "FFFFFF"](#FFFFFF_Specifier). | 2009-06-15T13:45:30.6175420 -> 617542<br /><br /> 2009-06-15T13:45:30.0000005 -> (nenhuma saída) |
| "FFFFFFF" | Se diferente de zero, os décimos de milionésimos de segundo em um valor de data e hora.<br /><br /> Mais informações: [Especificador de formato personalizado "FFFFFFF"](#FFFFFFF_Specifier). | 2009-06-15T13:45:30.6175425 -> 6175425<br /><br /> 2009-06-15T13:45:30.0001150 -> 000115 |
| "g", "gg" | O período ou a era.<br /><br /> Mais informações: [Especificador de formato personalizado "g" ou "gg"](#gSpecifier). | 2009-06-15T13:45:30.6170000 -> A.D. |
| "h" | A hora, usando um relógio de 12 horas de 1 a 12.<br /><br /> Mais informações: [o especificador de formato personalizado "h"](#hSpecifier). | 2009-06-15T01:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 1 |
| "hh" | A hora, usando um relógio de 12 horas de 01 a 12.<br /><br /> Mais informações: [o especificador de formato personalizado "HH"](#hhSpecifier). | 2009-06-15T01:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 01 |
| "H" | A hora, usando um relógio de 24 horas de 0 a 23.<br /><br /> Mais informações: [Especificador de formato personalizado "H"](#H_Specifier). | 2009-06-15T01:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 13 |
| "HH" | A hora, usando um relógio de 24 horas de 00 a 23.<br /><br /> Mais informações: [Especificador de formato personalizado "HH"](#HH_Specifier). | 2009-06-15T01:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 13 |
| "K" | Informações de fuso horário.<br /><br /> Mais informações: [Especificador de formato personalizado "K"](#KSpecifier). | Com valores de <xref:System.DateTime>:<br /><br /> 2009-06-15T13:45:30, Tipo não especificado -><br /><br /> 2009-06-15T13:45:30, Tipo Utc -> Z<br /><br /> 2009-06-15T13:45:30, Tipo local -> -07:00 (depende das configurações do computador local)<br /><br /> Com valores de <xref:System.DateTimeOffset>:<br /><br /> 2009-06-15T01:45:30-07:00 --> -07:00<br /><br /> 2009-06-15T08:45:30+00:00 --> +00:00 |
| "m" | O minuto, de 0 a 59.<br /><br /> Mais informações: [o especificador de formato personalizado "m"](#mSpecifier). | 2009-06-15T01:09:30 -> 9<br /><br /> 2009-06-15T13:29:30 -> 29 |
| "mm" | O minuto, de 00 a 59.<br /><br /> Mais informações: [Especificador de formato personalizado "MM"](#mmSpecifier). | 2009-06-15T01:09:30 -> 09<br /><br /> 2009-06-15T01:45:30 -> 45 |
| “M” | O mês, de 1 a 12.<br /><br /> Mais informações: [Especificador de formato personalizado "M"](#M_Specifier). | 2009-06-15T13:45:30 -> 6 |
| "MM" | O mês, de 01 a 12.<br /><br /> Mais informações: [o especificador de formato personalizado "mm"](#MM_Specifier). | 2009-06-15T13:45:30 -> 06 |
| "MMM" | O nome do mês abreviado.<br /><br /> Mais informações: [Especificador de formato personalizado "MMM"](#MMM_Specifier). | 2009-06-15T13:45:30 -> Jun (en-US)<br /><br /> 2009-06-15T13:45:30 -> juin (fr-FR)<br /><br /> 2009-06-15T13:45:30 -> Jun (zu-ZA) |
| "MMMM" | O nome completo do mês.<br /><br /> Mais informações: [Especificador de formato personalizado "MMMM"](#MMMM_Specifier). | 2009-06-15T13:45:30 -> June (en-US)<br /><br /> 2009-06-15T13:45:30 -> juni (da-DK)<br /><br /> 2009-06-15T13:45:30 -> uJuni (zu-ZA) |
| "s" | O segundo, de 0 a 59.<br /><br /> Mais informações: [Especificador de formato personalizado "s"](#sSpecifier). | 2009-06-15T13:45:09 -> 9 |
| "ss" | O segundo, de 00 a 59.<br /><br /> Mais informações: [Especificador de formato personalizado "ss"](#ssSpecifier). | 2009-06-15T13:45:09 -> 09 |
| "t" | O primeiro caractere do designador AM/PM.<br /><br /> Mais informações: [Especificador de formato personalizado "t"](#tSpecifier). | 2009-06-15T13:45:30 -> P (en-US)<br /><br /> 2009-06-15T13:45:30 -> 午 (ja-JP)<br /><br /> 2009-06-15T13:45:30 ->  (fr-FR) |
| "tt" | O designador AM/PM.<br /><br /> Mais informações: [Especificador de formato personalizado "tt"](#ttSpecifier). | 2009-06-15T13:45:30 -> PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 午後 (ja-JP)<br /><br /> 2009-06-15T13:45:30 ->  (fr-FR) |
| "y" | O ano, de 0 a 99.<br /><br /> Mais informações: [Especificador de formato personalizado "y"](#ySpecifier). | 0001-01-01T00:00:00 -> 1<br /><br /> 0900-01-01T00:00:00 -> 0<br /><br /> 1900-01-01T00:00:00 -> 0<br /><br /> 2009-06-15T13:45:30 -> 9<br /><br /> 2019-06-15T13:45:30 -> 19 |
| "yy" | O ano, de 00 a 99.<br /><br /> Mais informações: [Especificador de formato personalizado "yy"](#yySpecifier). | 0001-01-01T00:00:00 -> 01<br /><br /> 0900-01-01T00:00:00 -> 00<br /><br /> 1900-01-01T00:00:00 -> 00<br /><br /> 2019-06-15T13:45:30 -> 19 |
| "yyy" | O ano, com um mínimo de três dígitos.<br /><br /> Mais informações: [Especificador de formato personalizado "yyy"](#yyySpecifier). | 0001-01-01T00:00:00 -> 001<br /><br /> 0900-01-01T00:00:00 -> 900<br /><br /> 1900-01-01T00:00:00 -> 1900<br /><br /> 2009-06-15T13:45:30 -> 2009 |
| "yyyy" | O ano como um número de quatro dígitos.<br /><br /> Mais informações: [Especificador de formato personalizado "yyyy"](#yyyySpecifier). | 0001-01-01T00:00:00 -> 0001<br /><br /> 0900-01-01T00:00:00 -> 0900<br /><br /> 1900-01-01T00:00:00 -> 1900<br /><br /> 2009-06-15T13:45:30 -> 2009 |
| "yyyyy" | O ano como um número de cinco dígitos.<br /><br /> Mais informações: [Especificador de formato personalizado "yyyyy"](#yyyyySpecifier). | 0001-01-01T00:00:00 -> 00001<br /><br /> 2009-06-15T13:45:30 -> 02009 |
| "z" | Diferença de horas em relação ao UTC, sem zeros à esquerda.<br /><br /> Mais informações: [Especificador de formato personalizado "z"](#zSpecifier). | 2009-06-15T13:45:30-07:00 -> -7 |
| "zz" | Diferença de horas em relação ao UTC, com um zero à esquerda para um valor de dígito único.<br /><br /> Mais informações: [Especificador de formato personalizado "zz"](#zzSpecifier). | 2009-06-15T13:45:30-07:00 -> -07 |
| "zzz" | Diferença de horas e minutos em relação ao UTC.<br /><br /> Mais informações: [Especificador de formato personalizado "zzz"](#zzzSpecifier). | 2009-06-15T13:45:30-07:00 -> -07:00 |
| ":" | O separador de hora.<br /><br /> Mais informações: [Especificador de formato personalizado ":"](#timeSeparator). | 2009-06-15T13:45:30 -> : (en-US)<br /><br /> 2009-06-15T13:45:30 -> . (it-IT)<br /><br /> 2009-06-15T13:45:30 -> : (ja-JP) |
| "/" | O separador de data.<br /><br /> Mais informações: [o especificador de formato personalizado "/"](#dateSeparator). | 2009-06-15T13:45:30 -> / (en-US)<br /><br /> 2009-06-15T13:45:30 -> - (ar-DZ)<br /><br /> 2009-06-15T13:45:30 -> . (tr-TR) |
| "*String*"<br /><br /> '*cadeia de caracteres*' | Delimitador de cadeia de caracteres literal.<br /><br /> Para saber mais: [Literais de cadeia de caracteres](#Literals). | 2009-06-15T13:45:30 ("arr:" h:m t) -> arr: 1:45 P<br /><br /> 2009-06-15T13:45:30 ('arr:' h:m t) -> arr: 1:45 P |
| % | Define o caractere seguinte como um especificador de formato personalizado.<br /><br /> Mais informações:[Usar especificadores de formato personalizado simples](#UsingSingleSpecifiers). | 2009-06-15T13:45:30 (%h) -> 1 |
| &#92; | O caractere de escape.<br /><br /> Para saber mais: [Literais de cadeia de caracteres](#Literals) e [Como usar o caractere de escape](#escape). | 2009-06-15T13:45:30 (h \h) -> 1 h |
| Qualquer outro caractere | O caractere é copiado, inalterado, para a cadeia de caracteres de resultado.<br /><br /> Para saber mais: [Literais de cadeia de caracteres](#Literals). | 2009-06-15T01:45:30 (arr hh:mm t) -> arr 01:45 A |

As seções a seguir oferecem informações adicionais sobre cada especificador de formato de data e hora personalizado. A menos que observado do contrário, cada especificador produz uma representação de cadeia de caracteres idêntica independente de ela ser usada com um valor <xref:System.DateTime> ou um valor <xref:System.DateTimeOffset>.

## <a name="day-d-format-specifier"></a>Especificador de formato de dia "d"

### <a name="the-d-custom-format-specifier"></a><a name="dSpecifier"></a> O especificador de formato personalizado "d"

O especificador de formato personalizado "d" representa o dia do mês como um número de 1 a 31. Dias de dígito único são formatados sem um zero à esquerda.

Se o especificador de formato "d" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão "d". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "d" em várias cadeias de caracteres de formato.

[!code-csharp[Formatting.DateAndTime.Custom#1](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#1)]
[!code-vb[Formatting.DateAndTime.Custom#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#1)]

[Voltar à tabela](#table)

### <a name="the-dd-custom-format-specifier"></a><a name="ddSpecifier"></a> O especificador de formato personalizado "dd"

A cadeia de caracteres de formato personalizado "dd" representa o dia do mês como um número de 01 a 31. Dias de dígito único são formatados com um zero à esquerda.

O exemplo a seguir inclui o especificador de formato personalizado "dd" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#2](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#2)]
[!code-vb[Formatting.DateAndTime.Custom#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#2)]

[Voltar à tabela](#table)

### <a name="the-ddd-custom-format-specifier"></a><a name="dddSpecifier"></a> O especificador de formato personalizado "ddd"

O especificador de formato personalizado "ddd" representa o nome do dia da semana abreviado. O nome do dia da semana localizado abreviado é recuperado da propriedade <xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A?displayProperty=nameWithType> da cultura atual ou especificada.

O exemplo a seguir inclui o especificador de formato personalizado "ddd" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#3](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#3)]
[!code-vb[Formatting.DateAndTime.Custom#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#3)]

[Voltar à tabela](#table)

### <a name="the-dddd-custom-format-specifier"></a><a name="ddddSpecifier"></a> O especificador de formato personalizado "dddd"

O especificador de formato personalizado "dddd" (mais um número qualquer de especificadores "d" adicionais) representa o nome completo do dia da semana. O nome do dia da semana localizado é recuperado da propriedade <xref:System.Globalization.DateTimeFormatInfo.DayNames%2A?displayProperty=nameWithType> da cultura atual ou especificada.

O exemplo a seguir inclui o especificador de formato personalizado "dddd" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#4](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#4)]
[!code-vb[Formatting.DateAndTime.Custom#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#4)]

[Voltar à tabela](#table)

## <a name="lowercase-seconds-f-fraction-specifier"></a>Especificador de fração de segundos em minúsculas "f"

### <a name="the-f-custom-format-specifier"></a><a name="fSpecifier"></a> O especificador de formato personalizado "f"

O especificador de formato personalizado "f" representa o dígito mais significativo da fração de segundos, ou seja, representa os décimos de segundo em um valor de data e hora.

Se o especificador de formato "f" for usado sem outros especificadores de formato, ele será interpretado como o especificador padrão de formato de data e hora "f". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

Quando você usa especificadores de formato "f" como parte de uma cadeia de caracteres de formato fornecida para o método <xref:System.DateTime.ParseExact%2A>, <xref:System.DateTime.TryParseExact%2A>, <xref:System.DateTimeOffset.ParseExact%2A> ou <xref:System.DateTimeOffset.TryParseExact%2A>, o número de especificadores de formato "f" indica o número de dígitos mais significativos da fração de segundos que deve estar presente para analisar a cadeia de caracteres com sucesso.

O exemplo a seguir inclui o especificador de formato personalizado "f" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[Voltar à tabela](#table)

### <a name="the-ff-custom-format-specifier"></a><a name="ffSpecifier"></a> O especificador de formato personalizado "FF"

O especificador de formato personalizado "ff" representa os dois dígitos mais significativos da fração de segundos, ou seja, ele representa os centésimos de segundo em um valor de data e hora.

O exemplo a seguir inclui o especificador de formato personalizado "ff" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[Voltar à tabela](#table)

### <a name="the-fff-custom-format-specifier"></a><a name="fffSpecifier"></a> O especificador de formato personalizado "fff"

O especificador de formato personalizado "fff" representa os três dígitos mais significativos da fração de segundos, ou seja, ele representa os milissegundos em um valor de data e hora.

O exemplo a seguir inclui o especificador de formato personalizado "fff" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[Voltar à tabela](#table)

### <a name="the-ffff-custom-format-specifier"></a><a name="ffffSpecifier"></a> O especificador de formato personalizado "ffff"

O especificador de formato personalizado "ffff" representa os quatro dígitos mais significativos da fração de segundos, ou seja, ele representa os décimos de milésimos de um segundo em um valor de data e hora.

Embora seja possível exibir os décimos de milésimos de um componente de segundos de um valor temporal, esse valor pode não ser significativo. A precisão dos valores de data e hora depende da resolução do relação ao relógio do sistema. Nos sistemas operacionais Windows NT versão 3.5 (e posterior) e Windows Vista, a resolução do relógio é de aproximadamente 10 a 15 milissegundos.

[Voltar à tabela](#table)

### <a name="the-fffff-custom-format-specifier"></a><a name="fffffSpecifier"></a> O especificador de formato personalizado "fffff"

O especificador de formato personalizado "fffff" representa os cinco dígitos mais significativos da fração de segundos, ou seja, ele representa os centésimos de milésimos de um segundo em um valor de data e hora.

Embora seja possível exibir os centésimos de milésimos de um componente de segundos de um valor temporal, esse valor pode não ser significativo. A precisão dos valores de data e hora depende da resolução do relação ao relógio do sistema. Nos sistemas operacionais Windows NT 3.5 (e posterior) e Windows Vista, a resolução do relógio é de aproximadamente 10 a 15 milissegundos.

[Voltar à tabela](#table)

### <a name="the-ffffff-custom-format-specifier"></a><a name="ffffffSpecifier"></a> O especificador de formato personalizado "FFFFFF"

O especificador de formato personalizado "ffffff" representa os seis dígitos mais significativos da fração de segundos, ou seja, ele representa os milionésimos de um segundo em um valor de data e hora.

Embora seja possível exibir os milionésimos de um componente de segundos de um valor temporal, esse valor pode não ser significativo. A precisão dos valores de data e hora depende da resolução do relação ao relógio do sistema. Nos sistemas operacionais Windows NT 3.5 (e posterior) e Windows Vista, a resolução do relógio é de aproximadamente 10 a 15 milissegundos.

[Voltar à tabela](#table)

### <a name="the-fffffff-custom-format-specifier"></a><a name="fffffffSpecifier"></a> O especificador de formato personalizado "fffffff"

O especificador de formato personalizado "fffffff" representa os sete dígitos mais significativos da fração de segundos; ou seja, representa os décimos de milionésimos de segundo em um valor de data e hora.

Embora seja possível exibir os décimos de milionésimos de um componente de segundos de um valor temporal, esse valor pode não ser significativo. A precisão dos valores de data e hora depende da resolução do relação ao relógio do sistema. Nos sistemas operacionais Windows NT 3.5 (e posterior) e Windows Vista, a resolução do relógio é de aproximadamente 10 a 15 milissegundos.

[Voltar à tabela](#table)

## <a name="uppercase-seconds-f-fraction-specifier"></a>Especificador de fração "F" de segundos maiúsculos

### <a name="the-f-custom-format-specifier"></a><a name="F_Specifier"></a> O especificador de formato personalizado "F"

O especificador de formato personalizado "F" representa o dígito mais significativo da fração de segundos, ou seja, representa os décimos de segundo em um valor de data e hora. Nada será exibido se o dígito for zero.

Se o especificador de formato "F" for usado sem outros especificadores de formato, ele será interpretado como o especificador padrão de formato de data e hora "F". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O número de especificadores de formato "F" usados com o método <xref:System.DateTime.ParseExact%2A>, <xref:System.DateTime.TryParseExact%2A>, <xref:System.DateTimeOffset.ParseExact%2A> ou <xref:System.DateTimeOffset.TryParseExact%2A> indica o número máximo de dígitos significativos da fração de segundos que podem estar presentes para que a análise da cadeia de caracteres seja feita com êxito.

O exemplo a seguir inclui o especificador de formato personalizado "F" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[Voltar à tabela](#table)

### <a name="the-ff-custom-format-specifier"></a><a name="FF_Specifier"></a> O especificador de formato personalizado "FF"

O especificador de formato personalizado "FF" representa os dois dígitos mais significativos da fração de segundos, ou seja, ele representa os centésimos de segundo em um valor de data e hora. No entanto, zeros à direita ou dois dígitos zero não são exibidos.

O exemplo a seguir inclui o especificador de formato personalizado "FF" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[Voltar à tabela](#table)

### <a name="the-fff-custom-format-specifier"></a><a name="FFF_Specifier"></a> O especificador de formato personalizado "FFF"

O especificador de formato personalizado "FFF" representa os três dígitos mais significativos da fração de segundos, ou seja, ele representa os milissegundos em um valor de data e hora. No entanto, zeros à direita ou três dígitos zero não são exibidos.

O exemplo a seguir inclui o especificador de formato personalizado "FFF" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[Voltar à tabela](#table)

### <a name="the-ffff-custom-format-specifier"></a><a name="FFFF_Specifier"></a> O especificador de formato personalizado "FFFF"

O especificador de formato personalizado "FFFF" representa os quatro dígitos mais significativos da fração de segundos, ou seja, ele representa os décimos de milésimos de um segundo em um valor de data e hora. No entanto, zeros à direita ou quatro dígitos zero não são exibidos.

Embora seja possível exibir os décimos de milésimos de um componente de segundos de um valor temporal, esse valor pode não ser significativo. A precisão dos valores de data e hora depende da resolução do relação ao relógio do sistema. Nos sistemas operacionais Windows NT 3.5 (e posterior) e Windows Vista, a resolução do relógio é de aproximadamente 10 a 15 milissegundos.

[Voltar à tabela](#table)

### <a name="the-fffff-custom-format-specifier"></a><a name="FFFFF_Specifier"></a> O especificador de formato personalizado "FFFFF"

O especificador de formato personalizado "FFFFF" representa os cinco dígitos mais significativos da fração de segundos, ou seja, ele representa os centésimos de milésimos de um segundo em um valor de data e hora. No entanto, zeros à direita ou cinco dígitos zero não são exibidos.

Embora seja possível exibir os centésimos de milésimos de um componente de segundos de um valor temporal, esse valor pode não ser significativo. A precisão dos valores de data e hora depende da resolução do relação ao relógio do sistema. Nos sistemas operacionais Windows NT 3.5 (e posterior) e Windows Vista, a resolução do relógio é de aproximadamente 10 a 15 milissegundos.

[Voltar à tabela](#table)

### <a name="the-ffffff-custom-format-specifier"></a><a name="FFFFFF_Specifier"></a> O especificador de formato personalizado "FFFFFF"

O especificador de formato personalizado "FFFFFF" representa os seis dígitos mais significativos da fração de segundos, ou seja, ele representa os milionésimos de um segundo em um valor de data e hora. No entanto, zeros à direita ou seis dígitos zero não são exibidos.

Embora seja possível exibir os milionésimos de um componente de segundos de um valor temporal, esse valor pode não ser significativo. A precisão dos valores de data e hora depende da resolução do relação ao relógio do sistema. Nos sistemas operacionais Windows NT 3.5 (e posterior) e Windows Vista, a resolução do relógio é de aproximadamente 10 a 15 milissegundos.

[Voltar à tabela](#table)

### <a name="the-fffffff-custom-format-specifier"></a><a name="FFFFFFF_Specifier"></a> O especificador de formato personalizado "FFFFFFF"

O especificador de formato personalizado "FFFFFFF" representa os sete dígitos mais significativos da fração de segundos; ou seja, representa os décimos de milionésimos de segundo em um valor de data e hora. No entanto, zeros à direita ou sete dígitos zero não são exibidos.

Embora seja possível exibir os décimos de milionésimos de um componente de segundos de um valor temporal, esse valor pode não ser significativo. A precisão dos valores de data e hora depende da resolução do relação ao relógio do sistema. Nos sistemas operacionais Windows NT 3.5 (e posterior) e Windows Vista, a resolução do relógio é de aproximadamente 10 a 15 milissegundos.

[Voltar à tabela](#table)

## <a name="era-g-format-specifier"></a>Especificador de formato "g" de era

### <a name="the-g-or-gg-custom-format-specifier"></a><a name="gSpecifier"></a> O especificador de formato personalizado "g" ou "GG"

Os especificadores de formato personalizado "g" ou "gg" (mais qualquer número de especificadores "g" adicionais) representam o período ou a era, como A.D. A operação de formatação ignora esse especificador quando a data a ser formatada não tem uma cadeia de caracteres de era ou período associada.

Se o especificador de formato "g" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão "g". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "g" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#6](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#6)]
[!code-vb[Formatting.DateAndTime.Custom#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#6)]

[Voltar à tabela](#table)

## <a name="lowercase-hour-h-format-specifier"></a>Especificador de formato de hora minúscula "h"

### <a name="the-h-custom-format-specifier"></a><a name="hSpecifier"></a> O especificador de formato personalizado "h"

O especificador de formato personalizado "h" representa a hora como um número de 1 a 12, ou seja, a hora é representada por um relógio de 12 horas que conta todas as horas desde a meia-noite. Uma hora específica após a meia-noite é indistinguível da mesma hora depois do meio-dia. A hora não é arredondada e uma hora de dígito único é formatada sem um zero à esquerda. Por exemplo, considerando a hora 5:43 da manhã ou da tarde, este especificador de formato personalizado exibe “5".

Se o especificador de formato "h" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão e gerará uma <xref:System.FormatException>. Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "h" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[Voltar à tabela](#table)

### <a name="the-hh-custom-format-specifier"></a><a name="hhSpecifier"></a> O especificador de formato personalizado "HH"

O especificador de formato personalizado "hh" (mais qualquer número de especificadores "h" adicionais) representa a hora como um número de 01 a 12, ou seja, a hora é representada por um relógio de 12 horas que conta todas as horas desde a meia-noite ou o meio-dia. Uma hora específica após a meia-noite é indistinguível da mesma hora depois do meio-dia. A hora não é arredondada e uma hora de dígito único é formatada com um zero à esquerda. Por exemplo, considerando a hora 5:43 da manhã ou da tarde, este especificador de formato exibe “05".

O exemplo a seguir inclui o especificador de formato personalizado "hh" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[Voltar à tabela](#table)

## <a name="uppercase-hour-h-format-specifier"></a>Especificador de formato de hora em maiúsculas "H"

### <a name="the-h-custom-format-specifier"></a><a name="H_Specifier"></a> O especificador de formato personalizado "H"

O especificador de formato personalizado "H" representa a hora como um número de 0 a 23; ou seja, a hora é representada por um relógio de 24 horas baseado em zero que conta todas as horas desde a meia-noite. Uma hora de dígito único é formatada sem um zero à esquerda.

Se o especificador de formato "H" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão e gerará uma <xref:System.FormatException>. Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "H" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#9](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#9)]
[!code-vb[Formatting.DateAndTime.Custom#9](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#9)]

[Voltar à tabela](#table)

### <a name="the-hh-custom-format-specifier"></a><a name="HH_Specifier"></a> O especificador de formato personalizado "HH"

O especificador de formato personalizado "HH" (mais qualquer número de especificadores "H" adicionais) representa a hora como um número de 00 a 23; ou seja, a hora é representada por um relógio de 24 horas baseado em zero que conta todas as horas desde a meia-noite. Uma hora de dígito único é formatada com um zero à esquerda.

O exemplo a seguir inclui o especificador de formato personalizado "HH" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#10](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#10)]
[!code-vb[Formatting.DateAndTime.Custom#10](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#10)]

[Voltar à tabela](#table)

## <a name="time-zone-k-format-specifier"></a>Especificador de formato de fuso horário "K"

### <a name="the-k-custom-format-specifier"></a><a name="KSpecifier"></a> O especificador de formato personalizado "K"

O especificador de formato personalizado "K" representa as informações de fuso horário de um valor temporal. Quando esse especificador de formato é usado com valores <xref:System.DateTime>, a cadeia de caracteres de resultado é definida pelo valor da propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType>:

- Para o fuso horário local (um valor da propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> de <xref:System.DateTimeKind.Local?displayProperty=nameWithType>), esse especificador é equivalente ao especificador "zzz" e produz uma cadeia de caracteres resultante que contém a diferença local em relação ao Horário Universal Coordenado (UTC). Por exemplo, "-07h00".

- Para uma hora UTC (um valor da propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> de <xref:System.DateTimeKind.Utc?displayProperty=nameWithType>), a cadeia de caracteres de resultado inclui um caractere "Z" para representar uma data UTC.

- Para um horário de um fuso horário não especificado (um horário cuja propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> é igual a <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>), o resultado é equivalente a <xref:System.String.Empty?displayProperty=nameWithType>.

Para valores <xref:System.DateTimeOffset>, o especificador de formato "K" é equivalente ao especificador de formato "zzz" e produz uma cadeia de caracteres resultante que contém a diferença em relação ao valor de <xref:System.DateTimeOffset> do UTC.

Se o especificador de formato "K" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão e gerará uma <xref:System.FormatException>. Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir exibe a cadeia de caracteres resultante do uso do especificador de formato personalizado "K" com vários valores <xref:System.DateTime> e <xref:System.DateTimeOffset> em um sistema no fuso horário padrão do Pacífico dos EUA.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#12](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#12)]
[!code-vb[Formatting.DateAndTime.Custom#12](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#12)]

[Voltar à tabela](#table)

## <a name="minute-m-format-specifier"></a>Especificador de formato "m" de minuto

### <a name="the-m-custom-format-specifier"></a><a name="mSpecifier"></a> O especificador de formato personalizado "m"

O especificador de formato personalizado "m" representa o minuto como um número de 0 a 59. O minuto representa os minutos inteiros decorridos desde a última hora. Um minuto de dígito único é formatado sem um zero à esquerda.

Se o especificador de formato "m" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão "m". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "m" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[Voltar à tabela](#table)

### <a name="the-mm-custom-format-specifier"></a><a name="mmSpecifier"></a> O especificador de formato personalizado "mm"

O especificador de formato personalizado "mm" (mais qualquer número de especificadores "m" adicionais) representam o minuto como um número de 00 a 59. O minuto representa os minutos inteiros decorridos desde a última hora. Um minuto de dígito único é formatado com um zero à esquerda.

O exemplo a seguir inclui o especificador de formato personalizado "mm" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[Voltar à tabela](#table)

## <a name="month-m-format-specifier"></a>Especificador de formato de mês "M"

### <a name="the-m-custom-format-specifier"></a><a name="M_Specifier"></a> O especificador de formato personalizado "M"

O especificador de formato personalizado "M" representa o mês como um número de 1 a 12 (ou de 1 a 13 para os calendários com 13 meses). Um mês de dígito único é formatado sem um zero à esquerda.

Se o especificador de formato "M" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão "M". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "M" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#11](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#11)]
[!code-vb[Formatting.DateAndTime.Custom#11](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#11)]

[Voltar à tabela](#table)

### <a name="the-mm-custom-format-specifier"></a><a name="MM_Specifier"></a> O especificador de formato personalizado "MM"

O especificador de formato personalizado "MM" representa o mês como um número de 01 a 12 (ou de 1 a 13 para os calendários com 13 meses). Um mês de dígito único é formatado com um zero à esquerda.

O exemplo a seguir inclui o especificador de formato personalizado "MM" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#2](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#2)]
[!code-vb[Formatting.DateAndTime.Custom#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#2)]

[Voltar à tabela](#table)

### <a name="the-mmm-custom-format-specifier"></a><a name="MMM_Specifier"></a> O especificador de formato personalizado "MMM"

O especificador de formato personalizado "MMM" representa o nome do mês abreviado. O nome do dia do mês localizado abreviado é recuperado da propriedade <xref:System.Globalization.DateTimeFormatInfo.AbbreviatedMonthNames%2A?displayProperty=nameWithType> da cultura atual ou especificada.

O exemplo a seguir inclui o especificador de formato personalizado "MMM" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#3](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#3)]
[!code-vb[Formatting.DateAndTime.Custom#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#3)]

[Voltar à tabela](#table)

### <a name="the-mmmm-custom-format-specifier"></a><a name="MMMM_Specifier"></a> O especificador de formato personalizado "MMMM"

O especificador de formato personalizado "MMMM" representa o nome do mês completo. O nome do mês localizado é recuperado da propriedade <xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A?displayProperty=nameWithType> da cultura atual ou especificada.

O exemplo a seguir inclui o especificador de formato personalizado "MMMM" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#4](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#4)]
[!code-vb[Formatting.DateAndTime.Custom#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#4)]

[Voltar à tabela](#table)

## <a name="seconds-s-format-specifier"></a>Especificador de formato de segundos "s"

### <a name="the-s-custom-format-specifier"></a><a name="sSpecifier"></a> O especificador de formato personalizado "s"

O especificador de formato personalizado "s" representa os segundos como um número de 0 a 59. O resultado representa os segundos inteiros decorridos desde o último minuto. Um segundo de dígito único é formatado sem um zero à esquerda.

Se o especificador de formato "s" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão "s". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "s" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[Voltar à tabela](#table)

### <a name="the-ss-custom-format-specifier"></a><a name="ssSpecifier"></a> O especificador de formato personalizado "SS"

O especificador de formato personalizado "ss" (mais qualquer número de especificadores "s" adicionais) representa os segundos como um número de 00 a 59. O resultado representa os segundos inteiros decorridos desde o último minuto. Um segundo de dígito único é formatado com um zero à esquerda.

O exemplo a seguir inclui o especificador de formato personalizado "ss" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[Voltar à tabela](#table)

## <a name="meridiem-t-format-specifier"></a>Especificador de formato meridiem "t"

### <a name="the-t-custom-format-specifier"></a><a name="tSpecifier"></a> O especificador de formato personalizado "t"

O especificador de formato personalizado "t" representa o primeiro caractere do designador AM/PM. O designador localizado apropriado é recuperado da propriedade <xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A?displayProperty=nameWithType> ou <xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A?displayProperty=nameWithType> da cultura atual ou específica. O designador AM é usado para todas as horas de 0:00:00 (meia-noite) até 11:59:59,999. O designador PM é usado para todas as horas de 12:00:00 (meio-dia) até 23:59:59,999.

Se o especificador de formato "t" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão "t". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "t" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[Voltar à tabela](#table)

### <a name="the-tt-custom-format-specifier"></a><a name="ttSpecifier"></a> O especificador de formato personalizado "tt"

O especificador de formato personalizado "tt" (mais qualquer número de especificadores "t" adicionais) representa o designador AM/PM inteiro. O designador localizado apropriado é recuperado da propriedade <xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A?displayProperty=nameWithType> ou <xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A?displayProperty=nameWithType> da cultura atual ou específica. O designador AM é usado para todas as horas de 0:00:00 (meia-noite) até 11:59:59,999. O designador PM é usado para todas as horas de 12:00:00 (meio-dia) até 23:59:59,999.

Certifique-se de usar o especificador "tt" para linguagens para as quais é necessário manter a distinção entre AM e PM. Um exemplo é o idioma japonês, no qual os designadores AM e PM diferem no segundo caractere e não no primeiro.

O exemplo a seguir inclui o especificador de formato personalizado "tt" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[Voltar à tabela](#table)

## <a name="year-y-format-specifier"></a>Especificador de formato de ano "y"

### <a name="the-y-custom-format-specifier"></a><a name="ySpecifier"></a> O especificador de formato personalizado "y"

O especificador de formato personalizado "y" representa o ano como um número de um dígito ou de dois dígitos. Se o ano tiver mais que dois dígitos, somente os dois dígitos de ordem baixa aparecerão no resultado. Se o primeiro dígito de um ano de dois dígitos começa com zero (por exemplo, 2008), o número é formatado sem um zero à esquerda.

Se o especificador de formato "y" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão "y". Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "y" em uma cadeia de caracteres de formato personalizado.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[Voltar à tabela](#table)

### <a name="the-yy-custom-format-specifier"></a><a name="yySpecifier"></a> O especificador de formato personalizado "AA"

O especificador de formato personalizado "yy" representa o ano como um número de dois dígitos. Se o ano tiver mais que dois dígitos, somente os dois dígitos de ordem baixa aparecerão no resultado. Se o ano de dois dígitos tiver menos de dois dígitos significativos, o número será preenchido com zeros à esquerda para produzir dois dígitos.

Em uma operação de análise, um ano de dois dígitos é analisado usando o especificador de formato personalizado “yy” interpretado com base na propriedade <xref:System.Globalization.Calendar.TwoDigitYearMax%2A?displayProperty=nameWithType> do calendário atual do provedor de formato. O exemplo a seguir analisa a representação de cadeia de caracteres de uma data com um ano de dois dígitos usando o calendário gregoriano padrão da cultura en-US que, neste caso, é a cultura atual. Ele então altera o objeto <xref:System.Globalization.CultureInfo> da cultura atual para usar um objeto <xref:System.Globalization.GregorianCalendar> cuja propriedade <xref:System.Globalization.GregorianCalendar.TwoDigitYearMax%2A> foi modificada.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#19](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/parseexact2digityear1.cs#19)]
[!code-vb[Formatting.DateAndTime.Custom#19](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/parseexact2digityear1.vb#19)]

O exemplo a seguir inclui o especificador de formato personalizado "yy" em uma cadeia de caracteres de formato personalizado.

[!code-csharp[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[Voltar à tabela](#table)

### <a name="the-yyy-custom-format-specifier"></a><a name="yyySpecifier"></a> O especificador de formato personalizado "aaa"

O especificador de formato personalizado "yyy" representa o ano com, no mínimo, três dígitos. Se o ano tem mais de três dígitos significativos, eles são incluídos na cadeia de caracteres de resultado. Se o ano tem menos de três dígitos, o número é preenchido com zeros à esquerda para produzir três dígitos.

> [!NOTE]
> Para o calendário budista tailandês, que pode ter anos com cinco dígitos, este especificador de formato exibe todos os dígitos significativos.

O exemplo a seguir inclui o especificador de formato personalizado "yyy" em uma cadeia de caracteres de formato personalizado.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[Voltar à tabela](#table)

### <a name="the-yyyy-custom-format-specifier"></a><a name="yyyySpecifier"></a> O especificador de formato personalizado "aaaa"

O especificador de formato personalizado "yyyy" representa o ano com, no mínimo, quatro dígitos. Se o ano tem mais de quatro dígitos significativos, eles são incluídos na cadeia de caracteres de resultado. Se o ano possui menos de quatro dígitos, o número é preenchido com zeros à esquerda para produzir quatro dígitos.

> [!NOTE]
> Para o calendário budista tailandês, que pode ter anos de cinco dígitos, este especificador de formato exibe no mínimo quatro dígitos.

O exemplo a seguir inclui o especificador de formato personalizado "yyyy" em uma cadeia de caracteres de formato personalizado.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[Voltar à tabela](#table)

### <a name="the-yyyyy-custom-format-specifier"></a><a name="yyyyySpecifier"></a> O especificador de formato personalizado "yyyyy"

O especificador de formato personalizado "yyyyy" (mais qualquer número de especificadores "y" adicionais) representa o ano com, no mínimo, cinco dígitos. Se o ano tem mais de cinco dígitos significativos, eles são incluídos na cadeia de caracteres de resultado. Se o ano tem menos de cinco dígitos, o número é preenchido com zeros à esquerda para produzir cinco dígitos.

Se houver especificadores "y" adicionais, o número será preenchido com tantos zeros à esquerda quantos forem necessários para produzir o número de especificadores "y".

O exemplo a seguir inclui o especificador de formato personalizado "yyyyy" em uma cadeia de caracteres de formato personalizado.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[Voltar à tabela](#table)

## <a name="offset-z-format-specifier"></a>"Z" especificador de formato de deslocamento

### <a name="the-z-custom-format-specifier"></a><a name="zSpecifier"></a> O especificador de formato personalizado "z"

Com valores <xref:System.DateTime>, o especificador de formato personalizado "z" representa a diferença com sinal do fuso horário do sistema operacional local em relação ao UTC, medido em horas. Ele não reflete o valor de uma instância da propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType>. Por esse motivo, o especificador de formato "z" não é recomendado para uso com valores <xref:System.DateTime>.

Com os valores de <xref:System.DateTimeOffset>, este formato representa a diferença do valor de <xref:System.DateTimeOffset> em relação ao UTC em horas.

A diferença é sempre exibida com um sinal à esquerda. Um sinal de adição (+) indica horas depois do UTC, enquanto que um sinal de subtração (-) indica horas antes do UTC. Uma diferença com um único dígito único é formatada sem um zero à esquerda.

Se o especificador de formato "z" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão e gerará uma <xref:System.FormatException>. Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

O exemplo a seguir inclui o especificador de formato personalizado "z" em uma cadeia de caracteres de formato personalizado.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[Voltar à tabela](#table)

### <a name="the-zz-custom-format-specifier"></a><a name="zzSpecifier"></a> O especificador de formato personalizado "zz"

Com valores <xref:System.DateTime>, o especificador de formato personalizado "zz" representa a diferença com sinal do fuso horário do sistema operacional local em relação ao UTC em horas. Ele não reflete o valor de uma instância da propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType>. Por esse motivo, o especificador de formato "zz" não é recomendado para uso com valores <xref:System.DateTime>.

Com os valores de <xref:System.DateTimeOffset>, este formato representa a diferença do valor de <xref:System.DateTimeOffset> em relação ao UTC em horas.

A diferença é sempre exibida com um sinal à esquerda. Um sinal de adição (+) indica horas depois do UTC, enquanto que um sinal de subtração (-) indica horas antes do UTC. Uma diferença com um único dígito único é formatada com um zero à esquerda.

O exemplo a seguir inclui o especificador de formato personalizado "zz" em uma cadeia de caracteres de formato personalizado.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[Voltar à tabela](#table)

### <a name="the-zzz-custom-format-specifier"></a><a name="zzzSpecifier"></a> O especificador de formato personalizado "ZZZ"

Com valores <xref:System.DateTime>, o especificador de formato personalizado "zzz" representa a diferença com sinal do fuso horário do sistema operacional local em relação ao UTC em horas e minutos. Ele não reflete o valor de uma instância da propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType>. Por esse motivo, o especificador de formato "zzz" não é recomendado para uso com valores <xref:System.DateTime>.

Com valores <xref:System.DateTimeOffset>, esse especificador de formato representa a diferença do valor de <xref:System.DateTimeOffset> em relação ao UTC em horas e minutos.

A diferença é sempre exibida com um sinal à esquerda. Um sinal de adição (+) indica horas depois do UTC, enquanto que um sinal de subtração (-) indica horas antes do UTC. Uma diferença com um único dígito único é formatada com um zero à esquerda.

O exemplo a seguir inclui o especificador de formato personalizado "zzz" em uma cadeia de caracteres de formato personalizado.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[Voltar à tabela](#table)

## <a name="date-and-time-separator-specifiers"></a>Especificadores de separadores de data e hora

### <a name="the--custom-format-specifier"></a><a name="timeSeparator"></a> O especificador de formato personalizado ":"
O especificador de formato personalizado ":" representa o separador de hora, o qual é usado para diferenciar horas, minutos e segundos. O separador de hora localizado apropriado é recuperado da propriedade <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A?displayProperty=nameWithType> da cultura atual ou especificada.

> [!NOTE]
> Para alterar o separador de hora de uma sequência de data e hora específica, especifique o caractere separador dentro de um delimitador de cadeia de caracteres literal. Por exemplo, a cadeia de caracteres de formato personalizada `hh'_'dd'_'ss` produz uma cadeia de caracteres de resultado em que "\_" (um sublinhado) é sempre usado como o separador de hora. Para alterar o separador de hora de todas as datas de uma cultura, seja para alterar o valor da propriedade <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A?displayProperty=nameWithType> da cultura atual ou para instanciar um objeto <xref:System.Globalization.DateTimeFormatInfo>, atribua o caractere à sua propriedade <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A> e chame uma sobrecarga do método de formatação que inclua um parâmetro <xref:System.IFormatProvider>.

Se o especificador de formato ":" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão e gerará uma <xref:System.FormatException>. Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

[Voltar à tabela](#table)

### <a name="the--custom-format-specifier"></a><a name="dateSeparator"></a> O especificador de formato personalizado "/"

O especificador de formato personalizado "/" representa o separador de data, o qual é usado para diferenciar anos, meses e dias. O separador de data localizado apropriado é recuperado da propriedade <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A?displayProperty=nameWithType> da cultura atual ou especificada.

> [!NOTE]
> Para alterar o separador de data de uma sequência de data e hora específica, especifique o caractere separador dentro de um delimitador de cadeia de caracteres literal. Por exemplo, a sequência de formato personalizado `mm'/'dd'/'yyyy` produz uma cadeia de caracteres de resultado em que "/" é sempre usado como o separador de data. Para alterar o separador de data de todas as datas de uma cultura, seja para alterar o valor da propriedade <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A?displayProperty=nameWithType> da cultura atual ou para instanciar um objeto <xref:System.Globalization.DateTimeFormatInfo>, atribua o caractere à sua propriedade <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A> e chame uma sobrecarga do método de formatação que inclua um parâmetro <xref:System.IFormatProvider>.

Se o especificador de formato "/" for usado sem outros especificadores de formato personalizado, ele será interpretado como um especificador de formato de data e hora padrão e gerará uma <xref:System.FormatException>. Para saber mais sobre como usar um especificador de formato único, confira [Usar especificadores de formato único personalizados](#UsingSingleSpecifiers) posteriormente nesse artigo.

[Voltar à tabela](#table)

## <a name="character-literals"></a><a name="Literals"></a> Literais de caracteres

Os seguintes caracteres em uma cadeia de caracteres personalizada de formato de data e hora são reservados e sempre são interpretados como caracteres de formatação ou, no caso de `"` ,, `'` `/` e `\` , como caracteres especiais.

|     |     |     |     |     |
|-----|-----|-----|-----|-----|
| `F` | `H` | `K` | `M` | `d` |
| `f` | `g` | `h` | `m` | `s` |
| `t` | `y` | `z` | `%` | `:` |
| `/` | `"` | `'` | `\` |     |

Todos os outros caracteres sempre são interpretados como literais de caracteres e, em uma operação de formatação, são incluídos na cadeia de caracteres de resultado inalterada.  Em uma operação de análise, eles devem corresponder exatamente aos caracteres na cadeia de entrada; a comparação diferencia maiúsculas de minúsculas.

O exemplo a seguir inclui os caracteres literais "PST" (que indicam a Hora Padrão do Pacífico) e “PDT” (que indicam a Hora de Verão do Pacífico) para representar o fuso horário local em uma cadeia de caracteres de formato. Observe que a cadeia de caracteres está incluída na cadeia de caracteres de resultado e que uma cadeia de caracteres que inclui a cadeia de caracteres de fuso horário local também é analisada com êxito.

[!code-csharp[Formatting.DateAndTime.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx1.cs#20)]
[!code-vb[Formatting.DateAndTime.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx1.vb#20)]

Há duas maneiras de indicar que os caracteres devem ser interpretados como caracteres literais e não como caracteres reservados, para que possam ser incluídos em uma cadeia de caracteres de resultado ou analisados com êxito em uma cadeia de caracteres de entrada:

- Com o escape de cada caractere reservado. Para obter mais informações, consulte [usando o caractere de escape](#escape).

O exemplo a seguir inclui os caracteres literais "pst" (que indicam a Hora Padrão do Pacífico) para representar o fuso horário local em uma cadeia de caracteres de formato. Como "s" e "t" são cadeias de caracteres de formato personalizado, ambos os caracteres devem ter um escape para serem interpretados como caracteres literais.

[!code-csharp[Formatting.DateAndTime.Custom#21](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx2.cs#21)]
[!code-vb[Formatting.DateAndTime.Custom#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx2.vb#21)]

- Colocando toda a cadeia de caracteres literal entre aspas ou apóstrofos. O exemplo a seguir é semelhante ao anterior, mas "pst" é colocado entre aspas para indicar que toda a cadeia de caracteres delimitada deve ser interpretada como literais de caracteres.

[!code-csharp[Formatting.DateAndTime.Custom#22](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx3.cs#22)]
[!code-vb[Formatting.DateAndTime.Custom#22](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx3.vb#22)]

## <a name="notes"></a>Observações

### <a name="using-single-custom-format-specifiers"></a><a name="UsingSingleSpecifiers"></a> Usando especificadores de formato único personalizado

Uma cadeia de caracteres de formato de data e hora personalizado consiste em dois ou mais caracteres. Os métodos de formatação de data e hora interpretam qualquer cadeia de um único caractere como uma cadeia de caracteres de formato de data e hora padrão. Quando não reconhecem o caractere como um especificador de formato válido, eles geram uma <xref:System.FormatException>. Por exemplo, uma cadeia de caracteres de formato que consiste somente no especificador "h" é interpretada como uma cadeia de caracteres de formato padrão de data e hora. No entanto, nesse caso específico, uma exceção é gerada porque não há nenhum especificador "h" de formato padrão de data e hora.

Para usar qualquer um dos especificadores de formato de data e hora personalizado como sendo o único especificador em uma cadeia de caracteres de formato (ou seja, para usar o especificador de formato personalizado "d", "f", "F", "g", "h", "H", "K", "m", "M", "s", "t", "y", "z", ":" ou “/” por si só), inclua um espaço antes ou após o especificador ou inclua um especificador de formato de porcentagem "%" antes do especificador de data e hora personalizado simples.

Por exemplo, "`%h"` é interpretada como uma cadeia de caracteres de formato de data e hora personalizado que exibe a hora representada pelo valor atual de data e hora. Você também pode usar a cadeia de caracteres de formato " h" ou "H ", embora isso inclua um espaço na cadeia de caracteres resultante em conjunto com a hora. O exemplo a seguir ilustra essas três cadeias de caracteres de formatos.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#16](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/literal1.cs#16)]
[!code-vb[Formatting.DateAndTime.Custom#16](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/literal1.vb#16)]

#### <a name="using-the-escape-character"></a><a name="escape"></a> Usando o caractere de escape

Os caracteres "d", "f", "F", "g", "h", "H", "K", "m", "M", "s", "t", "y", "z", ":" ou "/" em uma cadeia de caracteres de formato são interpretados como especificadores de formato personalizados em vez de caracteres literais. Para impedir que um caractere seja interpretado como um especificador de formato, você pode precedê-lo com uma barra invertida (\\), que é o caractere de escape. O caractere de escape significa que o próximo caractere é um literal de caractere que deve ser incluído inalterado na cadeia de caracteres de resultado.

Para incluir uma barra invertida em uma cadeia de caracteres de resultado, você deve escapá-la com outra barra invertida (`\\`).

> [!NOTE]
> Alguns compiladores, como os compiladores C++ e C#, também podem interpretar um único caractere de barra invertida como um caractere de escape. Para garantir que uma cadeia de caracteres seja interpretada corretamente quando formatada, você poderá usar o caractere literal de cadeia de caracteres textual (o caractere @) antes da cadeia de caracteres em C# ou adicionar outro caractere de barra invertida antes de cada barra invertida em C# e em C++. O exemplo de C# a seguir ilustra ambas as abordagens.

O exemplo a seguir usa o caractere de escape para impedir que a operação de formatação interprete os caracteres de "h" e "m" como especificadores de formato.

[!code-csharp-interactive[Formatting.DateAndTime.Custom#15](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/escape1.cs#15)]
[!code-vb[Formatting.DateAndTime.Custom#15](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/escape1.vb#15)]

### <a name="control-panel-settings"></a>Configurações do Painel de Controle

As configurações de **Opções Regionais e de Idiomas** no Painel de Controle influenciam a cadeia de caracteres de resultado produzida por uma operação de formatação que inclui muitos dos especificadores de formato de data e hora personalizado. Essas configurações são usadas para inicializar o objeto <xref:System.Globalization.DateTimeFormatInfo> associado à cultura de thread atual, a qual fornece os valores usados para determinar a formatação. Computadores que usam configurações diferentes geram cadeias de caracteres de resultado diferentes.

Além disso, se o constructo <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29> for usado para criar uma instância de um novo objeto <xref:System.Globalization.CultureInfo> que representa a mesma cultura que a cultura atual do sistema, quaisquer personalizações estabelecidas pelo item **Opções Regionais e de Idioma** no Painel de Controle serão aplicadas ao novo objeto <xref:System.Globalization.CultureInfo>. Você pode usar o construtor <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> para criar um objeto <xref:System.Globalization.CultureInfo> que não reflita as personalizações de um sistema.

### <a name="datetimeformatinfo-properties"></a>Propriedades DateTimeFormatInfo

A formatação é influenciada pelas propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> atual, que é fornecido implicitamente pela cultura de thread atual ou explicitamente pelo parâmetro <xref:System.IFormatProvider> do método que invoca a formatação. Para o parâmetro <xref:System.IFormatProvider>, você deve especificar um objeto <xref:System.Globalization.CultureInfo>, o qual representa uma cultura ou um objeto <xref:System.Globalization.DateTimeFormatInfo>.

A cadeia de caracteres de resultado produzida por muitos dos especificadores de formato de data e hora personalizado também depende das propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> atual. Seu aplicativo pode alterar o resultado produzido por alguns especificadores de formato personalizado de data e hora ao alterar a propriedade <xref:System.Globalization.DateTimeFormatInfo> correspondente. Por exemplo, o especificador de formato "ddd" adiciona um nome de dia da semana abreviado encontrado na matriz de cadeia de caracteres <xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A> à cadeia de caracteres de resultado. Da mesma forma, o especificador de formato "MMMM" adiciona um nome de mês completo encontrado na matriz de cadeias de caracteres <xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A> à cadeia de caracteres de resultado.

## <a name="see-also"></a>Consulte também

- <xref:System.DateTime?displayProperty=nameWithType>
- <xref:System.IFormatProvider?displayProperty=nameWithType>
- [Tipos de formatação](formatting-types.md)
- [Cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md)
- [Exemplo: utilitário de formatação do WinForms do .NET Core (C#)](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-cs)
- [Exemplo: utilitário de formatação do WinForms do .NET Core (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-vb)
