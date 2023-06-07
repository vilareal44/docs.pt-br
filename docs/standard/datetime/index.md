---
title: Datas, horas e fusos horários
ms.date: 04/10/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- time zone objects [.NET Framework]
- date and time data [.NET Framework]
- time zones [.NET Framework]
- times [.NET Framework], time zones
- time [.NET Framework], time zones
ms.assetid: 295c16e0-641b-4771-94b3-39c1ffa98c13
ms.openlocfilehash: 86602cd6e662b1b1057832247babc558ef67b79f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84276927"
---
# <a name="dates-times-and-time-zones"></a>Datas, horas e fusos horários

Além da estrutura <xref:System.DateTime> básica, o .NET fornece as seguintes classes que dão suporte para trabalhar com fusos horários:

* <xref:System.TimeZone>

  Use esta classe para trabalhar com o fuso horário local do sistema e a zona UTC (Tempo Universal Coordenado). A funcionalidade da <xref:System.TimeZone> classe é amplamente substituída pela <xref:System.TimeZoneInfo> classe.

* <xref:System.TimeZoneInfo>

  Use essa classe para trabalhar com qualquer fuso horário que esteja predefinido em um sistema, para criar novos fusos horários e para converter facilmente datas e horas de um fuso horário para outro. Para novos desenvolvimentos, use a classe <xref:System.TimeZoneInfo> em vez da classe <xref:System.TimeZone>.

* <xref:System.DateTimeOffset>

  Use essa estrutura para trabalhar com datas e horas cujo deslocamento (ou diferença) em relação ao horário UTC é conhecido. A estrutura <xref:System.DateTimeOffset> combina um valor de data e hora com o deslocamento desse horário em relação ao UTC. Devido à sua relação com o UTC, um valor individual de data e hora identifica, sem ambiguidade um único ponto no tempo. Isso aumenta a portabilidade de um computador para outro de um valor de <xref:System.DateTimeOffset> em relação a um valor de <xref:System.DateTime>.

Esta seção da documentação fornece as informações de que você precisa para trabalhar com fusos horários e para criar aplicativos com reconhecimento de fuso horário que podem converter datas e horas de um fuso horário para outro.

## <a name="in-this-section"></a>Nesta seção

[Visão geral sobre fuso horário](time-zone-overview.md) Aborda a terminologia, os conceitos e as questões envolvidas na criação de aplicativos com reconhecimento de fuso horário.

[Escolhendo entre DateTime, DateTimeOffset, TimeSpan e TimeZoneInfo](choosing-between-datetime.md) Discute quando usar os <xref:System.DateTime> <xref:System.DateTimeOffset> tipos, e <xref:System.TimeZoneInfo> ao trabalhar com dados de data e hora.

[Encontrando os fusos horários definidos em um sistema local](finding-the-time-zones-on-local-system.md) Descreve como enumerar os fusos horários encontrados em um sistema local.

[Como enumerar os fusos horários presentes em um computador](enumerate-time-zones.md) Fornece exemplos que enumeram os fusos horários definidos no Registro de um computador e que permitem que os usuários selecionem um fuso horário predefinido em uma lista.

[Como acessar os objetos de fuso horário UTC e local predefinidos](access-utc-and-local.md) Descreve como acessar o Tempo Universal Coordenado e o fuso horário local.

[Como criar uma instância de um objeto TimeZoneInfo](instantiate-time-zone-info.md) Descreve como criar uma instância de um objeto <xref:System.TimeZoneInfo> do Registro do sistema local.

[Criando uma instância de um objeto DateTimeOffset](instantiating-a-datetimeoffset-object.md) Discute as maneiras de criar uma instância de um objeto <xref:System.DateTimeOffset> e maneiras de converter um valor <xref:System.DateTime> em um valor <xref:System.DateTimeOffset>.

[Como criar fusos horários sem regras de ajuste](create-time-zones-without-adjustment-rules.md) Descreve como criar um fuso horário personalizado sem suporte para transição de/para o horário de verão.

[Como criar fusos horários com regras de ajuste](create-time-zones-with-adjustment-rules.md) Descreve como criar um fuso horário personalizado com suporte a uma ou mais transições de/para o horário de verão.

[Salvando e restaurando fusos horários](saving-and-restoring-time-zones.md) Descreve o suporte de <xref:System.TimeZoneInfo> para serialização e desserialização dos dados de fuso horário e ilustra alguns dos cenários nos quais esses recursos podem ser usados.

[Como salvar fusos horários em um recurso inserido](save-time-zones-to-an-embedded-resource.md) Descreve como criar um fuso horário personalizado e salvar suas informações em um arquivo de recurso.

[Como restaurar fusos horários de um recurso inserido](restore-time-zones-from-an-embedded-resource.md) Descreve como criar instâncias de fusos horários personalizados que foram salvos em um arquivo de recurso inserido.

[Executando operações aritméticas com datas e horas](performing-arithmetic-operations.md) Discute os problemas envolvidos ao adicionar, subtrair e comparar valores <xref:System.DateTime> e <xref:System.DateTimeOffset>.

[Como usar fusos horários em aritmética de data e hora](use-time-zones-in-arithmetic.md) Descreve como realizar uma aritmética de data e hora que reflita as regras de ajuste de um fuso horário.

[Convertendo entre DateTime e DateTimeOffset](converting-between-datetime-and-offset.md) Descreve como converter entre valores <xref:System.DateTime> e <xref:System.DateTimeOffset>.

[Convertendo horários entre fusos horários](converting-between-time-zones.md) Descreve como converter horários de um fuso horário para outro.

[Como resolver horários ambíguos](resolve-ambiguous-times.md) Descreve como resolver um horário ambíguo, mapeando-o para o horário padrão do fuso horário.

[Como permitir que os usuários resolvam horários ambíguos](let-users-resolve-ambiguous-times.md) Descreve como permitir que um usuário determine o mapeamento entre uma hora local ambígua e o Tempo Universal Coordenado.

## <a name="reference"></a>Referência

<xref:System.TimeZoneInfo?displayProperty=nameWithType>
