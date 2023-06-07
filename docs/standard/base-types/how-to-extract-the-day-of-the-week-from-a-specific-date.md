---
title: 'Como: Extrair o dia da semana de uma data específica'
description: Saiba como determinar o dia ordinal da semana para uma determinada data no .NET. Saiba como exibir o nome do dia da semana localizado para uma determinada data.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [.NET Framework], dates
- DateTime.DayOfWeek property
- DateTime.ToString method
- dates [.NET Framework], retrieving week information
- DateTimeOffset.DayOfWeek property
- dates [.NET Framework], day of week
- Weekday function
- day of week [.NET Framework]
- extracting day of week
- weekday names
- WeekdayName function
- numbers [.NET Framework], day of week
- formatting [.NET Framework], time
- DateTimeOffset.ToString method
- full weekday names
ms.assetid: 1c9bef76-5634-46cf-b91c-9b9eb72091d7
ms.openlocfilehash: fa0eb6c36b88594543d08680af104b5408c295f9
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662609"
---
# <a name="how-to-extract-the-day-of-the-week-from-a-specific-date"></a>Como: Extrair o dia da semana de uma data específica
O .NET Framework facilita a verificação da posição numérica do dia da semana em uma determinada data e a exibição do nome do dia localizado em uma determinada data. O valor enumerado que indica o dia da semana correspondente a uma determinada data está disponível na propriedade <xref:System.DateTime.DayOfWeek%2A> ou <xref:System.DateTimeOffset.DayOfWeek%2A>. Em contrapartida, para recuperar o nome do dia da semana é necessário realizar uma operação de formatação que pode ser executada com a chamada de um método de formatação, como o método `ToString` ou <xref:System.String.Format%2A?displayProperty=nameWithType> de valor de data e hora. Este tópico mostra como executar essas operações de formatação.  
  
### <a name="to-extract-a-number-indicating-the-day-of-the-week-from-a-specific-date"></a>Para extrair um número que indique o dia da semana em uma determinada data  
  
1. Se você estiver trabalhando na representação da cadeia de caracteres de uma data, converta-a para um valor <xref:System.DateTime> ou <xref:System.DateTimeOffset> usando a estatística <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> ou o método <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType>.  
  
2. Use a propriedade <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> ou <xref:System.DateTimeOffset.DayOfWeek%2A?displayProperty=nameWithType> para recuperar um valor de <xref:System.DayOfWeek> que indica o dia da semana.  
  
3. Se necessário, execute coerção (no C#) ou converta (no Visual Basic) o valor de <xref:System.DayOfWeek> para um número inteiro.  
  
 O exemplo a seguir mostra um inteiro que representa o dia da semana de uma determinada data.  
  
 [!code-csharp[Formatting.Howto.WeekdayName#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/weekdaynumber7.cs#7)]
 [!code-vb[Formatting.Howto.WeekdayName#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/weekdaynumber7.vb#7)]  
  
### <a name="to-extract-the-abbreviated-weekday-name-from-a-specific-date"></a>Para extrair o nome do dia da semana abreviado de uma determinada data  
  
1. Se você estiver trabalhando na representação da cadeia de caracteres de uma data, converta-a para um valor <xref:System.DateTime> ou <xref:System.DateTimeOffset> usando a estatística <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> ou o método <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType>.  
  
2. Você pode extrair o nome de dia da semana abreviado da cultura atual ou de uma cultura específica:  
  
    1. Para extrair o nome de dia da semana abreviado da cultura atual, chame o método de instância <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> ou <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> do valor de data e hora e informe a cadeia de caracteres "ddd" como o parâmetro `format`. O exemplo a seguir ilustra a chamada para o método <xref:System.DateTime.ToString%28System.String%29>.  
  
         [!code-csharp[Formatting.Howto.WeekdayName#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/abbrname1.cs#1)]
         [!code-vb[Formatting.Howto.WeekdayName#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/abbrname1.vb#1)]  
  
    2. Para extrair o nome abreviado do dia da semana para uma cultura específica, chame o valor de data e hora <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> ou o <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> método de instância. Informe a cadeia de caracteres "ddd" como o parâmetro `format`. Apresente um objeto <xref:System.Globalization.CultureInfo> ou <xref:System.Globalization.DateTimeFormatInfo> que representa a cultura cujo nome de dia da semana deve ser recuperado como parâmetro `provider`. O código a seguir mostra um chamada ao método <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29> usando um objeto <xref:System.Globalization.CultureInfo> que representa a cultura fr-FR.  
  
         [!code-csharp[Formatting.Howto.WeekdayName#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/abbrname2.cs#2)]
         [!code-vb[Formatting.Howto.WeekdayName#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/abbrname2.vb#2)]  
  
### <a name="to-extract-the-full-weekday-name-from-a-specific-date"></a>Para extrair o nome completo do dia da semana de uma determinada data  
  
1. Se você estiver trabalhando na representação da cadeia de caracteres de uma data, converta-a para um valor <xref:System.DateTime> ou <xref:System.DateTimeOffset> usando a estatística <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> ou o método <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType>.  
  
2. Você pode extrair o nome completo de dia da semana da cultura atual ou de uma cultura específica:  
  
    1. Para extrair o nome do dia da semana para a cultura atual, chame o método de data e hora <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> ou de <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> instância e passe a cadeia de caracteres "dddd" como o `format` parâmetro. O exemplo a seguir ilustra a chamada para o método <xref:System.DateTime.ToString%28System.String%29>.  
  
         [!code-csharp[Formatting.Howto.WeekdayName#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/fullname4.cs#4)]
         [!code-vb[Formatting.Howto.WeekdayName#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/fullname4.vb#4)]  
  
    2. Para extrair o nome do dia da semana para uma cultura específica, chame o método de data e hora <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> ou a <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> instância. Informe a cadeia de caracteres "dddd" como o parâmetro `format`. Apresente um objeto <xref:System.Globalization.CultureInfo> ou <xref:System.Globalization.DateTimeFormatInfo> que representa a cultura cujo nome de dia da semana deve ser recuperado como parâmetro `provider`. O código a seguir mostra um chamada ao método <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29> usando um objeto <xref:System.Globalization.CultureInfo> que representa a cultura es-ES.  
  
         [!code-csharp[Formatting.Howto.WeekdayName#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/fullname5.cs#5)]
         [!code-vb[Formatting.Howto.WeekdayName#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/fullname5.vb#5)]  
  
## <a name="example"></a>Exemplo  
 O exemplo ilustra chamadas às propriedades <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> e<xref:System.DateTimeOffset.DayOfWeek%2A?displayProperty=nameWithType> e aos métodos <xref:System.DateTime.ToString%2A?displayProperty=nameWithType> e <xref:System.DateTimeOffset.ToString%2A?displayProperty=nameWithType> para recuperar o número que representa o dia da semana, o nome de dia da semana abreviado e o nome completo de dia da semana para uma determinada data.  
  
 [!code-csharp[Formatting.Howto.WeekdayName#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/example6.cs#6)]
 [!code-vb[Formatting.Howto.WeekdayName#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/example6.vb#6)]  
  
 Cada linguagem pode contar com funcionalidades que duplicam ou complementam a funcionalidade do .NET Framework. Por exemplo, o Visual Basic tem duas funções desse tipo:  
  
- `Weekday`, que retorna um número que indica o dia da semana de uma determinada data. Ele leva em consideração que o primeiro dia da semana tem valor ordinal um, enquanto a propriedade <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> considera o valor ordinal desse dia como zero.  
  
- `WeekdayName`, que retorna o nome da semana na cultura atual e que corresponde ao número de um determinado dia da semana.  
  
 O exemplo a seguir ilustra o uso das funções `Weekday` e `WeekdayName` do Visual Basic.  
  
 [!code-vb[Formatting.HowTo.WeekdayName#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/example9.vb#9)]  
  
 Você também pode usar o valor retornado pela propriedade <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> para recuperar o nome de dia da semana de uma determinada data. Isso requer apenas uma chamada ao método <xref:System.Enum.ToString%2A> no valor <xref:System.DayOfWeek> retornado pela propriedade. No entanto, essa técnica não gera um nome de dia da semana localizado para a cultura atual, como mostra o exemplo a seguir.  
  
 [!code-csharp[Formatting.HowTo.WeekdayName#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/Howto1.cs#8)]
 [!code-vb[Formatting.HowTo.WeekdayName#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/Howto1.vb#8)]

## <a name="see-also"></a>Confira também

- [Cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md)
- [Cadeias de caracteres de formato de data e hora personalizadas](custom-date-and-time-format-strings.md)
