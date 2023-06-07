---
title: Comparar DateTime, DateTimeOffset, TimeSpan e TimeZoneInfo
description: Saiba mais sobre as diferenças entre os tipos DateTime, DateTimeOffset, TimeSpan e TimeZoneInfo para representar informações de data e hora no .NET.
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DateTimeOffset structure
- TimeZoneInfo class
- time zones [.NET Framework], common uses
- date and time classes [.NET Framework]
- time zones [.NET Framework], type options
- DateTime structure
ms.assetid: 07f17aad-3571-4014-9ef3-b695a86f3800
ms.openlocfilehash: 03d00fb802032b981a5ebe80f7166eba0fb54a60
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85326056"
---
# <a name="choose-between-datetime-datetimeoffset-timespan-and-timezoneinfo"></a>Escolha entre DateTime, DateTimeOffset, TimeSpan e TimeZoneInfo

Os aplicativos .NET podem usar informações de data e hora de várias maneiras. Os usos mais comuns das informações de data e hora incluem:

- Para refletir somente uma data, uma vez que a informação de tempo não é importante.

- Para refletir somente um tempo, uma vez que a informação de data não é importante.

- Para refletir uma data abstrata e um tempo que não está vinculado a uma hora e local específicos (por exemplo, a maioria das lojas em um uma cadeia internacional abre em dias da semana às 9h).

- Para recuperar informações de data e hora de fontes fora do .NET, normalmente, onde as informações de data e hora são armazenadas em um tipo de dados simples.

- Para identificar um único ponto no tempo de maneira única e não ambígua. Alguns aplicativos exigem que uma data e hora sejam inequívocas apenas no sistema host. Outros aplicativos exigem que não sejam ambíguos em todos os sistemas (ou seja, uma data serializada em um sistema pode ser desserializada de forma significativa e usada em outro sistema em qualquer lugar do mundo).

- Para preservar vários horários relacionados (como o horário local do solicitante e o horário de recebimento de uma solicitação Web pelo servidor).

- Para realizar a aritmética de data e hora, possivelmente com um resultado que identifica de maneira única e não ambígua um único ponto no tempo.

O .NET inclui <xref:System.DateTime> os <xref:System.DateTimeOffset> tipos,, <xref:System.TimeSpan> e <xref:System.TimeZoneInfo> , que podem ser usados para criar aplicativos que funcionam com datas e horas.

> [!NOTE]
> Este tópico não aborda <xref:System.TimeZone> porque sua funcionalidade está quase totalmente incorporada à <xref:System.TimeZoneInfo> classe. Sempre que possível, use a <xref:System.TimeZoneInfo> classe em vez da <xref:System.TimeZone> classe.

## <a name="the-datetime-structure"></a>A estrutura DateTime

Um <xref:System.DateTime> valor define uma data e hora específicas. Ele inclui uma <xref:System.DateTime.Kind%2A> propriedade que fornece informações limitadas sobre o fuso horário ao qual essa data e hora pertencem. O <xref:System.DateTimeKind> valor retornado pela <xref:System.DateTime.Kind%2A> propriedade indica se o <xref:System.DateTime> valor representa a hora local ( <xref:System.DateTimeKind.Local?displayProperty=nameWithType> ), UTC (tempo Universal Coordenado) ( <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> ) ou uma hora não especificada ( <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> ).

A <xref:System.DateTime> estrutura é adequada para aplicativos com uma ou mais das seguintes características:

- Trabalhar apenas com datas.

- Trabalhar apenas com horas.

- Trabalhar com datas e horas abstratas.

- Trabalhar com datas e horas para as quais as informações de fuso horário estão ausentes.

- Trabalhar apenas com datas e horas UTC.

- Recupere informações de data e hora de fontes fora do .NET, como bancos de dados SQL. Normalmente, essas fontes armazenam informações de data e hora em um formato simples que é compatível com a <xref:System.DateTime> estrutura.

- Realizar aritmética de data e hora, mas que há preocupação com resultados gerais. Por exemplo, em uma operação de adição que soma seis meses a uma data e hora determinada, geralmente não é importante se o resultado é ajustado para horário de verão.

A menos <xref:System.DateTime> que um valor específico represente o UTC, esse valor de data e hora geralmente é ambíguo ou limitado em sua portabilidade. Por exemplo, se um <xref:System.DateTime> valor representa a hora local, ele será portátil dentro desse fuso horário local (ou seja, se o valor for desserializado em outro sistema no mesmo fuso horário, esse valor ainda identificará um único ponto no tempo). Fora do fuso horário local, esse <xref:System.DateTime> valor pode ter várias interpretações. Se a propriedade do valor <xref:System.DateTime.Kind%2A> for <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> , ela será ainda menos portátil: agora ela é ambígua dentro do mesmo fuso horário e possivelmente mesmo no mesmo sistema em que ele foi serializado pela primeira vez. Somente se um <xref:System.DateTime> valor representar UTC, esse valor identificará inequivocamente um único ponto no tempo, independentemente do sistema ou do fuso horário em que o valor é usado.

> [!IMPORTANT]
> Ao salvar ou compartilhar <xref:System.DateTime> dados, o UTC deve ser usado e a <xref:System.DateTime> Propriedade do valor <xref:System.DateTime.Kind%2A> deve ser definida como <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> .

## <a name="the-datetimeoffset-structure"></a>A estrutura DateTimeOffset

A <xref:System.DateTimeOffset> estrutura representa um valor de data e hora, junto com um deslocamento que indica o quanto esse valor difere do UTC. Portanto, o valor sempre identifica sem ambiguidade um único ponto no tempo.

O <xref:System.DateTimeOffset> tipo inclui toda a funcionalidade do <xref:System.DateTime> tipo junto com o reconhecimento de fuso horário. Isso o torna adequado para aplicativos que:

- Identifique de maneira única e não ambígua um único ponto no tempo. O <xref:System.DateTimeOffset> tipo pode ser usado para definir de forma não ambígua o significado de "Now", para registrar tempos de transação, registrar os tempos de eventos do sistema ou do aplicativo e registrar os horários de criação e modificação do arquivo.

- Realizar aritmética geral de data e hora.

- Preserve vários tempos relacionados contanto que esses tempos sejam armazenados como dois valores separados ou como dois membros de uma estrutura.

> [!NOTE]
> Esses usos para <xref:System.DateTimeOffset> valores são muito mais comuns do que aqueles para <xref:System.DateTime> valores. Como resultado, considere <xref:System.DateTimeOffset> como o tipo de data e hora padrão para o desenvolvimento de aplicativos.

Um <xref:System.DateTimeOffset> valor não está vinculado a um fuso horário específico, mas pode se originar de uma variedade de fusos horários. O exemplo a seguir lista os fusos horários nos quais um número de <xref:System.DateTimeOffset> valores (incluindo uma hora padrão do Pacífico local) pode pertencer.

[!code-csharp[System.DateTimeOffset.Conceptual#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual1.cs#1)]
[!code-vb[System.DateTimeOffset.Conceptual#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual1.vb#1)]

A saída mostra que cada valor de data e hora nesse exemplo pode pertencer a pelo menos três fusos horários diferentes. O <xref:System.DateTimeOffset> valor de 6/10/2007 mostra que se um valor de data e hora representa um horário de verão, seu deslocamento do UTC não necessariamente corresponde ao deslocamento UTC base do fuso horário de origem ou ao deslocamento do UTC encontrado em seu nome de exibição. Como um único <xref:System.DateTimeOffset> valor não está rigidamente acoplado a seu fuso horário, ele não pode refletir a transição de um fuso horário para e do horário de verão. Isso pode ser problemático quando a aritmética de data e hora é usada para manipular um <xref:System.DateTimeOffset> valor. Para ver uma discussão sobre como realizar a aritmética de data e hora de uma forma que considere as regras de ajuste de um fuso horário, consulte [Executando operações aritméticas com datas e horas](performing-arithmetic-operations.md).

## <a name="the-timespan-structure"></a>A estrutura TimeSpan

A <xref:System.TimeSpan> estrutura representa um intervalo de tempo. Seus dois usos típicos são:

- Refletir o intervalo de tempo entre dois valores de data e hora. Por exemplo, subtrair um <xref:System.DateTime> valor de outro retorna um <xref:System.TimeSpan> valor.

- Calcular o tempo decorrido. Por exemplo, a <xref:System.Diagnostics.Stopwatch.Elapsed%2A?displayProperty=nameWithType> propriedade retorna um <xref:System.TimeSpan> valor que reflete o intervalo de tempo decorrido desde a chamada para um dos <xref:System.Diagnostics.Stopwatch> métodos que começam a medir o tempo decorrido.

Um <xref:System.TimeSpan> valor também pode ser usado como uma substituição para um <xref:System.DateTime> valor quando esse valor reflete uma hora sem referência a um dia específico. Esse uso é semelhante às <xref:System.DateTime.TimeOfDay%2A?displayProperty=nameWithType> Propriedades e <xref:System.DateTimeOffset.TimeOfDay%2A?displayProperty=nameWithType> , que retornam um <xref:System.TimeSpan> valor que representa a hora sem referência a uma data. Por exemplo, a <xref:System.TimeSpan> estrutura pode ser usada para refletir o horário diário de abertura ou de fechamento de um repositório, ou pode ser usada para representar a hora em que qualquer evento regular ocorre.

O exemplo a seguir define uma `StoreInfo` estrutura que inclui <xref:System.TimeSpan> objetos para horas de abertura e fechamento da loja, bem como um <xref:System.TimeZoneInfo> objeto que representa o fuso horário do repositório. A estrutura também inclui dois métodos, `IsOpenNow` e `IsOpenAt`, que indica se o repositório está aberto em um momento especificado pelo usuário, que se considera estar no fuso horário local.

[!code-csharp[Conceptual.ChoosingDates#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.choosingdates/cs/datetimereplacement1.cs#1)]
[!code-vb[Conceptual.ChoosingDates#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.choosingdates/vb/datetimereplacement1.vb#1)]

A estrutura `StoreInfo` pode ser usada pelo código do cliente como o exposto a seguir.

[!code-csharp[Conceptual.ChoosingDates#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.choosingdates/cs/datetimereplacement1.cs#2)]
[!code-vb[Conceptual.ChoosingDates#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.choosingdates/vb/datetimereplacement1.vb#2)]

## <a name="the-timezoneinfo-class"></a>A classe TimeZoneInfo

A <xref:System.TimeZoneInfo> classe representa qualquer um dos fusos horários da terra e permite a conversão de qualquer data e hora em um fuso horário para seu equivalente em outro fuso horário. A <xref:System.TimeZoneInfo> classe torna possível trabalhar com datas e horas para que qualquer valor de data e hora identifique de forma não ambígua um único ponto no tempo. A <xref:System.TimeZoneInfo> classe também é extensível. Embora isso dependa das informações de fuso horário fornecidas para sistemas Windows e definidos no registro, ele dá suporte à criação de fusos horários personalizados. Ele também dá suporte à serialização e desserialização de informações de fuso horário.

Em alguns casos, aproveitar ao máximo a <xref:System.TimeZoneInfo> classe pode exigir mais trabalho de desenvolvimento. Se os valores de data e hora não estiverem firmemente acoplados aos fusos horários aos quais eles pertencem, será necessário um trabalho adicional. A menos que seu aplicativo forneça algum mecanismo para vincular uma data e hora com seu fuso horário associado, é fácil para um valor de data e hora específico se tornar desassociado de seu fuso horário. Um método de vinculação dessas informações é definir uma classe ou estrutura que contenha o valor de data e hora e seu objeto de fuso horário associado.

Para aproveitar o suporte de fuso horário no .NET, você deve saber o fuso horário ao qual um valor de data e hora pertence quando esse objeto de data e hora é instanciado. O fuso horário geralmente não é conhecido, especialmente em aplicativos Web ou de rede.

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
