---
title: Como usar fusos horários em aritmética de data e hora
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], arithmetic operations
- arithmetic operations [.NET Framework], dates and times
- dates [.NET Framework], adding and subtracting
ms.assetid: 83dd898d-1338-415d-8cd6-445377ab7871
ms.openlocfilehash: af19145f7caa9dbe8630ae7593734769e98720d0
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280914"
---
# <a name="how-to-use-time-zones-in-date-and-time-arithmetic"></a>Como usar fusos horários em aritmética de data e hora

Normalmente, quando você executa cálculos de data e hora <xref:System.DateTime> usando <xref:System.DateTimeOffset> valores ou, o resultado não reflete nenhuma regra de ajuste de fuso horário. Isso é verdadeiro mesmo quando o fuso horário do valor de data e hora é claramente identificável (por exemplo, quando a <xref:System.DateTime.Kind%2A> propriedade é definida como <xref:System.DateTimeKind.Local> ). Este tópico mostra como executar operações aritméticas em valores de data e hora que pertencem a um determinado fuso horário. Os resultados das operações aritméticas refletirão as regras de ajuste do fuso horário.

### <a name="to-apply-adjustment-rules-to-date-and-time-arithmetic"></a>Para aplicar as regras de ajuste à aritmética de data e hora

1. Implemente um método de acoplamento próximo de um valor de data e hora com o fuso horário ao qual ele pertence. Por exemplo, declare uma estrutura que inclui o valor de data e hora e seu fuso horário. O exemplo a seguir usa essa abordagem para vincular um <xref:System.DateTime> valor com seu fuso horário.

   [!code-csharp[System.DateTimeOffset.Conceptual#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual6.cs#6)]
   [!code-vb[System.DateTimeOffset.Conceptual#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual6.vb#6)]

2. Converta uma hora em UTC (tempo Universal Coordenado) chamando o <xref:System.TimeZoneInfo.ConvertTimeToUtc%2A> método ou o <xref:System.TimeZoneInfo.ConvertTime%2A> método.

3. Execute a operação aritmética na hora UTC.

4. Converta a hora de UTC para o fuso horário associado do tempo original chamando o <xref:System.TimeZoneInfo.ConvertTime%28System.DateTime%2CSystem.TimeZoneInfo%29?displayProperty=nameWithType> método.

## <a name="example"></a>Exemplo

O exemplo a seguir adiciona duas horas e trinta minutos a 9 de março de 2008, à 1h30 do Horário Padrão Central. A transição do fuso horário para o horário de verão ocorre 30 minutos depois, às 2h. em 9 de março de 2008. Como o exemplo segue as quatro etapas listadas na seção anterior, ele relata corretamente a hora resultante como 5h. em 9 de março de 2008.

[!code-csharp[System.DateTimeOffset.Conceptual#8](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual8.cs#8)]
[!code-vb[System.DateTimeOffset.Conceptual#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual8.vb#8)]

Os <xref:System.DateTime> <xref:System.DateTimeOffset> valores e são desassociados de qualquer fuso horário ao qual possam pertencer. Para executar a aritmética de data e hora de uma maneira que aplique automaticamente as regras de ajuste de um fuso horário, o fuso horário ao qual qualquer valor de data e hora pertence deve ser imediatamente identificável. Isso significa que uma data e hora e seu fuso horário associado devem estar estritamente acoplados. Há várias maneiras de fazer isso, que incluem o seguinte:

- Supor que todas as horas usadas em um aplicativo pertencem a um fuso horário específico. Embora adequada em alguns casos, essa abordagem oferece flexibilidade limitada e possivelmente portabilidade limitada.

- Definir um tipo que acople estritamente uma data e hora e seu fuso horário associado incluindo ambas como campos do tipo. Essa abordagem é usada no exemplo de código, que define uma estrutura para armazenar a data e hora e o fuso horário em dois campos de membro.

O exemplo ilustra como executar operações aritméticas em <xref:System.DateTime> valores para que as regras de ajuste de fuso horário sejam aplicadas ao resultado. No entanto, <xref:System.DateTimeOffset> os valores podem ser usados com a mesma facilidade. O exemplo a seguir ilustra como o código no exemplo original pode ser adaptado para uso <xref:System.DateTimeOffset> em vez de <xref:System.DateTime> valores.

[!code-csharp[System.DateTimeOffset.Conceptual#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual6.cs#7)]
[!code-vb[System.DateTimeOffset.Conceptual#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual6.vb#7)]

Observe que, se essa adição for simplesmente executada no <xref:System.DateTimeOffset> valor sem primeiro convertê-la para UTC, o resultado refletirá o ponto correto no tempo, mas seu deslocamento não refletirá o fuso horário designado para esse tempo.

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Que o <xref:System> namespace seja importado com a `using` instrução (necessária no código C#).

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
- [Executando operações aritméticas com datas e horas](performing-arithmetic-operations.md)
