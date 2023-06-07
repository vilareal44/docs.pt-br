---
title: Como acionar e consumir eventos
description: Gerar & consumir eventos no .NET. Veja exemplos que usam o delegado EventHandler, o <TEventArgs> delegado EventHandler, & um delegado personalizado.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- events [.NET Framework], raising
- raising events
- events [.NET Framework], samples
ms.assetid: 42afade7-3a02-4f2e-868b-95845f302f8f
ms.openlocfilehash: 4054e1a26c3392870af994a6eceafae92176a332
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769269"
---
# <a name="how-to-raise-and-consume-events"></a>Como acionar e consumir eventos
Os exemplos neste tópico mostram como trabalhar com eventos. Elas incluem exemplos do representante <xref:System.EventHandler>, o representante <xref:System.EventHandler%601> e um representante personalizado, para ilustrar eventos com e sem dados.  
  
 Os exemplos usam conceitos descritos no artigo [Eventos](index.md).  
  
## <a name="example"></a>Exemplo  
 O primeiro exemplo mostra como gerar e consumir um evento que não tem dados. Ele contém uma classe denominada `Counter` que tem um evento chamado `ThresholdReached`. Esse evento é gerado quando um valor de contador é igual ou maior que um valor de limite. O representante <xref:System.EventHandler> é associado ao evento, porque nenhum dado de evento é fornecido.  
  
 [!code-csharp[EventsOverview#5](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programnodata.cs#5)]
 [!code-vb[EventsOverview#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1nodata.vb#5)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como gerar e consumir um evento que fornece dados. O representante <xref:System.EventHandler%601> é associado ao evento e uma instância de um objeto de dados de evento personalizado é fornecida.  
  
 [!code-cpp[EventsOverview#6](../../../samples/snippets/cpp/VS_Snippets_CLR/eventsoverview/cpp/programwithdata.cpp#6)]
 [!code-csharp[EventsOverview#6](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programwithdata.cs#6)]
 [!code-vb[EventsOverview#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1withdata.vb#6)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como declarar um representante para um evento. O representante é chamado `ThresholdReachedEventHandler`. Isso é apenas uma ilustração. Normalmente, você não precisa declarar um representante para um evento, porque pode usar o representante <xref:System.EventHandler> ou <xref:System.EventHandler%601>. Você deve declarar um representante somente em cenários raros, como tornar a classe disponível para o código herdado que não pode usar genéricos.  
  
 [!code-csharp[EventsOverview#7](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programwithdelegate.cs#7)]
 [!code-vb[EventsOverview#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1withdelegate.vb#7)]  
  
## <a name="see-also"></a>Veja também

- [Eventos](index.md)
