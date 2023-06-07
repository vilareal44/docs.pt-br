---
title: Noções básicas de threading gerenciado
description: Consulte links para outros artigos de Threading gerenciados, abrangendo tópicos como exceções, sincronizando dados, primeiro plano & threads de segundo plano, armazenamento local e muito mais.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- multiple threads
- threading [.NET Framework], multiple threads
- threading [.NET Framework], about threading
- managed threading
ms.assetid: b2944911-0e8f-427d-a8bb-077550618935
ms.openlocfilehash: d4a4ceabf29bd0f6f537e59ba477f9da686b1ef5
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769087"
---
# <a name="managed-threading-basics"></a>Noções básicas de threading gerenciado

Os cinco primeiros tópicos desta seção destinam-se a ajudá-lo a determinar quando usar o threading gerenciado e explicar algumas funcionalidades básicas. Para obter informações sobre as classes que fornecem recursos adicionais, confira [Recursos e objetos de threading](threading-objects-and-features.md) e [Visão geral dos primitivos de sincronização](overview-of-synchronization-primitives.md).  
  
 O restante dos tópicos desta seção abordam tópicos avançados, incluindo a interação de threading gerenciado com o sistema operacional Windows.  
  
> [!NOTE]
> No .NET Framework 4, a biblioteca de paralelismo de tarefas e o PLINQ fornecem APIs para o paralelismo de tarefa e dados em programas multithreading. Para obter mais informações, consulte [programação paralela](../parallel-programming/index.md).  
  
## <a name="in-this-section"></a>Nesta seção

 [Threads e threading](threads-and-threading.md)  
 São discutidas as vantagens e desvantagens de vários threads e são descritos os cenários em que você pode criar threads ou usar threads de pool.  
  
 [Exceções em threads gerenciados](exceptions-in-managed-threads.md)  
 É descrito o comportamento de exceções sem tratamento em threads para diferentes versões do .NET Framework, em particular as situações em que elas resultam no encerramento do aplicativo.  
  
 [Sincronizando dados para multithreading](synchronizing-data-for-multithreading.md)  
 São descritas as estratégias para sincronizar dados em classes que serão usadas com vários threads.  
  
 [Threads em primeiro plano e em segundo plano](foreground-and-background-threads.md)  
 São explicadas as diferenças entre os threads de primeiro plano e segundo plano.  
  
 [Threads gerenciados e não gerenciados no Windows](managed-and-unmanaged-threading-in-windows.md)  
 É discutido o relacionamento entre o threading gerenciado e não gerenciado, são listados os equivalentes gerenciados para APIs de threading do Windows e é discutida a interação de apartments COM e threads gerenciados.  
  
 [Armazenamento local de thread: campos estáticos relativos a thread e slots de dados](thread-local-storage-thread-relative-static-fields-and-data-slots.md)  
 São descritos os mecanismos de armazenamento relativos a threads.  
  
## <a name="reference"></a>Referência

 <xref:System.Threading.Thread>  
 Fornece documentação de referência para a classe **Thread**, que representa um thread gerenciado, seja ele proveniente de código não gerenciado ou criado em um aplicativo gerenciado.  
  
 <xref:System.ComponentModel.BackgroundWorker>  
 É fornecida uma maneira segura de implementar multithreading juntamente com objetos de interface do usuário.  
  
## <a name="related-sections"></a>Seções relacionadas

 [Visão geral de primitivos de sincronização](overview-of-synchronization-primitives.md)  
 São descritas as classes gerenciadas usadas para sincronizar as atividades de vários threads.  
  
 [Práticas recomendadas de Threading gerenciado](managed-threading-best-practices.md)  
 São descritos problemas comuns com o multithreading e estratégias para evitar problemas.  
  
 [Programação paralela](../parallel-programming/index.md)  
 É descrita a biblioteca de paralelismo de tarefas e o PLINQ, que simplifica muito o trabalho de criação de aplicativos do .NET Framework de multithreading e assíncronos.
