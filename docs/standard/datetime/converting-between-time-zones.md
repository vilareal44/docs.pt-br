---
title: Convertendo horários entre fusos horários
description: Saiba como converter os horários entre um fuso horário para outro no .NET. Além disso, aprenda a converter valores de DateTimeOffset que têm reconhecimento de fuso horário limitado.
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- times [.NET Framework], converting
- time zones [.NET Framework], conversions
- UTC times, converting
- converting times
- local time conversions
ms.assetid: a51e1a3b-c983-4320-b31a-1f9fa3cf824a
ms.openlocfilehash: 7d1984866c5eacdfe21834389b8f0be4caf78fb7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84446835"
---
# <a name="converting-times-between-time-zones"></a>Convertendo horários entre fusos horários

Está se tornando cada vez mais importante para qualquer aplicativo que trabalha com datas e horas lidar com diferenças entre fusos horários. Um aplicativo não pode mais supor que todos os horários possam ser expressos na hora local, que é o tempo disponível da <xref:System.DateTime> estrutura. Por exemplo, uma página da Web que exibe a hora atual no leste dos Estados Unidos não terá credibilidade para um cliente no leste da Ásia. Este tópico explica como converter horários de um fuso horário para outro, bem como converter <xref:System.DateTimeOffset> valores que têm reconhecimento de fuso horário limitado.

## <a name="converting-to-coordinated-universal-time"></a>Convertendo para o Tempo Universal Coordenado

O UTC (Tempo Universal Coordenado) é um padrão de tempo atômico de alta precisão. Os fusos horários do mundo são expressos como deslocamentos positivos ou negativos com relação ao UTC. Sendo assim, o UTC fornece uma espécie de hora livre de fuso horário ou com fuso horário neutro. O uso da hora em UTC é recomendado quando a portabilidade da data e hora entre computadores é importante. (Para obter detalhes e outras práticas recomendadas usando datas e horas, confira [codificando práticas recomendadas usando DateTime no .NET Framework](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973825(v=msdn.10)).) Converter fusos horários individuais em UTC facilita as comparações de tempo.

> [!NOTE]
> Você também pode serializar uma <xref:System.DateTimeOffset> estrutura para representar de forma não ambígua um único ponto no tempo. Como <xref:System.DateTimeOffset> os objetos armazenam um valor de data e hora junto com seu deslocamento do UTC, eles sempre representam um determinado ponto no tempo em relação ao UTC.

A maneira mais fácil de converter uma hora em UTC é chamar o `static` método ( `Shared` no Visual Basic) <xref:System.TimeZoneInfo.ConvertTimeToUtc%28System.DateTime%29?displayProperty=nameWithType> . A conversão exata executada pelo método depende do valor da `dateTime` Propriedade do parâmetro <xref:System.DateTime.Kind%2A> , como mostra a tabela a seguir.

| `DateTime.Kind`            | Conversão                                                                     |
| -------------------------- | ------------------------------------------------------------------------------ |
| `DateTimeKind.Local`       | Converte a hora local para UTC.                                                    |
| `DateTimeKind.Unspecified` | Presume que o parâmetro `dateTime` é a hora local e converte a hora local para UTC. |
| `DateTimeKind.Utc`         | Retorna o parâmetro `dateTime` inalterado.                                    |

O código a seguir converte a hora local atual para UTC e exibe o resultado no console.

[!code-csharp[System.TimeZone2.Concepts#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#6)]
[!code-vb[System.TimeZone2.Concepts#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#6)]

Se o valor de data e hora não representar a hora local ou UTC, o <xref:System.DateTime.ToUniversalTime%2A> método provavelmente retornará um resultado errado. No entanto, você pode usar o <xref:System.TimeZoneInfo.ConvertTimeToUtc%2A?displayProperty=nameWithType> método para converter a data e a hora de um fuso horário especificado. (Para obter detalhes sobre como recuperar um <xref:System.TimeZoneInfo> objeto que representa o fuso horário de destino, consulte [localizando os fusos horários definidos em um sistema local](finding-the-time-zones-on-local-system.md).) O código a seguir usa o <xref:System.TimeZoneInfo.ConvertTimeToUtc%2A?displayProperty=nameWithType> método para converter o horário padrão do leste para UTC.

[!code-csharp[System.TimeZone2.Concepts#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#7)]
[!code-vb[System.TimeZone2.Concepts#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#7)]

Observe que esse método gera um <xref:System.ArgumentException> se a <xref:System.DateTime> Propriedade do objeto <xref:System.DateTime.Kind%2A> e o fuso horário não forem correspondentes. Uma incompatibilidade ocorrerá se a <xref:System.DateTime.Kind%2A> propriedade for <xref:System.DateTimeKind.Local?displayProperty=nameWithType> , mas o <xref:System.TimeZoneInfo> objeto não representar o fuso horário local ou se a <xref:System.DateTime.Kind%2A> propriedade for <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> , mas o objeto não for <xref:System.TimeZoneInfo> igual <xref:System.TimeZoneInfo.Utc?displayProperty=nameWithType> .

Todos esses métodos usam <xref:System.DateTime> valores como parâmetros e retornam um <xref:System.DateTime> valor. Para <xref:System.DateTimeOffset> valores, a <xref:System.DateTimeOffset> estrutura tem um <xref:System.DateTimeOffset.ToUniversalTime%2A> método de instância que converte a data e a hora da instância atual em UTC. O exemplo a seguir chama o <xref:System.DateTimeOffset.ToUniversalTime%2A> método para converter uma hora local e várias outras vezes em UTC (tempo Universal Coordenado).

[!code-csharp[System.DateTimeOffset.Methods#16](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Methods/cs/Methods.cs#16)]
[!code-vb[System.DateTimeOffset.Methods#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Methods/vb/Methods.vb#16)]

## <a name="converting-utc-to-a-designated-time-zone"></a>Convertendo UTC em um fuso horário designado

Para converter UTC em hora local, consulte a seção "convertendo UTC para a hora local" a seguir. Para converter o UTC para a hora em qualquer fuso horário que você designar, chame o <xref:System.TimeZoneInfo.ConvertTimeFromUtc%2A> método. O método utiliza dois parâmetros:

- O UTC a ser convertido. Deve ser um <xref:System.DateTime> valor cuja <xref:System.DateTime.Kind%2A> propriedade seja definida como `Unspecified` ou `Utc` .

- O fuso horário no qual o UTC deve ser convertido.

O código a seguir converte o UTC para o Horário Padrão Central.

[!code-csharp[System.TimeZone2.Concepts#8](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#8)]
[!code-vb[System.TimeZone2.Concepts#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#8)]

## <a name="converting-utc-to-local-time"></a>Convertendo UTC em hora local

Para converter UTC em hora local, chame o <xref:System.DateTime.ToLocalTime%2A> método do <xref:System.DateTime> objeto cujo tempo você deseja converter. O comportamento exato do método depende do valor da Propriedade do objeto <xref:System.DateTime.Kind%2A> , como mostra a tabela a seguir.

| `DateTime.Kind`            | Conversão                                                                               |
| -------------------------- | ---------------------------------------------------------------------------------------- |
| `DateTimeKind.Local`       | Retorna o <xref:System.DateTime> valor inalterado.                                      |
| `DateTimeKind.Unspecified` | Pressupõe que o <xref:System.DateTime> valor é UTC e converte o UTC para a hora local. |
| `DateTimeKind.Utc`         | Converte o <xref:System.DateTime> valor em hora local.                                 |

> [!NOTE]
> O <xref:System.TimeZone.ToLocalTime%2A?displayProperty=nameWithType> método se comporta de forma idêntica ao `DateTime.ToLocalTime` método. Ele usa um único parâmetro, que é o valor de data e hora a ser convertido.

Você também pode converter a hora em qualquer fuso horário designado para a hora local usando o `static` `Shared` método (no Visual Basic) <xref:System.TimeZoneInfo.ConvertTime%2A?displayProperty=nameWithType> . Essa técnica é discutida na próxima seção.

## <a name="converting-between-any-two-time-zones"></a>Convertendo entre dois fusos horários

Você pode converter entre dois fusos horários usando um dos dois `static` `Shared` métodos (em Visual Basic) a seguir da <xref:System.TimeZoneInfo> classe:

- <xref:System.TimeZoneInfo.ConvertTime%2A>

  Os parâmetros desse método são o valor de data e hora a ser convertido, um `TimeZoneInfo` objeto que representa o fuso horário do valor de data e hora e um `TimeZoneInfo` objeto que representa o fuso horário para o qual converter o valor de data e hora.

- <xref:System.TimeZoneInfo.ConvertTimeBySystemTimeZoneId%2A>

  Os parâmetros do método são o valor de data e hora a ser convertido, o identificador do fuso horário do valor de data e hora e o identificador do fuso horário para o qual converter o valor de data e hora.

Os dois métodos exigem que a <xref:System.DateTime.Kind%2A> Propriedade do valor de data e hora para converter e o <xref:System.TimeZoneInfo> identificador de objeto ou de fuso horário que representa seu fuso horário correspondam uns aos outros. Caso contrário, um <xref:System.ArgumentException> será gerado. Por exemplo, se a `Kind` Propriedade do valor de data e hora for `DateTimeKind.Local` , uma exceção será gerada se o `TimeZoneInfo` objeto passado como um parâmetro para o método não for igual a `TimeZoneInfo.Local` . Uma exceção também será gerada se o identificador passado como um parâmetro para o método não for igual a `TimeZoneInfo.Local.Id` .

O exemplo a seguir usa o <xref:System.TimeZoneInfo.ConvertTime%2A> método para converter do horário padrão do Havaí para a hora local.

[!code-csharp[System.TimeZone2.Concepts#9](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#9)]
[!code-vb[System.TimeZone2.Concepts#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#9)]

## <a name="converting-datetimeoffset-values"></a>Convertendo valores de DateTimeOffset

Os valores de data e hora representados por <xref:System.DateTimeOffset> objetos não têm reconhecimento total de fuso horário porque o objeto é desassociado de seu fuso horário no momento em que é instanciado. No entanto, em muitos casos o aplicativo precisa apenas converter uma data e hora com base em dois deslocamentos diferentes do UTC, em vez da hora em fusos horários específicos. Para executar essa conversão, você pode chamar o método da instância atual <xref:System.DateTimeOffset.ToOffset%2A> . O parâmetro único do método é o deslocamento do novo valor de data e hora que o método deve retornar.

Por exemplo, se a data e hora da solicitação de um usuário de uma página da Web for conhecida e for serializada como uma cadeia de caracteres no formato MM/dd/aaaa hh:mm:ss zzzz, o seguinte método `ReturnTimeOnServer` converte esse valor de data e hora para a data e hora no servidor Web.

[!code-csharp[System.DateTimeOffset.Conceptual.OffsetConversions#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.OffsetConversions/cs/TimeConversions.cs#1)]
[!code-vb[System.DateTimeOffset.Conceptual.OffsetConversions#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.OffsetConversions/vb/TimeConversions.vb#1)]

Se o método for passado para a cadeia de caracteres "9/1/2007 5:32:07 -05:00", que representa a data e a hora em um fuso horário cinco horas anteriores à UTC, ele retornará 9/1/2007 3:32:07 AM-07:00 para um servidor localizado no fuso horário padrão do Pacífico americano.

A <xref:System.TimeZoneInfo> classe também inclui uma sobrecarga do <xref:System.TimeZoneInfo.ConvertTime%28System.DateTimeOffset%2CSystem.TimeZoneInfo%29?displayProperty=nameWithType> método que executa conversões de fuso horário com <xref:System.DateTimeOffset.ToOffset(System.TimeSpan)> valores. Os parâmetros do método são um <xref:System.DateTimeOffset> valor e uma referência ao fuso horário no qual o tempo deve ser convertido. A chamada do método retorna um <xref:System.DateTimeOffset> valor. Por exemplo, o `ReturnTimeOnServer` método no exemplo anterior poderia ser reescrito da seguinte maneira para chamar o <xref:System.TimeZoneInfo.ConvertTime%28System.DateTimeOffset%2CSystem.TimeZoneInfo%29> método.

[!code-csharp[System.DateTimeOffset.Conceptual.OffsetConversions#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.OffsetConversions/cs/timeconversions2.cs#2)]
[!code-vb[System.DateTimeOffset.Conceptual.OffsetConversions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.OffsetConversions/vb/TimeConversions2.vb#2)]

## <a name="see-also"></a>Confira também

- <xref:System.TimeZoneInfo>
- [Datas, horas e fusos horários](index.md)
- [Localizando os fusos horários definidos em um sistema local](finding-the-time-zones-on-local-system.md)
