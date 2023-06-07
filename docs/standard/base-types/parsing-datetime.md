---
title: Converter cadeias de caracteres em DateTime
description: Aprenda técnicas para analisar cadeias de caracteres que representam datas e horas a fim de criar um DateTime com base na cadeia de caracteres de data e hora.
ms.date: 02/15/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parsing strings, date and time strings
- date and time strings
- ParseExact method
- enumerations [.NET Framework], parsing strings
- base types, parsing strings
- DateTime object
- time strings
ms.openlocfilehash: 9fba80e4dbe1e4950ed24e7489ac48ea1b6ff20b
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662895"
---
# <a name="parse-date-and-time-strings-in-net"></a>Analisar cadeias de caracteres de data e hora no .NET

A análise de cadeias de caracteres para convertê-las em objetos <xref:System.DateTime> exige que você especifique as informações de como as datas e horas são representadas como texto. Diferentes culturas usam ordens diferentes para dia, mês e ano. Algumas representações usam o relógio de 24 horas, outras especificam "AM" e "PM". Alguns aplicativos precisam apenas da data. Outros precisam apenas da hora. Há também outros que precisam especificar tanto a data quanto a hora. Os métodos que convertem cadeias de caracteres em objetos <xref:System.DateTime> permitem que você forneça informações detalhadas sobre os formatos esperados e os elementos de uma data e hora que seu aplicativo precisa. Há três subtarefas para converter corretamente o texto em um <xref:System.DateTime>:

1. Você deve especificar o formato esperado do texto que representa a data e hora.
1. Você pode especificar a cultura para o formato de data hora.
1. Você pode especificar como os componentes ausentes na representação de texto serão definidos na data e hora.

Os métodos <xref:System.DateTime.Parse%2A> e <xref:System.DateTime.TryParse%2A> convertem muitas representações comuns de data e hora. Os métodos <xref:System.DateTime.ParseExact%2A> e <xref:System.DateTime.TryParseExact%2A> convertem uma representação em cadeia de caracteres que cumpra o padrão especificado por uma cadeia de caracteres de formato de data e hora. (Consulte os artigos sobre [cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md) e [cadeias de caracteres de formato de data e hora personalizadas](custom-date-and-time-format-strings.md) para maiores detalhes).

O objeto <xref:System.Globalization.DateTimeFormatInfo> atual fornece mais controle sobre como o texto deve ser interpretado como data e hora. As propriedades de um <xref:System.Globalization.DateTimeFormatInfo> descrevem os separadores de data e hora e os nomes dos meses, dias e eras, bem como o formato das designações "AM" e "PM". A cultura do thread atual fornece um <xref:System.Globalization.DateTimeFormatInfo> que representa a cultura atual. Se você quer uma cultura específica ou configurações personalizadas, é necessário especificar o parâmetro <xref:System.IFormatProvider> de um método de análise. Para o parâmetro <xref:System.IFormatProvider>, especifique um objeto <xref:System.Globalization.CultureInfo>, o qual representa uma cultura ou um objeto <xref:System.Globalization.DateTimeFormatInfo>.

O texto que representa uma data ou hora pode ter algumas informações ausentes. Por exemplo, a maioria das pessoas supõe que a data "12 de março" representa o ano atual. Da mesma forma, "Março de 2018" representa o mês de março do ano 2018. O texto que representa a hora geralmente só inclui horas, minutos e uma designação de AM/PM.  Os métodos de análise lidam com essas informações ausentes usando alguns padrões razoáveis:

- Quando apenas a hora estiver presente, a parte de data usará a data atual.
- Quando houver apenas uma data, a parte das horas será meia-noite.
- Quando o ano não for especificado em uma data, o ano atual será usado.
- Quando o dia do mês não for especificado, o primeiro dia do mês será usado.

Se a data estiver presente na cadeia de caracteres, ele deverá incluir o mês e, pelo menos, o dia ou o ano. Se a hora estiver presente, ela deverá incluir a hora e os minutos ou o designador AM/PM.

Você pode especificar a constante <xref:System.Globalization.DateTimeStyles.NoCurrentDateDefault> para substituir esses padrões. Ao usar essa constante, as propriedades ausentes de ano, mês ou dia serão definidas com o valor `1`. O [último exemplo](#styles-example) usando <xref:System.DateTime.Parse%2A> demonstra esse comportamento.

Além de um componente de data e hora, a representação de cadeia de caracteres de data e hora pode incluir um deslocamento que indique a diferença de tempo em relação ao UTC (Tempo Universal Coordenado). Por exemplo, a cadeia de caracteres “2/14/2007 5:32:00 -7:00” define um horário sete horas antes do UTC. Se um deslocamento for omitido da representação de cadeia de caracteres de um horário, a análise retornará um <xref:System.DateTime> do objeto com a propriedade <xref:System.DateTime.Kind%2A> definida como <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>. Se um deslocamento for especificado, a análise retorna um objeto <xref:System.DateTime> com a propriedade <xref:System.DateTime.Kind%2A> definida como <xref:System.DateTimeKind.Local?displayProperty=nameWithType> e seu valor ajustado de acordo com o fuso horário local do seu computador. É possível modificar esse comportamento usando um valor <xref:System.Globalization.DateTimeStyles> com o método de análise.

O provedor de formato também é usado para interpretar uma data numérica ambígua. Não está claro quais componentes da data representada pela cadeia de caracteres "02/03/04" são o mês, o dia e o ano. Os componentes são interpretados de acordo com a ordem dos formatos de data semelhantes no provedor de formato.

## <a name="parse"></a>Analisar

O exemplo a seguir ilustra o uso do método <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> para converter uma `string` em um <xref:System.DateTime>. Este exemplo usa a cultura associada com o thread atual. Se o <xref:System.Globalization.CultureInfo> associado à cultura atual não puder analisar a cadeia de caracteres de entrada, um <xref:System.FormatException> será gerado.

> [!TIP]
> Todos os exemplos de C# neste artigo são executados no navegador. Pressione o botão **Executar** para ver a saída. Você também pode editá-los para experimentar como quiser.

> [!NOTE]
> Esses exemplos estão disponíveis no repositório de documentos do GitHub para [C#](https://github.com/dotnet/docs/tree/master/samples/snippets/csharp/how-to/conversions) e [Visual Basic](https://github.com/dotnet/docs/tree/master/samples/snippets/visualbasic/how-to/conversions).

[!code-csharp-interactive[Parsing.DateAndTime#1](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#1)]
[!code-vb[Parsing.DateAndTime#1](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#1)]

É possível definir explicitamente a cultura cujas convenções de formatação serão usadas ao analisar uma cadeia de caracteres. Você especifica um dos objetos <xref:System.Globalization.DateTimeFormatInfo> padrão retornados pela propriedade <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType>. O exemplo seguir usa um provedor de formato para analisar uma cadeia de caracteres em alemão em um <xref:System.DateTime>. Ele cria um <xref:System.Globalization.CultureInfo> que representa a cultura `de-DE`. Esse objeto `CultureInfo` garante o sucesso da análise dessa cadeia de caracteres específica. Isso impede qualquer configuração que está no <xref:System.Threading.Thread.CurrentCulture> do <xref:System.Threading.Thread.CurrentThread>.

[!code-csharp-interactive[Parsing.DateAndTime#2](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#2)]
[!code-vb[Parsing.DateAndTime#2](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#2)]

No entanto, apesar de ser possível usar sobrecargas do método <xref:System.DateTime.Parse%2A> para especificar provedores de formato personalizados, o método não é compatível com a análise de formatos não padrão. Para analisar data e hora expressadas em um formato não padrão, use o método <xref:System.DateTime.ParseExact%2A>.

<a name="styles-example"></a>O exemplo a seguir usa a enumeração <xref:System.Globalization.DateTimeStyles> para especificar que as informações de data e hora atuais não devem ser adicionadas ao <xref:System.DateTime> nos campos não especificados.

[!code-csharp-interactive[Parsing.DateAndTime#3](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#3)]
[!code-vb[Parsing.DateAndTime#3](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#3)]

## <a name="parseexact"></a>ParseExact

O método <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> converte uma cadeia de caracteres em um objeto <xref:System.DateTime> se ele estiver em conformidade com um dos padrões da cadeia de caracteres especificada. Quando uma cadeia de caracteres que não é de uma das formas especificadas é passada para esse método, é lançada uma <xref:System.FormatException>. Você pode especificar um dos especificadores de formato de data e hora padrão ou uma combinação dos especificadores de formato personalizados. Usando os especificadores de formato personalizados, é possível construir uma cadeia de caracteres de reconhecimento personalizada. Para ver uma explicação dos especificadores, consulte os tópicos [cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md) e [cadeias de caracteres de formato de data e hora personalizadas](custom-date-and-time-format-strings.md).

No exemplo a seguir, o método <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> recebe um objeto de cadeia de caracteres para analisar, seguido por um especificador de formato, seguido por um objeto <xref:System.Globalization.CultureInfo>. Esse método <xref:System.DateTime.ParseExact%2A> só consegue analisar cadeias de caracteres que seguem o padrão de data por extenso na cultura `en-US`.

[!code-csharp-interactive[Parsing.DateAndTime#4](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#4)]
[!code-vb[Parsing.DateAndTime#4](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#4)]

Cada sobrecarga dos métodos <xref:System.DateTime.Parse%2A> e <xref:System.DateTime.ParseExact%2A> também tem um parâmetro <xref:System.IFormatProvider> que oferece informações específicas da cultura sobre a formatação da cadeia de caracteres. Esse objeto <xref:System.IFormatProvider> é um objeto <xref:System.Globalization.CultureInfo> que representa uma cultura padrão ou um objeto <xref:System.Globalization.DateTimeFormatInfo> que é retornado pela propriedade <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType>.  O <xref:System.DateTime.ParseExact%2A> também usa uma cadeia de caracteres adicional ou um argumento de matriz de cadeia de caracteres que define um ou mais formatos de data e hora personalizados.

## <a name="see-also"></a>Confira também

- [Analisando cadeias de caracteres](parsing-strings.md)
- [Tipos de formatação](formatting-types.md)
- [Conversão de tipo no .NET](type-conversion.md)
- [Formatos de data e hora padrão](standard-date-and-time-format-strings.md)
- [Cadeias de caracteres de formato de data e hora personalizado](custom-date-and-time-format-strings.md)
