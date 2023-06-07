---
title: Convertendo entre DateTime e DateTimeOffset
description: Converter entre valores de DateTimeOffset e valores de DateTime no .NET. A estrutura DateTimeOffset fornece mais reconhecimento de fuso horário do que a estrutura DateTime.
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DateTime structure, converting
- time zones [.NET Framework], conversions
- UTC times, converting
- DateTimeOffset structure, converting
- converting DateTimeOffset and DateTime values
- dates [.NET Framework], converting
- converting times
- Date data type, converting
- local time conversions
ms.assetid: b605ff97-0c45-4c24-833f-4c6a3e8be64c
ms.openlocfilehash: 86f2c982d7f87e83102933d1de73d6e13086dc87
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924897"
---
# <a name="converting-between-datetime-and-datetimeoffset"></a>Convertendo entre DateTime e DateTimeOffset

Embora a <xref:System.DateTimeOffset> estrutura forneça um grau maior de reconhecimento de fuso horário do que a <xref:System.DateTime> estrutura, os <xref:System.DateTime> parâmetros são usados com mais frequência em chamadas de método. Por isso, a capacidade de converter <xref:System.DateTimeOffset> valores em <xref:System.DateTime> valores e vice-versa é particularmente importante. Este tópico mostra como executar essas conversões de uma maneira que preserva o máximo possível de informações de fuso horário.

> [!NOTE]
> Os <xref:System.DateTime> <xref:System.DateTimeOffset> tipos e têm algumas limitações ao representar os horários em fusos horários. Com sua <xref:System.DateTime.Kind%2A> propriedade, <xref:System.DateTime> é capaz de refletir apenas o UTC (tempo Universal Coordenado) e o fuso horário local do sistema. <xref:System.DateTimeOffset>reflete o deslocamento de um tempo do UTC, mas não reflete o fuso horário real ao qual o deslocamento pertence. Para obter detalhes sobre valores de tempo e suporte para fusos horários, consulte [escolhendo entre DateTime, DateTimeOffset, TimeSpan e TimeZoneInfo](choosing-between-datetime.md).

## <a name="conversions-from-datetime-to-datetimeoffset"></a>Conversões de DateTime para DateTimeOffset

A <xref:System.DateTimeOffset> estrutura fornece duas maneiras equivalentes de executar <xref:System.DateTime> a <xref:System.DateTimeOffset> conversão que são adequadas para a maioria das conversões:

- O <xref:System.DateTimeOffset.%23ctor%2A> Construtor, que cria um novo <xref:System.DateTimeOffset> objeto com base em um <xref:System.DateTime> valor.

- O operador de conversão implícita, que permite atribuir um <xref:System.DateTime> valor a um <xref:System.DateTimeOffset> objeto.

Para valores UTC e local <xref:System.DateTime> , a <xref:System.DateTimeOffset.Offset%2A> Propriedade do valor resultante <xref:System.DateTimeOffset> reflete com precisão o deslocamento de fuso horário local ou UTC. Por exemplo, o código a seguir converte uma hora UTC em seu <xref:System.DateTimeOffset> valor equivalente.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#1)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#1)]

Nesse caso, o deslocamento da variável `utcTime2` é 00:00. Da mesma forma, o código a seguir converte uma hora local em seu <xref:System.DateTimeOffset> valor equivalente.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#2)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#2)]

No entanto, para <xref:System.DateTime> valores cuja <xref:System.DateTime.Kind%2A> propriedade é <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> , esses dois métodos de conversão produzem um <xref:System.DateTimeOffset> valor cujo deslocamento é o do fuso horário local. Isso é mostrado no exemplo a seguir, que é executado no fuso horário padrão do Pacífico americano.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#3)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#3)]

Se o <xref:System.DateTime> valor refletir a data e a hora em algo diferente do fuso horário local ou UTC, você poderá convertê-lo em um <xref:System.DateTimeOffset> valor e preservar suas informações de fuso horário chamando o <xref:System.DateTimeOffset.%23ctor%2A> construtor sobrecarregado. Por exemplo, o exemplo a seguir instancia um <xref:System.DateTimeOffset> objeto que reflete a hora padrão central.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#4)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#4)]

O segundo parâmetro para essa sobrecarga de construtor, um <xref:System.TimeSpan> objeto que representa o deslocamento de tempo do UTC, deve ser recuperado chamando o <xref:System.TimeZoneInfo.GetUtcOffset%28System.DateTime%29?displayProperty=nameWithType> método do fuso horário correspondente do tempo. O parâmetro único do método é o <xref:System.DateTime> valor que representa a data e a hora a serem convertidas. Se o fuso horário der suporte ao horário de verão, este parâmetro permite que o método determine o deslocamento apropriado para aquela data e hora determinada.

## <a name="conversions-from-datetimeoffset-to-datetime"></a>Conversões de DateTimeOffset em DateTime

A <xref:System.DateTimeOffset.DateTime%2A> propriedade é mais comumente usada para realizar <xref:System.DateTimeOffset> a <xref:System.DateTime> conversão. No entanto, ele retorna um <xref:System.DateTime> valor cuja <xref:System.DateTime.Kind%2A> propriedade é <xref:System.DateTimeKind.Unspecified> , como ilustra o exemplo a seguir.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#5)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#5)]

Isso significa que todas as informações sobre a <xref:System.DateTimeOffset> relação do valor com o UTC são perdidas pela conversão quando a <xref:System.DateTimeOffset.DateTime%2A> propriedade é usada. Isso afeta os <xref:System.DateTimeOffset> valores que correspondem à hora UTC ou à hora local do sistema, pois a <xref:System.DateTimeOffset.DateTime%2A> estrutura reflete apenas esses dois fusos horários em sua <xref:System.DateTime.Kind%2A> propriedade.

Para preservar o máximo possível de informações de fuso horário ao converter um <xref:System.DateTimeOffset> para um <xref:System.DateTime> valor, você pode usar <xref:System.DateTimeOffset.UtcDateTime%2A?displayProperty=nameWithType> as <xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> Propriedades e.

### <a name="converting-a-utc-time"></a>Convertendo uma hora UTC

Para indicar que um <xref:System.DateTimeOffset.DateTime%2A> valor convertido é a hora UTC, você pode recuperar o valor da <xref:System.DateTimeOffset.UtcDateTime%2A?displayProperty=nameWithType> propriedade. Ele difere da <xref:System.DateTimeOffset.DateTime%2A> propriedade de duas maneiras:

- Ele retorna um <xref:System.DateTime> valor cuja <xref:System.DateTime.Kind%2A> propriedade é <xref:System.DateTimeKind.Utc> .

- Se o <xref:System.DateTimeOffset.Offset%2A> valor da propriedade não for igual <xref:System.TimeSpan.Zero?displayProperty=nameWithType> , ele converterá a hora em UTC.

> [!NOTE]
> Se seu aplicativo exigir que os <xref:System.DateTime> valores convertidos identifiquem inequivocamente um único ponto no tempo, considere usar a <xref:System.DateTimeOffset.UtcDateTime%2A?displayProperty=nameWithType> propriedade para lidar com todas as <xref:System.DateTimeOffset> <xref:System.DateTime> conversões.

O código a seguir usa a <xref:System.DateTimeOffset.UtcDateTime%2A> propriedade para converter um <xref:System.DateTimeOffset> valor cujo deslocamento é igual <xref:System.TimeSpan.Zero?displayProperty=nameWithType> a um <xref:System.DateTime> valor.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#6)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#6)]

O código a seguir usa a <xref:System.DateTimeOffset.UtcDateTime%2A> propriedade para executar uma conversão de fuso horário e uma conversão de tipo em um <xref:System.DateTimeOffset> valor.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#12](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#12)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#12)]

### <a name="converting-a-local-time"></a>Convertendo uma hora local

Para indicar que um <xref:System.DateTimeOffset> valor representa a hora local, você pode passar o <xref:System.DateTime> valor retornado pela <xref:System.DateTimeOffset.DateTime%2A?displayProperty=nameWithType> propriedade para o `static` método ( `Shared` no Visual Basic) <xref:System.DateTime.SpecifyKind%2A> . O método retorna a data e a hora passados para ele como seu primeiro parâmetro, mas define a <xref:System.DateTime.Kind%2A> propriedade para o valor especificado pelo segundo parâmetro. O código a seguir usa o <xref:System.DateTime.SpecifyKind%2A> método ao converter um <xref:System.DateTimeOffset> valor cujo deslocamento corresponde ao do fuso horário local.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#7)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#7)]

Você também pode usar a <xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> propriedade para converter um <xref:System.DateTimeOffset> valor em um <xref:System.DateTime> valor local. A <xref:System.DateTime.Kind%2A> Propriedade do valor retornado <xref:System.DateTime> é <xref:System.DateTimeKind.Local> . O código a seguir usa a <xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> propriedade ao converter um <xref:System.DateTimeOffset> valor cujo deslocamento corresponde ao do fuso horário local.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#10](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#10)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#10)]

Quando você recupera um <xref:System.DateTime> valor usando a <xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> propriedade, o `get` acessador da propriedade primeiro converte o <xref:System.DateTimeOffset> valor em UTC e, em seguida, converte-o em hora local chamando o <xref:System.DateTimeOffset.ToLocalTime%2A> método. Isso significa que você pode recuperar um valor da <xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> propriedade para executar uma conversão de fuso horário ao mesmo tempo em que executa uma conversão de tipo. Isso também significa que as regras de ajuste do fuso horário local são aplicadas na execução da conversão. O código a seguir ilustra o uso da <xref:System.DateTimeOffset.LocalDateTime%2A?displayProperty=nameWithType> propriedade para executar um tipo e uma conversão de fuso horário. A saída de exemplo é para um computador definido para o fuso horário do Pacífico (EUA e Canadá). A data de novembro é hora oficial do Pacífico, que é o UTC-8, enquanto a data de junho é o horário de verão, que é o UTC-7.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#11](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#11)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#11)]

### <a name="a-general-purpose-conversion-method"></a>Um método de conversão de uso geral

O exemplo a seguir define um método chamado `ConvertFromDateTimeOffset` que converte <xref:System.DateTimeOffset> valores em <xref:System.DateTime> valores. Com base em seu deslocamento, ele determina se o <xref:System.DateTimeOffset> valor é uma hora UTC, uma hora local ou algum outro momento e define a propriedade de valor de data e hora retornada de forma <xref:System.DateTime.Kind%2A> adequada.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#8](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#8)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#8)]

O exemplo a seguir chama o `ConvertFromDateTimeOffset` método para converter <xref:System.DateTimeOffset> valores que representam uma hora UTC, uma hora local e uma hora no fuso horário padrão central dos EUA.

[!code-csharp[System.DateTimeOffset.Conceptual.Conversions#9](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/cs/Conversions.cs#9)]
[!code-vb[System.DateTimeOffset.Conceptual.Conversions#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Conversions/vb/Conversions.vb#9)]

Observe que esse código faz duas suposições que, dependendo do aplicativo e da fonte de seus valores de data e hora, podem não ser sempre válidas:

- Ele assume que um valor de data e hora cujo deslocamento é <xref:System.TimeSpan.Zero?displayProperty=nameWithType> representa o UTC. Na verdade, UTC não é uma hora em um determinado fuso horário, mas a hora em relação à qual as horas nos fusos horários do mundo são padronizadas. Os fusos horários também podem ter um deslocamento de <xref:System.TimeSpan.Zero> .

- Ele supõe que uma data e hora cujo deslocamento é igual ao do fuso horário local representa o fuso horário local. Como os valores de data e hora são dissociados do seu fuso horário original, este pode não ser o caso. A data e hora podem ter sido originadas em outro fuso horário com o mesmo deslocamento.

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
