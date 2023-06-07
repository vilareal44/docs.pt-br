---
title: 'Como: permitir que os usuários resolvam tempos ambíguos'
ms.date: 04/10/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- time zones [.NET Framework], ambiguous time
- ambiguous time [.NET Framework]
ms.assetid: bca874ee-5b68-4654-8bbd-3711220ef332
ms.openlocfilehash: ac723738d80a2f686a5fcaf279cec791b3c58619
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281577"
---
# <a name="how-to-let-users-resolve-ambiguous-times"></a>Como: permitir que os usuários resolvam tempos ambíguos

Um horário ambíguo é um horário que aponta para mais de um UTC (Tempo Universal Coordenado). Ocorre quando o horário do relógio é atrasado, como durante a transição do horário de verão de um fuso horário para seu horário padrão. Ao processar um horário ambíguo, você pode executar uma das seguintes ações:

- Se o horário ambíguo for um item de dados inserido pelo usuário, você pode deixar que o usuário resolva a ambiguidade.

- Faça uma suposição de como o tempo aponta para o UTC. Por exemplo, você pode supor que um horário ambíguo é sempre expresso no horário padrão do fuso horário.

Este tópico mostra como permitir que um usuário resolva um horário ambíguo.

### <a name="to-let-a-user-resolve-an-ambiguous-time"></a>Permitir que o usuário resolva um horário ambíguo

1. Obtenha a entrada de data e hora do usuário.

2. Chame o <xref:System.TimeZoneInfo.IsAmbiguousTime%2A> método para determinar se o tempo é ambíguo.

3. Se o tempo for ambíguo, chame o <xref:System.TimeZoneInfo.GetAmbiguousTimeOffsets%2A> método para recuperar uma matriz de <xref:System.TimeSpan> objetos. Cada elemento na matriz contém um deslocamento UTC para o qual a hora ambígua pode ser mapeada.

4. Permita que o usuário selecione o deslocamento desejado.

5. Obtenha a data e hora de UTC subtraindo o deslocamento selecionado pelo usuário do horário local.

6. Chame o `static` `Shared` método (no Visual Basic .net) <xref:System.DateTime.SpecifyKind%2A> para definir a propriedade de valor de data e hora UTC <xref:System.DateTime.Kind%2A> como <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> .

## <a name="example"></a>Exemplo

O exemplo a seguir solicita que o usuário insira uma data e hora e, se ela for ambígua, permite que o usuário selecione o horário UTC para o qual o horário ambíguo aponta.

[!code-csharp[System.TimeZone2.Concepts#11](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#11)]
[!code-vb[System.TimeZone2.Concepts#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#11)]

O núcleo do código de exemplo usa uma matriz de <xref:System.TimeSpan> objetos para indicar possíveis deslocamentos do tempo ambíguo do UTC. No entanto, esses deslocamentos provavelmente não serão significativos para o usuário. Para esclarecer o significado dos deslocamentos, o código também observa se um deslocamento representa o horário padrão do fuso horário local ou seu horário de verão. O código determina qual é a hora padrão e em qual horário é o horário de verão comparando o deslocamento com o valor da <xref:System.TimeZoneInfo.BaseUtcOffset%2A> propriedade. Essa propriedade indica a diferença entre o UTC e o horário padrão do fuso horário.

Neste exemplo, todas as referências ao fuso horário local são feitas por meio da <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> Propriedade; o fuso horário local nunca é atribuído a uma variável de objeto. Essa é uma prática recomendada porque uma chamada para o <xref:System.TimeZoneInfo.ClearCachedData%2A?displayProperty=nameWithType> método invalida todos os objetos aos quais o fuso horário local está atribuído.

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Que o <xref:System> namespace seja importado com a `using` instrução (necessária no código C#).

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
- [Como: resolver horários ambíguos](resolve-ambiguous-times.md)
