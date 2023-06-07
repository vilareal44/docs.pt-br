---
title: 'Como: resolver horários ambíguos'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], ambiguous time
- ambiguous time [.NET Framework]
ms.assetid: 2cf5fb25-492c-4875-9245-98cac8348e97
ms.openlocfilehash: ad69c0984a9d8c01ebd2198486cd0f6492a6116e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281499"
---
# <a name="how-to-resolve-ambiguous-times"></a>Como: resolver horários ambíguos

Um horário ambíguo é um horário que aponta para mais de um UTC (Tempo Universal Coordenado). Ocorre quando o horário do relógio é atrasado, como durante a transição do horário de verão de um fuso horário para seu horário padrão. Ao processar um horário ambíguo, você pode executar uma das seguintes ações:

- Faça uma suposição de como o tempo aponta para o UTC. Por exemplo, você pode supor que um horário ambíguo é sempre expresso no horário padrão do fuso horário.

- Se o horário ambíguo for um item de dados inserido pelo usuário, você pode deixar que o usuário resolva a ambiguidade.

Este tópico mostra como resolver um tempo ambíguo supondo que ele representa o horário padrão do fuso horário.

### <a name="to-map-an-ambiguous-time-to-a-time-zones-standard-time"></a>Para apontar um horário ambíguo para o horário padrão de um fuso horário

1. Chame o <xref:System.TimeZoneInfo.IsAmbiguousTime%2A> método para determinar se o tempo é ambíguo.

2. Se o tempo for ambíguo, subtraia a hora do <xref:System.TimeSpan> objeto retornado pela propriedade do fuso horário <xref:System.TimeZoneInfo.BaseUtcOffset%2A> .

3. Chame o `static` `Shared` método (no Visual Basic .net) <xref:System.DateTime.SpecifyKind%2A> para definir a propriedade de valor de data e hora UTC <xref:System.DateTime.Kind%2A> como <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> .

## <a name="example"></a>Exemplo

O exemplo a seguir ilustra como converter um horário ambíguo em UTC supondo que ele representa o horário padrão do fuso horário local.

[!code-csharp[System.TimeZone2.Concepts#10](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#10)]
[!code-vb[System.TimeZone2.Concepts#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#10)]

O exemplo consiste em um método chamado `ResolveAmbiguousTime` que determina se o <xref:System.DateTime> valor passado para ele é ambíguo. Se o valor for ambíguo, o método retornará um <xref:System.DateTime> valor que representa a hora UTC correspondente. O método manipula essa conversão subtraindo o valor da Propriedade do fuso horário local <xref:System.TimeZoneInfo.BaseUtcOffset%2A> da hora local.

Normalmente, uma hora ambígua é tratada chamando o <xref:System.TimeZoneInfo.GetAmbiguousTimeOffsets%2A> método para recuperar uma matriz de <xref:System.TimeSpan> objetos que contêm os deslocamentos de UTC possíveis. No entanto, esse exemplo faz a suposição arbitrária de que um horário ambíguo deve sempre apontar para o horário padrão do fuso horário. A <xref:System.TimeZoneInfo.BaseUtcOffset%2A> propriedade retorna o deslocamento entre o UTC e o horário padrão de um fuso horário.

Neste exemplo, todas as referências ao fuso horário local são feitas por meio da <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> Propriedade; o fuso horário local nunca é atribuído a uma variável de objeto. Essa é uma prática recomendada porque uma chamada para o <xref:System.TimeZoneInfo.ClearCachedData%2A?displayProperty=nameWithType> método invalida todos os objetos aos quais o fuso horário local está atribuído.

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Que o <xref:System> namespace seja importado com a `using` instrução (necessária no código C#).

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
- [Como: permitir que os usuários resolvam tempos ambíguos](let-users-resolve-ambiguous-times.md)
