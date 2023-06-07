---
title: Criando uma instância de um objeto DateTimeOffset
description: Leia como instanciar (criar uma instância de) um objeto DateTimeOffset no .NET. Saiba mais sobre data & literais de tempo, construtores, conversão de tipo implícito & mais.
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- instantiating time zone objects
- time zone objects [.NET Framework], instantiation
- DateTimeOffset structure, converting to DateTime
- DateTimeOffset structure, instantiating
ms.assetid: 9648375f-d368-4373-a976-3332ece00c0a
ms.openlocfilehash: c2b71a2a98353a4ec9ed249acf18939dd4740e99
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768892"
---
# <a name="instantiating-a-datetimeoffset-object"></a>Criando uma instância de um objeto DateTimeOffset

A <xref:System.DateTimeOffset> estrutura oferece várias maneiras de criar novos <xref:System.DateTimeOffset> valores. Muitos deles correspondem diretamente aos métodos disponíveis para instanciar novos <xref:System.DateTime> valores, com aprimoramentos que permitem que você especifique o deslocamento do valor de data e hora do UTC (tempo Universal Coordenado). Em particular, você pode criar uma instância de um <xref:System.DateTimeOffset> valor das seguintes maneiras:

- Usando um literal de data e hora.

- Chamando um <xref:System.DateTimeOffset> Construtor.

- Convertendo implicitamente um valor em <xref:System.DateTimeOffset> valor.

- Analisando a representação de cadeia de caracteres de data e hora.

Este tópico fornece mais detalhes e exemplos de código que ilustram esses métodos de instanciação de novos <xref:System.DateTimeOffset> valores.

## <a name="date-and-time-literals"></a>Literais de data e hora

Para idiomas que dão suporte a ele, uma das maneiras mais comuns de instanciar um <xref:System.DateTime> valor é fornecer a data e a hora como um valor literal embutido em código. Por exemplo, o código de Visual Basic a seguir cria um <xref:System.DateTime> objeto cujo valor é 1 de janeiro de 2008, às 10:00.

[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#1)]

<xref:System.DateTimeOffset>os valores também podem ser inicializados usando literais de data e hora ao usar linguagens que dão suporte a <xref:System.DateTime> literais. Por exemplo, o código de Visual Basic a seguir cria um <xref:System.DateTimeOffset> objeto.

[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#2)]

À medida que a saída do console é mostrada, o <xref:System.DateTimeOffset> valor criado dessa forma é atribuído ao deslocamento do fuso horário local. Isso significa que um <xref:System.DateTimeOffset> valor atribuído usando um literal de caractere não identificará um único ponto de tempo se o código for executado em computadores diferentes.

## <a name="datetimeoffset-constructors"></a>Construtores de DateTimeOffset

O <xref:System.DateTimeOffset> tipo define seis construtores. Quatro delas correspondem diretamente aos <xref:System.DateTime> construtores, com um parâmetro adicional do tipo <xref:System.TimeSpan> que define o deslocamento de data e hora do UTC. Eles permitem que você defina um <xref:System.DateTimeOffset> valor com base no valor de seus componentes individuais de data e hora. Por exemplo, o código a seguir usa esses quatro construtores para criar uma instância <xref:System.DateTimeOffset> de objetos com valores idênticos de 7/1/2008 12:05 am + 01:00.

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#3)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#3)]

Observe que, quando o valor do <xref:System.DateTimeOffset> objeto instanciado usando um <xref:System.Globalization.PersianCalendar> objeto como um dos argumentos para seu construtor é exibido para o console, ele é expresso como uma data no gregoriano em vez do calendário persa. Para gerar uma data usando o calendário persa, consulte o exemplo no <xref:System.Globalization.PersianCalendar> tópico.

Os outros dois construtores criam um <xref:System.DateTimeOffset> objeto a partir de um <xref:System.DateTime> valor. O primeiro deles tem um único parâmetro, o <xref:System.DateTime> valor a ser convertido em um <xref:System.DateTimeOffset> valor. O deslocamento do valor resultante <xref:System.DateTimeOffset> depende da <xref:System.DateTime.Kind%2A> Propriedade do parâmetro único do construtor. Se seu valor for <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> , o deslocamento será definido como igual a <xref:System.TimeSpan.Zero?displayProperty=nameWithType> . Caso contrário, o deslocamento será definido como igual ao fuso horário local. O exemplo a seguir ilustra o uso desse construtor para criar uma instância de <xref:System.DateTimeOffset> objetos que representam UTC e o fuso horário local:

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#4)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#4)]

> [!NOTE]
> Chamar a sobrecarga do <xref:System.DateTimeOffset> Construtor que tem um único <xref:System.DateTime> parâmetro é equivalente a executar uma conversão implícita de um <xref:System.DateTime> valor em um <xref:System.DateTimeOffset> valor.

O segundo construtor que cria um <xref:System.DateTimeOffset> objeto de um <xref:System.DateTime> valor tem dois parâmetros: o <xref:System.DateTime> valor a ser convertido e um <xref:System.TimeSpan> valor que representa o deslocamento de data e hora do UTC. Esse valor de deslocamento deve corresponder à <xref:System.DateTime.Kind%2A> Propriedade do primeiro parâmetro do construtor ou um <xref:System.ArgumentException> é gerado. Se a <xref:System.DateTime.Kind%2A> Propriedade do primeiro parâmetro for <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> , o valor do segundo parâmetro deverá ser <xref:System.TimeSpan.Zero?displayProperty=nameWithType> . Se a <xref:System.DateTime.Kind%2A> Propriedade do primeiro parâmetro for <xref:System.DateTimeKind.Local?displayProperty=nameWithType> , o valor do segundo parâmetro deverá ser o deslocamento do fuso horário do sistema local. Se a <xref:System.DateTime.Kind%2A> Propriedade do primeiro parâmetro for <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> , o deslocamento poderá ser qualquer valor válido. O código a seguir ilustra as chamadas para esse construtor para converter <xref:System.DateTime> em <xref:System.DateTimeOffset> valores.

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#5)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#5)]

## <a name="implicit-type-conversion"></a>Conversão de tipo implícita

O <xref:System.DateTimeOffset> tipo dá suporte a uma conversão de tipo implícito: de um <xref:System.DateTime> valor para um <xref:System.DateTimeOffset> valor. (Uma conversão implícita de tipo é uma conversão de um tipo para outro que não requer uma conversão explícita (em C#) ou conversão (no Visual Basic) e não perde informações). Isso torna o código, como o seguinte, possível.

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#6)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#6)]

O deslocamento do valor resultante <xref:System.DateTimeOffset> depende do valor da <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> propriedade. Se seu valor for <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> , o deslocamento será definido como igual a <xref:System.TimeSpan.Zero?displayProperty=nameWithType> . Se seu valor for <xref:System.DateTimeKind.Local?displayProperty=nameWithType> ou <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> , o deslocamento será definido igual ao do fuso horário local.

## <a name="parsing-the-string-representation-of-a-date-and-time"></a>Analisando a representação da cadeia de caracteres de uma data e hora

O <xref:System.DateTimeOffset> tipo dá suporte a quatro métodos que permitem converter a representação de cadeia de caracteres de uma data e hora em um <xref:System.DateTimeOffset> valor:

- <xref:System.DateTimeOffset.Parse%2A>, que tenta converter a representação de cadeia de caracteres de uma data e hora em um <xref:System.DateTimeOffset> valor e gera uma exceção se a conversão falhar.

- <xref:System.DateTimeOffset.TryParse%2A>, que tenta converter a representação de cadeia de caracteres de uma data e hora em um <xref:System.DateTimeOffset> valor e retorna `false` se a conversão falha.

- <xref:System.DateTimeOffset.ParseExact%2A>, que tenta converter a representação de cadeia de caracteres de uma data e hora em um formato especificado para um <xref:System.DateTimeOffset> valor. Na ocorrência de erros, o método lança uma exceção.

- <xref:System.DateTimeOffset.TryParseExact%2A>, que tenta converter a representação de cadeia de caracteres de uma data e hora em um formato especificado para um <xref:System.DateTimeOffset> valor. O método retornará `false` se a conversão falhar.

O exemplo a seguir ilustra as chamadas para cada um desses quatro métodos de conversão de cadeia de caracteres para criar uma instância de um <xref:System.DateTimeOffset> valor.

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#7)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#7)]

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
