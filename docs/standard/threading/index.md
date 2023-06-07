---
title: Threading gerenciado
description: Consulte links para artigos sobre threads gerenciados no .NET que abrangem as noções básicas, práticas recomendadas, objetos de Threading & recursos, páginas de referência & mais.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], about threading
- managed threading
ms.assetid: 7b46a7d9-c6f1-46d1-a947-ae97471bba87
ms.openlocfilehash: 570db45138c85c4252967404da4404d434660d69
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599744"
---
# <a name="managed-threading"></a>Threading gerenciado
Quer você esteja desenvolvendo para computadores com um processador, quer para com vários, é conveniente que seu aplicativo forneça uma interação mais dinâmica com o usuário, mesmo se o aplicativo, no momento, estiver realizando outro trabalho. Usar vários threads de execução é uma das maneiras mais eficazes de manter seu aplicativo responsivo ao usuário e, ao mesmo tempo, usar o processador entre ou, até mesmo, durante os eventos do usuário. Embora esta seção introduza os conceitos básicos de threading, ele se concentra nos conceitos de threading gerenciado e em como usá-lo.  
  
> [!NOTE]
> Começando no .NET Framework 4, a programação multi-threaded ficou bastante simplificada com as classes <xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>, o [PLINQ (LINQ paralelo)](../parallel-programming/introduction-to-plinq.md), as novas classes de coleção simultânea no namespace <xref:System.Collections.Concurrent?displayProperty=nameWithType> e um novo modelo de programação que se baseia no conceito de tarefas e não em threads. Para obter mais informações, consulte [programação paralela](../parallel-programming/index.md).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Noções básicas sobre Threading gerenciado](managed-threading-basics.md)  
 Fornece uma visão geral de threading gerenciado e descreve quando usar vários threads.  
  
 [Usando threads e threading](using-threads-and-threading.md)  
 Explica como criar, iniciar, pausar, retomar e abortar threads.  
  
 [Práticas recomendadas de Threading gerenciado](managed-threading-best-practices.md)  
 Aborda os níveis de sincronização, como evitar deadlocks e condições de corrida, além de outros problemas de threading.  
  
 [Objetos e recursos de Threading](threading-objects-and-features.md)  
 Descreve as classes gerenciadas que você pode usar para sincronizar as atividades de threads e os dados de objetos acessados em threads diferentes, bem como fornece uma visão geral dos threads de pool do thread.  
  
## <a name="reference"></a>Referência  
 <xref:System.Threading>  
 Contém classes para uso e sincronização de threads gerenciados.  
  
 <xref:System.Collections.Concurrent>  
 Contém classes de coleção que são seguras para uso com vários threads.  
  
 <xref:System.Threading.Tasks>  
 Contém classes para criação e agendamento de tarefas de processamento simultâneo.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Domínios do aplicativo](../../framework/app-domains/application-domains.md)  
 Fornece uma visão geral de domínios do aplicativo e seu uso pelo Common Language Infrastructure.  
  
 [E/s de arquivo assíncrono](../io/asynchronous-file-i-o.md)  
 Descreve as vantagens de desempenho e a operação básica da E/S assíncrona.  
  
 [TAP (Padrão Assíncrono Baseado em Tarefa)](../asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)  
 Fornece uma visão geral do padrão recomendado para programação assíncrona no .NET.  
  
 [Chamar métodos síncronos de forma assíncrona](../asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md)  
 Explica como chamar métodos nos threads de pool do thread usando recursos internos de representantes.  
  
 [Programação paralela](../parallel-programming/index.md)  
 Descreve as bibliotecas de programação paralela, que simplificam o uso de vários threads em aplicativos.  
  
 [LINQ paralelo (PLINQ)](../parallel-programming/introduction-to-plinq.md)  
 Descreve um sistema para executar consultas paralelamente, de modo a aproveitar vários processadores.
