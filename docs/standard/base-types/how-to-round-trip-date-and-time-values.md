---
title: 'Como: Valores de data e hora de viagem de ida e volta'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- round-trip date and time values
- dates [.NET Framework], round-trip values
- time zones [.NET Framework], round-trip date and time values
- time [.NET Framework], round-trip values
- formatting strings [.NET Framework], round-trip values
ms.assetid: b609b277-edc6-4c74-b03e-ea73324ecbdb
ms.openlocfilehash: 60483a6e29c65fc0c5803e8084053d53d9fc3c37
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290442"
---
# <a name="how-to-round-trip-date-and-time-values"></a>Como: Valores de data e hora de viagem de ida e volta

Em muitos aplicativos, um valor de data e hora destina-se a identificar sem ambiguidade um único ponto no tempo. Este artigo mostra como salvar e restaurar um <xref:System.DateTime> valor, um <xref:System.DateTimeOffset> valor e um valor de data e hora com informações de fuso horário para que o valor restaurado identifique a mesma hora que o valor salvo.

## <a name="round-trip-a-datetime-value"></a>Ida e volta de um valor DateTime

1. Converta o valor <xref:System.DateTime> em sua representação de cadeia de caracteres chamando o método <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> com o especificador de formato "o".

2. Salve a representação de cadeia de caracteres do valor <xref:System.DateTime> em um arquivo ou passe-a por um processo, domínio de aplicativo ou limite de computador.

3. Recupere a cadeia de caracteres que representa o valor <xref:System.DateTime>.

4. Chame o método <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> e passe <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType> como o valor do parâmetro `styles`.

O exemplo a seguir mostra como fazer a viagem de ida e volta de um valor <xref:System.DateTime>.

[!code-csharp[Formatting.HowTo.RoundTrip#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#1)]
[!code-vb[Formatting.HowTo.RoundTrip#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#1)]

Fazendo a viagem de ida e volta de um valor <xref:System.DateTime>, essa técnica consegue preservar o horário para todos os horários locais e universais. Por exemplo, se um <xref:System.DateTime> valor local for salvo em um sistema no fuso horário padrão do Pacífico americano e for restaurado em um sistema no fuso horário padrão dos EUA, a data e a hora restauradas serão duas horas posteriores à hora original, o que refletirá a diferença de tempo entre os dois fusos horários. No entanto, essa técnica não é necessariamente exata para horários não especificados. Todos os valores <xref:System.DateTime> cuja propriedade <xref:System.DateTime.Kind%2A> é <xref:System.DateTimeKind.Unspecified> são tratados como se fossem horários locais. Se não for uma hora local, o <xref:System.DateTime> não identificará com êxito o ponto no tempo correto. A solução alternativa para essa limitação é acoplar rigorosamente um valor de data e hora com seu fuso horário para salvar e restaurar a operação.

## <a name="round-trip-a-datetimeoffset-value"></a>Ida e volta de um valor de DateTimeOffset

1. Converta o valor <xref:System.DateTimeOffset> em sua representação de cadeia de caracteres chamando o método <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> com o especificador de formato "o".

2. Salve a representação de cadeia de caracteres do valor <xref:System.DateTimeOffset> em um arquivo ou passe-a por um processo, domínio de aplicativo ou limite de computador.

3. Recupere a cadeia de caracteres que representa o valor <xref:System.DateTimeOffset>.

4. Chame o método <xref:System.DateTimeOffset.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> e passe <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType> como o valor do parâmetro `styles`.

O exemplo a seguir mostra como fazer a viagem de ida e volta de um valor <xref:System.DateTimeOffset>.

[!code-csharp[Formatting.HowTo.RoundTrip#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#2)]
[!code-vb[Formatting.HowTo.RoundTrip#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#2)]

Essa técnica sempre identifica sem ambiguidade um valor <xref:System.DateTimeOffset> como um único momento. O valor pode ser convertido em UTC (Tempo Universal Coordenado) chamando o método <xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> ou pode ser convertido em determinado fuso horário chamando o método <xref:System.DateTimeOffset.ToOffset%2A?displayProperty=nameWithType> ou <xref:System.TimeZoneInfo.ConvertTime%28System.DateTimeOffset%2CSystem.TimeZoneInfo%29?displayProperty=nameWithType>. A principal limitação dessa técnica é que a aritmética de data e hora, quando executada em um valor <xref:System.DateTimeOffset> que representa o horário em determinado fuso horário, talvez não produza resultados precisos para esse fuso horário. Isso ocorre porque, quando um valor <xref:System.DateTimeOffset> é instanciado, é dissociado do fuso horário. Portanto, as regras de ajuste do fuso horário não podem mais ser aplicadas quando você executa cálculos de data e hora. É possível solucionar esse problema definindo um tipo personalizado que inclui um valor de data e hora e o respectivo fuso horário.

## <a name="round-trip-a-date-and-time-value-with-its-time-zone"></a>Ida e volta de um valor de data e hora com seu fuso horário

1. Defina uma classe ou uma estrutura com dois campos. O primeiro campo é um objeto <xref:System.DateTime> ou <xref:System.DateTimeOffset>, e o segundo é um objeto <xref:System.TimeZoneInfo>. O exemplo a seguir é uma versão simples desse tipo.

    [!code-csharp[Formatting.HowTo.RoundTrip#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#3)]
    [!code-vb[Formatting.HowTo.RoundTrip#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#3)]

2. Marcar a classe com o atributo <xref:System.SerializableAttribute>.

3. Serialize o objeto usando o método <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType>.

4. Restaure o objeto usando o método <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A>.

5. Transmita (em C#) ou converta (em Visual Basic) o objeto desserializado em um objeto do tipo apropriado.

O exemplo a seguir ilustra como fazer uma viagem de ida e volta de um objeto que armazena as informações de fuso horário e data e hora.

[!code-csharp[Formatting.HowTo.RoundTrip#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#4)]
[!code-vb[Formatting.HowTo.RoundTrip#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#4)]

Essa técnica deve refletir sempre inequivocamente o ponto no tempo correto, antes e depois de ser salvo e restaurado, desde que a implementação do objeto combinado de data e hora e fuso horário não permita que o valor de data fique fora de sincronia com o valor de fuso horário.

## <a name="compile-the-code"></a>Compilar o código

Estes exemplos exigem que:

- Os namespaces a seguir são importados com `using` diretivas C# ou `Imports` instruções Visual Basic:

  - <xref:System>(Somente C#)

  - <xref:System.Globalization?displayProperty=nameWithType>

  - <xref:System.IO?displayProperty=nameWithType>

  - <xref:System.Runtime.Serialization?displayProperty=nameWithType>

  - <xref:System.Runtime.Serialization.Formatters.Binary?displayProperty=nameWithType>

- Cada exemplo de código, diferente da `DateInTimeZone` classe, ser incluído em uma classe ou Visual Basic módulo, encapsulado em métodos e chamado a partir do `Main` método.

## <a name="see-also"></a>Veja também

- [Escolhendo entre DateTime, DateTimeOffset, TimeSpan e TimeZoneInfo](../datetime/choosing-between-datetime.md)
- [Cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md)
