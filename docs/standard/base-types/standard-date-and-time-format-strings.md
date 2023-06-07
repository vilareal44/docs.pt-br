---
title: Cadeias de caracteres de formato de data e hora padrão
description: Neste artigo, aprenda a usar uma cadeia de caracteres de formato de data e hora padrão para definir a representação de texto de um valor de data e hora no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
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
ms.assetid: bb79761a-ca08-44ee-b142-b06b3e2fc22b
ms.openlocfilehash: 5db9088a6b0d75ae5293b9be35346c4c2ddf81c6
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447219"
---
# <a name="standard-date-and-time-format-strings"></a>Cadeias de caracteres de formato de data e hora padrão

Uma cadeia de caracteres de formato de data e hora padrão usa um especificador de formato único para definir a representação do texto de um valor de data e hora. Qualquer cadeia de caracteres de formato de data e hora que contém mais de um caractere, incluindo espaço em branco, é interpretada como uma cadeia de caracteres de formato de data e hora personalizado. Para obter mais informações, consulte [Cadeias de caracteres de formato de data e hora personalizado](custom-date-and-time-format-strings.md). Uma cadeia de caracteres de formato padrão ou personalizado pode ser usada de duas maneiras:

- Para definir a cadeia de caracteres que resulta de uma operação de formatação.

- Para definir a representação de texto de um valor de data e hora que possa ser convertido em valor de <xref:System.DateTime> ou <xref:System.DateTimeOffset> por uma operação de análise.

> [!TIP]
> Baixe o **Utilitário de Formatação**, um aplicativo do Windows Forms do .NET Core que permite aplicar cadeias de caracteres de formato a valores numéricos ou de data e hora e exibir a cadeia de caracteres de resultado. O código-fonte está disponível para o [C#](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-cs) e o [Visual Basic](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-vb).

Cadeias de caracteres de formato de data e hora padrão podem ser usadas tanto com valores <xref:System.DateTime> quanto <xref:System.DateTimeOffset>.

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-with-utc-partial-note.md)]

<a name="table"></a>A tabela a seguir descreve os especificadores de formato de data e hora padrão. Salvo indicação em contrário, um determinado especificador de formato de data e hora padrão produz uma representação de cadeia de caracteres idêntica independente de ela ser usada com um valor <xref:System.DateTime> ou <xref:System.DateTimeOffset>. Consulte a seção [Observações](#Notes) para obter informações adicionais sobre como usar as cadeias de caracteres de formato de data e hora padrão.

|Especificador de formato|Descrição|Exemplos|
|----------------------|-----------------|--------------|
|"d"|Padrão de data abreviada.<br /><br /> Para saber mais: [especificador de formato abreviado de data ("d")](#ShortDate).|2009-06-15T13:45:30 -> 6/15/2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 (fr-FR)<br /><br /> 2009-06-15T13:45:30 -> 2009/06/15 (ja-JP)|
|"D"|Padrão de data completa.<br /><br /> Para saber mais: [especificador de formato de data completa ("D")](#LongDate).|2009-06-15T13:45:30 -> Monday, June 15, 2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15 июня 2009 г. (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> Montag, 15. Juni 2009 (de-DE)|
|"f"|Padrão de data/hora completa (hora abreviada).<br /><br /> Para saber mais: [especificador de formato de data completa e hora abreviada ("f")](#FullDateShortTime).|2009-06-15T13:45:30 -> Monday, June 15, 2009 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 13:45 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 1:45 μμ (el-GR)|
|"F"|Padrão de data/hora completa (hora completa).<br /><br /> Para saber mais: [especificador de formato de data completa e hora completa ("F")](#FullDateLongTime).|2009-06-15T13:45:30 -> Monday, June 15, 2009 1:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 13:45:30 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 1:45:30 μμ (el-GR)|
|"g"|Padrão geral de data/hora (hora abreviada).<br /><br /> Para saber mais: [especificador de formato geral de data e hora abreviada ("g")](#GeneralDateShortTime).|2009-06-15T13:45:30 -> 6/15/2009 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 13:45 (es-ES)<br /><br /> 2009-06-15T13:45:30 -> 2009/6/15 13:45 (zh-CN)|
|"G"|Padrão geral de data/hora (hora completa).<br /><br /> Para saber mais: [especificador de formato geral de data e hora completa ("G")](#GeneralDateLongTime).|2009-06-15T13:45:30 -> 6/15/2009 1:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 13:45:30 (es-ES)<br /><br /> 2009-06-15T13:45:30 -> 2009/6/15 13:45:30 (zh-CN)|
|"M", "m"|Padrão de mês/dia.<br /><br /> Para saber mais: [especificador de formato de mês ("M", "m")](#MonthDay).|2009-06-15T13:45:30 -> June 15 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15. juni (da-DK)<br /><br /> 2009-06-15T13:45:30 -> 15 Juni (id-ID)|
|"O", "o"|Padrão de data/hora de ida e volta.<br /><br /> Para saber mais: [especificador de formato de viagem de ida e volta ("O", "o")](#Roundtrip).|Valores <xref:System.DateTime>:<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Local) --> 2009-06-15T13:45:30.0000000-07:00<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Utc) --> 2009-06-15T13:45:30.0000000Z<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Unspecified) --> 2009-06-15T13:45:30.0000000<br /><br /> Valores <xref:System.DateTimeOffset>:<br /><br /> 2009-06-15T13:45:30-07:00 --> 2009-06-15T13:45:30.0000000-07:00|
|"R", "r"|Padrão RFC1123.<br /><br /> Para saber mais: [especificador de formato RFC1123 ("R", "r")](#RFC1123).|2009-06-15T13:45:30 -> Mon, 15 Jun 2009 20:45:30 GMT|
|"s"|Padrão de data/hora classificável.<br /><br /> Para saber mais: [especificador de formato classificável ("s")](#Sortable).|2009-06-15T13:45:30 (DateTimeKind.Local) -> 2009-06-15T13:45:30<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Utc) -> 2009-06-15T13:45:30|
|"t"|Padrão de hora abreviada.<br /><br /> Para saber mais: [especificador de formato de hora abreviada ("t")](#ShortTime).|2009-06-15T13:45:30 -> 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 13:45 (hr-HR)<br /><br /> 2009-06-15T13:45:30 -> 01:45 م (ar-EG)|
|"T"|Padrão de hora completa.<br /><br /> Para saber mais: [especificador de formato de hora completa ("T")](#LongTime).|2009-06-15T13:45:30 -> 1:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 13:45:30 (hr-HR)<br /><br /> 2009-06-15T13:45:30 -> 01:45:30 م (ar-EG)|
|"u"|Padrão classificável universal de data/hora.<br /><br /> Para saber mais: [especificador de formato de padrão classificável universal ("u")](#UniversalSortable).|Com um valor <xref:System.DateTime>: 2009-06-15T13:45:30 -> 2009-06-15 13:45:30Z<br /><br /> Com um valor <xref:System.DateTimeOffset>: 2009-06-15T13:45:30 -> 2009-06-15 20:45:30Z|
|"U"|Padrão universal de data/hora completa.<br /><br /> Para saber mais: [especificador de formato de padrão universal completo ("U")](#UniversalFull).|2009-06-15T13:45:30 -> Monday, June 15, 2009 8:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 20:45:30 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 8:45:30 μμ (el-GR)|
|"Y", "y"|Padrão ano mês.<br /><br /> Para saber mais: [especificador de formato de ano mês ("Y")](#YearMonth).|2009-06-15T13:45:30-> junho de 2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> juni 2009 (da-DK)<br /><br /> 2009-06-15T13:45:30 -> Juni 2009 (id-ID)|
|Qualquer outro caractere único|Especificador desconhecido.|Gera uma <xref:System.FormatException> de tempo de execução.|

## <a name="how-standard-format-strings-work"></a>Como funcionam as cadeias de caracteres de formato padrão

Em uma operação de formatação, uma cadeia de formato padrão é simplesmente um alias para uma cadeia de caracteres de formato personalizado. A vantagem de usar um alias para se referir a uma cadeia de caracteres de formato personalizado é que, embora o alias permaneça invariável, a cadeia de caracteres de formato personalizado em si pode variar. Isso é importante porque as representações de cadeia de caracteres de valores de data e hora normalmente variam de acordo com a cultura. Por exemplo, a cadeia de caracteres de formato padrão "d" indica que um valor de data e hora deve ser exibido usando um formato de data abreviada. Para a cultura invariável, esse padrão é "MM/dd/aaaa". Para a cultura fr-FR, ele é " dd/MM/aaaa ". Para a cultura ja-JP, ele é " aaaa/MM/dd ".

Se uma cadeia de caracteres de formato padrão em uma operação de formatação aponta para uma cadeia de caracteres de formato personalizado de uma cultura, seu aplicativo pode definir a cultura específica cujas cadeias de caracteres de formato personalizado são usadas de uma dessas maneiras:

- Você pode usar a cultura padrão (ou atual). O exemplo a seguir exibe uma data usando o formato de data abreviada da cultura atual. Nesse caso, a cultura atual é en-US.

  [!code-csharp-interactive[System.DateTime.Conceptual.Formatting#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#1)]
  [!code-vb[System.DateTime.Conceptual.Formatting#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#1)]

- Você pode passar um objeto <xref:System.Globalization.CultureInfo> que representa a cultura cuja formatação deve ser usada para um método que possua um parâmetro <xref:System.IFormatProvider>. O exemplo a seguir exibe uma data usando o formato de data abreviada da cultura pt-BR.

  [!code-csharp[System.DateTime.Conceptual.Formatting#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#2)]
  [!code-vb[System.DateTime.Conceptual.Formatting#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#2)]

- Você pode passar um objeto <xref:System.Globalization.DateTimeFormatInfo> que fornece informações de formatação para um método que possua um parâmetro <xref:System.IFormatProvider>. O exemplo a seguir exibe uma data usando o formato de data abreviada de um objeto <xref:System.Globalization.DateTimeFormatInfo> para a cultura hr-HR.

  [!code-csharp[System.DateTime.Conceptual.Formatting#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#3)]
  [!code-vb[System.DateTime.Conceptual.Formatting#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#3)]

> [!NOTE]
> Para obter informações sobre como personalizar os padrões ou as cadeias de caracteres usados na formatação de valores de data e hora, consulte o tópico da classe <xref:System.Globalization.NumberFormatInfo>.

Em alguns casos, a cadeia de caracteres de formato padrão funciona como uma abreviação conveniente de uma cadeia de caracteres de formato personalizado maior que é invariável. Quatro cadeias de caracteres de formato padrão se enquadram nesta categoria: "O" (ou "o"), "R" (ou "r"), "s" e "u". Estas cadeias de caracteres correspondem às cadeias de caracteres de formato personalizado definidas pela cultura invariável. Elas produzem representações de cadeias de caracteres de valores de data e hora que são feitos para ser idênticos entre culturas. A tabela a seguir fornece informações sobre essas quatro cadeias de caracteres de formato de data e hora padrão.

|Cadeias de caracteres de formato padrão|Definidas pela propriedade DateTimeFormatInfo.InvariantInfo|Cadeia de caracteres de formato personalizado|
|----------------------------|----------------------------------------------------------|--------------------------|
|"O" ou "o"|Nenhum|yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffzz|
|"R" ou "r"|<xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A>|ddd, dd MMM yyyy HH':'mm':'ss 'GMT'|
|"s"|<xref:System.Globalization.DateTimeFormatInfo.SortableDateTimePattern%2A>|yyyy'-'MM'-'dd'T'HH':'mm':'ss|
|"u"|<xref:System.Globalization.DateTimeFormatInfo.UniversalSortableDateTimePattern%2A>|yyyy'-'MM'-'dd HH':'mm':'ss'Z'|

As cadeias de caracteres de formato padrão também podem ser usadas em operações de análise com os métodos <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> ou <xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType>, o que exige que uma cadeia de caracteres de entrada atenda com exatidão a um padrão específico para que a operação de análise seja bem-sucedida. Muitas cadeias de caracteres de formato padrão são mapeadas em muitas cadeias de caracteres de formato personalizado. Assim, um valor de data e hora poderá ser representado em vários formatos e a operação de análise ainda terá êxito. Você pode determinar a cadeia ou as cadeias de caracteres de formato que correspondem a uma cadeia de formato padrão ao chamar o método <xref:System.Globalization.DateTimeFormatInfo.GetAllDateTimePatterns%28System.Char%29?displayProperty=nameWithType>. O exemplo a seguir exibe as cadeias de caracteres de formato personalizado que são mapeados para a cadeia de caracteres de formato padrão “d” (padrão de data abreviada).

[!code-csharp[Formatting.DateAndTime.Standard#17](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/stdandparsing1.cs#17)]
[!code-vb[Formatting.DateAndTime.Standard#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/stdandparsing1.vb#17)]

As seções a seguir descrevem os especificadores de formato padrão para valores <xref:System.DateTime> e <xref:System.DateTimeOffset>.

<a name="ShortDate"></a>

## <a name="the-short-date-d-format-specifier"></a>Especificador de formato de data abreviada ("d")

O especificador de formato padrão “d” representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> de uma cultura específica. Por exemplo, a cadeia de caracteres de formato personalizado retornada pela propriedade <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A> da cultura invariável é " MM/dd/aaaa ".

A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que controlam a formatação da cadeia de caracteres retornada.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|Define o formato geral da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|Define a cadeia de caracteres que separa o ano, o mês e os componentes de dia de uma data.|

O exemplo a seguir utiliza o especificador de formato "d" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#1)]
[!code-vb[Formatting.DateAndTime.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#1)]

[Voltar à tabela](#table)

<a name="LongDate"></a>

## <a name="the-long-date-d-format-specifier"></a>Especificador de formato de data completa ("D")

O especificador de formato padrão “D” representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> atual. Por exemplo, a cadeia de caracteres de formato personalizado para a cultura invariável é "dddd, dd MMMM aaaa".

A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que controlam a formatação da cadeia de caracteres retornada.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A>|Define o formato geral da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|Define os nomes de dias localizados que podem aparecer na cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|Define os nomes dos meses localizados que podem aparecer na cadeia de caracteres de resultado.|

O exemplo a seguir utiliza o especificador de formato "D" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#2)]
[!code-vb[Formatting.DateAndTime.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#2)]

[Voltar à tabela](#table)

<a name="FullDateShortTime"></a>

## <a name="the-full-date-short-time-f-format-specifier"></a>Especificador de formato de data completa e hora abreviada (“f”)

O especificador de formato padrão "f" representa uma combinação dos padrões de data completa ("D") e hora abreviada ("t"), separados por um espaço.

A cadeia de caracteres do resultado é afetada pelas informações de formatação de um objeto <xref:System.Globalization.DateTimeFormatInfo> específico. A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que podem controlar a formatação da cadeia de caracteres retornada. O especificador personalizado de formato retornado pelas propriedades <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> e <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> de algumas culturas não pode usar todas as propriedades.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A>|Define o formato do componente de data da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|Define o formato do componente de hora da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|Define os nomes de dias localizados que podem aparecer na cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|Define os nomes dos meses localizados que podem aparecer na cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|Define a cadeia de caracteres que separa os componentes de hora, minuto e segundo de uma hora.|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|Define a cadeia de caracteres que indica as horas da meia-noite até antes do meio-dia em um relógio de 12 horas.|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|Define a cadeia de caracteres que indica as horas do meio-dia até antes da meia-noite em um relógio de 12 horas.|

O exemplo a seguir utiliza o especificador de formato "f" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#3)]
[!code-vb[Formatting.DateAndTime.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#3)]

[Voltar à tabela](#table)

<a name="FullDateLongTime"></a>

## <a name="the-full-date-long-time-f-format-specifier"></a>Especificador de formato de data completa e hora completa (“F”)

O especificador de formato padrão “F” representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A?displayProperty=nameWithType> atual. Por exemplo, a cadeia de caracteres de formato personalizado para a cultura invariável é "dddd, dd MMMM aaaa HH:mm:ss".

A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que podem controlar a formatação da cadeia de caracteres retornada. O especificador de formato personalizado que é retornado pela propriedade <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A> de algumas culturas não pode usar todas as propriedades.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A>|Define o formato geral da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|Define os nomes de dias localizados que podem aparecer na cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|Define os nomes dos meses localizados que podem aparecer na cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|Define a cadeia de caracteres que separa os componentes de hora, minuto e segundo de uma hora.|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|Define a cadeia de caracteres que indica as horas da meia-noite até antes do meio-dia em um relógio de 12 horas.|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|Define a cadeia de caracteres que indica as horas do meio-dia até antes da meia-noite em um relógio de 12 horas.|

O exemplo a seguir utiliza o especificador de formato "F" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#4)]
[!code-vb[Formatting.DateAndTime.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#4)]

[Voltar à tabela](#table)

<a name="GeneralDateShortTime"></a>

## <a name="the-general-date-short-time-g-format-specifier"></a>Especificador de formato de data geral e hora abreviada (“g”)

O especificador de formato padrão "g" representa uma combinação dos padrões de data abreviada ("d") e hora abreviada ("t"), separados por um espaço.

A cadeia de caracteres do resultado é afetada pelas informações de formatação de um objeto <xref:System.Globalization.DateTimeFormatInfo> específico. A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que podem controlar a formatação da cadeia de caracteres retornada. O especificador personalizado de formato que é retornado pelas propriedades <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> e <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> de algumas culturas não pode usar todas as propriedades.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|Define o formato do componente de data da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|Define o formato do componente de hora da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|Define a cadeia de caracteres que separa o ano, o mês e os componentes de dia de uma data.|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|Define a cadeia de caracteres que separa os componentes de hora, minuto e segundo de uma hora.|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|Define a cadeia de caracteres que indica as horas da meia-noite até antes do meio-dia em um relógio de 12 horas.|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|Define a cadeia de caracteres que indica as horas do meio-dia até antes da meia-noite em um relógio de 12 horas.|

O exemplo a seguir utiliza o especificador de formato "g" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#5)]
[!code-vb[Formatting.DateAndTime.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#5)]

[Voltar à tabela](#table)

<a name="GeneralDateLongTime"></a>

## <a name="the-general-date-long-time-g-format-specifier"></a>Especificador de formato de data geral e hora completa (“G”)

O especificador de formato padrão "G" representa uma combinação dos padrões de data abreviada ("d") e hora completa ("T"), separados por um espaço.

A cadeia de caracteres do resultado é afetada pelas informações de formatação de um objeto <xref:System.Globalization.DateTimeFormatInfo> específico. A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que podem controlar a formatação da cadeia de caracteres retornada. O especificador personalizado de formato que é retornado pelas propriedades <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> e <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> de algumas culturas não pode usar todas as propriedades.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|Define o formato do componente de data da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A>|Define o formato do componente de hora da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|Define a cadeia de caracteres que separa o ano, o mês e os componentes de dia de uma data.|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|Define a cadeia de caracteres que separa os componentes de hora, minuto e segundo de uma hora.|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|Define a cadeia de caracteres que indica as horas da meia-noite até antes do meio-dia em um relógio de 12 horas.|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|Define a cadeia de caracteres que indica as horas do meio-dia até antes da meia-noite em um relógio de 12 horas.|

O exemplo a seguir utiliza o especificador de formato "G" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#6)]
[!code-vb[Formatting.DateAndTime.Standard#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#6)]

[Voltar à tabela](#table)

<a name="MonthDay"></a>

## <a name="the-month-m-m-format-specifier"></a>Especificador de formato de mês ("M", "m")

O especificador de formato padrão “M” ou "m" representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.MonthDayPattern%2A?displayProperty=nameWithType> atual. Por exemplo, a cadeia de caracteres de formato personalizado para o cultura invariável é "MMMM dd".

A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que controlam a formatação da cadeia de caracteres retornada.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.MonthDayPattern%2A>|Define o formato geral da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|Define os nomes dos meses localizados que podem aparecer na cadeia de caracteres de resultado.|

O exemplo a seguir utiliza o especificador de formato "m" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#7)]
[!code-vb[Formatting.DateAndTime.Standard#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#7)]

[Voltar à tabela](#table)

<a name="Roundtrip"></a>

## <a name="the-round-trip-o-o-format-specifier"></a>Especificador de formato da viagem de ida e volta ("O", "o")

O especificador de formato padrão "O" ou "o" representa uma cadeia de caracteres de data e hora personalizada usando um padrão que preserva as informações de fuso horário e emite uma cadeia de caracteres de resultado compilada com ISO 8601. Para valores <xref:System.DateTime>, este especificador de formato foi projetado para manter valores de data e hora junto à propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> no texto. A cadeia de caracteres formatada pode ser analisada de volta usando o método <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> ou <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> quando o parâmetro `styles` está definido como <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType>.

O especificador de formato padrão “O” ou “o” corresponde à cadeia de caracteres de formato personalizado "aaaa'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffK" para valores <xref:System.DateTime> e à cadeia de caracteres de formato personalizado "yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffzzz" para valores <xref:System.DateTimeOffset>. Nesta cadeia de caracteres, os pares de aspas simples que delimitam caracteres individuais, como hifens, dois-pontos e a letra "T", indicam que o caractere individual é literal e não pode ser alterado. As aspas simples em si não aparecem na cadeia de caracteres de saída.

O especificador de formato "O" ou "o" padrão (e a cadeia de caracteres de formato personalizado "yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffK") aproveita as três maneiras do ISO 8601 representar as informações de fuso horário para preservar a propriedade <xref:System.DateTime.Kind%2A> dos valores <xref:System.DateTime>:

- O componente de fuso horário dos valores de data e hora <xref:System.DateTimeKind.Local?displayProperty=nameWithType> é um deslocamento em relação ao UTC (por exemplo, +01:00, -07:00). Todos os valores <xref:System.DateTimeOffset> também estão representados nesse formato.

- O componente de fuso horário dos valores de data e hora <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> usa "Z" (que significa deslocamento zero) para representar o UTC.

- Os valores de data e hora <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> não têm informações de fuso horário.

Como o especificador de formato padrão "O" ou "o" está de acordo com um padrão internacional, a operação de formatação ou análise que usa o especificador sempre usa a cultura invariável e o calendário gregoriano.

As cadeias de caracteres passadas para os métodos `Parse`, `TryParse`, `ParseExact` e `TryParseExact` de <xref:System.DateTime> e <xref:System.DateTimeOffset> podem ser analisadas usando o especificador de formato "O" ou "o", caso estejam em um desses formatos. No caso de objetos <xref:System.DateTime>, a sobrecarga de análise chamada também inclui um parâmetro `styles` com um valor de <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType>. Se chamar um método de análise com a cadeia de caracteres de formato personalizado correspondente ao especificador de formato "O" ou "o", você não terá os mesmos resultados de "O" ou "o". Isso porque os métodos de análise que usam uma cadeia de caracteres de formato personalizado não podem analisar a representação da cadeia de caracteres de valores de data e hora que não tenham um componente de fuso horário ou usem "Z" para indicar o UTC.

O exemplo a seguir usa o especificador de formato "o" para exibir uma série de <xref:System.DateTime> valores e um <xref:System.DateTimeOffset> valor em um sistema no fuso horário do Pacífico dos EUA.

[!code-csharp[Formatting.DateAndTime.Standard#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/o1.cs#8)]
[!code-vb[Formatting.DateAndTime.Standard#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/o1.vb#8)]

O exemplo a seguir usa o especificador do formato “o” para criar uma cadeia de caracteres formatada e, em seguida, restaurar o valor de data e hora original ao chamar um método `Parse` de data e hora.

[!code-csharp[Formatting.DateandTime.Standard#16](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Roundtrip1.cs#16)]
[!code-vb[Formatting.DateandTime.Standard#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/RoundTrip1.vb#16)]

[Voltar à tabela](#table)

<a name="RFC1123"></a>

## <a name="the-rfc1123-r-r-format-specifier"></a>Especificador de formato RFC1123 ("R", "r")

O especificador de formato padrão “R” ou "r" representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A?displayProperty=nameWithType>. O padrão reflete um padrão definido e a propriedade é somente leitura. Portanto, ele é sempre o mesmo, independentemente da cultura usada ou do provedor de formato fornecido. A cadeia de caracteres de formato personalizado é "ddd, dd MMM aaaa HH':'mm':'ss 'GMT' ". Quando esse especificador de formato padrão é usado, a operação de formatação ou análise sempre usa a cultura invariável.

A cadeia de caracteres de resultado é afetada pelas seguintes propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> retornado pela propriedade <xref:System.Globalization.DateTimeFormatInfo.InvariantInfo%2A?displayProperty=nameWithType> que representa a cultura invariável.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A>|Define o formato da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A>|Define os nomes de dias abreviados que podem aparecer na cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.AbbreviatedMonthNames%2A>|Define os nomes dos meses abreviados que podem aparecer na cadeia de caracteres de resultado.|

Embora o padrão RFC 1123 expresse a hora no formato UTC (Hora Universal Coordenada), a operação de formatação não altera o valor do objeto <xref:System.DateTime> que está sendo formatado. Assim, você deve converter o valor de <xref:System.DateTime> em UTC ao chamar o método <xref:System.DateTime.ToUniversalTime%2A?displayProperty=nameWithType> antes de executar a operação de formatação. Em contraste, os valores de <xref:System.DateTimeOffset> executam essa conversão automaticamente; não há necessidade de chamar o método <xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> antes da operação de formatação.

O exemplo a seguir usa o especificador de formato "f" para exibir um valor <xref:System.DateTime> e um valor <xref:System.DateTimeOffset> em um sistema no fuso horário Pacífico dos EUA.

[!code-csharp-interactive[Formatting.DateAndTime.Standard#9](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#9)]
[!code-vb[Formatting.DateAndTime.Standard#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#9)]

[Voltar à tabela](#table)

<a name="Sortable"></a>

## <a name="the-sortable-s-format-specifier"></a>Especificador de formato classificável ("s")

O especificador de formato padrão “s" representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.SortableDateTimePattern%2A?displayProperty=nameWithType>. O padrão reflete um padrão definido (ISO 8601) e a propriedade é somente leitura. Portanto, ele é sempre o mesmo, independentemente da cultura usada ou do provedor de formato fornecido. A cadeia de caracteres de formato personalizado é "aaaa'-'MM'-'dd'T'HH':'mm':'ss".

A finalidade do especificador de formato "s" é produzir cadeias de caracteres de resultado que classificam consistentemente em ordem crescente ou decrescente com base nos valores de data e hora. Como resultado, embora o especificador de formato padrão "s" represente um valor de data e hora em um formato consistente, a operação de formatação não modifica o valor do objeto de data e hora que está sendo formatado para refletir sua propriedade <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> ou seu valor <xref:System.DateTimeOffset.Offset%2A?displayProperty=nameWithType>. Por exemplo, as cadeias de caracteres de resultado produzidas pela formatação de valores de data e hora 2014-11-15T18:32:17+00:00 e 2014-11-15T18:32:17+08:00 são idênticas.

Quando esse especificador de formato padrão é usado, a operação de formatação ou análise sempre usa a cultura invariável.

O exemplo a seguir usa o especificador de formato "s" para exibir um valor <xref:System.DateTime> e um valor <xref:System.DateTimeOffset> em um sistema no fuso horário Pacífico dos EUA.

[!code-csharp-interactive[Formatting.DateAndTime.Standard#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#10)]
[!code-vb[Formatting.DateAndTime.Standard#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#10)]

[Voltar à tabela](#table)

<a name="ShortTime"></a>

## <a name="the-short-time-t-format-specifier"></a>Especificador de formato de hora abreviada ("t")

O especificador de formato padrão “t” representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> atual. Por exemplo, a cadeia de caracteres de formato personalizado para a cultura invariável é "HH:mm".

A cadeia de caracteres do resultado é afetada pelas informações de formatação de um objeto <xref:System.Globalization.DateTimeFormatInfo> específico. A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que podem controlar a formatação da cadeia de caracteres retornada. O especificador de formato personalizado que é retornado pela propriedade <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> de algumas culturas não pode usar todas as propriedades.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|Define o formato do componente de hora da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|Define a cadeia de caracteres que separa os componentes de hora, minuto e segundo de uma hora.|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|Define a cadeia de caracteres que indica as horas da meia-noite até antes do meio-dia em um relógio de 12 horas.|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|Define a cadeia de caracteres que indica as horas do meio-dia até antes da meia-noite em um relógio de 12 horas.|

O exemplo a seguir utiliza o especificador de formato "t" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#11)]
[!code-vb[Formatting.DateAndTime.Standard#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#11)]

[Voltar à tabela](#table)

<a name="LongTime"></a>

## <a name="the-long-time-t-format-specifier"></a>Especificador de formato de hora completa ("T")

O especificador de formato padrão “T” representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> de uma cultura específica. Por exemplo, a cadeia de caracteres de formato personalizado para a cultura invariável é "HH:mm:ss".

A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que podem controlar a formatação da cadeia de caracteres retornada. O especificador de formato personalizado que é retornado pela propriedade <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> de algumas culturas não pode usar todas as propriedades.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A>|Define o formato do componente de hora da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|Define a cadeia de caracteres que separa os componentes de hora, minuto e segundo de uma hora.|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|Define a cadeia de caracteres que indica as horas da meia-noite até antes do meio-dia em um relógio de 12 horas.|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|Define a cadeia de caracteres que indica as horas do meio-dia até antes da meia-noite em um relógio de 12 horas.|

O exemplo a seguir utiliza o especificador de formato "T" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#12](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#12)]
[!code-vb[Formatting.DateAndTime.Standard#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#12)]

[Voltar à tabela](#table)

<a name="UniversalSortable"></a>

## <a name="the-universal-sortable-u-format-specifier"></a>Especificador de formato classificável universal ("u").

O especificador de formato padrão “u" representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.UniversalSortableDateTimePattern%2A?displayProperty=nameWithType>. O padrão reflete um padrão definido e a propriedade é somente leitura. Portanto, ele é sempre o mesmo, independentemente da cultura usada ou do provedor de formato fornecido. A cadeia de caracteres de formato personalizado é "aaaa'-'MM'-'dd HH':'mm':'ss'Z'". Quando esse especificador de formato padrão é usado, a operação de formatação ou análise sempre usa a cultura invariável.

Embora a cadeia de caracteres de resultado deva expressar a hora no formato UTC, nenhuma conversão do valor de <xref:System.DateTime> original é executada durante a operação de formatação. Assim, você deve converter um valor de <xref:System.DateTime> em UTC ao chamar o método <xref:System.DateTime.ToUniversalTime%2A?displayProperty=nameWithType> antes de formatá-lo.  Em contraste, os valores de <xref:System.DateTimeOffset> executam essa conversão automaticamente; não há necessidade de chamar o método <xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> antes da operação de formatação.

O exemplo a seguir utiliza o especificador de formato "u" para exibir um valor de data e hora.

[!code-csharp-interactive[Formatting.DateAndTime.Standard#13](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#13)]
[!code-vb[Formatting.DateAndTime.Standard#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#13)]

[Voltar à tabela](#table)

<a name="UniversalFull"></a>

## <a name="the-universal-full-u-format-specifier"></a>Especificador de formato completo universal ("U").

O especificador de formato padrão “U” representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A?displayProperty=nameWithType> de uma cultura específica. O padrão é o mesmo que o padrão "F". Entretanto, o valor de <xref:System.DateTime> é automaticamente convertido em UTC antes de ser formatado.

A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que podem controlar a formatação da cadeia de caracteres retornada. O especificador de formato personalizado que é retornado pela propriedade <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A> de algumas culturas não pode usar todas as propriedades.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A>|Define o formato geral da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|Define os nomes de dias localizados que podem aparecer na cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|Define os nomes dos meses localizados que podem aparecer na cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|Define a cadeia de caracteres que separa os componentes de hora, minuto e segundo de uma hora.|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|Define a cadeia de caracteres que indica as horas da meia-noite até antes do meio-dia em um relógio de 12 horas.|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|Define a cadeia de caracteres que indica as horas do meio-dia até antes da meia-noite em um relógio de 12 horas.|

O especificador de formato "U" não é suportado pelo tipo <xref:System.DateTimeOffset> e gera uma <xref:System.FormatException> ao ser usado para formatar um valor <xref:System.DateTimeOffset>.

O exemplo a seguir utiliza o especificador de formato "U" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#14](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#14)]
[!code-vb[Formatting.DateAndTime.Standard#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#14)]

[Voltar à tabela](#table)

<a name="YearMonth"></a>

## <a name="the-year-month-y-y-format-specifier"></a>Especificador de formato de ano mês ("Y", "y")

O especificador de formato padrão “Y” ou "y" representa uma cadeia de caracteres de formato de data e hora personalizado que é definida pela propriedade <xref:System.Globalization.DateTimeFormatInfo.YearMonthPattern%2A?displayProperty=nameWithType> de uma cultura específica. Por exemplo, a cadeia de caracteres de formato personalizado para o cultura invariável é "aaaa MMMM".

A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> que controlam a formatação da cadeia de caracteres retornada.

|Propriedade|Descrição|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.YearMonthPattern%2A>|Define o formato geral da cadeia de caracteres de resultado.|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|Define os nomes dos meses localizados que podem aparecer na cadeia de caracteres de resultado.|

O exemplo a seguir utiliza o especificador de formato "y" para exibir um valor de data e hora.

[!code-csharp[Formatting.DateAndTime.Standard#15](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#15)]
[!code-vb[Formatting.DateAndTime.Standard#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#15)]

[Voltar à tabela](#table)

<a name="Notes"></a>

## <a name="notes"></a>Observações

### <a name="control-panel-settings"></a>Configurações do Painel de Controle

As configurações no item **Opções Regionais e de Idioma** do Painel de Controle influenciam a cadeia de caracteres de resultado produzida por uma operação de formatação. Essas configurações são usadas para inicializar o objeto <xref:System.Globalization.DateTimeFormatInfo> associado à cultura de thread atual, a qual fornece os valores usados para determinar a formatação. Computadores que usam configurações diferentes geram cadeias de caracteres de resultado diferentes.

Além disso, se o constructo <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29> for usado para criar uma instância de um novo objeto <xref:System.Globalization.CultureInfo> que representa a mesma cultura que a cultura atual do sistema, quaisquer personalizações estabelecidas pelo item **Opções Regionais e de Idioma** no Painel de Controle serão aplicadas ao novo objeto <xref:System.Globalization.CultureInfo>. Você pode usar o construtor <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> para criar um objeto <xref:System.Globalization.CultureInfo> que não reflita as personalizações de um sistema.

### <a name="datetimeformatinfo-properties"></a>Propriedades DateTimeFormatInfo

A formatação é influenciada pelas propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> atual, que é fornecido implicitamente pela cultura de thread atual ou explicitamente pelo parâmetro <xref:System.IFormatProvider> do método que invoca a formatação. Para o parâmetro <xref:System.IFormatProvider>, seu aplicativo deve especificar um objeto <xref:System.Globalization.CultureInfo>, que representa uma cultura, ou um objeto <xref:System.Globalization.DateTimeFormatInfo>, que representa as convenções de formatação de data e hora de uma determinada cultura. Muitos dos especificadores de formato padrão de data e hora são aliases para padrões de formatação definidos pelas propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo> atual. Seu aplicativo pode alterar o resultado produzido por alguns especificadores de formato padrão de data e hora alterando os padrões de formatação de data e hora correspondentes da propriedade <xref:System.Globalization.DateTimeFormatInfo>.

## <a name="see-also"></a>Confira também

- <xref:System.DateTime?displayProperty=nameWithType>
- <xref:System.DateTimeOffset?displayProperty=nameWithType>
- [Formatar tipos](formatting-types.md)
- [Cadeias de caracteres de formato de data e hora personalizadas](custom-date-and-time-format-strings.md)
- [Amostra: Utilitário de Formatação do WinForms do .NET Core (C#)](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-cs)
- [Amostra: Utilitário de Formatação do WinForms do .NET Core (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-vb)
