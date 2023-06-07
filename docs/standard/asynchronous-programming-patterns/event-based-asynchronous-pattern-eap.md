---
title: Padrão assíncrono baseado em evento (EAP)
description: Consulte links para artigos sobre o padrão assíncrono baseado em evento (EAP) no .NET, como implementação, práticas recomendadas, implementação de um cliente EAP e muito mais.
ms.date: 07/23/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- asynchronous calls
- asynchronous programming, design patterns
- asynchronous programming
ms.assetid: c6baed9f-2a25-4728-9a9a-53b7b14840cf
ms.openlocfilehash: 03b4d914d72b96b882c774565654c022b145b5f2
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768866"
---
# <a name="event-based-asynchronous-pattern-eap"></a>Padrão assíncrono baseado em evento (EAP)

Há várias maneiras de expor recursos assíncronos para o código cliente. O Padrão Assíncrono baseado em evento prescreve uma maneira de as classes apresentarem comportamento assíncrono.  
  
> [!NOTE]
> A partir do .NET Framework 4, a Biblioteca de Paralelismo de Tarefas fornece um novo modelo de programação paralela e assíncrona. Para obter mais informações, veja [Biblioteca de tarefas paralelas (TPL)](../parallel-programming/task-parallel-library-tpl.md) e [Padrão assíncrono baseado em tarefa (TAP)](task-based-asynchronous-pattern-tap.md).
  
## <a name="in-this-section"></a>Nesta seção

 [Visão geral do padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md)  
 Descreve como o Padrão Assíncrono Baseado em Evento disponibiliza as vantagens de aplicativos de vários threads enquanto oculta muitos problemas complexos inerentes ao design com vários threads.  
  
 [Implementando o Padrão Assíncrono baseado em Evento](implementing-the-event-based-asynchronous-pattern.md)  
 Descreve a maneira padronizada de empacotar uma classe com recursos assíncronos.  
  
 [Práticas recomendadas para a implementação do padrão assíncrono baseado em evento](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)  
 Descreve as exigências para expor recursos assíncronos de acordo com o Padrão Assíncrono Baseado em Evento.  
  
 [Decidindo quando implementar o padrão assíncrono baseado em evento](deciding-when-to-implement-the-event-based-asynchronous-pattern.md)  
 Descreve como determinar quando você deve optar por implementar o Padrão assíncrono baseado em evento, em vez do padrão <xref:System.IAsyncResult> representado pelo [APM (Modelo de programação assíncrona)](asynchronous-programming-model-apm.md)
  
 [Como implementar um componente compatível com o padrão assíncrono baseado em evento](component-that-supports-the-event-based-asynchronous-pattern.md)  
 Descreve como criar um componente que implemente o Padrão assíncrono baseado em evento. É implementado usando classes do auxiliar do namespace <xref:System.ComponentModel?displayProperty=nameWithType>, o que garante que o componente funcione corretamente em qualquer modelo de aplicativo.  

 [Como implementar um cliente do Padrão Assíncrono baseado em Evento](how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md)  
 Descreve como criar um cliente que usa um componente que implemente o Padrão assíncrono baseado em evento.
  
 [Como usar componentes compatíveis com o padrão assíncrono baseado em evento](how-to-use-components-that-support-the-event-based-asynchronous-pattern.md)  
 Descreve como usar um componente com suporte ao Padrão Assíncrono Baseado em Evento.  
  
## <a name="reference"></a>Referência

 <xref:System.ComponentModel.AsyncOperation>  
 Descreve a classe <xref:System.ComponentModel.AsyncOperation> e tem links a todos os seus membros.  
  
 <xref:System.ComponentModel.AsyncOperationManager>  
 Descreve a classe <xref:System.ComponentModel.AsyncOperationManager> e tem links a todos os seus membros.  
  
 <xref:System.ComponentModel.BackgroundWorker>  
 Descreve o componente <xref:System.ComponentModel.BackgroundWorker> e tem links a todos os seus membros.  
  
## <a name="related-sections"></a>Seções relacionadas

 [Biblioteca de tarefas paralelas (TPL)](../parallel-programming/task-parallel-library-tpl.md)  
 Descreve um modelo de programação para operações paralelas e assíncronas.  
  
 [Threading](../threading/index.md)  
 Descreve recursos de multithreading no .NET.  
  
## <a name="see-also"></a>Veja também

- [Práticas recomendadas de Threading gerenciado](../threading/managed-threading-best-practices.md)
- [Eventos](../events/index.md)
- [Padrões de design de programação assíncrona](index.md)
