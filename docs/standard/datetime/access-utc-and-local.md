---
title: Como acessar o UTC predefinido e os objetos de fuso horário local
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], local
- predefined time zones
- UTC times, predefined
- local time zone access
- time zones [.NET Framework], retrieving
- time zones [.NET Framework], UTC
ms.assetid: 961fb70b-83f0-4dab-a042-cb5fcd817cf5
ms.openlocfilehash: ebb07800b2a35f4faf312dc55b8c5679079b4b68
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291403"
---
# <a name="how-to-access-the-predefined-utc-and-local-time-zone-objects"></a>Como acessar o UTC predefinido e os objetos de fuso horário local

A <xref:System.TimeZoneInfo> classe fornece duas propriedades <xref:System.TimeZoneInfo.Utc%2A> e <xref:System.TimeZoneInfo.Local%2A> , que dão ao seu código acesso a objetos de fuso horário predefinidos. Este tópico discute como acessar os objetos <xref:System.TimeZoneInfo> retornados por essas propriedades.

### <a name="to-access-the-coordinated-universal-time-utc-timezoneinfo-object"></a>Para acessar o objeto TimeZoneInfo de UTC (Tempo Universal Coordenado)

1. Use a `static` `Shared` Propriedade (em Visual Basic) <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> para acessar o tempo universal coordenado.

2. Em vez de atribuir o <xref:System.TimeZoneInfo> objeto retornado pela propriedade a uma variável de objeto, continue acessando o tempo universal coordenado por meio da <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> propriedade.

### <a name="to-access-the-local-time-zone"></a>Para acessar o fuso horário local

1. Use a `static` `Shared` Propriedade (em Visual Basic) <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> para acessar o fuso horário do sistema local.

2. Em vez de atribuir o <xref:System.TimeZoneInfo> objeto retornado pela propriedade a uma variável de objeto, continue a acessar o fuso horário local por meio da <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> propriedade.

## <a name="example"></a>Exemplo

O código a seguir usa <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> as <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> Propriedades e para converter uma hora do fuso horário padrão do leste dos EUA e do Canadá, bem como para exibir o nome do fuso horário para o console do.

[!code-csharp[System.TimeZone2.Concepts#13](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#13)]
[!code-vb[System.TimeZone2.Concepts#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#13)]

Você sempre deve acessar o fuso horário local por meio da <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> propriedade em vez de atribuir o fuso horário local a uma <xref:System.TimeZoneInfo> variável de objeto. Da mesma forma, você sempre deve acessar o tempo universal coordenado por meio da <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> propriedade em vez de atribuir a zona UTC a uma <xref:System.TimeZoneInfo> variável de objeto. Isso impede que a <xref:System.TimeZoneInfo> variável de objeto seja invalidada por uma chamada para o <xref:System.TimeZoneInfo.ClearCachedData%2A?displayProperty=nameWithType> método.

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Que o <xref:System> namespace seja importado com a `using` instrução (necessária no código C#).

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
- [Localizando os fusos horários definidos em um sistema local](finding-the-time-zones-on-local-system.md)
- [Como criar uma instância de um objeto TimeZoneInfo](instantiate-time-zone-info.md)
