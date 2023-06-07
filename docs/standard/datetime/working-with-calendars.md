---
title: Trabalhar com calendários
ms.date: 04/01/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- globalization [.NET], calendars
- calendars, global applications
- global applications, calendars
- world-ready applications, calendars
- international applications [.NET], calendars
- culture, calendars
ms.assetid: 0c1534e5-979b-4c8a-a588-1c24301aefb3
ms.openlocfilehash: d15bafd097c72515a33bed7dd85d88aef54246ba
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280888"
---
# <a name="work-with-calendars"></a>Trabalhar com calendários

Embora um valor de data e hora representem um momento, sua representação de cadeia de caracteres depende da cultura e também das convenções usadas para exibir valores de data e hora por uma cultura específica e do calendário usado por essa cultura. Este tópico explora o suporte para calendários no .NET e discute o uso das classes de calendário ao trabalhar com valores de data.

## <a name="calendars-in-net"></a>Calendários no .NET

Todos os calendários no .NET derivam da <xref:System.Globalization.Calendar?displayProperty=nameWithType> classe, que fornece a implementação do calendário base. Uma das classes que herda da classe <xref:System.Globalization.Calendar> é a classe <xref:System.Globalization.EastAsianLunisolarCalendar>, a qual é a classe base para todos os calendários lunissolares. O .NET inclui as seguintes implementações de calendário:

- <xref:System.Globalization.ChineseLunisolarCalendar>, que representa o calendário lunissolar chinês.

- <xref:System.Globalization.GregorianCalendar>, que representa o calendário gregoriano. Esse calendário é particionado em subtipos adicionais (como árabe e francês do Oriente Médio) que são definidos pela enumeração <xref:System.Globalization.GregorianCalendarTypes?displayProperty=nameWithType>. A propriedade <xref:System.Globalization.GregorianCalendar.CalendarType%2A?displayProperty=nameWithType> especifica o subtipo do calendário gregoriano.

- <xref:System.Globalization.HebrewCalendar>, que representa o calendário hebraico.

- <xref:System.Globalization.HijriCalendar>, que representa o calendário islâmico.

- <xref:System.Globalization.JapaneseCalendar>, que representa o calendário japonês.

- <xref:System.Globalization.JapaneseLunisolarCalendar>, que representa o calendário lunissolar japonês.

- <xref:System.Globalization.JulianCalendar>, que representa o calendário juliano.

- <xref:System.Globalization.KoreanCalendar>, que representa o calendário coreano.

- <xref:System.Globalization.KoreanLunisolarCalendar>, que representa o calendário lunissolar coreano.

- <xref:System.Globalization.PersianCalendar>, que representa o calendário persa.

- <xref:System.Globalization.TaiwanCalendar>, que representa o calendário de Taiwan.

- <xref:System.Globalization.TaiwanLunisolarCalendar>, que representa o calendário lunissolar de Taiwan.

- <xref:System.Globalization.ThaiBuddhistCalendar>, que representa o calendário tailandês budista.

- <xref:System.Globalization.UmAlQuraCalendar>, que representa o calendário Um Al Qura.

Um calendário pode ser usado em uma de duas formas:

- Como o calendário usado por uma cultura específica. Cada objeto <xref:System.Globalization.CultureInfo> contém um calendário atual, que é o calendário que o objeto está usando no momento. As representações de cadeia de caracteres de todos os valores de data e hora refletem automaticamente a cultura e seu calendário atual. Normalmente, o calendário atual é o calendário padrão da cultura. <xref:System.Globalization.CultureInfo>os objetos também têm calendários opcionais, que incluem calendários adicionais que a cultura pode usar.

- Como calendário autônomo independente de uma cultura específica. Nesse caso, os métodos <xref:System.Globalization.Calendar> são usados para expressar datas como os valores que refletem o calendário.

Observe que seis classes de calendário – <xref:System.Globalization.ChineseLunisolarCalendar>, <xref:System.Globalization.JapaneseLunisolarCalendar>, <xref:System.Globalization.JulianCalendar>, <xref:System.Globalization.KoreanLunisolarCalendar>, <xref:System.Globalization.PersianCalendar> e <xref:System.Globalization.TaiwanLunisolarCalendar> – podem ser usadas apenas como calendários autônomos. Elas não são usadas por nenhuma cultura, seja como o calendário padrão ou como um calendário opcional.

## <a name="calendars-and-cultures"></a>Calendários e culturas

Cada cultura tem um calendário padrão, o qual é definido pela propriedade <xref:System.Globalization.CultureInfo.Calendar%2A?displayProperty=nameWithType>. A propriedade <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> retorna uma matriz de objetos <xref:System.Globalization.Calendar> que especifica todos os calendários suportados por uma cultura específica, inclusive o calendário padrão da cultura.

O exemplo a seguir ilustra as propriedades <xref:System.Globalization.CultureInfo.Calendar%2A?displayProperty=nameWithType> e <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType>. Ele cria objetos `CultureInfo` para as culturas Tailandês (Tailândia) e Japonês (Japão) e exibe seus calendários padrão e opcionais. Observe que, em ambos os casos, o calendário padrão da cultura também é incluído na coleção <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType>.

[!code-csharp[Conceptual.Calendars#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/calendarinfo1.cs#1)]
[!code-vb[Conceptual.Calendars#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/calendarinfo1.vb#1)]

O calendário em uso no momento por um objeto <xref:System.Globalization.CultureInfo> específico é definido pela propriedade <xref:System.Globalization.DateTimeFormatInfo.Calendar%2A?displayProperty=nameWithType> da cultura. O objeto <xref:System.Globalization.DateTimeFormatInfo> de uma cultura é retornado pela propriedade <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType>. Quando uma cultura é criada, o valor padrão é o mesmo valor da propriedade <xref:System.Globalization.CultureInfo.Calendar%2A?displayProperty=nameWithType>. No entanto, você pode mudar o calendário atual da cultura para qualquer calendário contido na matriz retornada pela propriedade <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType>. Se você tentar definir o calendário atual como um calendário que não está incluído no valor da propriedade <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType>, uma <xref:System.ArgumentException> será gerada.

O exemplo a seguir altera o calendário usado pela cultura Árabe (Arábia Saudita). Ele primeiro cria uma instância de um valor <xref:System.DateTime> e o exibe usando a cultura atual – que, nesse caso, é Inglês (Estados Unidos) – e o calendário atual da cultura (que, nesse caso, é o calendário gregoriano). Em seguida, ele muda a cultura atual para Árabe (Arábia Saudita) e exibe a data usando seu calendário padrão Um Al Qura. Finalmente, ele chama o método `CalendarExists` para determinar se o calendário islâmico é suportado pela cultura Árabe (Arábia Saudita). Como o calendário é suportado, ele muda o calendário atual para Hijri e, novamente, exibe a data. Observe que, em cada caso, a data é exibida usando o calendário atual da cultura atual.

[!code-csharp[Conceptual.Calendars#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/changecalendar2.cs#2)]
[!code-vb[Conceptual.Calendars#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/changecalendar2.vb#2)]

## <a name="dates-and-calendars"></a>Datas e calendários

Com exceção dos construtores que incluem um parâmetro de tipo <xref:System.Globalization.Calendar> e permitem que os elementos de uma data (ou seja, mês, dia e ano), reflitam valores em um calendário designado, os valores de <xref:System.DateTime> e <xref:System.DateTimeOffset> sempre são baseados no calendário gregoriano. Isso significa, por exemplo, que a propriedade <xref:System.DateTime.Year%2A?displayProperty=nameWithType> retorna o ano no calendário gregoriano e que a propriedade <xref:System.DateTime.Day%2A?displayProperty=nameWithType> retorna o dia do mês no calendário gregoriano.

> [!IMPORTANT]
> É importante lembrar que há uma diferença entre um valor de data e sua representação de cadeia de caracteres. O primeiro baseia-se no calendário gregoriano; o último baseia-se no calendário atual de uma determinada cultura.

O exemplo a seguir ilustra a diferença entre as propriedades <xref:System.DateTime> e seus métodos <xref:System.Globalization.Calendar> correspondentes. No exemplo, a cultura atual é Árabe (Egito), e o calendário atual é Um Al Qura. Um valor de <xref:System.DateTime> é definido como o décimo quinto dia do sétimo mês de 2011. Está claro que esse é interpretado como uma data gregoriana, pois esses mesmos valores são retornados pelo método <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> quando as convenções da cultura invariável são usadas. A representação de cadeia de caracteres da data que é formatada usando as convenções de cultura atual é 14/08/32, que é a data equivalente no calendário Um Al Qura. Em seguida, membros de `DateTime` e `Calendar` são usados para retornar o dia, mês e o ano do valor de <xref:System.DateTime>. Em cada caso, os valores retornados pelos membros de <xref:System.DateTime> refletem os valores do calendário gregoriano, enquanto que os valores retornados pelos membros de <xref:System.Globalization.UmAlQuraCalendar> refletem os valores do calendário Um Al Qura.

[!code-csharp[Conceptual.Calendars#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/datesandcalendars2.cs#3)]
[!code-vb[Conceptual.Calendars#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/datesandcalendars2.vb#3)]

### <a name="instantiate-dates-based-on-a-calendar"></a>Instanciar datas com base em um calendário

Como os valores <xref:System.DateTime> e <xref:System.DateTimeOffset> baseiam-se no calendário gregoriano, você deve chamar um construtor sobrecarregado que inclua um parâmetro do tipo <xref:System.Globalization.Calendar> para criar uma instância de um valor de data se você quiser usar os valores de dia, mês ou ano em um calendário diferente. Você também pode chamar uma das sobrecargas do método <xref:System.Globalization.Calendar.ToDateTime%2A?displayProperty=nameWithType> de um calendário específico para criar uma instância de um objeto <xref:System.DateTime> com base nos valores de um calendário específico.

O exemplo a seguir cria uma instância de um valor de <xref:System.DateTime> ao passar um objeto <xref:System.Globalization.HebrewCalendar> para um construtor de <xref:System.DateTime> e cria uma instância de um segundo valor <xref:System.DateTime> ao chamar o método <xref:System.Globalization.HebrewCalendar.ToDateTime%28System.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Int32%29?displayProperty=nameWithType>. Como os dois valores são criados com valores idênticos do calendário hebraico, a chamada ao método <xref:System.DateTime.Equals%2A?displayProperty=nameWithType> mostra que os dois valores de <xref:System.DateTime> são iguais.

[!code-csharp[Conceptual.Calendars#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/instantiatehcdate1.cs#4)]
[!code-vb[Conceptual.Calendars#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/instantiatehcdate1.vb#4)]

### <a name="represent-dates-in-the-current-calendar"></a>Representar datas no calendário atual

Os métodos de formatação de data e hora sempre usam o calendário atual ao converter datas em cadeias de caracteres. Isso significa que a representação de cadeia de caracteres do ano, mês e dia do mês reflete o calendário atual, e não necessariamente o calendário gregoriano.

O exemplo a seguir mostra como o calendário atual afeta a representação de cadeia de caracteres de uma data. Ele muda a cultura atual de Chinês (Tradicional, Taiwan) e cria uma instância de um valor de data. Ele então exibe o calendário e a data atuais, altera o calendário atual para <xref:System.Globalization.TaiwanCalendar> e exibe o calendário e a data atuais mais uma vez. Na primeira vez que a data é exibida, ela é representada como uma data no calendário gregoriano. Na segunda vez que a data é exibida, ela é representada como uma data no calendário de Taiwan.

[!code-csharp[Conceptual.Calendars#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/currentcalendar1.cs#5)]
[!code-vb[Conceptual.Calendars#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/currentcalendar1.vb#5)]

### <a name="represent-dates-in-a-non-current-calendar"></a>Representar datas em um calendário não atual

Para representar uma data usando um calendário que não é o calendário atual de uma cultura específica, você deve chamar métodos do objeto <xref:System.Globalization.Calendar>. Por exemplo, os métodos <xref:System.Globalization.Calendar.GetYear%2A?displayProperty=nameWithType>, <xref:System.Globalization.Calendar.GetMonth%2A?displayProperty=nameWithType> e <xref:System.Globalization.Calendar.GetDayOfMonth%2A?displayProperty=nameWithType> convertem o ano, o mês e o dia para valores que refletem um calendário específico.

> [!WARNING]
> Como alguns calendários são calendários não opcionais de qualquer cultura, representar datas nesses calendários sempre exige que você chame métodos de calendário. Isso é verdadeiro para todos os calendários que derivam das classes <xref:System.Globalization.EastAsianLunisolarCalendar>, de <xref:System.Globalization.JulianCalendar> e <xref:System.Globalization.PersianCalendar>.

O exemplo a seguir usa um objeto <xref:System.Globalization.JulianCalendar> para criar uma instância de uma data, 9 de janeiro de 1905, no calendário juliano. Quando essa data é exibida no calendário gregoriano (padrão), ela é representada como 22 de janeiro de 1905. Chamadas para métodos <xref:System.Globalization.JulianCalendar> individuais permitem que a data seja representada no calendário juliano.

[!code-csharp[Conceptual.Calendars#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/noncurrentcalendar1.cs#6)]
[!code-vb[Conceptual.Calendars#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/noncurrentcalendar1.vb#6)]

### <a name="calendars-and-date-ranges"></a>Calendários e intervalos de datas

A data mais antiga suportada por um calendário é indicada pela propriedade <xref:System.Globalization.Calendar.MinSupportedDateTime%2A?displayProperty=nameWithType> desse calendário. Para a classe <xref:System.Globalization.GregorianCalendar>, essa data é 1º de janeiro de 0001. C.E. A maioria dos outros calendários no .NET oferece suporte a uma data posterior. Tentar trabalhar com um valor de data e hora que antecedem a data com suporte mais antiga de um calendário gerará uma exceção <xref:System.ArgumentOutOfRangeException>.

Porém, há uma exceção importante. O valor padrão (não inicializado) de um objeto <xref:System.DateTime> e um objeto <xref:System.DateTimeOffset> é igual ao valor de <xref:System.Globalization.GregorianCalendar.MinSupportedDateTime%2A?displayProperty=nameWithType>. Se você tentar formatar essa data em um calendário que não dá suporte a 1º de janeiro de 0001 C.E. e você não fornece um especificador de formato, o método de formatação usa o especificador de formato "s" (padrão de data/hora classificável) em vez do especificador de formato "G" (padrão de data/hora geral). Como resultado, a operação de formatação não gerará uma exceção <xref:System.ArgumentOutOfRangeException>. Em vez disso, retornará uma data sem suporte. Isso é ilustrado no exemplo a seguir, que exibe o valor de <xref:System.DateTime.MinValue?displayProperty=nameWithType> quando a cultura atual é configurada para Japonês (Japão) com o calendário japonês, e para Árabe (Egito) com o calendário Um Al Qura. Ela também define a cultura atual como Inglês (Estados Unidos) e chama o método <xref:System.DateTime.ToString%28System.IFormatProvider%29?displayProperty=nameWithType> com cada um desses objetos <xref:System.Globalization.CultureInfo>. Em cada caso, a data é exibida usando o padrão classificável de data/hora.

[!code-csharp[Conceptual.Calendars#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/minsupporteddatetime1.cs#11)]
[!code-vb[Conceptual.Calendars#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/minsupporteddatetime1.vb#11)]

## <a name="work-with-eras"></a>Trabalhar com apagar

Os calendários normalmente dividem as datas em eras. No entanto, as <xref:System.Globalization.Calendar> classes no .net não oferecem suporte a toda era definida por um calendário, e a maioria das <xref:System.Globalization.Calendar> classes dá suporte apenas a uma única época. Somente as classes <xref:System.Globalization.JapaneseCalendar> e <xref:System.Globalization.JapaneseLunisolarCalendar> oferecem suporte a várias eras.

> [!IMPORTANT]
> A era Reiwa, uma nova era no <xref:System.Globalization.JapaneseCalendar> e <xref:System.Globalization.JapaneseLunisolarCalendar> , começa em 1º de maio de 2019. Essa alteração afeta todos os aplicativos que usam esses calendários. Veja os seguintes artigos para obter mais informações:
>
> - [Tratar de uma nova era no calendário do japonês no .net](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/), que documenta os recursos adicionados ao .net para dar suporte a calendários com vários apagar e discute as práticas recomendadas a serem usadas na manipulação de calendários de várias seleções.
> - [Prepare seu aplicativo para a alteração de era japonesa](/windows/uwp/design/globalizing/japanese-era-change), que fornece informações sobre como testar seus aplicativos no Windows para garantir a prontidão da alteração de era.
> - [Resumo de novas atualizações de era japonesas para .NET Framework](https://support.microsoft.com/help/4477957/new-japanese-era-updates-for-net-framework), que lista .NET Framework atualizações para versões individuais do Windows relacionadas à nova era do calendário japonês, observa novos recursos de .NET Framework para suporte de várias versões e inclui itens a serem procurados no teste de seus aplicativos.

Uma era na maioria dos calendários denota um período de tempo muito longo. No calendário gregoriano, por exemplo, a era atual se estende por mais de duas milênios. Para o <xref:System.Globalization.JapaneseCalendar> e o <xref:System.Globalization.JapaneseLunisolarCalendar> , os dois calendários que dão suporte a vários apagar, esse não é o caso. Uma era corresponde ao período do Reino de um imperador. O suporte para vários apagamentos, especialmente quando o limite superior da era atual é desconhecido, apresenta desafios especiais.

### <a name="eras-and-era-names"></a>Apagar e nomes de era

No .NET, os inteiros que representam o apagamento com suporte de uma implementação de calendário em particular são armazenados em ordem inversa na <xref:System.Globalization.Calendar.Eras%2A?displayProperty=nameWithType> matriz. A era atual (que é a era com o intervalo de tempo mais recente) está no índice zero e para <xref:System.Globalization.Calendar> classes que dão suporte a vários apagamentos, cada índice sucessivo reflete a era anterior. A propriedade estática <xref:System.Globalization.Calendar.CurrentEra?displayProperty=nameWithType> define o índice de era atual na matriz <xref:System.Globalization.Calendar.Eras%2A?displayProperty=nameWithType> ; ela é uma constante cujo valor é sempre zero. As classes <xref:System.Globalization.Calendar> individuais também incluem os campos estáticos que retornam o valor da era atual. Elas são listadas na tabela a seguir.

| Classe do calendário                                        | Campo de era atual                                                 |
| ----------------------------------------------------- | ----------------------------------------------------------------- |
| <xref:System.Globalization.ChineseLunisolarCalendar>  | <xref:System.Globalization.ChineseLunisolarCalendar.ChineseEra>   |
| <xref:System.Globalization.GregorianCalendar>         | <xref:System.Globalization.GregorianCalendar.ADEra>               |
| <xref:System.Globalization.HebrewCalendar>            | <xref:System.Globalization.HebrewCalendar.HebrewEra>              |
| <xref:System.Globalization.HijriCalendar>             | <xref:System.Globalization.HijriCalendar.HijriEra>                |
| <xref:System.Globalization.JapaneseLunisolarCalendar> | <xref:System.Globalization.JapaneseLunisolarCalendar.JapaneseEra> |
| <xref:System.Globalization.JulianCalendar>            | <xref:System.Globalization.JulianCalendar.JulianEra>              |
| <xref:System.Globalization.KoreanCalendar>            | <xref:System.Globalization.KoreanCalendar.KoreanEra>              |
| <xref:System.Globalization.KoreanLunisolarCalendar>   | <xref:System.Globalization.KoreanLunisolarCalendar.GregorianEra>  |
| <xref:System.Globalization.PersianCalendar>           | <xref:System.Globalization.PersianCalendar.PersianEra>            |
| <xref:System.Globalization.ThaiBuddhistCalendar>      | <xref:System.Globalization.ThaiBuddhistCalendar.ThaiBuddhistEra>  |
| <xref:System.Globalization.UmAlQuraCalendar>          | <xref:System.Globalization.UmAlQuraCalendar.UmAlQuraEra>          |

O nome que corresponde a um número de era específico não pode ser recuperado com a passagem do número da era para o método <xref:System.Globalization.DateTimeFormatInfo.GetEraName%2A?displayProperty=nameWithType> ou <xref:System.Globalization.DateTimeFormatInfo.GetAbbreviatedEraName%2A?displayProperty=nameWithType>. O exemplo a seguir chama esses métodos para recuperar informações sobre o suporte a eras na classe <xref:System.Globalization.GregorianCalendar>. Ele exibe a data do calendário gregoriano que corresponde a 1º de Janeiro do segundo ano da era atual, bem como a data do calendário gregoriano que corresponde a 1º de Janeiro do segundo ano de cada era do calendário japonês com suporte.

[!code-csharp[Conceptual.Calendars#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/instantiatewithera1.cs)]
[!code-vb[Conceptual.Calendars#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/instantiatewithera1.vb)]

Além disso, a cadeia de caracteres de formato de data e hora personalizado "g" inclui o nome da era na representação de cadeia de caracteres de uma data e hora. Para obter mais informações, consulte [cadeias de caracteres de formato personalizado de data e hora](../base-types/custom-date-and-time-format-strings.md).

### <a name="instantiatie-a-date-with-an-era"></a>Instanciar uma data com uma época

Para as duas <xref:System.Globalization.Calendar> classes que dão suporte a vários apagamentos, uma data que consiste em um determinado ano, mês e dia do valor do mês pode ser ambígua. Por exemplo, todos os apagados têm suporte do <xref:System.Globalization.JapaneseCalendar> que têm anos cujo número é 1. Normalmente, se uma era não é especificada, os métodos de data e hora e calendário assumem que os valores pertencem à era atual. Isso é verdadeiro nos <xref:System.DateTime.%23ctor%2A> construtores e <xref:System.DateTimeOffset.%23ctor%2A> que incluem parâmetros do tipo <xref:System.Globalization.Calendar> , bem como os métodos [JapaneseCalendar. ToDateTime](xref:System.Globalization.Calendar.ToDateTime(System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32)) e [JapaneseLunisolarCalendar. ToDateTime](xref:System.Globalization.Calendar.ToDateTime(System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32)) . O exemplo a seguir instancia uma data que representa 1 de Janeiro do segundo ano de uma era não especificada. Se você executar o exemplo quando a era Reiwa for a época atual, a data será interpretada como o segundo ano da era Reiwa. A era, 令和, precede o ano na cadeia de caracteres retornada pelo <xref:System.DateTime.ToString(System.String,System.IFormatProvider)?displayProperty=nameWithType> método e corresponde a 1º de janeiro de 2020, no calendário gregoriano. (A era Reiwa começa no ano 2019 do calendário gregoriano.)

[!code-csharp[A date in the current era](~/samples/snippets/standard/datetime/calendars/current-era/cs/program.cs)]
[!code-vb[A date in the current era](~/samples/snippets/standard/datetime/calendars/current-era/vb/program.vb)]

No entanto, se a era for alterada, a intenção desse código se tornará ambígua. A data deve representar o segundo ano da era atual ou se pretende representar o segundo ano da era do Heisei? Há duas maneiras de evitar essa ambiguidade:

- Crie uma instância do valor de data e hora usando a <xref:System.Globalization.GregorianCalendar> classe padrão. Você pode usar o calendário japonês ou o calendário lunisolar japonês para a representação de cadeia de caracteres de datas, como mostra o exemplo a seguir.

  [!code-csharp[Insantiating a Gregorian date](~/samples/snippets/standard/datetime/calendars/gregorian/cs/program.cs)]
  [!code-vb[Instantiating a Gregorian date](~/samples/snippets/standard/datetime/calendars/gregorian/vb/program.vb)]

- Chame um método de data e hora que especifica explicitamente uma era. Entre elas estão os seguintes métodos:

  - O <xref:System.Globalization.Calendar.ToDateTime(System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32)> método da <xref:System.Globalization.JapaneseCalendar> classe ou <xref:System.Globalization.JapaneseLunisolarCalendar> .

  - Um <xref:System.DateTime> <xref:System.DateTimeOffset> método de análise, como,, <xref:System.DateTime.Parse%2A> <xref:System.DateTime.TryParse%2A> <xref:System.DateTime.ParseExact%2A> ou <xref:System.DateTime.TryParseExact%2A> , que inclui a cadeia de caracteres a ser analisada e, opcionalmente, um <xref:System.Globalization.DateTimeStyles> argumento se a cultura atual for Japanese-Japão ("ja-JP") e o calendário da cultura for o <xref:System.Globalization.JapaneseCalendar> . A cadeia de caracteres a ser analisada deve incluir a era.

  - Um <xref:System.DateTime> <xref:System.DateTimeOffset> método de análise ou que inclui um `provider` parâmetro do tipo <xref:System.IFormatProvider> . `provider`deve ser um <xref:System.Globalization.CultureInfo> objeto que representa a cultura japonesa-Japão ("ja-JP") cujo calendário atual é <xref:System.Globalization.JapaneseCalendar> ou um <xref:System.Globalization.DateTimeFormatInfo> objeto cuja <xref:System.Globalization.DateTimeFormatInfo.Calendar> propriedade é <xref:System.Globalization.JapaneseCalendar> . A cadeia de caracteres a ser analisada deve incluir a era.

  O exemplo a seguir usa três desses métodos para criar uma instância de data e hora na era Meiji, que começou em 8 de setembro de 1868 e terminou em 29 de julho de 1912.

  [!code-csharp[A date in a specified era](~/samples/snippets/standard/datetime/calendars/specify-era/cs/program.cs)]
  [!code-vb[A date in a specified era](~/samples/snippets/standard/datetime/calendars/specify-era/vb/program.vb)]

> [!TIP]
> Ao trabalhar com calendários que dão suporte a vários apagamentos, *sempre* use a data gregoriana para criar uma instância de uma data ou especifique a era quando você criar uma instância de uma data e hora com base nesse calendário.

Ao especificar uma era para o <xref:System.Globalization.Calendar.ToDateTime(System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32,System.Int32)> método, você fornece o índice da era na Propriedade do calendário <xref:System.Globalization.Calendar.Eras> . No entanto, para calendários cujo apagamento está sujeito a alterações, esses índices não são valores constantes; a era atual está no índice 0 e a era mais antiga está no índice `Eras.Length - 1` . Quando uma nova era adicionada a um calendário, os índices do apagamento anterior são aumentados em um. Você pode fornecer o índice de era apropriado da seguinte maneira:

- Para datas na era atual, sempre use a propriedade do calendário <xref:System.Globalization.Calendar.CurrentEra> .

- Para datas em uma era especificada, use o <xref:System.Globalization.DateTimeFormatInfo.GetEraName%2A?displayProperty=nameWithType> método para recuperar o índice que corresponde a um nome de era especificado. Isso requer que <xref:System.Globalization.JapaneseCalendar> seja o calendário atual do <xref:System.Globalization.CultureInfo> objeto que representa a cultura ja-JP.  (Essa técnica também funciona para o <xref:System.Globalization.JapaneseLunisolarCalendar> , pois ela dá suporte ao mesmo apagamento que o <xref:System.Globalization.JapaneseCalendar> .) O exemplo anterior ilustra essa abordagem.

### <a name="calendars-eras-and-date-ranges-relaxed-range-checks"></a>Calendários, apagar e intervalos de datas: verificações de intervalo relaxadas

Muito parecido com os calendários individuais têm suporte para intervalos de datas, o apagamento nas <xref:System.Globalization.JapaneseCalendar> <xref:System.Globalization.JapaneseLunisolarCalendar> classes e também tem intervalos com suporte. Anteriormente, o .NET usava verificações de intervalo de era estrito para garantir que uma data específica era dentro do intervalo dessa era. Ou seja, se uma data estiver fora do intervalo da era especificada, o método lançará um <xref:System.ArgumentOutOfRangeException> . Atualmente, o .NET usa a verificação de intervalo reduzida por padrão. As atualizações para todas as versões do .NET introduziram verificações de intervalo de era reduzida; a tentativa de criar uma instância de uma data específica que está fora do intervalo da era "estouros" de uma época especificada na seguinte era e nenhuma exceção é lançada.

O exemplo a seguir tenta criar uma instância de uma data no ano 65th da era Showa, que começou em 25 de dezembro de 1926 e terminou em 7 de janeiro de 1989. Essa data corresponde a 9 de janeiro de 1990, que está fora do intervalo da era de Showa no <xref:System.Globalization.JapaneseCalendar> . Como a saída do exemplo ilustra, a data exibida pelo exemplo é 9 de janeiro de 1990, no segundo ano da era do Heisei.

  [!code-csharp[Relaxed range checks](~/samples/snippets/standard/datetime/calendars/relaxed-range/cs/program.cs)]
  [!code-vb[Relaxed range checks](~/samples/snippets/standard/datetime/calendars/relaxed-range/vb/program.vb)]

Se as verificações de intervalo relaxadas não forem indesejáveis, você poderá restaurar verificações de intervalo estritas de várias maneiras, dependendo da versão do .NET na qual seu aplicativo está sendo executado:

- **.NET Core:** Adicione o seguinte ao arquivo de configuração *. NetCore. Runtime. JSON* :

  ```json
  "runtimeOptions": {
    "configProperties": {
        "Switch.System.Globalization.EnforceJapaneseEraYearRanges": true
    }
  }
  ```

- **.NET Framework 4,6 ou posterior:** Defina a seguinte opção AppContext no arquivo *app. config* :

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Globalization.EnforceJapaneseEraYearRanges=true" />
    </runtime>
  </configuration>
  ```

- **.NET Framework 4.5.2 ou anterior:** Defina o seguinte valor de registro:

   |  |  |
   |--|--|
   | **Chave** | **HKEY_LOCAL_MACHINE \Software\Microsoft \\ . NETFramework\AppContext** |
   | **Nome** | Switch. System. Globalization. EnforceJapaneseEraYearRanges |
   | **Tipo** | REG_SZ |
   | **Valor** | true |

Com as verificações de intervalo estrita habilitadas, o exemplo anterior gera um <xref:System.ArgumentOutOfRangeException> e exibe a seguinte saída:

```console
Unhandled Exception: System.ArgumentOutOfRangeException: Valid values are between 1 and 64, inclusive.
Parameter name: year
   at System.Globalization.GregorianCalendarHelper.GetYearOffset(Int32 year, Int32 era, Boolean throwOnError)
   at System.Globalization.GregorianCalendarHelper.ToDateTime(Int32 year, Int32 month, Int32 day, Int32 hour, Int32 minute, Int32 second, Int32 millisecond, Int32 era)
   at Example.Main()
```

### <a name="represent-dates-in-calendars-with-multiple-eras"></a>Representar datas em calendários com vários apagamentos

Se um objeto <xref:System.Globalization.Calendar> oferece suporte a eras e é o calendário atual de um objeto <xref:System.Globalization.CultureInfo>, a era está incluída na representação de cadeia de caracteres de um valor de data e hora para os padrões de data e hora completa, data completa e data abreviada. O exemplo a seguir exibe esses padrões de data quando a cultura atual é Japão (japanese) e o calendário atual é o calendário japonês.

[!code-csharp[Conceptual.Calendars#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/formatstrings1.cs#8)]
[!code-vb[Conceptual.Calendars#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/formatstrings1.vb#8)]

> [!WARNING]
> A <xref:System.Globalization.JapaneseCalendar> classe é a única classe Calendar no .NET que dá suporte a datas em mais de uma era e que pode ser o calendário atual de um <xref:System.Globalization.CultureInfo> objeto, especificamente, de um <xref:System.Globalization.CultureInfo> objeto que representa a cultura japonesa (Japão).

Para todos os calendários, o especificador de formato personalizado “g” inclui a era na cadeia de caracteres de resultado. O exemplo a seguir usa a cadeia de caracteres de formato personalizado "MM-dd-aaaa g" para incluir a era na cadeia de caracteres de resultado quando o calendário atual é o calendário gregoriano.

[!code-csharp[Conceptual.Calendars#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/formatstrings2.cs#9)]
[!code-vb[Conceptual.Calendars#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/formatstrings2.vb#9)]

Em casos em que a representação de cadeia de caracteres de uma data é expressa em um calendário que não é o calendário atual, a classe <xref:System.Globalization.Calendar> inclui um método <xref:System.Globalization.Calendar.GetEra%2A?displayProperty=nameWithType> que pode ser usado junto com <xref:System.Globalization.Calendar.GetYear%2A?displayProperty=nameWithType>, <xref:System.Globalization.Calendar.GetMonth%2A?displayProperty=nameWithType> e os métodos <xref:System.Globalization.Calendar.GetDayOfMonth%2A?displayProperty=nameWithType> para indicar inequivocamente uma data e a era à qual ela pertence. O exemplo a seguir usa a classe <xref:System.Globalization.JapaneseLunisolarCalendar> para fornecer uma ilustração. No entanto, observe que incluir um nome ou abreviação significativa em vez de um inteiro para a era na cadeia de caracteres de resultado exige que você crie uma instância de um objeto <xref:System.Globalization.DateTimeFormatInfo> e faça de <xref:System.Globalization.JapaneseCalendar> o calendário atual. (O calendário <xref:System.Globalization.JapaneseLunisolarCalendar> não pode ser o calendário atual de qualquer cultura, mas, nesse caso, os dois calendários compartilham as mesmas eras.)

[!code-csharp[Conceptual.Calendars#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.calendars/cs/formatstrings3.cs#10)]
[!code-vb[Conceptual.Calendars#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.calendars/vb/formatstrings3.vb#10)]

Nos calendários japoneses, o primeiro ano de uma era chamado de Gannen (元年). Por exemplo, em vez de Heisei 1, o primeiro ano da era Heisei pode ser descrito como Heisei Gannen. O .NET adota essa convenção em operações de formatação para datas e horas formatadas com as seguintes cadeias de caracteres de formato de data e hora personalizadas ou padrão quando são usadas com um <xref:System.Globalization.CultureInfo> objeto que representa a cultura japonesa-Japão ("ja-JP") com a <xref:System.Globalization.JapaneseCalendar> classe:

- [O padrão de data por extenso](../base-types/standard-date-and-time-format-strings.md#LongDate), indicado pela cadeia de caracteres de formato de data e hora padrão "D".
- [O padrão de data por hora completa](../base-types/standard-date-and-time-format-strings.md#FullDateLongTime), indicado pela cadeia de caracteres de formato de data e hora padrão "F".
- [O padrão de hora abreviada de data completa](../base-types/standard-date-and-time-format-strings.md#FullDateShortTime), indicado pela cadeia de caracteres de formato de data e hora padrão "f".
- [O padrão de ano/mês](../base-types/standard-date-and-time-format-strings.md#YearMonth), indicado pela cadeia de caracteres de formato de data e hora padrão Y "ou" y ".
- [A [cadeia de caracteres de formato de data e hora personalizada](../base-types/custom-date-and-time-format-strings.md)do "GGY" 年 "ou" ggy年 ".

Por exemplo, o exemplo a seguir exibe uma data no primeiro ano da era do Heisei no <xref:System.Globalization.JapaneseCalendar> .

  [!code-csharp[gannen](~/samples/snippets/standard/datetime/calendars/gannen/cs/program.cs)]
  [!code-vb[gannen](~/samples/snippets/standard/datetime/calendars/gannen/vb/gannen-fmt.vb)]

Se esse comportamento for indesejável em operações de formatação, você poderá restaurar o comportamento anterior, que sempre representa o primeiro ano de uma era como "1" em vez de "gannen", fazendo o seguinte, dependendo da versão do .NET:

- **.NET Core:** Adicione o seguinte ao arquivo de configuração *. NetCore. Runtime. JSON* :

  ```json
  "runtimeOptions": {
    "configProperties": {
        "Switch.System.Globalization.FormatJapaneseFirstYearAsANumber": true
    }
  }
  ```

- **.NET Framework 4,6 ou posterior:** Defina a seguinte opção AppContext no arquivo *app. config* :

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Globalization.FormatJapaneseFirstYearAsANumber=true" />
    </runtime>
  </configuration>
  ```

- **.NET Framework 4.5.2 ou anterior:** Defina o seguinte valor de registro:

   |  |  |
   |--|--|
   | **Chave** | **HKEY_LOCAL_MACHINE \Software\Microsoft \\ . NETFramework\AppContext** |
   | **Nome** | Switch. System. Globalization. FormatJapaneseFirstYearAsANumber |
   | **Tipo** | REG_SZ |
   | **Valor** | true |

Com o suporte do Gannen nas operações de formatação desabilitadas, o exemplo anterior exibe a seguinte saída:

```console
Japanese calendar date: 平成1年8月18日 (Gregorian: Friday, August 18, 1989)
```

O .NET também foi atualizado para que as operações de análise de data e hora ofereçam suporte a cadeias de caracteres que contêm o ano representado como "1" ou Gannen. Embora não seja necessário fazer isso, você pode restaurar o comportamento anterior para que ele reconheça apenas "1" como o primeiro ano de uma era. Você pode fazer isso da seguinte maneira, dependendo da versão do .NET:

- **.NET Core:** Adicione o seguinte ao arquivo de configuração *. NetCore. Runtime. JSON* :

  ```json
  "runtimeOptions": {
    "configProperties": {
        "Switch.System.Globalization.EnforceLegacyJapaneseDateParsing": true
    }
  }
  ```

- **.NET Framework 4,6 ou posterior:** Defina a seguinte opção AppContext no arquivo *app. config* :

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Globalization.EnforceLegacyJapaneseDateParsing=true" />
    </runtime>
  </configuration>
  ```

- **.NET Framework 4.5.2 ou anterior:** Defina o seguinte valor de registro:

   |  |  |
   |--|--|
   | **Chave** | **HKEY_LOCAL_MACHINE \Software\Microsoft \\ . NETFramework\AppContext** |
   | **Nome** | Switch. System. Globalization. EnforceLegacyJapaneseDateParsing |
   | **Tipo** | REG_SZ |
   | **Valor** | true |

## <a name="see-also"></a>Veja também

- [Como: exibir datas em calendários não gregorianos](../base-types/how-to-display-dates-in-non-gregorian-calendars.md)
- [Exemplo: utilitário de intervalo da semana do calendário](https://code.msdn.microsoft.com/NET-Framework-4-Calendar-3360a84a)
- [Classe do calendário](xref:System.Globalization.Calendar)
